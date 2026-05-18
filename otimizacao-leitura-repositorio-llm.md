# Otimizacao de Leitura de Repositorio para LLMs

## Cenario Alvo: Navegacao Inteligente + Recuperacao Cirurgica

O objetivo e permitir que o LLM navegue por **codigo e documentos** de forma
estruturada e recupere **apenas o conteudo exato que precisa**, sem perda de
qualidade. A estrategia central e:

```
Navegacao estrutural (mapa/indice) → Identificacao do necessario →
Recuperacao cirurgica (secao/simbolo/funcao) → Conteudo completo e preciso
```

**Principio fundamental**: Nao comprimir conteudo. A reducao de tokens vem
de **nao enviar o que e irrelevante**, nao de alterar o que e relevante.

| Abordagem | Como reduz tokens | Perda de qualidade? |
|---|---|---|
| **Knowledge Graph / Mapas** | Queries retornam apenas o relevante | Nao — informacao completa e precisa |
| **Skeletons / Extracao** | Envia estrutura, nao corpos | Nao — o enviado e exato; o omitido era irrelevante |
| **Indexacao de Documentos** | Navega por secao, nao por arquivo | Nao — conteudo original preservado |
| **Selecao Ranked** | Rankeia e seleciona por relevancia | Nao — arquivos relevantes sao enviados completos |
| **Compressao** | Altera texto (abrevia, remove) | **Potencial sim** — texto e modificado |

---

## Categoria 1: Knowledge Graph / Mapas Estruturais (Codigo)

