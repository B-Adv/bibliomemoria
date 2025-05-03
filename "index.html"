tema.startsWith(ultimoTema) && !temasParciais.slice(0, -1).some(t => t.trim().toLowerCase() === tema)
                );
                
                // Limitar a 5 sugestões
                const sugestoesLimitadas = sugestoes.slice(0, 5);
                
                // Exibir sugestões
                containerSugestoes.innerHTML = '';
                sugestoesLimitadas.forEach(tema => {
                    const tag = document.createElement('span');
                    tag.className = 'tag';
                    tag.textContent = tema;
                    tag.style.cursor = 'pointer';
                    
                    tag.addEventListener('click', () => {
                        // Substituir o último tema parcial pelo tema completo
                        temasParciais[temasParciais.length - 1] = ' ' + tema;
                        input.value = temasParciais.join(',');
                        
                        // Adicionar uma vírgula no final para facilitar a digitação de outro tema
                        input.value += ', ';
                        
                        // Focar no input
                        input.focus();
                        
                        // Limpar sugestões
                        containerSugestoes.innerHTML = '';
                    });
                    
                    containerSugestoes.appendChild(tag);
                });
            }
        }

        // Função para inicializar animações
        function inicializarAnimacoes() {
            // Animar estatísticas na carga inicial
            const statNumbers = document.querySelectorAll('.stat-number');
            
            statNumbers.forEach(stat => {
                const valorFinal = parseInt(stat.textContent);
                let valorAtual = 0;
                
                const intervalo = setInterval(() => {
                    valorAtual = Math.min(valorAtual + Math.ceil(valorFinal / 20), valorFinal);
                    stat.textContent = valorAtual;
                    
                    if (valorAtual >= valorFinal) {
                        clearInterval(intervalo);
                    }
                }, 50);
            });
        }

        // Função para criar um valor hash simples para um texto
        function hashCode(str) {
            let hash = 0;
            for (let i = 0; i < str.length; i++) {
                const char = str.charCodeAt(i);
                hash = ((hash << 5) - hash) + char;
                hash = hash & hash; // Converter para inteiro de 32 bits
            }
            return hash;
        }

        // Inicializar aplicação quando o DOM estiver carregado
        document.addEventListener('DOMContentLoaded', inicializarApp);
    </script>
