# contexto-comparacao

## Propósito

Comunicar a diferença entre contexto bruto (sem curadoria) e contexto
curado (com seleção, compressão e isolamento).

## Mensagens

- Contexto bruto: jogar tudo que o agente pode acessar na janela de
  contexto faz a informação relevante se perder — o agente erra ou alucina
- Contexto curado: selecionar só os arquivos relevantes, comprimir o
  acessório e isolar contextos entre agentes especializados faz o agente
  acertar
- A diferença de resultado não está no modelo nem no prompt — está na
  curadoria do contexto
- O exemplo concreto: buscar "desconto" em 200 arquivos traz 3 relevantes;
  o resto vira resumo

## Decisões

- A imagem deve contrastar dois cenários (ruim vs bom) de forma imediata
- Usar um exemplo concreto (bug de desconto, arquivos payment.py,
  discount.py, validator.py) para ancorar o conceito abstrato
