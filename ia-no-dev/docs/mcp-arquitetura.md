# mcp-arquitetura

## Propósito

Comunicar que o Model Context Protocol é um conector padronizado entre
clientes de IA (Claude, ChatGPT, VS Code) e servidores que expõem
ferramentas e dados (bancos, APIs, sistemas de arquivos).

## Mensagens

- MCP Client e MCP Server são papéis distintos — um consome, o outro provê
- A comunicação é padronizada via JSON-RPC (resources, tools, prompts)
- Um mesmo servidor funciona com qualquer client compatível com MCP
- Existem dois modos de transporte: stdio (local) e Streamable HTTP (remoto)
- Diversos clients e servers já existem no ecossistema

## Decisões

- A imagem deve mostrar os dois lados (client e server) e a conexão entre eles
