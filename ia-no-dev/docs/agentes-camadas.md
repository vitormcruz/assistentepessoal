# agentes-camadas

## Propósito

Comunicar a distinção entre Agente-Ferramenta e Agente Customizado — a
principal fonte de confusão no uso do termo "agente".

## Mensagens

- Agente-Ferramenta é a plataforma: software que executa comandos, lê e
  escreve arquivos, interage com git e APIs (ex: Claude Code, OpenCode,
  Cursor Agent)
- Agente Customizado é uma configuração que roda dentro da plataforma:
  system prompt + regras + ferramentas selecionadas (ex: "Engenheiro de
  Software Sênior Python")
- Skills são módulos de conhecimento que o agente customizado carrega sob
  demanda (ex: code review, security audit, test writer)
- A relação é hierárquica: plataforma contém configuração que contém skills
- "Agente" sozinho é ambíguo — é preciso especificar qual dos dois

## Decisões

- A imagem deve deixar clara a relação de contenção entre os três níveis
