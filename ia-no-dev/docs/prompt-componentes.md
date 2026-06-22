# prompt-componentes

## Propósito

Mostrar que tudo que chega ao modelo é prompt — independente da origem.
O modelo está no centro e recebe entradas de 5 fontes diferentes, todas
tratadas como prompt pelo LLM.

## Mensagens

- System Prompt, User Prompt, Skills.md, Command e AGENTS.md convergem para o modelo
- Não importa a origem — tudo vira prompt
- O modelo não distingue "tipo" de prompt, só processa tokens

## Exemplos

Cada entrada do diagrama deve conter um exemplo concreto, máximo 4 linhas.

### System Prompt

Seja objetivo e educado. Vá direto ao ponto.
Não use linguagem vulgar ou ofensiva.
Explique apenas quando solicitado.

### User Prompt

Refatore o módulo payment.py:
extraia a validação de cartão para
uma função separada validate_card().

### Skills.md

Commit: use conventional commits.
Formato: tipo(escopo): descrição.
Sempre confirme a mensagem com o usuário.

### Command

/testes-processo-compra
mvn test -pl modulo-pagamento
Verifique cobertura > 80%.

### AGENTS.md

Use tabs para indentação.
Testes: pytest.
Não mexer em /infra.

## Decisões

- Layout circular com o modelo no centro
- 5 entradas distribuídas ao redor, cada uma com um exemplo concreto
- Exemplos curtos e reais, sem abstração
