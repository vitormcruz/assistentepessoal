# AGENTS.md — Regras para o agente neste diretório

Este diretório contém uma apresentação sobre IA no desenvolvimento de
software. O agente deve seguir estas regras ao trabalhar aqui.

## Imagens

### Fluxo de criação e alteração

1. Toda imagem tem um arquivo de **especificação** em `docs/<nome>.md`
   que descreve **o que a imagem deve comunicar** — seu propósito e as
   mensagens que precisa transmitir.
2. O SVG (`imagens/<nome>.svg`) é criado ou alterado a partir da
   especificação, em conversa com o humano. A forma visual (layout, cores,
   elementos gráficos) é decidida nessa conversa.
3. O PNG (`imagens/<nome>.png`) é gerado a partir do SVG usando
   `~/.config/opencode/scripts/opencode-svgtoimage.sh`.
4. Se a especificação mudar, o SVG e o PNG devem ser atualizados.
5. A especificação é a **fonte da verdade** — ela descreve o conteúdo e a
   mensagem, não a forma visual. A mesma especificação pode resultar em
   imagens completamente diferentes se o design for refeito.

### O que vai na especificação

- **Propósito:** o que a imagem quer comunicar
- **Mensagens:** os pontos que o espectador deve extrair da imagem
- **Decisões:** escolhas conceituais registradas ao longo do tempo

### O que NÃO vai na especificação

- Cores, fontes, dimensões
- Tipos de elementos visuais (caixas, setas, ícones específicos)
- Decisões de layout

## Apresentação

- Arquivo principal: `apresentacao.md`
- Conversão para PowerPoint: `pandoc apresentacao.md -o apresentacao.pptx`
- Slides são delimitados por `---`
- Imagens referenciadas com caminho relativo: `![](imagens/<nome>.png)`

### Princípio: slides são headlines

Cada slide deve conter apenas o essencial — headlines, palavras-chave,
diagramas ou tabelas compactas. O apresentador fala o conteúdo; o slide
ancora a atenção.

- Evite blocos densos de texto — se uma ideia precisa de parágrafo, ela
  vai na fala, não no slide
- Prefira imagens descritivas e esquemas visuais a listas de texto
- Tabelas são aceitáveis quando comparam dados de forma sucinta
- Um slide deve comunicar **uma** ideia principal

### Princípio: slides são headlines

Cada slide deve conter apenas o essencial — headlines, palavras-chave,
diagramas ou tabelas compactas. O apresentador fala o conteúdo; o slide
ancora a atenção.

- Evite blocos densos de texto — se uma ideia precisa de parágrafo, ela
  vai na fala, não no slide
- Prefira imagens descritivas e esquemas visuais a listas de texto
- Tabelas são aceitáveis quando comparam dados de forma sucinta
- Um slide deve comunicar **uma** ideia principal