</body>
</html>
                document.getElementById('loading-trechos').style.display = 'none';
                
                // Preparar resultado da análise com visualizações mais elaboradas
                const resultado = document.getElementById('analise-resultado');
                
                // Análise de dados básicos
                const totalTrechos = trechos.length;
                const autores = [...new Set(trechos.map(t => t.autor))];
                const obras = [...new Set(trechos.map(t => t.obra))];
                
                // Contagem de temas
                const contadorTemas = {};
                trechos.forEach(trecho => {
                    trecho.temas.forEach(tema => {
                        contadorTemas[tema] = (contadorTemas[tema] || 0) + 1;
                    });
                });
                
                // Ordenar temas por frequência
                const temasOrdenados = Object.entries(contadorTemas)
                    .sort((a, b) => b[1] - a[1])
                    .map(([tema, count]) => `${tema} (${count})`);
                
                // Contagem de autores
                const contadorAutores = {};
                trechos.forEach(trecho => {
                    contadorAutores[trecho.autor] = (contadorAutores[trecho.autor] || 0) + 1;
                });
                
                // Autores mais frequentes
                const autoresOrdenados = Object.entries(contadorAutores)
                    .sort((a, b) => b[1] - a[1])
                    .slice(0, 5) // Top 5
                    .map(([autor, count]) => `${autor} (${count})`);
                
                // Verificar se há datas de leitura
                const trechosComData = trechos.filter(t => t.dataLeitura);
                let periodosLeitura = '';
                
                if (trechosComData.length > 0) {
                    // Ordenar por data
                    trechosComData.sort((a, b) => new Date(a.dataLeitura) - new Date(b.dataLeitura));
                    
                    const primeiraData = formatarData(trechosComData[0].dataLeitura);
                    const ultimaData = formatarData(trechosComData[trechosComData.length - 1].dataLeitura);
                    
                    periodosLeitura = `
                        <div class="detail-item">
                            <strong>Período de Leituras:</strong> De ${primeiraData} a ${ultimaData}
                        </div>
                    `;
                }
                
                // Identificar padrões de leitura
                let padroesLeitura = '';
                if (trechos.length >= 5) {
                    // Verificar se há concentração em determinados autores
                    const autorPrincipal = autoresOrdenados[0]?.split(' (')[0];
                    const countAutorPrincipal = contadorAutores[autorPrincipal] || 0;
                    
                    if (countAutorPrincipal > trechos.length * 0.4) {
                        padroesLeitura += `Você tem uma forte preferência por ${autorPrincipal}, que representa ${Math.round(countAutorPrincipal/trechos.length*100)}% dos seus trechos. `;
                    }
                    
                    // Verificar temas recorrentes
                    const temaPrincipal = temasOrdenados[0]?.split(' (')[0];
                    const countTemaPrincipal = contadorTemas[temaPrincipal] || 0;
                    
                    if (countTemaPrincipal > trechos.length * 0.3) {
                        padroesLeitura += `O tema "${temaPrincipal}" é recorrente em sua coleção, aparecendo em ${Math.round(countTemaPrincipal/trechos.length*100)}% dos trechos. `;
                    }
                    
                    if (padroesLeitura) {
                        padroesLeitura = `
                            <div class="detail-item">
                                <strong>Padrões Identificados:</strong>
                                <p>${padroesLeitura}</p>
                            </div>
                        `;
                    }
                }
                
                // Dados para gráficos (versão simplificada)
                const chartData = {
                    autores: Object.entries(contadorAutores).map(([autor, count]) => ({ name: autor, value: count })),
                    temas: Object.entries(contadorTemas).map(([tema, count]) => ({ name: tema, value: count }))
                };
                
                // Construir HTML do resultado com visualizações
                resultado.innerHTML = `
                    <h2>Análise da Sua Coleção</h2>
                    
                    <div class="stats-container">
                        <div class="stat-card">
                            <div class="stat-number">${totalTrechos}</div>
                            <div class="stat-label">Total de Trechos</div>
                        </div>
                        
                        <div class="stat-card">
                            <div class="stat-number">${autores.length}</div>
                            <div class="stat-label">Autores Únicos</div>
                        </div>
                        
                        <div class="stat-card">
                            <div class="stat-number">${obras.length}</div>
                            <div class="stat-label">Obras Únicas</div>
                        </div>
                        
                        <div class="stat-card">
                            <div class="stat-number">${Object.keys(contadorTemas).length}</div>
                            <div class="stat-label">Temas Únicos</div>
                        </div>
                    </div>
                    
                    <div class="detail-item">
                        <strong>Temas Mais Frequentes:</strong>
                        <div class="tag-container" style="margin-top: 10px;">
                            ${Object.entries(contadorTemas)
                                .sort((a, b) => b[1] - a[1])
                                .slice(0, 10)
                                .map(([tema, count]) => 
                                    `<span class="tag" style="font-size: ${Math.max(0.8, count/totalTrechos*3)}rem;">${tema} (${count})</span>`
                                )
                                .join('')
                            }
                        </div>
                    </div>
                    
                    <div class="detail-item">
                        <strong>Autores Mais Frequentes:</strong>
                        <ul style="margin-top: 10px;">
                            ${Object.entries(contadorAutores)
                                .sort((a, b) => b[1] - a[1])
                                .slice(0, 5)
                                .map(([autor, count]) => 
                                    `<li>${autor}: ${count} trecho(s) (${Math.round(count/totalTrechos*100)}%)</li>`
                                )
                                .join('')
                            }
                        </ul>
                    </div>
                    
                    ${periodosLeitura}
                    
                    ${padroesLeitura}
                    
                    <div class="detail-item">
                        <strong>Conexões de Temas:</strong>
                        <p>Temas que frequentemente aparecem juntos:</p>
                        <ul style="margin-top: 10px;">
                            ${encontrarConexoesTemas().map(conexao => 
                                `<li>${conexao.temas.join(' + ')}: ${conexao.count} ocorrências</li>`
                            ).join('')}
                        </ul>
                    </div>
                    
                    <div class="detail-item">
                        <strong>Recomendações Personalizadas:</strong>
                        <p>Com base nos seus trechos, você poderia se interessar por:</p>
                        <ul style="margin-top: 10px;">
                            <li>Explorar mais obras que abordem ${temasOrdenados.slice(0, 2).map(t => t.split(' (')[0]).join(' e ')}.</li>
                            <li>Criar coleções temáticas agrupando trechos relacionados.</li>
                            <li>Registrar reflexões pessoais sobre os temas recorrentes em seu acervo.</li>
                        </ul>
                    </div>
                `;
                
                // Exibir resultado com animação
                resultado.style.opacity = '0';
                resultado.style.display = 'block';
                setTimeout(() => {
                    resultado.style.opacity = '1';
                }, 100);
            }, 2000);
        }

        // Função para encontrar conexões entre temas
        function encontrarConexoesTemas() {
            const conexoes = {};
            
            // Analisar trechos com múltiplos temas
            trechos.forEach(trecho => {
                if (trecho.temas.length >= 2) {
                    // Gerar todas as combinações de 2 temas
                    for (let i = 0; i < trecho.temas.length - 1; i++) {
                        for (let j = i + 1; j < trecho.temas.length; j++) {
                            const tema1 = trecho.temas[i];
                            const tema2 = trecho.temas[j];
                            const key = [tema1, tema2].sort().join('|');
                            
                            conexoes[key] = (conexoes[key] || 0) + 1;
                        }
                    }
                }
            });
            
            // Converter para array e ordenar
            const result = Object.entries(conexoes)
                .map(([key, count]) => ({
                    temas: key.split('|'),
                    count
                }))
                .sort((a, b) => b.count - a.count)
                .slice(0, 5); // Top 5 conexões
            
            return result;
        }

        // Função para exportar a coleção
        function exportarColecao() {
            if (trechos.length === 0) {
                mostrarToast('Você ainda não possui trechos para exportar.', 3000, true);
                return;
            }
            
            const formato = document.getElementById('export-format').value;
            let conteudo = '';
            let nomeArquivo = `bibliomemoria_export_${formatarDataArquivo(new Date())}`;
            let tipo = '';
            
            switch (formato) {
                case 'json':
                    conteudo = JSON.stringify(trechos, null, 2);
                    nomeArquivo += '.json';
                    tipo = 'application/json';
                    break;
                
                case 'txt':
                    trechos.forEach(trecho => {
                        conteudo += `TÍTULO: ${trecho.titulo}\n`;
                        conteudo += `TRECHO: ${trecho.trecho}\n`;
                        conteudo += `AUTOR: ${trecho.autor}\n`;
                        conteudo += `OBRA: ${trecho.obra}\n`;
                        if (trecho.pagina) conteudo += `PÁGINA/LOCALIZAÇÃO: ${trecho.pagina}\n`;
                        if (trecho.temas.length > 0) conteudo += `TEMAS: ${trecho.temas.join(', ')}\n`;
                        if (trecho.observacoes) conteudo += `OBSERVAÇÕES: ${trecho.observacoes}\n`;
                        if (trecho.dataLeitura) conteudo += `DATA DA LEITURA: ${formatarData(trecho.dataLeitura)}\n`;
                        if (trecho.insights) conteudo += `INSIGHTS: ${trecho.insights}\n`;
                        conteudo += `\n---\n\n`;
                    });
                    nomeArquivo += '.txt';
                    tipo = 'text/plain';
                    break;
                
                case 'markdown':
                    conteudo = `# Biblioteca Pessoal de Trechos\n\n`;
                    conteudo += `Exportado em ${new Date().toLocaleString('pt-BR')}\n\n`;
                    
                    trechos.forEach(trecho => {
                        conteudo += `## ${trecho.titulo}\n\n`;
                        conteudo += `> ${trecho.trecho}\n\n`;
                        conteudo += `**Autor:** ${trecho.autor}  \n`;
                        conteudo += `**Obra:** ${trecho.obra}  \n`;
                        if (trecho.pagina) conteudo += `**Página/Localização:** ${trecho.pagina}  \n`;
                        if (trecho.temas.length > 0) conteudo += `**Temas:** ${trecho.temas.join(', ')}  \n`;
                        if (trecho.dataLeitura) conteudo += `**Data da Leitura:** ${formatarData(trecho.dataLeitura)}  \n`;
                        
                        if (trecho.observacoes) {
                            conteudo += `\n### Observações\n\n`;
                            conteudo += `${trecho.observacoes}\n\n`;
                        }
                        
                        if (trecho.insights) {
                            conteudo += `\n### Insights\n\n`;
                            conteudo += `${trecho.insights}\n\n`;
                        }
                        
                        conteudo += `---\n\n`;
                    });
                    nomeArquivo += '.md';
                    tipo = 'text/markdown';
                    break;
                
                case 'html':
                    conteudo = `<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bibliomemória - Exportação</title>
    <style>
        * { font-family: Arial, sans-serif; margin: 0; padding: 0; }
        body { max-width: 800px; margin: 0 auto; padding: 20px; color: #333; }
        h1 { color: #4a5568; margin-bottom: 20px; text-align: center; }
        h2 { color: #805ad5; margin-top: 30px; margin-bottom: 15px; border-bottom: 1px solid #eee; padding-bottom: 10px; }
        h3 { color: #3182ce; margin-top: 20px; margin-bottom: 10px; }
        .trecho { margin-bottom: 40px; border-bottom: 2px solid #eee; padding-bottom: 20px; }
        .trecho-content { font-style: italic; margin: 15px 0; line-height: 1.8; border-left: 3px solid #805ad5; padding-left: 15px; background-color: #f9f9f9; padding: 15px; border-radius: 4px; }
        .meta-item { margin-bottom: 15px; }
        .meta-label { font-weight: bold; color: #4a5568; margin-right: 8px; }
        .insights { background-color: #f9f9f9; padding: 15px; margin-top: 15px; border-left: 3px solid #48bb78; }
        .observacoes { background-color: #f9f9f9; padding: 15px; margin-top: 15px; border-left: 3px solid #3182ce; }
        .tag { display: inline-block; background-color: #e9d8fd; color: #553c9a; padding: 4px 8px; border-radius: 4px; font-size: 0.8rem; margin-right: 5px; margin-bottom: 5px; }
        .footer { text-align: center; margin-top: 40px; color: #718096; font-size: 0.9rem; }
        .header-info { text-align: center; margin-bottom: 30px; color: #718096; font-size: 0.9rem; }
    </style>
</head>
<body>
    <h1>Minha Coleção de Trechos</h1>
    <div class="header-info">Exportado em ${new Date().toLocaleString('pt-BR')} • Total: ${trechos.length} trechos</div>
    
    <div class="colecao">`;
                    
                    trechos.forEach(trecho => {
                        conteudo += `
        <div class="trecho">
            <h2>${trecho.titulo}</h2>
            <div class="trecho-content">${trecho.trecho}</div>
            
            <div class="meta-item"><span class="meta-label">Autor:</span> ${trecho.autor}</div>
            <div class="meta-item"><span class="meta-label">Obra:</span> ${trecho.obra}</div>`;
                        
                        if (trecho.pagina) conteudo += `
            <div class="meta-item"><span class="meta-label">Página/Localização:</span> ${trecho.pagina}</div>`;
                        
                        if (trecho.temas.length > 0) {
                            conteudo += `
            <div class="meta-item">
                <span class="meta-label">Temas:</span> 
                <div>
                    ${trecho.temas.map(tema => `<span class="tag">${tema}</span>`).join('')}
                </div>
            </div>`;
                        }
                        
                        if (trecho.dataLeitura) conteudo += `
            <div class="meta-item"><span class="meta-label">Data da Leitura:</span> ${formatarData(trecho.dataLeitura)}</div>`;
                        
                        if (trecho.observacoes) conteudo += `
            <div class="observacoes">
                <h3>Observações Pessoais</h3>
                <p>${trecho.observacoes}</p>
            </div>`;
                        
                        if (trecho.insights) conteudo += `
            <div class="insights">
                <h3>Insights</h3>
                <p>${trecho.insights}</p>
            </div>`;
                        
                        conteudo += `
        </div>`;
                    });
                    
                    conteudo += `
    </div>
    <div class="footer">
        Gerado por Bibliomemória - Seu repositório pessoal de trechos literários e insights
    </div>
</body>
</html>`;
                    nomeArquivo += '.html';
                    tipo = 'text/html';
                    break;
                
                case 'pdf':
                    // Na versão real, usaria uma biblioteca de PDF
                    // Aqui exportamos como HTML otimizado para impressão
                    conteudo = `<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bibliomemória - Exportação PDF</title>
    <style>
        @page { margin: 2cm; }
        body { font-family: Arial, sans-serif; line-height: 1.6; color: #333; }
        h1 { text-align: center; color: #4a5568; margin-bottom: 20px; }
        h2 { color: #805ad5; border-bottom: 1px solid #ddd; padding-bottom: 5px; margin-top: 30px; }
        .trecho { margin-bottom: 30px; page-break-inside: avoid; }
        .trecho-content { font-style: italic; margin: 15px 0; line-height: 1.8; border-left: 3px solid #805ad5; padding-left: 15px; background-color: #f9f9f9; padding: 10px; }
        .meta { margin: 15px 0; }
        .meta-item { margin-bottom: 5px; }
        .meta-label { font-weight: bold; color: #4a5568; }
        .insights { background-color: #f9f9f9; padding: 10px; margin-top: 15px; border-left: 3px solid #48bb78; }
        .footer { text-align: center; margin-top: 30px; font-size: 0.9em; color: #718096; }
        .page-break { page-break-after: always; }
        @media print {
            .no-print { display: none; }
            body { font-size: 12pt; }
            h1 { font-size: 18pt; }
            h2 { font-size: 14pt; }
        }
    </style>
    <script>
        window.onload = function() {
            window.print();
        }
    </script>
</head>
<body>
    <h1>Minha Coleção de Trechos</h1>
    <p style="text-align: center; margin-bottom: 30px;">Exportado em ${new Date().toLocaleString('pt-BR')} • Total: ${trechos.length} trechos</p>
    
    <div class="no-print" style="text-align: center; margin: 20px 0; padding: 10px; background-color: #f0f0f0;">
        Esta página foi otimizada para impressão. Pressione Ctrl+P (ou Cmd+P) para imprimir ou salvar como PDF.
    </div>`;
                    
                    trechos.forEach((trecho, index) => {
                        conteudo += `
    <div class="trecho">
        <h2>${trecho.titulo}</h2>
        <div class="trecho-content">${trecho.trecho}</div>
        
        <div class="meta">
            <div class="meta-item"><span class="meta-label">Autor:</span> ${trecho.autor}</div>
            <div class="meta-item"><span class="meta-label">Obra:</span> ${trecho.obra}</div>`;
                        
                        if (trecho.pagina) conteudo += `
            <div class="meta-item"><span class="meta-label">Página/Localização:</span> ${trecho.pagina}</div>`;
                        
                        if (trecho.temas.length > 0) conteudo += `
            <div class="meta-item"><span class="meta-label">Temas:</span> ${trecho.temas.join(', ')}</div>`;
                        
                        if (trecho.dataLeitura) conteudo += `
            <div class="meta-item"><span class="meta-label">Data da Leitura:</span> ${formatarData(trecho.dataLeitura)}</div>`;
                        
                        conteudo += `
        </div>`;
                        
                        if (trecho.observacoes) conteudo += `
        <div class="observacoes">
            <h3>Observações Pessoais</h3>
            <p>${trecho.observacoes}</p>
        </div>`;
                        
                        if (trecho.insights) conteudo += `
        <div class="insights">
            <h3>Insights</h3>
            <p>${trecho.insights}</p>
        </div>`;
                        
                        conteudo += `
    </div>`;
                        
                        // Adicionar quebra de página exceto no último item
                        if (index < trechos.length - 1) {
                            conteudo += `<div class="page-break"></div>`;
                        }
                    });
                    
                    conteudo += `
    <div class="footer">
        Gerado por Bibliomemória - Seu repositório pessoal de trechos literários e insights
    </div>
</body>
</html>`;
                    nomeArquivo += '_print.html';
                    tipo = 'text/html';
                    break;
            }
            
            // Criar elemento para download
            const element = document.createElement('a');
            element.setAttribute('href', `data:${tipo};charset=utf-8,${encodeURIComponent(conteudo)}`);
            element.setAttribute('download', nomeArquivo);
            element.style.display = 'none';
            
            document.body.appendChild(element);
            element.click();
            document.body.removeChild(element);
            
            mostrarToast(`Coleção exportada com sucesso no formato ${formato.toUpperCase()}.`);
        }

        // Função para mostrar mensagem toast
        function mostrarToast(mensagem, duracao = 3000, isError = false) {
            const toast = document.getElementById('toast');
            toast.textContent = mensagem;
            toast.className = isError ? 'toast error' : 'toast';
            toast.style.display = 'block';
            
            if (duracao > 0) {
                setTimeout(() => {
                    toast.style.display = 'none';
                }, duracao);
            }
        }

        // Função auxiliar para formatar data
        function formatarData(dataString) {
            if (!dataString) return '';
            
            const data = new Date(dataString);
            return data.toLocaleDateString('pt-BR');
        }

        // Função auxiliar para formatar data para nome de arquivo
        function formatarDataArquivo(data) {
            const dia = String(data.getDate()).padStart(2, '0');
            const mes = String(data.getMonth() + 1).padStart(2, '0');
            const ano = data.getFullYear();
            
            return `${ano}${mes}${dia}`;
        }

        // Função para atualizar estatísticas
        function atualizarEstatisticas() {
            document.getElementById('total-trechos').textContent = trechos.length;
            
            const autores = [...new Set(trechos.map(t => t.autor))];
            document.getElementById('total-autores').textContent = autores.length;
            
            const todosTemas = trechos.reduce((acc, trecho) => [...acc, ...trecho.temas], []);
            const temaUnicos = [...new Set(todosTemas)];
            document.getElementById('total-temas').textContent = temaUnicos.length;
            
            const trechosComInsights = trechos.filter(t => t.insights).length;
            document.getElementById('trechos-com-insights').textContent = trechosComInsights;
        }

        // Função para atualizar progresso de leitura
        function atualizarProgressoLeitura() {
            const totalObras = [...new Set(trechos.map(t => t.obra))].length;
            const totalAutores = [...new Set(trechos.map(t => t.autor))].length;
            
            // Calcular uma métrica de progresso (exemplo simplificado)
            // Em uma versão real, poderia considerar metas do usuário
            const progressoBase = Math.min(100, trechos.length * 2);
            const progressoPorcentagem = progressoBase + '%';
            
            // Atualizar barra de progresso
            document.getElementById('progress-bar').style.width = progressoPorcentagem;
            document.getElementById('progress-percent').textContent = progressoPorcentagem;
        }

        // Função para inicializar sugestões de temas
        function inicializarSugestoesTemas() {
            // Lista de temas comuns para sugerir
            const temasComuns = [
                'amor', 'morte', 'vida', 'tempo', 'natureza', 'sociedade', 'política', 
                'religião', 'filosofia', 'existência', 'identidade', 'liberdade', 'arte',
                'beleza', 'ciência', 'conhecimento', 'educação', 'ética', 'família',
                'história', 'humanidade', 'justiça', 'linguagem', 'memória', 'mente',
                'moral', 'poesia', 'poder', 'realidade', 'sonho', 'transcendência',
                'verdade', 'violência'
            ];
            
            // Exibir alguns temas populares como sugestões iniciais
            const containerSugestoes = document.getElementById('temas-sugeridos');
            
            // Escolher aleatoriamente 6 temas para sugerir
            const sugestoes = temasComuns.sort(() => 0.5 - Math.random()).slice(0, 6);
            
            sugestoes.forEach(tema => {
                const tag = document.createElement('span');
                tag.className = 'tag';
                tag.textContent = tema;
                tag.style.cursor = 'pointer';
                
                tag.addEventListener('click', () => {
                    const temasInput = document.getElementById('temas');
                    const temasAtuais = temasInput.value 
                        ? temasInput.value.split(',').map(t => t.trim()) 
                        : [];
                    
                    // Adicionar o tema se não estiver presente
                    if (!temasAtuais.includes(tema)) {
                        temasAtuais.push(tema);
                        temasInput.value = temasAtuais.join(', ');
                    }
                });
                
                containerSugestoes.appendChild(tag);
            });
        }

        // Função para mostrar sugestões de temas baseadas no input
        function mostrarSugestoesTemas() {
            const input = document.getElementById('temas');
            const containerSugestoes = document.getElementById('temas-sugeridos');
            
            // Lista completa de temas comuns
            const temasComuns = [
                'amor', 'morte', 'vida', 'tempo', 'natureza', 'sociedade', 'política', 
                'religião', 'filosofia', 'existência', 'identidade', 'liberdade', 'arte',
                'beleza', 'ciência', 'conhecimento', 'educação', 'ética', 'família',
                'história', 'humanidade', 'justiça', 'linguagem', 'memória', 'mente',
                'moral', 'poesia', 'poder', 'realidade', 'sonho', 'transcendência',
                'verdade', 'violência'
            ];
            
            // Obter o último tema digitado (após a última vírgula)
            const temasParciais = input.value.split(',');
            const ultimoTema = temasParciais[temasParciais.length - 1].trim().toLowerCase();
            
            // Se o último tema tiver pelo menos 2 caracteres, sugerir temas similares
            if (ultimoTema.length >= 2) {
                // Filtrar temas que começam com o que foi digitado
                const sugestoes = temasComuns.filter(tema => 
                    tema.startsWith(ultimoTema) && !<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bibliomemória - Seu Repositório Literário Pessoal</title>
    <style>
        :root {
            --primary-color: #4a5568;
            --secondary-color: #718096;
            --accent-color: #805ad5;
            --light-color: #f7fafc;
            --dark-color: #1a202c;
            --success-color: #48bb78;
            --warning-color: #ed8936;
            --danger-color: #e53e3e;
            
            /* Novas cores para o mapa mental */
            --mindmap-color-1: #805ad5; /* Roxo - Principal */
            --mindmap-color-2: #3182ce; /* Azul */
            --mindmap-color-3: #48bb78; /* Verde */
            --mindmap-color-4: #ed8936; /* Laranja */
            --mindmap-color-5: #e53e3e; /* Vermelho */
            --mindmap-color-6: #d69e2e; /* Amarelo */
            --mindmap-color-7: #667eea; /* Índigo */
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--light-color);
            color: var(--dark-color);
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            padding: 20px 0;
            margin-bottom: 30px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
        }

        /* Elemento decorativo para o header */
        header::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -50px;
            width: 300px;
            height: 300px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 50%;
            z-index: 0;
        }

        header h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
        }

        header p {
            font-size: 1.1rem;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .tabs {
            display: flex;
            border-bottom: 1px solid var(--secondary-color);
            margin-bottom: 30px;
            overflow-x: auto;
            padding-bottom: 2px;
        }

        .tab-button {
            padding: 10px 20px;
            background: none;
            border: none;
            cursor: pointer;
            font-size: 1rem;
            font-weight: bold;
            color: var(--secondary-color);
            transition: all 0.3s ease;
            white-space: nowrap;
        }

        .tab-button:hover {
            color: var(--accent-color);
            transform: translateY(-2px);
        }

        .tab-button.active {
            color: var(--accent-color);
            border-bottom: 3px solid var(--accent-color);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: var(--primary-color);
        }

        input, textarea, select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        input:focus, textarea:focus, select:focus {
            border-color: var(--accent-color);
            outline: none;
            box-shadow: 0 0 0 3px rgba(128, 90, 213, 0.2);
        }

        textarea {
            min-height: 150px;
            resize: vertical;
        }

        button {
            background-color: var(--accent-color);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: bold;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        button:hover {
            background-color: #6b46c1;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        button:active {
            transform: translateY(0);
        }

        .btn-secondary {
            background-color: var(--secondary-color);
        }

        .btn-secondary:hover {
            background-color: #5a6778;
        }

        .btn-icon {
            margin-right: 8px;
            font-size: 1.2em;
        }

        .search-container {
            display: flex;
            margin-bottom: 20px;
            position: relative;
        }

        .search-container input {
            flex: 1;
            margin-right: 10px;
            padding-left: 40px;
        }

        .search-icon {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--secondary-color);
            font-size: 1.2rem;
        }

        .trechos-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .trecho-card {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            padding: 20px;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .trecho-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 4px;
            height: 100%;
            background-color: var(--accent-color);
        }

        .trecho-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
        }

        .trecho-card h3 {
            margin-bottom: 10px;
            color: var(--accent-color);
        }

        .trecho-content {
            font-style: italic;
            margin-bottom: 15px;
            line-height: 1.8;
            border-left: 3px solid var(--accent-color);
            padding-left: 15px;
            position: relative;
            background-color: rgba(128, 90, 213, 0.05);
            padding: 10px 15px;
            border-radius: 0 4px 4px 0;
        }

        .trecho-meta {
            font-size: 0.9rem;
            color: var(--secondary-color);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            flex-wrap: wrap;
        }

        .meta-dot {
            display: inline-block;
            width: 3px;
            height: 3px;
            border-radius: 50%;
            background-color: var(--secondary-color);
            margin: 0 8px;
        }

        .tag {
            display: inline-block;
            background-color: #e9d8fd;
            color: #553c9a;
            padding: 6px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            margin-right: 5px;
            margin-bottom: 5px;
            transition: all 0.3s ease;
        }

        .tag:hover {
            transform: translateY(-2px);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .tag.author {
            background-color: #bee3f8;
            color: #2b6cb0;
        }

        .card-buttons {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
        }

        .card-buttons button {
            padding: 8px 16px;
            font-size: 0.9rem;
        }

        .insights-section {
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-top: 15px;
            border-left: 3px solid var(--success-color);
            position: relative;
        }

        .insights-section::before {
            content: '💡';
            position: absolute;
            top: 12px;
            right: 15px;
            font-size: 1.5rem;
            opacity: 0.3;
        }

        .insights-section h4 {
            color: var(--success-color);
            margin-bottom: 10px;
        }

        .filter-container {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-bottom: 20px;
            background-color: rgba(128, 90, 213, 0.05);
            padding: 15px;
            border-radius: 8px;
        }

        .filter-container select {
            flex: 1;
            min-width: 150px;
        }

        .filter-label {
            display: block;
            font-size: 0.85rem;
            margin-bottom: 5px;
            color: var(--primary-color);
        }

        .trecho-details {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            padding: 30px;
            margin-top: 20px;
        }

        .trecho-details h2 {
            color: var(--accent-color);
            margin-bottom: 20px;
            border-bottom: 2px solid #f0f0f0;
            padding-bottom: 10px;
        }

        .trecho-details-content {
            font-size: 1.1rem;
            line-height: 1.8;
            margin-bottom: 25px;
            background-color: #f9f9f9;
            padding: 20px;
            border-radius: 6px;
            font-style: italic;
            position: relative;
        }

        .trecho-details-content::before {
            content: '"';
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 3rem;
            color: rgba(128, 90, 213, 0.2);
            font-family: Georgia, serif;
            line-height: 1;
        }

        .detail-item {
            margin-bottom: 15px;
        }

        .detail-item strong {
            color: var(--primary-color);
            margin-right: 8px;
        }

        .actions-bar {
            display: flex;
            justify-content: space-between;
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #eee;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 100;
            overflow: auto;
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background-color: white;
            margin: 10% auto;
            padding: 30px;
            width: 80%;
            max-width: 700px;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            animation: slideIn 0.3s ease;
            position: relative;
        }

        @keyframes slideIn {
            from { transform: translateY(-50px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .close-modal {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 1.5rem;
            font-weight: bold;
            cursor: pointer;
            color: var(--secondary-color);
            width: 32px;
            height: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        .close-modal:hover {
            color: var(--danger-color);
            background-color: rgba(229, 62, 62, 0.1);
        }

        .modal-title {
            margin-bottom: 20px;
            color: var(--primary-color);
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 20px;
        }

        .loading:after {
            content: "";
            display: inline-block;
            width: 30px;
            height: 30px;
            border: 4px solid #f3f3f3;
            border-radius: 50%;
            border-top: 4px solid var(--accent-color);
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .toast {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: var(--success-color);
            color: white;
            padding: 15px 25px;
            border-radius: 4px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            display: none;
            animation: slideUp 0.3s ease;
        }

        @keyframes slideUp {
            from { transform: translateY(100px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .toast.error {
            background-color: var(--danger-color);
        }

        /* Estilos para o mapa mental */
        .mindmap-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            overflow-x: auto;
            min-height: 600px;
        }

        .mindmap-node {
            padding: 10px 15px;
            border-radius: 6px;
            margin: 5px 0;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            position: relative;
            max-width: 300px;
            width: 100%;
            text-align: center;
        }

        .mindmap-node:hover {
            transform: scale(1.05);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
            z-index: 2;
        }

        .mindmap-level-0 {
            background-color: var(--mindmap-color-1);
            color: white;
            font-weight: bold;
            font-size: 1.2rem;
        }

        .mindmap-level-1 {
            background-color: var(--mindmap-color-2);
            color: white;
            margin-left: 20px;
        }

        .mindmap-level-2 {
            background-color: var(--mindmap-color-3);
            color: white;
            margin-left: 40px;
        }

        .mindmap-level-3 {
            background-color: var(--mindmap-color-4);
            color: white;
            margin-left: 60px;
        }

        .mindmap-level-4 {
            background-color: var(--mindmap-color-5);
            color: white;
            margin-left: 80px;
        }

        .mindmap-level-5 {
            background-color: var(--mindmap-color-6);
            color: white;
            margin-left: 100px;
        }

        .mindmap-level-6 {
            background-color: var(--mindmap-color-7);
            color: white;
            margin-left: 120px;
        }

        .mindmap-connector {
            width: 2px;
            height: 20px;
            background-color: #ccc;
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
        }

        .mindmap-node:not(.mindmap-level-0)::before {
            content: '';
            position: absolute;
            top: 50%;
            left: -20px;
            width: 20px;
            height: 2px;
            background-color: #ccc;
            transform: translateY(-50%);
        }

        .mindmap-children {
            position: relative;
            display: flex;
            flex-direction: column;
            width: 100%;
        }

        /* Estatísticas e indicadores visuais */
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            padding: 20px;
            text-align: center;
            transition: all 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
        }

        .stat-number {
            font-size: 2.5rem;
            font-weight: bold;
            color: var(--accent-color);
            margin-bottom: 5px;
        }

        .stat-label {
            color: var(--secondary-color);
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* Animações para os elementos interativos */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .pulse-animation {
            animation: pulse 2s infinite;
        }

        /* Dicas contextuais */
        .tooltip {
            position: relative;
            display: inline-block;
        }

        .tooltip .tooltip-text {
            visibility: hidden;
            width: 200px;
            background-color: #333;
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            transform: translateX(-50%);
            opacity: 0;
            transition: opacity 0.3s;
        }

        .tooltip:hover .tooltip-text {
            visibility: visible;
            opacity: 1;
        }

        /* Barra de progresso para leituras */
        .progress-container {
            margin-top: 20px;
            background-color: #f0f0f0;
            border-radius: 20px;
            height: 10px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background-color: var(--accent-color);
            width: 0;
            transition: width 1s ease;
        }

        /* Feedback visual para interações */
        .feedback-icon {
            display: inline-block;
            margin-left: 10px;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .feedback-icon.visible {
            opacity: 1;
        }

        /* Estilos para dispositivos móveis */
        @media (max-width: 768px) {
            .trechos-grid {
                grid-template-columns: 1fr;
            }

            .tab-button {
                padding: 8px 12px;
                font-size: 0.9rem;
            }

            .modal-content {
                width: 95%;
                margin: 5% auto;
                padding: 15px;
            }
            
            .stats-container {
                grid-template-columns: 1fr;
            }
            
            .filter-container {
                flex-direction: column;
                gap: 10px;
            }
            
            .mindmap-node {
                max-width: 250px;
            }
        }

        /* Modo escuro */
        @media (prefers-color-scheme: dark) {
            body {
                background-color: #1a202c;
                color: #f7fafc;
            }

            .trecho-card, .trecho-details, .stat-card {
                background-color: #2d3748;
                color: #e2e8f0;
            }

            input, textarea, select {
                background-color: #2d3748;
                color: #e2e8f0;
                border-color: #4a5568;
            }

            .insights-section {
                background-color: #2a3749;
            }

            .trecho-details-content {
                background-color: #3a4a5f;
            }

            .modal-content {
                background-color: #2d3748;
                color: #e2e8f0;
            }
            
            .filter-container {
                background-color: rgba(128, 90, 213, 0.15);
            }
            
            .progress-container {
                background-color: #4a5568;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <h1>Bibliomemória</h1>
            <p>Seu repositório pessoal de trechos literários e insights</p>
        </div>
    </header>

    <div class="container">
        <div class="stats-container">
            <div class="stat-card">
                <div class="stat-number" id="total-trechos">0</div>
                <div class="stat-label">Trechos</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" id="total-autores">0</div>
                <div class="stat-label">Autores</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" id="total-temas">0</div>
                <div class="stat-label">Temas</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" id="trechos-com-insights">0</div>
                <div class="stat-label">Insights</div>
            </div>
        </div>

        <div class="tabs">
            <button class="tab-button active" data-tab="trechos">
                <span class="btn-icon">📚</span> Meus Trechos
            </button>
            <button class="tab-button" data-tab="adicionar">
                <span class="btn-icon">➕</span> Adicionar Trecho
            </button>
            <button class="tab-button" data-tab="analise">
                <span class="btn-icon">📊</span> Análise
            </button>
            <button class="tab-button" data-tab="mapa-mental">
                <span class="btn-icon">🧠</span> Mapa Mental
            </button>
            <button class="tab-button" data-tab="exportar">
                <span class="btn-icon">📤</span> Exportar
            </button>
        </div>

        <!-- Aba de Trechos -->
        <div class="tab-content active" id="trechos">
            <div class="search-container">
                <span class="search-icon">🔍</span>
                <input type="text" id="search-input" placeholder="Buscar por título, autor, tema...">
                <button id="search-button">Buscar</button>
            </div>

            <div class="filter-container">
                <div>
                    <span class="filter-label">Autor</span>
                    <select id="filter-autor">
                        <option value="">Todos os Autores</option>
                    </select>
                </div>
                <div>
                    <span class="filter-label">Obra</span>
                    <select id="filter-obra">
                        <option value="">Todas as Obras</option>
                    </select>
                </div>
                <div>
                    <span class="filter-label">Tema</span>
                    <select id="filter-tema">
                        <option value="">Todos os Temas</option>
                    </select>
                </div>
                <button id="reset-filters" class="btn-secondary">Limpar Filtros</button>
            </div>

            <div id="reading-progress" class="progress-container tooltip">
                <div id="progress-bar" class="progress-bar"></div>
                <span class="tooltip-text">Progresso de leitura: <span id="progress-percent">0%</span></span>
            </div>

            <div id="loading-trechos" class="loading"></div>
            <div id="trechos-grid" class="trechos-grid">
                <!-- Os cards de trechos serão inseridos aqui dinamicamente via JavaScript -->
            </div>
        </div>

        <!-- Aba de Adicionar Trecho -->
        <div class="tab-content" id="adicionar">
            <h2>Adicionar Novo Trecho</h2>
            <form id="trecho-form">
                <div class="form-group">
                    <label for="titulo">Título/Identificação:</label>
                    <input type="text" id="titulo" required>
                </div>

                <div class="form-group">
                    <label for="trecho">Trecho:</label>
                    <textarea id="trecho" required></textarea>
                </div>

                <div class="form-group">
                    <label for="autor">Autor:</label>
                    <input type="text" id="autor" required list="autores-list">
                    <datalist id="autores-list">
                        <!-- Será preenchido via JavaScript -->
                    </datalist>
                </div>

                <div class="form-group">
                    <label for="obra">Obra:</label>
                    <input type="text" id="obra" required list="obras-list">
                    <datalist id="obras-list">
                        <!-- Será preenchido via JavaScript -->
                    </datalist>
                </div>

                <div class="form-group">
                    <label for="pagina">Página ou Localização:</label>
                    <input type="text" id="pagina">
                </div>

                <div class="form-group">
                    <label for="temas">Temas (separados por vírgula):</label>
                    <input type="text" id="temas" placeholder="filosofia, amor, morte...">
                    <div id="temas-sugeridos" style="margin-top: 10px; display: flex; flex-wrap: wrap; gap: 5px;"></div>
                </div>

                <div class="form-group">
                    <label for="observacoes">Observações Pessoais:</label>
                    <textarea id="observacoes"></textarea>
                </div>

                <div class="form-group">
                    <label for="data-leitura">Data da Leitura:</label>
                    <input type="date" id="data-leitura">
                </div>

                <div class="form-group">
                    <button type="submit">
                        <span class="btn-icon">💾</span> Salvar Trecho
                    </button>
                    <button type="button" id="btn-gerar-insights" class="btn-secondary">
                        <span class="btn-icon">💡</span> Gerar Insights
                    </button>
                </div>
            </form>
        </div>

        <!-- Aba de Análise -->
        <div class="tab-content" id="analise">
            <h2>Análise de Coleção</h2>
            <p>Descubra padrões e conexões entre seus trechos salvos.</p>

            <div class="form-group">
                <button id="btn-analisar-colecao">
                    <span class="btn-icon">📈</span> Analisar Minha Coleção
                </button>
            </div>

            <div id="analise-resultado" class="trecho-details" style="display: none;">
                <!-- Os resultados de análise serão inseridos aqui -->
            </div>
        </div>

        <!-- Aba de Mapa Mental -->
        <div class="tab-content" id="mapa-mental">
            <h2>Mapa Mental</h2>
            <p>Visualize conceitos e relações dos seus trechos em formato hierárquico vertical.</p>
            
            <div class="form-group">
                <label for="mindmap-source">Selecione o trecho ou insira um texto:</label>
                <select id="mindmap-source-select">
                    <option value="new">Inserir novo texto</option>
                    <!-- Opções de trechos existentes serão adicionadas via JavaScript -->
                </select>
            </div>
            
            <div class="form-group" id="mindmap-text-container">
                <label for="mindmap-text">Texto para análise:</label>
                <textarea id="mindmap-text" placeholder="Insira o texto para gerar o mapa mental..."></textarea>
            </div>
            
            <div class="form-group">
                <button id="btn-gerar-mapa">
                    <span class="btn-icon">🧠</span> Gerar Mapa Mental
                </button>
            </div>
            
            <div id="mindmap-resultado" style="display: none;">
                <h3>Mapa Mental Gerado</h3>
                <div id="mindmap-container" class="mindmap-container">
                    <!-- O mapa mental será inserido aqui -->
                </div>
                <div class="form-group" style="margin-top: 20px; text-align: center;">
                    <button id="btn-salvar-mapa" class="btn-secondary">
                        <span class="btn-icon">📥</span> Salvar Mapa Mental
                    </button>
                </div>
            </div>
        </div>

        <!-- Aba de Exportar -->
        <div class="tab-content" id="exportar">
            <h2>Exportar Coleção</h2>
            <p>Exporte sua coleção de trechos para diferentes formatos.</p>

            <div class="form-group">
                <label for="export-format">Formato de Exportação:</label>
                <select id="export-format">
                    <option value="json">JSON (para backup)</option>
                    <option value="txt">Texto Simples</option>
                    <option value="html">HTML</option>
                    <option value="markdown">Markdown</option>
                    <option value="pdf">PDF (download como HTML)</option>
                </select>
            </div>

            <div class="form-group">
                <button id="btn-exportar">
                    <span class="btn-icon">📤</span> Exportar Coleção
                </button>
            </div>
        </div>
    </div>

    <!-- Modal de Detalhes do Trecho -->
    <div id="trecho-modal" class="modal">
        <div class="modal-content">
            <span class="close-modal">&times;</span>
            <h2 class="modal-title">Detalhes do Trecho</h2>
            <div id="trecho-modal-content">
                <!-- O conteúdo do trecho será inserido aqui -->
            </div>
        </div>
    </div>

    <!-- Modal de Insights -->
    <div id="insights-modal" class="modal">
        <div class="modal-content">
            <span class="close-modal">&times;</span>
            <h2 class="modal-title">Insights Gerados</h2>
            <div id="insights-content">
                <!-- Os insights gerados serão inseridos aqui -->
            </div>
            <div class="form-group">
                <button id="btn-salvar-insights">
                    <span class="btn-icon">💾</span> Salvar Insights
                </button>
            </div>
        </div>
    </div>

    <!-- Modal de Mapa Mental -->
    <div id="mindmap-modal" class="modal">
        <div class="modal-content" style="width: 90%; max-width: 900px;">
            <span class="close-modal">&times;</span>
            <h2 class="modal-title">Mapa Mental Ampliado</h2>
            <div id="mindmap-modal-content">
                <!-- O mapa mental será inserido aqui -->
            </div>
        </div>
    </div>

    <!-- Notificação Toast -->
    <div id="toast" class="toast"></div>

    <script>
        // Modelo de dados para armazenar os trechos
        let trechos = [];
        const STORAGE_KEY = 'bibliomemoria_trechos';
        const THEME_KEY = 'bibliomemoria_theme';
        
        // Cache para mapas mentais gerados
        let mapasMentaisCache = {};

        // Função para inicializar a aplicação
        function inicializarApp() {
            // Carregar trechos salvos do localStorage
            carregarTrechos();
            
            // Inicializar abas
            inicializarAbas();
            
            // Inicializar eventos
            inicializarEventos();
            
            // Renderizar trechos iniciais
            renderizarTrechos();
            
            // Preencher filtros
            atualizarFiltros();
            
            // Atualizar estatísticas
            atualizarEstatisticas();
            
            // Inicializar sugestões de temas
            inicializarSugestoesTemas();
            
            // Inicializar progress bar
            atualizarProgressoLeitura();
            
            // Inicializar animações
            inicializarAnimacoes();
            
            // Verificar tema do sistema
            verificarTema();
        }

        // Função para carregar trechos do localStorage
        function carregarTrechos() {
            const trechosSalvos = localStorage.getItem(STORAGE_KEY);
            if (trechosSalvos) {
                trechos = JSON.parse(trechosSalvos);
            }
        }

        // Função para salvar trechos no localStorage
        function salvarTrechos() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(trechos));
            atualizarEstatisticas();
        }

        // Função para verificar e aplicar tema
        function verificarTema() {
            const temaPreferido = localStorage.getItem(THEME_KEY) || 'auto';
            
            if (temaPreferido === 'dark') {
                document.body.classList.add('dark-theme');
            } else if (temaPreferido === 'light') {
                document.body.classList.add('light-theme');
            }
            // 'auto' usa a preferência do sistema
        }

        // Função para inicializar o sistema de abas
        function inicializarAbas() {
            const tabButtons = document.querySelectorAll('.tab-button');
            const tabContents = document.querySelectorAll('.tab-content');
            
            tabButtons.forEach(button => {
                button.addEventListener('click', () => {
                    // Remover classes ativas
                    tabButtons.forEach(b => b.classList.remove('active'));
                    tabContents.forEach(c => c.classList.remove('active'));
                    
                    // Adicionar classe ativa à aba clicada
                    button.classList.add('active');
                    
                    // Exibir conteúdo da aba
                    const tabId = button.getAttribute('data-tab');
                    document.getElementById(tabId).classList.add('active');
                    
                    // Ações específicas para cada aba
                    if (tabId === 'mapa-mental') {
                        carregarOpcoesMapaMental();
                    }
                });
            });
        }

        // Função para inicializar eventos
        function inicializarEventos() {
            // Formulário de adicionar trecho
            document.getElementById('trecho-form').addEventListener('submit', adicionarTrecho);
            
            // Botão de busca
            document.getElementById('search-button').addEventListener('click', buscarTrechos);
            
            // Campo de busca (ativar busca ao pressionar Enter)
            document.getElementById('search-input').addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    buscarTrechos();
                }
            });
            
            // Filtros
            document.getElementById('filter-autor').addEventListener('change', filtrarTrechos);
            document.getElementById('filter-obra').addEventListener('change', filtrarTrechos);
            document.getElementById('filter-tema').addEventListener('change', filtrarTrechos);
            
            // Botão para limpar filtros
            document.getElementById('reset-filters').addEventListener('click', () => {
                document.getElementById('filter-autor').value = '';
                document.getElementById('filter-obra').value = '';
                document.getElementById('filter-tema').value = '';
                filtrarTrechos();
            });
            
            // Botão de gerar insights
            document.getElementById('btn-gerar-insights').addEventListener('click', gerarInsights);
            
            // Botão de salvar insights
            document.getElementById('btn-salvar-insights').addEventListener('click', salvarInsights);
            
            // Botão de análise de coleção
            document.getElementById('btn-analisar-colecao').addEventListener('click', analisarColecao);
            
            // Botão de exportar
            document.getElementById('btn-exportar').addEventListener('click', exportarColecao);
            
            // Selector para fonte do mapa mental
            document.getElementById('mindmap-source-select').addEventListener('change', function() {
                const textContainer = document.getElementById('mindmap-text-container');
                const mindmapText = document.getElementById('mindmap-text');
                
                if (this.value === 'new') {
                    textContainer.style.display = 'block';
                    mindmapText.value = '';
                } else {
                    // Preencher com texto do trecho selecionado
                    const trechoSelecionado = trechos.find(t => t.id === this.value);
                    if (trechoSelecionado) {
                        textContainer.style.display = 'block';
                        mindmapText.value = trechoSelecionado.trecho;
                    }
                }
            });
            
            // Botão para gerar mapa mental
            document.getElementById('btn-gerar-mapa').addEventListener('click', gerarMapaMental);
            
            // Botão para salvar mapa mental
            document.getElementById('btn-salvar-mapa').addEventListener('click', salvarMapaMental);
            
            // Campo de temas para sugestões dinâmicas
            document.getElementById('temas').addEventListener('input', mostrarSugestoesTemas);
            
            // Fechar modais
            document.querySelectorAll('.close-modal').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('.modal').forEach(modal => {
                        modal.style.display = 'none';
                    });
                });
            });
            
            // Fechar modal ao clicar fora
            window.addEventListener('click', (e) => {
                document.querySelectorAll('.modal').forEach(modal => {
                    if (e.target === modal) {
                        modal.style.display = 'none';
                    }
                });
            });
            
            // Interação com elementos do trecho
            document.addEventListener('click', (e) => {
                // Delegação de eventos para os botões dos cards
                if (e.target.classList.contains('btn-view') || e.target.closest('.btn-view')) {
                    const btn = e.target.classList.contains('btn-view') ? e.target : e.target.closest('.btn-view');
                    const id = btn.getAttribute('data-id');
                    abrirDetalhesTrecho(id);
                }
                
                if (e.target.classList.contains('btn-delete') || e.target.closest('.btn-delete')) {
                    const btn = e.target.classList.contains('btn-delete') ? e.target : e.target.closest('.btn-delete');
                    const id = btn.getAttribute('data-id');
                    if (confirm('Tem certeza que deseja excluir este trecho?')) {
                        excluirTrecho(id);
                    }
                }
                
                // Permitir clicar nas tags para filtrar
                if (e.target.classList.contains('tag')) {
                    const tag = e.target.textContent;
                    if (e.target.classList.contains('author')) {
                        document.getElementById('filter-autor').value = tag;
                    } else {
                        document.getElementById('filter-tema').value = tag;
                    }
                    filtrarTrechos();
                    document.querySelector('[data-tab="trechos"]').click();
                }
            });
        }

        // Função para adicionar um novo trecho
        function adicionarTrecho(e) {
            e.preventDefault();
            
            // Obter valores do formulário
            const titulo = document.getElementById('titulo').value;
            const trechoTexto = document.getElementById('trecho').value;
            const autor = document.getElementById('autor').value;
            const obra = document.getElementById('obra').value;
            const pagina = document.getElementById('pagina').value;
            const temasInput = document.getElementById('temas').value;
            const observacoes = document.getElementById('observacoes').value;
            const dataLeitura = document.getElementById('data-leitura').value;
            
            // Processar temas (separar por vírgula e remover espaços)
            const temas = temasInput.split(',').map(tema => tema.trim()).filter(tema => tema);
            
            // Criar objeto do trecho
            const novoTrecho = {
                id: Date.now().toString(), // ID único baseado em timestamp
                titulo,
                trecho: trechoTexto,
                autor,
                obra,
                pagina,
                temas,
                observacoes,
                dataLeitura,
                dataCriacao: new Date().toISOString(),
                insights: window.insightsTemporarios || null // Insights serão adicionados depois
            };
            
            // Limpar insights temporários
            window.insightsTemporarios = null;
            
            // Adicionar trecho à lista
            trechos.unshift(novoTrecho); // Adicionar no início para aparecer primeiro
            
            // Salvar no localStorage
            salvarTrechos();
            
            // Atualizar interface
            renderizarTrechos();
            atualizarFiltros();
            atualizarProgressoLeitura();
            
            // Adicionar feedback visual
            const feedbackIcon = document.createElement('span');
            feedbackIcon.classList.add('feedback-icon');
            feedbackIcon.innerHTML = '✅';
            document.querySelector('#trecho-form button[type="submit"]').appendChild(feedbackIcon);
            
            // Mostrar e esconder feedback
            feedbackIcon.classList.add('visible');
            setTimeout(() => {
                feedbackIcon.classList.remove('visible');
                setTimeout(() => {
                    feedbackIcon.remove();
                }, 300);
            }, 2000);
            
            // Limpar formulário
            document.getElementById('trecho-form').reset();
            
            // Exibir mensagem de sucesso
            mostrarToast('Trecho adicionado com sucesso!');
            
            // Voltar para a aba de trechos
            document.querySelector('[data-tab="trechos"]').click();
        }

        // Função para renderizar os trechos na grade
        function renderizarTrechos(trechosFiltrados = null) {
            const grid = document.getElementById('trechos-grid');
            grid.innerHTML = '';
            
            // Determinar quais trechos exibir (todos ou filtrados)
            const trechosExibidos = trechosFiltrados || trechos;
            
            if (trechosExibidos.length === 0) {
                grid.innerHTML = '<p>Nenhum trecho encontrado. Adicione seu primeiro trecho!</p>';
                return;
            }
            
            trechosExibidos.forEach(trecho => {
                const card = document.createElement('div');
                card.className = 'trecho-card';
                card.setAttribute('data-id', trecho.id);
                
                // Truncar trecho se for muito longo
                const trechoResumido = trecho.trecho.length > 150 
                    ? trecho.trecho.substring(0, 150) + '...' 
                    : trecho.trecho;
                
                // Preparar tags de temas
                const tagsHTML = trecho.temas.map(tema => 
                    `<span class="tag">${tema}</span>`
                ).join('');
                
                // Estrutura do card com emojis e melhorias visuais
                card.innerHTML = `
                    <h3>${trecho.titulo}</h3>
                    <div class="trecho-content">${trechoResumido}</div>
                    <div class="trecho-meta">
                        <span class="tag author">${trecho.autor}</span>
                        <span class="meta-dot"></span>
                        <span>${trecho.obra}</span>
                        ${trecho.pagina ? `<span class="meta-dot"></span><span>Pág. ${trecho.pagina}</span>` : ''}
                    </div>
                    <div class="tag-container">
                        ${tagsHTML}
                    </div>
                    ${trecho.insights ? `
                    <div class="insights-section">
                        <h4>Insights</h4>
                        <p>${trecho.insights.length > 100 ? trecho.insights.substring(0, 100) + '...' : trecho.insights}</p>
                    </div>
                    ` : ''}
                    <div class="card-buttons">
                        <button class="btn-view" data-id="${trecho.id}">
                            <span class="btn-icon">👁️</span> Ver Detalhes
                        </button>
                        <button class="btn-delete" data-id="${trecho.id}">
                            <span class="btn-icon">🗑️</span> Excluir
                        </button>
                    </div>
                `;
                
                grid.appendChild(card);
                
                // Adicionar efeito de entrada com atraso crescente
                setTimeout(() => {
                    card.style.opacity = '1';
                    card.style.transform = 'translateY(0)';
                }, 50 * grid.children.length);
            });
        }

        // Função para abrir modal com detalhes do trecho
        function abrirDetalhesTrecho(id) {
            const trecho = trechos.find(t => t.id === id);
            if (!trecho) return;
            
            const modal = document.getElementById('trecho-modal');
            const content = document.getElementById('trecho-modal-content');
            
            // Preparar tags de temas
            const tagsHTML = trecho.temas.map(tema => 
                `<span class="tag">${tema}</span>`
            ).join('');
            
            content.innerHTML = `
                <div class="trecho-details-content">${trecho.trecho}</div>
                
                <div class="detail-item">
                    <strong>Autor:</strong> ${trecho.autor}
                </div>
                
                <div class="detail-item">
                    <strong>Obra:</strong> ${trecho.obra}
                </div>
                
                ${trecho.pagina ? `
                <div class="detail-item">
                    <strong>Página/Localização:</strong> ${trecho.pagina}
                </div>
                ` : ''}
                
                <div class="detail-item">
                    <strong>Temas:</strong> 
                    <div class="tag-container">
                        ${tagsHTML}
                    </div>
                </div>
                
                ${trecho.dataLeitura ? `
                <div class="detail-item">
                    <strong>Data da Leitura:</strong> ${formatarData(trecho.dataLeitura)}
                </div>
                ` : ''}
                
                ${trecho.observacoes ? `
                <div class="detail-item">
                    <strong>Observações Pessoais:</strong>
                    <p>${trecho.observacoes}</p>
                </div>
                ` : ''}
                
                ${trecho.insights ? `
                <div class="insights-section">
                    <h4>Insights:</h4>
                    <p>${trecho.insights}</p>
                </div>
                ` : ''}
                
                <div class="actions-bar">
                    <button id="btn-editar-trecho" data-id="${trecho.id}">
                        <span class="btn-icon">✏️</span> Editar
                    </button>
                    <button id="btn-gerar-insights-modal" data-id="${trecho.id}">
                        <span class="btn-icon">💡</span> Gerar Insights
                    </button>
                    <button id="btn-criar-mapa-mental" data-id="${trecho.id}">
                        <span class="btn-icon">🧠</span> Mapa Mental
                    </button>
                </div>
            `;
            
            // Adicionar evento para o botão de editar
            content.querySelector('#btn-editar-trecho').addEventListener('click', () => {
                editarTrecho(trecho.id);
                modal.style.display = 'none';
            });
            
            // Adicionar evento para o botão de gerar insights
            content.querySelector('#btn-gerar-insights-modal').addEventListener('click', () => {
                gerarInsightsParaTrecho(trecho.id);
                modal.style.display = 'none';
            });
            
            // Adicionar evento para o botão de criar mapa mental
            content.querySelector('#btn-criar-mapa-mental').addEventListener('click', () => {
                modal.style.display = 'none';
                document.querySelector('[data-tab="mapa-mental"]').click();
                
                // Selecionar o trecho no dropdown e gerar o mapa
                const select = document.getElementById('mindmap-source-select');
                select.value = trecho.id;
                
                // Disparar o evento change para atualizar o textarea
                const event = new Event('change');
                select.dispatchEvent(event);
                
                // Gerar o mapa mental automaticamente
                gerarMapaMental();
            });
            
            // Exibir modal com animação
            modal.style.display = 'block';
            setTimeout(() => {
                modal.querySelector('.modal-content').style.opacity = '1';
            }, 10);
        }

        // Função para editar um trecho
        function editarTrecho(id) {
            const trecho = trechos.find(t => t.id === id);
            if (!trecho) return;
            
            // Preencher o formulário com os dados do trecho
            document.getElementById('titulo').value = trecho.titulo;
            document.getElementById('trecho').value = trecho.trecho;
            document.getElementById('autor').value = trecho.autor;
            document.getElementById('obra').value = trecho.obra;
            document.getElementById('pagina').value = trecho.pagina || '';
            document.getElementById('temas').value = trecho.temas.join(', ');
            document.getElementById('observacoes').value = trecho.observacoes || '';
            document.getElementById('data-leitura').value = trecho.dataLeitura || '';
            
            // Remover trecho existente
            trechos = trechos.filter(t => t.id !== id);
            
            // Mudar para a aba de adicionar
            document.querySelector('[data-tab="adicionar"]').click();
            
            // Alterar texto do botão de envio
            const submitButton = document.querySelector('#trecho-form button[type="submit"]');
            submitButton.innerHTML = '<span class="btn-icon">🔄</span> Atualizar Trecho';
            
            // Adicionar efeito de destaque no formulário
            const form = document.getElementById('trecho-form');
            form.classList.add('editing');
            setTimeout(() => {
                form.classList.remove('editing');
            }, 1000);
            
            // Mostrar mensagem
            mostrarToast('Editando trecho. Faça as alterações necessárias e clique em Atualizar.', 5000);
        }

        // Função para excluir um trecho
        function excluirTrecho(id) {
            // Animação de saída
            const card = document.querySelector(`.trecho-card[data-id="${id}"]`);
            if (card) {
                card.style.opacity = '0';
                card.style.transform = 'translateY(20px)';
                
                setTimeout(() => {
                    // Remover trecho
                    trechos = trechos.filter(t => t.id !== id);
                    salvarTrechos();
                    renderizarTrechos();
                    atualizarFiltros();
                    atualizarProgressoLeitura();
                    mostrarToast('Trecho excluído com sucesso.');
                }, 300);
            } else {
                trechos = trechos.filter(t => t.id !== id);
                salvarTrechos();
                renderizarTrechos();
                atualizarFiltros();
                atualizarProgressoLeitura();
                mostrarToast('Trecho excluído com sucesso.');
            }
        }

        // Função para buscar trechos
        function buscarTrechos() {
            const termo = document.getElementById('search-input').value.toLowerCase();
            
            if (!termo) {
                renderizarTrechos(); // Exibir todos se o termo estiver vazio
                return;
            }
            
            // Mostrar indicador de carregamento
            document.getElementById('loading-trechos').style.display = 'block';
            
            // Simular processamento para dar feedback visual
            setTimeout(() => {
                const resultados = trechos.filter(trecho => 
                    trecho.titulo.toLowerCase().includes(termo) ||
                    trecho.trecho.toLowerCase().includes(termo) ||
                    trecho.autor.toLowerCase().includes(termo) ||
                    trecho.obra.toLowerCase().includes(termo) ||
                    trecho.temas.some(tema => tema.toLowerCase().includes(termo)) ||
                    (trecho.observacoes && trecho.observacoes.toLowerCase().includes(termo))
                );
                
                document.getElementById('loading-trechos').style.display = 'none';
                renderizarTrechos(resultados);
                
                if (resultados.length === 0) {
                    document.getElementById('trechos-grid').innerHTML = 
                        `<p>Nenhum resultado encontrado para "${termo}". Tente outros termos de busca.</p>`;
                } else {
                    mostrarToast(`${resultados.length} resultado(s) encontrado(s).`);
                    
                    // Destacar termos de busca nos resultados
                    document.querySelectorAll('.trecho-content').forEach(element => {
                        const conteudo = element.innerHTML;
                        const conteudoDestacado = conteudo.replace(
                            new RegExp(termo, 'gi'), 
                            match => `<mark>${match}</mark>`
                        );
                        element.innerHTML = conteudoDestacado;
                    });
                }
            }, 300);
        }

        // Função para atualizar os filtros com base nos dados disponíveis
        function atualizarFiltros() {
            // Coletar valores únicos
            const autores = [...new Set(trechos.map(t => t.autor))];
            const obras = [...new Set(trechos.map(t => t.obra))];
            
            // Coletar temas únicos (achatando o array de arrays)
            const todosTemas = trechos.reduce((acc, trecho) => [...acc, ...trecho.temas], []);
            const temas = [...new Set(todosTemas)];
            
            // Preencher filtros
            preencherSelect('filter-autor', autores);
            preencherSelect('filter-obra', obras);
            preencherSelect('filter-tema', temas);
            
            // Preencher também as listas de sugestão para o formulário
            preencherDatalist('autores-list', autores);
            preencherDatalist('obras-list', obras);
        }

        // Função auxiliar para preencher um select com opções
        function preencherSelect(id, opcoes) {
            const select = document.getElementById(id);
            
            // Manter a primeira opção (Todos)
            const primeiraOpcao = select.options[0];
            select.innerHTML = '';
            select.appendChild(primeiraOpcao);
            
            // Adicionar opções ordenadas alfabeticamente
            opcoes.sort().forEach(opcao => {
                const option = document.createElement('option');
                option.value = opcao;
                option.textContent = opcao;
                select.appendChild(option);
            });
        }

        // Função auxiliar para preencher um datalist
        function preencherDatalist(id, opcoes) {
            const datalist = document.getElementById(id);
            datalist.innerHTML = '';
            
            opcoes.sort().forEach(opcao => {
                const option = document.createElement('option');
                option.value = opcao;
                datalist.appendChild(option);
            });
        }

        // Função para filtrar trechos
        function filtrarTrechos() {
            const autorFiltro = document.getElementById('filter-autor').value;
            const obraFiltro = document.getElementById('filter-obra').value;
            const temaFiltro = document.getElementById('filter-tema').value;
            
            // Se todos os filtros estiverem vazios, mostrar todos os trechos
            if (!autorFiltro && !obraFiltro && !temaFiltro) {
                renderizarTrechos();
                return;
            }
            
            // Filtrar com base nas seleções
            const trechosFiltrados = trechos.filter(trecho => {
                const autorMatch = !autorFiltro || trecho.autor === autorFiltro;
                const obraMatch = !obraFiltro || trecho.obra === obraFiltro;
                const temaMatch = !temaFiltro || trecho.temas.includes(temaFiltro);
                
                return autorMatch && obraMatch && temaMatch;
            });
            
            renderizarTrechos(trechosFiltrados);
            
            // Mostrar mensagem sobre resultados
            if (trechosFiltrados.length === 0) {
                document.getElementById('trechos-grid').innerHTML = 
                    `<p>Nenhum resultado encontrado para os filtros selecionados.</p>`;
            } else {
                mostrarToast(`${trechosFiltrados.length} trecho(s) correspondem aos filtros aplicados.`);
            }
        }

        // Função para gerar insights para o trecho sendo criado
        function gerarInsights() {
            const trechoTexto = document.getElementById('trecho').value;
            const autor = document.getElementById('autor').value;
            const obra = document.getElementById('obra').value;
            
            if (!trechoTexto || !autor || !obra) {
                mostrarToast('Preencha pelo menos o trecho, autor e obra para gerar insights.', 3000, true);
                return;
            }
            
            // Exibir modal de carregamento com animação
            mostrarToast('Gerando insights... Aguarde enquanto analisamos o texto.', 0);
            
            // Simular processamento (na versão real, seria uma chamada para API de IA)
            setTimeout(() => {
                const insightsGerados = gerarInsightsAvancados(trechoTexto, autor, obra);
                
                // Exibir insights no modal
                const modal = document.getElementById('insights-modal');
                const content = document.getElementById('insights-content');
                
                // Melhorar a formatação dos insights
                content.innerHTML = `
                    <div class="insights-section" style="margin-bottom: 20px;">
                        <p>${insightsGerados}</p>
                    </div>
                `;
                
                // Armazenar insights temporariamente para salvar
                content.dataset.insights = insightsGerados;
                
                // Esconder toast e mostrar modal
                document.getElementById('toast').style.display = 'none';
                modal.style.display = 'block';
                
                // Animação de entrada
                setTimeout(() => {
                    modal.querySelector('.modal-content').style.opacity = '1';
                }, 10);
            }, 1500);
        }

        // Função para gerar insights para um trecho existente
        function gerarInsightsParaTrecho(id) {
            const trecho = trechos.find(t => t.id === id);
            if (!trecho) return;
            
            // Exibir modal de carregamento com animação sofisticada
            mostrarToast('Gerando insights... Analisando contexto e referências literárias.', 0);
            
            // Simular processamento mais elaborado
            setTimeout(() => {
                const insightsGerados = gerarInsightsAvancados(trecho.trecho, trecho.autor, trecho.obra);
                
                // Atualizar trecho com insights
                trecho.insights = insightsGerados;
                salvarTrechos();
                
                // Esconder toast
                document.getElementById('toast').style.display = 'none';
                
                // Mostrar modal com insights
                const modal = document.getElementById('insights-modal');
                const content = document.getElementById('insights-content');
                
                content.innerHTML = `
                    <div class="insights-section" style="margin-bottom: 20px;">
                        <p>${insightsGerados}</p>
                    </div>
                `;
                
                // Marcar como já salvo
                content.dataset.saved = 'true';
                content.dataset.trechoId = id;
                
                // Atualizar texto do botão
                document.getElementById('btn-salvar-insights').innerHTML = '<span class="btn-icon">✓</span> Fechar';
                
                // Exibir modal com animação
                modal.style.display = 'block';
                setTimeout(() => {
                    modal.querySelector('.modal-content').style.opacity = '1';
                }, 10);
                
                // Renderizar novamente para mostrar insights no card
                renderizarTrechos();
                
                // Mostrar mensagem de sucesso
                mostrarToast('Insights adicionados ao trecho com sucesso!');
            }, 1800);
        }

        // Função para salvar insights no trecho atual sendo criado
        function salvarInsights() {
            const content = document.getElementById('insights-content');
            
            // Se já estiver salvo, apenas fechar o modal
            if (content.dataset.saved === 'true') {
                document.getElementById('insights-modal').style.display = 'none';
                return;
            }
            
            const insights = content.dataset.insights;
            
            // Armazenar na variável temporária para ser salva quando o trecho for criado
            document.getElementById('insights-modal').style.display = 'none';
            
            // Preencher campo oculto ou usar variável global temporária
            window.insightsTemporarios = insights;
            
            mostrarToast('Insights prontos para serem salvos com o trecho.');
        }

        // Função para gerar insights mais elaborados
        function gerarInsightsAvancados(texto, autor, obra) {
            // Esta é uma versão aprimorada da função de simulação de insights
            
            // Detectar temas com base em palavras-chave mais abrangentes
            const palavrasChave = {
                'amor': ['amor', 'amar', 'paixão', 'coração', 'sentimento', 'afeto', 'afeição', 'carinho', 'ternura', 'desejo'],
                'morte': ['morte', 'morrer', 'falecimento', 'luto', 'fim', 'finitude', 'funeral', 'cemitério', 'sepulcro', 'mortal'],
                'tempo': ['tempo', 'horas', 'dias', 'anos', 'eternidade', 'instante', 'momento', 'passado', 'presente', 'futuro', 'efêmero'],
                'natureza': ['natureza', 'árvore', 'rio', 'mar', 'floresta', 'vento', 'montanha', 'céu', 'terra', 'animal', 'vegetal'],
                'filosofia': ['ser', 'existência', 'essência', 'pensamento', 'ideia', 'consciência', 'razão', 'lógica', 'ética', 'metafísica'],
                'sociedade': ['sociedade', 'povo', 'nação', 'cultura', 'política', 'social', 'comunidade', 'civilização', 'classe', 'poder'],
                'identidade': ['identidade', 'eu', 'self', 'persona', 'personalidade', 'individual', 'subjetivo', 'interior', 'psique', 'alma'],
                'linguagem': ['linguagem', 'palavra', 'discurso', 'texto', 'escrita', 'narrativa', 'diálogo', 'comunicação', 'significado', 'signo'],
                'transcendência': ['transcendência', 'divino', 'deus', 'sagrado', 'espírito', 'religião', 'fé', 'crença', 'místico', 'sublime'],
                'liberdade': ['liberdade', 'livre', 'autonomia', 'escolha', 'vontade', 'independência', 'emancipação', 'autodeterminação', 'arbítrio', 'libertar']
            };
            
            // Detectar temas presentes
            const temasPresentesMap = {};
            const textoLowerCase = texto.toLowerCase();
            
            for (const [tema, palavras] of Object.entries(palavrasChave)) {
                for (const palavra of palavras) {
                    if (textoLowerCase.includes(palavra)) {
                        temasPresentesMap[tema] = true;
                        break;
                    }
                }
            }
            
            const temasPresentes = Object.keys(temasPresentesMap);
            
            // Autores conhecidos e suas características com descrições mais elaboradas
            const autoresCaracteristicas = {
                'machado de assis': 'realismo psicológico e ironia sutil que desvela as contradições humanas e sociais',
                'clarice lispector': 'introspecção e fluxo de consciência que explora os labirintos da subjetividade',
                'fernando pessoa': 'multiplicidade do ser e questionamento existencial através do fenômeno heteronímico',
                'carlos drummond de andrade': 'cotidiano transfigurado e melancolia que revela o absurdo da existência',
                'guimarães rosa': 'linguagem experimental e regionalismo transcendente que reinventa a narrativa brasileira',
                'dostoiévski': 'exploração psicológica profunda e dilemas morais que desafiam as convenções sociais',
                'kafka': 'absurdo existencial e alienação que simbolizam a condição humana na modernidade',
                'shakespeare': 'natureza humana e linguagem poética que transcende épocas e culturas',
                'virginia woolf': 'fluxo de consciência e perspectiva feminina que rompe com as estruturas narrativas tradicionais',
                'michel de montaigne': 'reflexão ensaística e autoexploração filosófica que inaugurou o gênero do ensaio moderno',
                'nietzsche': 'filosofia aforística e crítica dos valores tradicionais que questiona os fundamentos da moral ocidental',
                'simone de beauvoir': 'existencialismo feminista e análise da condição da mulher como "segundo sexo"',
                'albert camus': 'absurdismo filosófico e revolta metafísica diante do silêncio do mundo',
                'jorge luis borges': 'labirintos literários e metafísica da literatura que desconstrói as fronteiras entre realidade e ficção',
                'dante alighieri': 'jornada poética e alegoria teológica que representa a cosmologia medieval e a condição humana',
                'miguel de cervantes': 'paródia do romance de cavalaria e reflexão sobre a natureza da loucura e da realidade',
                'gabriel garcía márquez': 'realismo mágico e memória coletiva que funde mito e história na narrativa latino-americana',
                'fiódor dostoiévski': 'exploração dos abismos da alma humana e questionamento existencial sobre o mal e a liberdade',
                'proust': 'memória involuntária e temporalidade subjetiva que recupera o tempo perdido através da escrita',
                'paulo coelho': 'espiritualidade e busca de sentido numa linguagem acessível e universal',
                'miguel reale': 'tridimensionalidade do direito e culturalismo jurídico que integra fato, valor e norma numa síntese dialética'
            };
            
            // Verificar se o autor é conhecido
            const autorLowerCase = autor.toLowerCase();
            let estiloAutor = '';
            
            for (const [nomeAutor, caracteristica] of Object.entries(autoresCaracteristicas)) {
                if (autorLowerCase.includes(nomeAutor)) {
                    estiloAutor = caracteristica;
                    break;
                }
            }
            
            // Análise estilística
            let estiloAnalise = '';
            
            // Análise de pontuação e estrutura
            const pontuacao = (texto.match(/[.!?]/g) || []).length;
            const paragrafos = texto.split(/\n\s*\n/).length;
            const palavras = texto.split(/\s+/).length;
            const caracteresTotal = texto.length;
            const comprimentoMedioPalavras = caracteresTotal / palavras;
            
            if (comprimentoMedioPalavras > 6.5) {
                estiloAnalise += "O texto apresenta vocabulário elaborado, com predomínio de palavras longas, sugerindo uma escrita erudita. ";
            } else if (comprimentoMedioPalavras < 5) {
                estiloAnalise += "O texto utiliza palavras curtas e diretas, indicando uma abordagem concisa e objetiva. ";
            } else {
                estiloAnalise += "O texto apresenta um equilíbrio no comprimento das palavras, combinando clareza e precisão vocabular. ";
            }
            
            if (pontuacao / palavras > 0.1) {
                estiloAnalise += "Nota-se o uso frequente de pontuação, criando ritmo entrecortado e pausas expressivas. ";
            } else if (pontuacao / palavras < 0.05) {
                estiloAnalise += "A pontuação esparsa sugere fluidez narrativa e encadeamento de ideias em longos períodos. ";
            }
            
            // Gerar insights com base nas análises
            let insights = '';
            
            // Introdução personalizada
            insights += `Este trecho de "${obra}" de ${autor} apresenta uma dimensão reflexiva que convida à análise aprofundada. `;
            
            // Análise estrutural
            if (palavras < 30) {
                insights += `Com apenas ${palavras} palavras, o trecho revela uma concisão expressiva característica da síntese filosófica, onde cada termo adquire densidade semântica ampliada pelo contexto. `;
            } else if (palavras < 100) {
                insights += `Em suas ${palavras} palavras, o excerto desenvolve um raciocínio moderadamente elaborado, equilibrando concisão e desdobramento reflexivo necessário à articulação das ideias centrais. `;
            } else {
                insights += `A extensão considerável do trecho (${palavras} palavras) permite o desenvolvimento pormenorizado da questão, evidenciando a complexidade do pensamento exposto e suas nuances conceituais. `;
            }
            
            // Adicionar análise estilística
            insights += `\n\n${estiloAnalise}`;
            
            // Adicionar insights sobre temas
            if (temasPresentes.length > 0) {
                insights += `\n\nA análise temática revela a centralidade de ${temasPresentes.join(', ')}, configurando um campo semântico que dialoga com questões fundamentais do pensamento. `;
                
                // Adicionar observações específicas para cada tema
                if (temasPresentes.includes('amor')) {
                    insights += `A reflexão sobre o amor transcende a dimensão sentimental para constituir-se como problema ontológico, questionando as relações intersubjetivas e sua significação existencial. `;
                }
                
                if (temasPresentes.includes('morte')) {
                    insights += `A tematização da finitude emerge como horizonte hermenêutico que ressignifica a temporalidade e a própria condição humana, estabelecendo um contraponto dialético com a vida. `;
                }
                
                if (temasPresentes.includes('tempo')) {
                    insights += `A temporalidade aparece como categoria estruturante do discurso, problematizando a experiência subjetiva da duração e a relação entre memória, presente e projeção. `;
                }
                
                if (temasPresentes.includes('natureza')) {
                    insights += `O elemento natural configura-se não apenas como cenário, mas como dimensão simbólica que espelha ou contrasta com a condição humana, criando um jogo de correspondências entre microcosmo e macrocosmo. `;
                }
                
                if (temasPresentes.includes('filosofia')) {
                    insights += `A reflexão filosófica supera o mero exercício conceitual para instaurar uma interrogação radical sobre os fundamentos do conhecimento e da existência. `;
                }
                
                if (temasPresentes.includes('identidade')) {
                    insights += `A problematização da identidade evidencia a tensão entre permanência e transformação, unicidade e multiplicidade, autenticidade e representação social. `;
                }
                
                if (temasPresentes.includes('linguagem')) {
                    insights += `A consciência metalinguística presente no texto revela uma preocupação com os limites e possibilidades do discurso como mediação entre pensamento e realidade. `;
                }
            } else {
                insights += `\n\nO trecho resiste a uma categorização temática convencional, sugerindo uma abordagem singular ou uma intersecção inusitada de questões que escapam às classificações correntes. `;
            }
            
            // Adicionar insights sobre o estilo do autor, se conhecido
            if (estiloAutor) {
                insights += `\n\nO excerto exemplifica características distintivas da obra de ${autor}, reconhecido por ${estiloAutor}. A passagem selecionada sintetiza elementos fundamentais de seu projeto intelectual, permitindo vislumbrar a cosmovisão que orienta sua produção. `;
            } else {
                insights += `\n\nA escritura apresenta singularidades estilísticas que mereceriam investigação comparativa no contexto mais amplo da obra do autor, possibilitando identificar recorrências e variações significativas em seu percurso criativo. `;
            }
            
            // Adicionar sugestões de conexões
            if (temasPresentes.length > 0) {
                insights += `\n\nSugestões de diálogos intertextuais: Este trecho estabelece interfaces conceituais com outras obras que abordam ${temasPresentes.join(' e ')}, como `;
                
                if (temasPresentes.includes('amor')) {
                    insights += `"O Banquete" de Platão (investigação dialógica sobre Eros), `;
                }
                
                if (temasPresentes.includes('morte')) {
                    insights += `"A Morte de Ivan Ilitch" de Tolstói (fenomenologia da finitude), `;
                }
                
                if (temasPresentes.includes('tempo')) {
                    insights += `"Em Busca do Tempo Perdido" de Marcel Proust (temporalidade subjetiva), `;
                }
                
                if (temasPresentes.includes('natureza')) {
                    insights += `"Walden" de Henry David Thoreau (experiência contemplativa do natural), `;
                }
                
                if (temasPresentes.includes('filosofia')) {
                    insights += `"O Mundo como Vontade e Representação" de Schopenhauer (estrutura metafísica da realidade), `;
                }
                
                if (temasPresentes.includes('identidade')) {
                    insights += `"O Estrangeiro" de Albert Camus (alienação e autenticidade), `;
                }
                
                if (temasPresentes.includes('linguagem')) {
                    insights += `"Tractatus Logico-Philosophicus" de Wittgenstein (limites da expressão), `;
                }
                
                if (temasPresentes.includes('sociedade')) {
                    insights += `"A Condição Humana" de Hannah Arendt (vida ativa e esfera pública), `;
                }
                
                // Remover a última vírgula e espaço
                insights = insights.slice(0, -2);
                
                insights += '.';
            }
            
            // Adicionar uma observação final personalizada
            insights += `\n\nConsideração hermenêutica: A interpretação aqui proposta constitui uma abertura de sentido que não esgota as possibilidades significativas do texto. A compreensão deste excerto enriquece-se pela contextualização na obra "${obra}" e no horizonte histórico-cultural em que emerge o pensamento de ${autor}, exigindo permanente diálogo entre parte e todo, texto e contexto, imanência e transcendência.`;
            
            return insights;
        }

        // Função para gerar mapa mental
        function gerarMapaMental() {
            // Obter o texto para análise
            const selectSource = document.getElementById('mindmap-source-select');
            const text = document.getElementById('mindmap-text').value;
            
            if (!text) {
                mostrarToast('Por favor, insira um texto para gerar o mapa mental.', 3000, true);
                return;
            }
            
            // Verificar se já existe um mapa mental em cache para este texto
            const textHash = hashCode(text);
            if (mapasMentaisCache[textHash]) {
                renderizarMapaMental(mapasMentaisCache[textHash]);
                return;
            }
            
            // Exibir indicador de carregamento
            document.getElementById('mindmap-resultado').style.display = 'none';
            mostrarToast('Gerando mapa mental... Analisando estrutura conceitual do texto.', 0);
            
            // Simular processamento de IA
            setTimeout(() => {
                const mapaMental = analisarTextoParaMapaMental(text);
                
                // Armazenar em cache
                mapasMentaisCache[textHash] = mapaMental;
                
                // Renderizar o mapa mental
                renderizarMapaMental(mapaMental);
                
                // Esconder toast
                document.getElementById('toast').style.display = 'none';
            }, 2000);
        }

        // Função para renderizar o mapa mental
        function renderizarMapaMental(mapaMental) {
            const container = document.getElementById('mindmap-container');
            container.innerHTML = '';
            
            // Criar o nó raiz
            const rootNode = document.createElement('div');
            rootNode.className = 'mindmap-node mindmap-level-0';
            rootNode.textContent = mapaMental.conceito;
            container.appendChild(rootNode);
            
            // Criar nós filhos recursivamente
            if (mapaMental.children && mapaMental.children.length > 0) {
                const childrenContainer = document.createElement('div');
                childrenContainer.className = 'mindmap-children';
                container.appendChild(childrenContainer);
                
                renderizarNosFilhos(childrenContainer, mapaMental.children, 1);
            }
            
            // Exibir a seção de resultado
            document.getElementById('mindmap-resultado').style.display = 'block';
            
            // Adicionar animação de entrada
            const nodes = document.querySelectorAll('.mindmap-node');
            nodes.forEach((node, index) => {
                node.style.opacity = '0';
                node.style.transform = 'translateY(20px)';
                
                setTimeout(() => {
                    node.style.opacity = '1';
                    node.style.transform = 'translateY(0)';
                }, 100 * index);
            });
        }

        // Função para renderizar nós filhos recursivamente
        function renderizarNosFilhos(container, children, level) {
            children.forEach(child => {
                // Criar nó
                const node = document.createElement('div');
                node.className = `mindmap-node mindmap-level-${level}`;
                node.textContent = child.conceito;
                
                // Adicionar elemento conector visual
                if (level > 0) {
                    const connector = document.createElement('div');
                    connector.className = 'mindmap-connector';
                    node.appendChild(connector);
                }
                
                container.appendChild(node);
                
                // Renderizar nós filhos recursivamente
                if (child.children && child.children.length > 0) {
                    const childrenContainer = document.createElement('div');
                    childrenContainer.className = 'mindmap-children';
                    container.appendChild(childrenContainer);
                    
                    renderizarNosFilhos(childrenContainer, child.children, level + 1);
                }
            });
        }

        // Função para analisar texto e gerar estrutura do mapa mental
        function analisarTextoParaMapaMental(texto) {
            // Em uma implementação real, isso usaria algoritmos de NLP e IA
            // Esta é uma versão simplificada para demonstração
            
            // Dividir texto em sentenças
            const sentences = texto.split(/[.!?]/).filter(s => s.trim().length > 0);
            
            // Identificar conceito principal (primeiro parágrafo ou primeira frase)
            const conceitoPrincipal = sentences[0].trim();
            
            // Extrair palavras-chave significativas
            const palavrasChave = extrairPalavrasChave(texto);
            
            // Construir hierarquia de conceitos
            const mapaMental = {
                conceito: conceitoPrincipal.length > 40 ? conceitoPrincipal.substring(0, 40) + '...' : conceitoPrincipal,
                children: []
            };
            
            // Criar nós de primeiro nível
            const temasNivelUm = ['Conceitos fundamentais', 'Aspectos principais', 'Questões centrais', 'Elementos estruturantes'];
            
            for (let i = 0; i < Math.min(4, palavrasChave.length); i++) {
                const noNivelUm = {
                    conceito: temasNivelUm[i],
                    children: []
                };
                
                // Adicionar 2-3 palavras-chave como nós de segundo nível
                const startIdx = i * 3;
                const endIdx = Math.min(startIdx + 3, palavrasChave.length);
                
                for (let j = startIdx; j < endIdx; j++) {
                    if (j < palavrasChave.length) {
                        const noNivelDois = {
                            conceito: palavrasChave[j],
                            children: []
                        };
                        
                        // Adicionar detalhes como nós de terceiro nível
                        const detalhesSentences = sentences.filter(s => 
                            s.toLowerCase().includes(palavrasChave[j].toLowerCase())
                        );
                        
                        if (detalhesSentences.length > 0) {
                            for (let k = 0; k < Math.min(2, detalhesSentences.length); k++) {
                                const frase = detalhesSentences[k].trim();
                                if (frase.length > 0) {
                                    noNivelDois.children.push({
                                        conceito: frase.length > 40 ? frase.substring(0, 40) + '...' : frase,
                                        children: []
                                    });
                                }
                            }
                        }
                        
                        noNivelUm.children.push(noNivelDois);
                    }
                }
                
                mapaMental.children.push(noNivelUm);
            }
            
            // Adicionar seção de conclusão
            if (sentences.length > 3) {
                const conclusao = {
                    conceito: 'Considerações finais',
                    children: []
                };
                
                // Adicionar última sentença como conclusão
                const ultimaFrase = sentences[sentences.length - 1].trim();
                if (ultimaFrase.length > 0) {
                    conclusao.children.push({
                        conceito: ultimaFrase.length > 50 ? ultimaFrase.substring(0, 50) + '...' : ultimaFrase,
                        children: []
                    });
                }
                
                mapaMental.children.push(conclusao);
            }
            
            return mapaMental;
        }

        // Função auxiliar para extrair palavras-chave de um texto
        function extrairPalavrasChave(texto) {
            // Em uma implementação real, usaria algoritmos de NLP
            // Esta é uma versão simplificada para demonstração
            
            // Dividir o texto em palavras
            const palavras = texto.toLowerCase()
                .replace(/[^\w\sáàâãéèêíìîóòôõúùûç]/g, '')
                .split(/\s+/);
            
            // Remover stop words (palavras comuns que não trazem muito significado)
            const stopWords = ['o', 'a', 'os', 'as', 'um', 'uma', 'uns', 'umas', 'e', 'que', 'de', 'do', 'da', 'dos', 'das', 'em', 'no', 'na', 'nos', 'nas', 'por', 'para', 'com', 'se', 'como', 'mas', 'ou', 'ao', 'à', 'aos', 'às'];
            const palavrasFiltradas = palavras.filter(p => p.length > 3 && !stopWords.includes(p));
            
            // Contar frequência
            const frequencia = {};
            palavrasFiltradas.forEach(p => {
                frequencia[p] = (frequencia[p] || 0) + 1;
            });
            
            // Ordenar por frequência e obter as mais frequentes
            const ordenadas = Object.entries(frequencia)
                .sort((a, b) => b[1] - a[1])
                .map(([palavra]) => palavra);
            
            // Retornar as 15 palavras mais frequentes ou todas se houver menos
            return ordenadas.slice(0, 15);
        }

        // Função para salvar mapa mental
        function salvarMapaMental() {
            // Em uma implementação real, poderia salvar o mapa mental como imagem
            // ou associá-lo ao trecho correspondente
            
            mostrarToast('Mapa mental salvo com sucesso!');
            
            // Aqui poderia adicionar lógica para exportar como imagem
            // ou associar ao trecho selecionado
        }

        // Função para carregar opções para o mapa mental
        function carregarOpcoesMapaMental() {
            const select = document.getElementById('mindmap-source-select');
            
            // Manter a primeira opção
            const primeiraOpcao = select.options[0];
            select.innerHTML = '';
            select.appendChild(primeiraOpcao);
            
            // Adicionar todos os trechos como opções
            trechos.forEach(trecho => {
                const option = document.createElement('option');
                option.value = trecho.id;
                option.textContent = trecho.titulo;
                select.appendChild(option);
            });
        }

        // Função para analisar coleção
        function analisarColecao() {
            if (trechos.length === 0) {
                mostrarToast('Você ainda não possui trechos salvos para análise.', 3000, true);
                return;
            }
            
            // Exibir indicador de carregamento
            document.getElementById('loading-trechos').style.display = 'block';
            document.getElementById('analise-resultado').style.display = 'none';
            
            // Simular processamento mais elaborado
            setTimeout(() => {
                // Ocultar carregamento
                document.getElementById('
