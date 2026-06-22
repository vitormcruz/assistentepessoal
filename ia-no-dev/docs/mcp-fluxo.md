# mcp-fluxo

## Propósito

Diagramar o fluxo de uma chamada MCP na prática: do usuário à resposta,
passando pelo agente, MCP Server e banco de dados.

## Mensagens

- O usuário faz uma pergunta em linguagem natural
- O agente recebe as tools no contexto e decide qual usar
- O MCP Server executa com credenciais controladas (não do agente)
- O resultado volta pelo mesmo caminho
- O agente não "sabe SQL" — ele decide usar a ferramenta, não executa

## Decisões

- Fluxo horizontal: Você → Agente → MCP Server → PostgreSQL
- Cada etapa com um rótulo descritivo
- Resposta volta na direção oposta
- Exemplo concreto: query("SELECT COUNT(*) FROM users...")