**Problema que resolvem**: O agente gasta milhares de tokens lendo arquivos
brutos para responder perguntas estruturais ("quem chama X?", "qual a
arquitetura?"). Essas ferramentas constroem um mapa do codebase uma vez e
permitem queries pontuais com retrieval cirurgico.

### 1.1 [codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp)
- **Como funciona**: MCP server em C estatico que parseia o codebase com
  tree-sitter (66 linguagens) e mantem grafo de conhecimento em SQLite.
  14 ferramentas MCP: `search_graph`, `trace_call_path`, `get_code_snippet`,
  `search_code`, `get_architecture`, `detect_changes`, etc. Background
  watcher para sync automatico. Sem LLM embutido.
- **Reducao**: 121x (5 queries: ~3.400 vs ~412.000 tokens)
- **Cobertura**: Codigo (66 linguagens). **Nao cobre docs de texto.**
- **Recuperacao cirurgica**: `get_code_snippet` recupera funcoes/classes por
  qualified name sem ler arquivo inteiro. `search_code` para busca textual.
  Mas blocos arbitrarios de codigo nao indexados ainda exigem leitura bruta.
- **Licenca**: MIT | **Estrelas**: ~780 | **Multi-agente**: Sim (MCP:
  OpenCode, Claude Code, Codex, Gemini, Zed, Antigravity, Aider, KiloCode)

### 1.2 [graphify](https://github.com/AGurindapalli/graphify-llm-kbase)
- **Como funciona**: Skill que transforma pastas de codigo, docs, papers e
  imagens em knowledge graph. Extracao AST via tree-sitter para codigo +
  subagentes LLM em paralelo para docs/papers/imagens. Clusterizacao Leiden,
  export como JSON consultavel e HTML. Cache SHA256 incremental. Hook
  PreToolUse que redireciona Glob/Grep para o grafo.
- **Reducao**: 71.5x menos tokens por query vs ler arquivos brutos
- **Cobertura**: **Codigo + docs + papers + imagens** — unico que cobre
  documentacao de texto de forma nativa com extracao semantica.
- **Recuperacao cirurgica**: Nao indexa conteudo interno dos arquivos. Se o
  agente precisa do corpo de uma funcao ou logica detalhada, ainda precisa
  ler o arquivo bruto.
- **Licenca**: Nao explicita | **Multi-agente**: Claude Code, Codex, OpenCode,
  OpenClaw

### 1.3 [code-review-graph](https://github.com/tirth8205/code-review-graph)
- **Como funciona**: MCP server Python com tree-sitter (24 linguagens +
  Jupyter). Constroi grafo AST persistente, rastreia mudancas incremental-
  mente, calcula blast radius de mudancas. 6 ferramentas MCP:
  `get_minimal_context`, `get_impact_radius`, `get_review_context`,
  `query_graph`, `traverse_graph`, `build_or_update_graph`.
- **Reducao**: 8.2x em media (ate 49x em tarefas diarias)
- **Cobertura**: Codigo (24 linguagens). **Nao cobre docs.**
- **Recuperacao cirurgica**: Envia contexto estrutural compacto (~100 tokens)
  com blast radius, cadeias de dependencia e gaps de teste. Nao substitui
  leitura de conteudo interno.
- **Licenca**: MIT | **Estrelas**: ~16.7k | **Multi-agente**: Sim (MCP)

### 1.4 [cortex](https://github.com/DanielBlomma/cortex)
- **Como funciona**: Context engine local com tree-sitter, grafo de
  entidades (files, symbols, rules, ADRs) e relacoes (CALLS, DEFINES,
  CONSTRAINS, IMPLEMENTS, IMPORTS, SUPERSEDES). Armazena em RyuGraph
  (grafo local) + indice vetorial opcional. MCP tools: `context.search`,
  `context.traverse`, `context.impact`, `context.rules`. Policy layer
  filtra conteudo conflitante/deprecated antes de chegar ao assistente.
  Git hooks para index incremental. Dashboard TUI.
- **Reducao**: Fracao do contexto de leitura bruta (tipico: ~3 queries vs
  ~12 files)
- **Cobertura**: Codigo (multi-linguagem via tree-sitter) + rules + ADRs.
  **Docs de texto nao cobertos explicitamente.**
- **Recuperacao cirurgica**: Retrieval ranked + graph traversal — retorna
  pacote de contexto compacto, nao arquivos brutos.
- **Licenca**: Nao explicita | **Multi-agente**: Claude Code, Claude Desktop,
  Codex (MCP)

### 1.5 [ContextGraph](https://github.com/chrispaulintheory/ContextGraph)
- **Como funciona**: Motor local multi-linguagem que indexa codebase (AST
  signatures, dependency graphs, skeletons) em SQLite por projeto. API HTTP
  com endpoints: `/skeleton` (signatures-only de um arquivo), `/capsule`
  (contexto estrutural de funcao/classe), `/resume` (catch-up prompt para
  nova sessao). File watcher para sync. Persiste memoria de sessao.
- **Reducao**: 88% em media (ex: 1.250 → 145 tokens)
- **Cobertura**: Codigo. **Nao cobre docs.**
- **Recuperacao cirurgica**: `/skeleton` retorna apenas signatures; `/capsule`
  retorna contexto estrutural com dependencias.
- **Licenca**: Nao explicita | **Multi-agente**: Via API HTTP (integravel a
  qualquer agente)

### 1.6 [mcp-codebase-index](https://github.com/MikeRecognex/mcp-codebase-index)
- **Como funciona**: Indexer estrutural com 17 MCP query tools. Python via
  `ast` module + regex para TS/JS, Go, Rust, C#. Zero dependencias. Tools:
  `find_symbol`, `get_function_source`, `get_class_source`,
  `get_dependencies`, `get_dependents`, `get_change_impact`,
  `get_call_chain`, `search_codebase`, etc. Budget controls
  (`max_results`, `max_lines`).
- **Reducao**: 87%
- **Cobertura**: Python, TS/JS, Go, Rust, C#, Markdown. **Nao cobre docs
  gerais.**
- **Recuperacao cirurgica**: `get_function_source` e `get_class_source`
  retornam apenas o simbolo, nao o arquivo inteiro.
- **Licenca**: AGPL-3.0 (open source) / Comercial (proprietario)
  | **Multi-agente**: Sim (MCP)

### 1.7 [codebase-graph](https://github.com/broskees/codebase-graph)
- **Como funciona**: Gera mapa estrutural comprimido (~2-5K tokens) do
  codebase inteiro em `.codebase.md`, injetado no system prompt de toda
  chamada LLM via plugin OpenCode. Usa tree-sitter (Kit) para extracao de
  simbolos. Formato TOON (40-60% menor que JSON). Watcher em tempo real
  (<50ms por mudanca).
- **Reducao**: Substitui exploracao inicial de codebase por mapa de 2-5K
  tokens sempre disponivel no contexto
- **Cobertura**: Codigo. **Nao cobre docs.**
- **Recuperacao cirurgica**: Nao substitui leitura — e um mapa de orientacao
  injetado automaticamente.
- **Licenca**: Nao explicita | **Multi-agente**: Plugin OpenCode (todos os
  providers)

---

## Categoria 2: Skeleton / Extracao de Assinaturas (Codigo)

**Problema que resolvem**: Quando o agente precisa "entender" um arquivo,
ele le tudo — incluindo corpos de funcoes, imports repetidos, boilerplate.
Essas ferramentas enviam apenas a estrutura (assinaturas, tipos, interfaces).
**Nao ha perda de qualidade** — o que e enviado e exato; o que nao e enviado
era irrelevante para a tarefa.

### 2.1 [TokenSlayer](https://github.com/ajvikram/TokenSlayer)
- **Como funciona**: Extensao VS Code que registra Language Model Tool para
  Copilot. Extrai skeletons via LSP (1 chamada API), mantendo assinaturas e
  removendo corpos. Cache LRU com file-watcher. Tambem tem MCP server
  standalone e CLI independente. Detecao de segredos.
- **Reducao**: 40-95% (ex: 5.000 → 200 tokens por arquivo)
- **Cobertura**: TS/JS, Python, Go, Java, Rust, C#, Kotlin. **Nao cobre docs.**
- **Recuperacao cirurgica**: Skeletons estruturais — assinaturas, interfaces,
  tipos. Se o agente precisa do corpo da funcao, o skeleton nao tem.
- **Licenca**: MIT | **Estrelas**: 1 | **Multi-agente**: VS Code + Copilot
  nativo; MCP para Cursor/Claude Desktop; CLI standalone

### 2.2 [Distil](https://github.com/joshuaboys/distil)
- **Como funciona**: Extrai estrutura em 5 camadas de profundidade:
  L1 (AST: funcoes, classes, imports, signatures), L2 (Call Graph),
  L3 (CFG: control flow + complexidade), L4 (DFG: data flow + def-use),
  L5 (Program Slice: o que afeta linha X). CLI + MCP server.
- **Reducao**: ~95% (ex: 10.000 → 500 tokens no L1)
- **Cobertura**: TypeScript/JavaScript (Python e Rust planejados). **Nao
  cobre docs.**
- **Recuperacao cirurgica**: Extracao estrutural em camadas — pede so o nivel
  de detalhe necessario.
- **Licenca**: Nao explicita | **Multi-agente**: Sim (MCP)

### 2.3 [mcp-code-context](https://github.com/achatainga/mcp-code-context)
- **Como funciona**: MCP server com Tree-sitter WASM (100% AST accuracy,
  zero deps nativas). 13 tools — 6 para leitura (skeletons, extracao
  cirurgica de simbolos, repo map semantico), 2 para cleanup, 5 para
  escrita (replace, insert, rename, remove no nivel de simbolo). Suporte
  a `className` para desambiguacao. Cache SQLite persistente, fuzzy search,
  file watcher.
- **Reducao**: 50-80%
- **Cobertura**: TS/JS, Python, PHP, Dart (AST). Outros: passthrough.
  **Nao cobre docs.**
- **Recuperacao cirurgica**: `read_file_surgical` extrai simbolo especifico;
  `get_semantic_repo_map` gera mapa do repo.
- **Licenca**: Nao explicita | **Multi-agente**: Sim (MCP: Claude, Cursor,
  Windsurf, Copilot, Amazon Q)

### 2.4 [KBLens](https://github.com/disrei/KBLens)
- **Como funciona**: Gera knowledge base em Markdown hierarquico a partir
  do codebase. tree-sitter extrai AST skeletons (C++, Python, TS, JS),
  empacota em batches com token budget, LLM gera sumarios concisos
  (~400 tokens/batch). 3 niveis de detalhe: project → package → component
  (progressive disclosure). Anti-hallucination prompts. Output em Markdown
  puro — integravel a qualquer agente via referencia de arquivo.
- **Reducao**: Substitui leitura de codebase inteiro por navegacao em
  sumarios hierarquicos
- **Cobertura**: **Codigo + gera sumarios em Markdown** — os sumarios
  podem ser usados como referencia de documentacao. Mas nao indexa docs
  existentes do repo.
- **Recuperacao cirurgica**: Sumarios hierarquicos em Markdown substituem
  exploracao inicial. Para detalhes, o agente ainda pode ler os arquivos
  brutos ou as assinaturas AST preservadas nos arquivos de output.
- **Licenca**: Nao explicita | **Multi-agente**: Qualquer agente que suporte
  referencia de arquivos (Cursor, Copilot, OpenCode, etc.)

---

## Categoria 3: Indexacao de Documentos (Texto)

**Problema que resolvem**: O equivalente a um "TokenSlayer para documentos
de texto". Documentos nao tem AST como codigo, mas possuem hierarquia de
headings, seccoes e estrutura semantica. Essas ferramentas indexam a
estrutura dos documentos e permitem navegacao por secao — o agente le apenas
a secao relevante, nao o documento inteiro. **Nao ha perda de qualidade** —
o conteudo recuperado e o original, sem compressao.

### 3.1 [doctree-mcp](https://github.com/joesaby/doctree-mcp)
- **Estrategia**: Indexa headings de Markdown/CSV/JSONL → arvore navegavel
  + BM25. Zero embeddings, zero LLM calls no index.
- **Como funciona**: Agente faz `search_documents` → ve outline com
  `get_tree` → navega com `navigate_tree` → le conteudo com
  `get_node_content`. Skills embutidas (`/doc-read`, `/doc-write`,
  `/doc-lint`) ensinam o agente a navegar a arvore como um bibliotecario.
- **Reducao**: 2K-8K tokens com conteudo preciso vs 4K-20K de chunks
  ruidosos de RAG vetorial
- **Indexacao**: 900 docs em 2-5s, 0 tokens LLM. Re-index incremental em
  ~50ms. Suporte a wiki mantida pelo proprio LLM.
- **Licenca**: MIT | **Multi-agente**: Sim (MCP: Claude Code, Cursor,
  Windsurf, Codex, OpenCode)
- **Diferencial**: O mais proximo de "TokenSlayer para documentos".
  Navegacao iterativa com audit trail (search → outline → navigate →
  retrieve). Multi-instance routing para varias colecoes de docs.

### 3.2 [markdown-matters](https://github.com/srobinson/markdown-matters)
- **Estrategia**: Extrai estrutura de Markdown → sumarios LLM-ready em
  3 niveis de detalhe (`brief`, `summary`, `full`) + section filtering
  preciso.
- **Como funciona**: `mdm index` constroi indice. `mdm context doc.md
  --sections` mostra todas as secoes com contagem de tokens. `mdm context
  doc.md --section "2.3"` extrai apenas aquela secao. `mdm search "auth"`
  busca semantica ou keyword.
- **Reducao**: 84% (ex: 2.500 → 400 tokens por doc; 25.000 → 4.000 para
  10 docs)
- **Indexacao**: Local, com `--watch` para re-index automatico
- **Licenca**: Nao explicita | **Multi-agente**: CLI + MCP tools
  (`md_search`, `md_context`, `md_structure`)
- **Diferencial**: Section filtering pelo numero ou titulo do heading.
  Token budget por secao (`-t 500`).

### 3.3 [yore](https://github.com/rahulrajaram/yore)
- **Estrategia**: BM25 + analise estrutural + link graph + extractive
  refinement com token budget. Deterministico, sem embeddings.
- **Como funciona**: `yore build` indexa docs. `yore query "auth setup"`
  busca. `yore assemble "auth setup" --max-tokens 3000` monta digest com
  citacoes de fonte, pre-filtrado e ranked, pronto para LLM. Pipeline:
  BM25 → expansao de cross-references → extractive refinement (preserva
  code blocks, listas, headings) → trimming por token budget.
- **Reducao**: 3K tokens relevantes vs 182K brutos (exemplo real)
- **Indexacao**: Deterministica, sem embeddings, sem sampling. Detecta
  duplicatas, orphaos, backlinks. Valida links.
- **Licenca**: Nao explicita | **Multi-agente**: CLI (integravel a qualquer
  agente)
- **Diferencial**: Evaluation harness para qualidade de retrieval. Cross-
  reference expansion (Markdown links, ADR refs).

### 3.4 [gnosis-mcp](https://github.com/nicholasglazer/gnosis-mcp)
- **Estrategia**: Hybrid search (FTS5 BM25 + embeddings ONNX locais) sobre
  docs indexados com smart chunking (heading-aware).
- **Como funciona**: `pip install gnosis-mcp` → ingest docs → `search_docs`
  retorna excerpts ranked (~600 tokens) com highlights. `get_doc` le doc
  completo. `get_related` encontra docs vinculados. Watch mode para
  re-ingest automatico.
- **Reducao**: ~600 tokens por search vs doc inteiro
- **Indexacao**: SQLite por padrao, PostgreSQL opcional. Embeddings ONNX
  locais (23MB, sem API key). Suporta `.md`, `.txt`, `.ipynb`, `.toml`,
  `.csv`, `.json`, `.rst`, `.pdf`.
- **Licenca**: MIT | **Multi-agente**: Sim (MCP)
- **Diferencial**: Zero config. Web crawling built-in. Git history indexing.
  550+ testes. Auto-linking via frontmatter `relates_to`.

### 3.5 [mcp-local-rag](https://github.com/shinpr/mcp-local-rag)
- **Estrategia**: Semantic chunking (por significado, nao por caractere) +
  embeddings locais (Transformers.js ~90MB) + busca semantica com keyword
  boost.
- **Como funciona**: Inger PDF/DOCX/TXT/Markdown → chunks semanticos
  coerentes (500-1000 chars) → busca com `query_documents`. Code blocks
  nunca sao partidos no meio.
- **Reducao**: Retorna apenas chunks relevantes (1-20 por query) vs
  documento inteiro
- **Indexacao**: LanceDB local, zero servidor. Modelo baixa uma vez, depois
  offline.
- **Licenca**: MIT | **Estrelas**: ~260 | **Multi-agente**: Sim (MCP + CLI)
- **Diferencial**: Keyword boost para termos exatos (`useEffect` encontra
  o termo exato, nao apenas similar semantico). Agent skills opcionais.

### 3.6 [TreeSearch](https://github.com/shibing624/TreeSearch)
- **Estrategia**: Parse docs em arvores por heading hierarchy → FTS5 SQLite
  sobre colunas estruturais (title/summary/body/code/front_matter) com
  weighting. Zero embeddings, zero API key.
- **Como funciona**: Indexa Markdown, texto, codigo (Python AST + regex,
  Java/Go/JS/C++), HTML, XML, JSON, CSV, PDF, DOCX. Modo Flat (FTS5
  direto) ou Tree (best-first search sobre arvore com heuristic scoring).
- **Reducao**: Millisecond latency, sem custo de embedding
- **Indexacao**: SQLite FTS5, incremental updates, suporte CJK
- **Licenca**: Nao explicita | **Multi-agente**: Biblioteca Python
  (integravel)
- **Diferencial**: GrepFilter para matching exato de simbolos. Source-type
  routing automatico (codigo usa GrepFilter + FTS5).

---

## Categoria 4: Context Compilation / Selecao Ranked (Codigo)

**Problema que resolvem**: O agente nao sabe quais arquivos sao relevantes
para a tarefa. Essas ferramentas rankeiam arquivos por importancia e
selecionam apenas o que cabe no token budget, enviando os arquivos
relevantes **completos** (sem compressao).

### 4.1 [graphsift](https://github.com/maheshmakvana/graphsift)
- **Como funciona**: Biblioteca Python com grafo AST (14 linguagens, 7 edge
  types). Rankeia arquivos por relevancia (BM25 + graph-distance) e
  seleciona por token budget. Modos sem compressao: FULL (source completo
  para alta relevancia), SIGNATURES (apenas assinaturas para media
  relevancia). Modo COMPRESSED via tokenpruner (opcional).
- **Reducao**: 80-150x vs source bruto (com compressao); 10-15x com apenas
  FULL + SIGNATURES
- **Cobertura**: Codigo (14 linguagens). **Nao cobre docs.**
- **Recuperacao cirurgica**: Para alta relevancia, envia source completo
  (sem perda). Para media, signatures (10-20% do original, sem perda —
  sao as assinaturas exatas). Focado em code review com diff.
- **Licenca**: MIT | **Estrelas**: ~3 | **Multi-agente**: Sim (MCP +
  adapters)

### 4.2 [codectx](https://github.com/hey-granth/codectx)
- **Como funciona**: CLI que "compila" contexto do repositorio em um unico
  `CONTEXT.md`. Scanneia o repo, constroi grafo de dependencias, scoreia
  arquivos por fan-in, frequencia de commits, proximidade de entry points.
  3 tiers: top 15% = sumarios estruturais, proximos 30% = signatures,
  resto = one-liners. Token budget enforced.
- **Reducao**: 76% em media (ate 92% no rich)
- **Cobertura**: Python, TS/JS, Go, Rust, Java, C, C++, Ruby (tree-sitter).
  **Nao cobre docs existentes, mas gera sumarios como documentacao.**
- **Recuperacao cirurgica**: O `CONTEXT.md` gerado substitui a exploracao
  inicial do codebase. O agente le o documento compilado e so vai para
  arquivos brutos se necessario. Sumarios sao AST-driven (sem perda de
  informacao estrutural).
- **Licenca**: Nao explicita | **Multi-agente**: Qualquer agente (output
  em Markdown)

---

## Categoria 5: Compressao de Conteudo (Informativo)

> **Nota**: Estas ferramentas **alteram o texto** antes de enviar ao LLM.
> Ha risco de perda de qualidade. Listadas aqui apenas como referencia para
> demandas futuras onde a completude nao seja prioritaria.

### 5.1 [Claw Compactor](https://github.com/open-compress/claw-compactor)
- **Como funciona**: Pipeline de 14 estagios: deteccao de tipo de conteudo,
  compressao AST-aware (tree-sitter), amostragem de JSON, folding de logs,
  deduplicacao semantica (SimHash), compressao de diffs, otimizacao de
  tokenizer. Compressao reversivel com RewindStore (LLM pode recuperar
  secoes originais por marcador, mas exige chamada extra).
- **Reducao**: 15-82% (code: 15-25%, JSON: ate 81.9%)
- **Cobertura**: Qualquer conteudo de texto.
- **Licenca**: MIT | **Estrelas**: ~2.2k | **Multi-agente**: Sim
  (biblioteca Python + skill OpenClaw)

### 5.2 [tokenpruner](https://github.com/tokenpruner-py/tokenpruner)
- **Como funciona**: Biblioteca Python com code minification (40-60%),
  template stripping, Jaccard deduplication (30-70%), semantic scoring,
  sliding window, hard truncation.
- **Reducao**: 70-80% (prompts), 40-60% (codigo)
- **Cobertura**: Qualquer texto.
- **Licenca**: MIT | **Multi-agente**: Sim (biblioteca independente)

---

## Matriz de Cobertura

### Ferramentas Principais (Sem Perda de Qualidade)

| Ferramenta | Codigo | Docs Texto | Recuperacao Cirurgica | Multi-agente |
|---|---|---|---|---|
| [codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) | Sim (66 langs) | Nao | Sim (snippet por simbolo) | Sim (MCP) |
| [graphify](https://github.com/AGurindapalli/graphify-llm-kbase) | Sim (AST) | Sim (LLM) | Parcial (grafo, nao conteudo) | Sim (4 agentes) |
| [code-review-graph](https://github.com/tirth8205/code-review-graph) | Sim (24 langs) | Nao | Parcial (contexto estrutural) | Sim (MCP) |
| [cortex](https://github.com/DanielBlomma/cortex) | Sim (multi) | Nao | Sim (retrieval ranked) | Sim (MCP) |
| [ContextGraph](https://github.com/chrispaulintheory/ContextGraph) | Sim (multi) | Nao | Sim (skeleton/capsule) | Via HTTP |
| [mcp-codebase-index](https://github.com/MikeRecognex/mcp-codebase-index) | Sim (5 langs) | Nao | Sim (symbol source) | Sim (MCP) |
| [codebase-graph](https://github.com/broskees/codebase-graph) | Sim (multi) | Nao | Nao (mapa de orientacao) | OpenCode |
| [TokenSlayer](https://github.com/ajvikram/TokenSlayer) | Sim (6 langs) | Nao | Sim (skeletons) | VS Code + MCP |
| [Distil](https://github.com/joshuaboys/distil) | Sim (TS/JS) | Nao | Sim (5 camadas) | Sim (MCP) |
| [mcp-code-context](https://github.com/achatainga/mcp-code-context) | Sim (5 langs) | Nao | Sim (surgical) | Sim (MCP) |
| [KBLens](https://github.com/disrei/KBLens) | Sim (4 langs) | Parcial | Sim (hierarquico) | Qualquer agente |
| [doctree-mcp](https://github.com/joesaby/doctree-mcp) | Nao | Sim (MD/CSV/JSONL) | Sim (por secao) | Sim (MCP) |
| [markdown-matters](https://github.com/srobinson/markdown-matters) | Nao | Sim (MD) | Sim (por secao) | CLI + MCP |
| [yore](https://github.com/rahulrajaram/yore) | Nao | Sim (MD/TXT) | Sim (digest ranked) | CLI |
| [gnosis-mcp](https://github.com/nicholasglazer/gnosis-mcp) | Nao | Sim (multi-format) | Sim (excerpts ranked) | Sim (MCP) |
| [mcp-local-rag](https://github.com/shinpr/mcp-local-rag) | Nao | Sim (PDF/DOCX/MD) | Sim (chunks semanticos) | Sim (MCP) |
| [TreeSearch](https://github.com/shibing624/TreeSearch) | Sim (multi) | Sim (multi-format) | Sim (tree-aware FTS5) | Biblioteca |
| [graphsift](https://github.com/maheshmakvana/graphsift) | Sim (14 langs) | Nao | Sim (FULL/SIGNATURES) | Sim (MCP) |
| [codectx](https://github.com/hey-granth/codectx) | Sim (9 langs) | Parcial | Sim (CONTEXT.md) | Qualquer agente |

### Ferramentas de Compressao (Informativo — Potencial Perda de Qualidade)

| Ferramenta | Codigo | Docs Texto | Reversivel | Multi-agente |
|---|---|---|---|---|
| [Claw Compactor](https://github.com/open-compress/claw-compactor) | Sim | Sim | Sim (RewindStore) | Biblioteca |
| [tokenpruner](https://github.com/tokenpruner-py/tokenpruner) | Sim | Sim | Nao | Biblioteca |

---

## Combos Complementares: Navegacao + Recuperacao Cirurgica

### Combo 1: "Navegacao Completa" (codebase-memory-mcp + doctree-mcp)

**Cenario**: Voce quer navegacao estrutural do codigo com retrieval de
simbolos individuais E navegacao por secao de documentos — tudo sem
perda de qualidade.

```
codebase-memory-mcp (grafo AST do codigo)
    │
    ├── Query estrutural? → MCP tools respondem (~200 tokens)
    │   (search_graph, trace_call_path, get_architecture)
    │
    ├── Precisa de uma funcao especifica? → get_code_snippet
    │   (retorna apenas a funcao, conteudo completo e preciso)
    │
    └── Precisa de contexto maior? → Le arquivo bruto (sem compressao)

doctree-mcp (indice de documentos)
    │
    ├── Busca por tema? → search_documents retorna resumos ranked
    │
    ├── Quer ver estrutura? → get_tree mostra outline com word counts
    │
    └── Precisa do conteudo? → navigate_tree ou get_node_content
        (retorna secao completa, conteudo original sem alteracao)
```

**Vantagem**: codebase-memory cobre codigo (66 linguagens, 14 MCP tools).
doctree-mcp cobre documentos (navegacao por secao, zero embeddings, MIT).
Juntos cobrem todo o repositorio sem compressao.

**Stack**: OpenCode (MCP config para ambos) + Copilot/VS Code (MCP via
extensao para codebase-memory; doctree-mcp via MCP)

---

### Combo 2: "Codebase + Docs com Semantica" (graphify + gnosis-mcp)

**Cenario**: Voce quer graphify para codigo + docs + papers + imagens,
e gnosis-mcp para busca semantica de documentos com highlights.

```
graphify (grafo de codebase + docs + papers + imagens)
    │
    ├── Query estrutural? → graphify responde sem ler arquivos
    │   ("quem chama X?", "qual a arquitetura?")
    │
    └── Precisa ler conteudo de um arquivo? → Le bruto (sem compressao)

gnosis-mcp (busca semantica de documentos)
    │
    ├── Busca por significado? → search_docs retorna excerpts com highlights
    │   (encontra mesmo com wording diferente da query)
    │
    └── Precisa do doc completo? → get_doc retorna original
```

**Vantagem**: graphify e unico que cobre codigo + docs + papers + imagens
em um unico grafo. gnosis-mcp adiciona busca semantica local (ONNX, sem
API key) com smart chunking heading-aware.

**Stack**: OpenCode (graphify skill + gnosis-mcp MCP)

---

### Combo 3: "Orientacao + Skeleton + Navegacao de Docs" (codebase-graph + TokenSlayer + doctree-mcp)

**Cenario**: Voce quer que o agente sempre tenha um mapa do codebase no
contexto, use skeletons para orientacao rapida de arquivos de codigo, e
navegue documentos por secao.

```
codebase-graph (mapa injetado no system prompt)
    │ → Agente sempre sabe a estrutura do codebase (2-5K tokens)
    │
TokenSlayer (skeletons no VS Code + Copilot)
    │ → Agente le estrutura do arquivo de codigo sem corpos
    │   (200 tokens vs 5.000 — sem perda, sao assinaturas exatas)
    │
doctree-mcp (navegacao de documentos por secao)
    └ → Agente navega docs por heading → le apenas a secao necessaria
```

**Vantagem**: codebase-graph e invisivel (injetado automaticamente).
TokenSlayer e nativo do Copilot no VS Code. doctree-mcp resolve a
navegacao de documentos com precisao cirurgica.

**Stack**: OpenCode (codebase-graph plugin + doctree-mcp MCP) + VS Code
(TokenSlayer extension)

---

### Combo 4: "Contexto Compilado + Docs" (codectx + yore)

**Cenario**: Voce quer um documento unico que resume o codebase de codigo,
e um sistema de retrieval ranked para documentos de texto.

```
codectx (gera CONTEXT.md do codebase de codigo)
    │
    ├── Tier 1 (top 15%): sumarios estruturais AST-driven
    ├── Tier 2 (proximos 30%): signatures exatas
    └── Tier 3 (resto): one-liners
    │
    └── Precisa de mais detalhe? → Le arquivo bruto (sem compressao)

yore (retrieval ranked de documentos)
    │
    ├── Busca? → yore query "auth setup"
    │
    └── Monta contexto? → yore assemble --max-tokens 3000
        (digest com citacoes, preservando code blocks e headings)
```

**Vantagem**: codectx gera um unico `CONTEXT.md` que substitui toda a
exploracao inicial do codebase. yore faz retrieval deterministico de docs
com cross-reference expansion e extractive refinement (sem embeddings).

**Stack**: Qualquer agente (ambos output em Markdown/CLI)

---

### Combo 5: "Codebase Completo com Multi-Formato" (codebase-memory-mcp + TreeSearch)

**Cenario**: Voce quer retrieval cirurgico de codigo e documentos em
multiplos formatos, tudo com zero embeddings e zero API keys.

```
codebase-memory-mcp (codigo)
    │
    ├── 66 linguagens via tree-sitter
    ├── get_code_snippet para retrieval de simbolos
    └── search_code para busca textual

TreeSearch (documentos + codigo)
    │
    ├── Markdown, texto, HTML, XML, JSON, CSV, PDF, DOCX
    ├── FTS5 SQLite sobre colunas estruturais
    ├── GrepFilter para matching exato
    └── Zero embeddings, zero API key
```

**Vantagem**: codebase-memory tem o retrieval mais rico de codigo (14 MCP
tools). TreeSearch cobre documentos em multiplos formatos com FTS5
heading-aware. Ambos zero embeddings, zero custo de API.

**Stack**: OpenCode (codebase-memory MCP + TreeSearch como biblioteca)

---

## Recomendacao para seu Stack (OpenCode + Copilot/VS Code)

Para o cenario de **navegacao inteligente por codigo e documentos com
recuperacao cirurgica sem perda de qualidade**:

### Stack Completo

| Camada | Ferramenta | Resolve |
|---|---|---|
| **Mapa estrutural (codigo)** | [codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) | Queries AST + snippet retrieval (66 langs, 14 tools) |
| **Navegacao por secao (docs)** | [doctree-mcp](https://github.com/joesaby/doctree-mcp) | Busca → outline → navega → le secao (zero embeddings) |
| **Busca semantica (docs)** | [gnosis-mcp](https://github.com/nicholasglazer/gnosis-mcp) | Encontra conteudo mesmo com wording diferente (ONNX local) |
| **Skeletons no VS Code** | [TokenSlayer](https://github.com/ajvikram/TokenSlayer) | Orientacao rapida no Copilot (assinaturas exatas) |
| **Mapa automatico** | [codebase-graph](https://github.com/broskees/codebase-graph) | Injetado no system prompt do OpenCode (2-5K tokens) |
| **Monitoramento** | [Tokenscope](https://github.com/ramtinj95/opencode-tokenscope) ou [OpenCode Quota](https://github.com/slkiser/opencode-quota) | Validar economia |

### Stack Minimal (menos pecas, maxima cobertura)

| Camada | Ferramenta | Resolve |
|---|---|---|
| **Codigo + Docs** | [graphify](https://github.com/AGurindapalli/graphify-llm-kbase) | Grafo unico de codigo + docs + papers + imagens |
| **Navegacao de docs** | [doctree-mcp](https://github.com/joesaby/doctree-mcp) | Retrieval por secao quando graphify nao basta |
| **Skeletons no VS Code** | [TokenSlayer](https://github.com/ajvikram/TokenSlayer) | Orientacao rapida no Copilot |

### Stack Focado em Codigo

| Camada | Ferramenta | Resolve |
|---|---|---|
| **Retrieval cirurgico** | [codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) | Grafo AST + get_code_snippet |
| **Skeletons no VS Code** | [TokenSlayer](https://github.com/ajvikram/TokenSlayer) | Orientacao rapida no Copilot |
| **Mapa automatico** | [codebase-graph](https://github.com/broskees/codebase-graph) | Injetado no system prompt |

### Stack Focado em Documentos

| Camada | Ferramenta | Resolve |
|---|---|---|
| **Navegacao por secao** | [doctree-mcp](https://github.com/joesaby/doctree-mcp) | BM25 + tree navigation, zero embeddings |
| **Busca semantica** | [gnosis-mcp](https://github.com/nicholasglazer/gnosis-mcp) | Hybrid search com ONNX local |
| **Digest ranked** | [yore](https://github.com/rahulrajaram/yore) | BM25 + extractive refinement com token budget |
