# Desafio RAG Multi-Agente com LangGraph

## Objetivo

Construa um backend Python containerizado com Docker que ingere documentos em um vector store e responde perguntas por meio de um fluxo de trabalho **multi-agente com LangGraph**. Você decide a topologia do grafo, a estratégia de recuperação (retrieval) e os controles de qualidade — este desafio descreve *o que* o sistema deve fazer, não *como* implementá-lo.

Implemente um pequeno serviço que:

1. **Ingere** documentos (PDF, Markdown, TXT) em um **vector store**
2. **Responde perguntas** por meio de um fluxo de trabalho multi-agente em **LangGraph** que usa **RAG**, pelo menos **duas tools**, e **recuperação e/ou geração com controle de qualidade** (roteamento, correção, verificação — ou outra abordagem que você justifique)
3. Expõe uma **API REST** para que a ingestão e a consulta possam ser testadas de ponta a ponta

Você é responsável pela organização dos módulos, pelo design dos nós (nodes) e por como o controle de qualidade é implementado.

## Requisitos obrigatórios

Estes são requisitos de capacidade. Atendê-los com um design que não seja uma cópia de livro-texto de nenhum paper específico de RAG é aceitável — e incentivado —, desde que o sistema seja sólido.

### LangGraph

- Use um `StateGraph` com **estado tipado**
- Inclua **roteamento condicional**
- Inclua ao menos um **ciclo ou caminho de retry** com uma **condição de término explícita** (sem loops não limitados)

### RAG

- Faça chunking dos documentos, gere embeddings e recupere (retrieve) usando um vector store
- Fundamente (ground) as respostas no contexto recuperado quando apropriado

### Vector store

- Use um **vector store à sua escolha** (por exemplo, Qdrant, Chroma, pgvector, Weaviate, FAISS — justifique a escolha no seu `ARCHITECTURE.md`)
- Execute-o via Docker Compose quando for um serviço; stores embutidos/in-process são aceitáveis se o cold start ainda funcionar apenas com o `.env`
- Suporte o ciclo de vida de coleções (ou índice/namespace equivalente) através da API (listar / deletar, e criar ou popular como parte da ingestão)

### Tools

- Implemente pelo menos **duas** tools de LangGraph/LangChain (por exemplo: recuperação vetorial + busca na web)
- As tools devem ser **chamáveis a partir do grafo** (não podem ser código morto)

### Python

- Projeto tipado e empacotado, com uma organização de módulos clara, à sua escolha
- Tratamento de erros sensato para falhas de API e do pipeline

### Docker

- `Dockerfile` + `docker-compose.yml` com pelo menos o serviço `app`, mais qualquer serviço de vector store que o seu design exija
- Cold start apenas a partir do `.env` (candidato deve fornecer um `.env.example`)

### Contrato de API

Implemente estes endpoints. Os **formatos** de request/response são livres, desde que o contrato abaixo seja respeitado e um revisor consiga testar o fluxo com curl ou similar.

| Método | Caminho | Comportamento |
|--------|---------|---------------|
| `POST` | `/ingest` | Aceita documentos enviados (PDF, Markdown, TXT); executa a ingestão no seu vector store |
| `POST` | `/query` | Executa o grafo do agente; retorna a **resposta** mais **metadados** suficientes para entender como a execução se comportou (por exemplo, fontes, etapas percorridas) — o formato é definido pelo candidato |
| `GET` | `/collections` | Lista as coleções (ou índices/namespaces equivalentes) do seu vector store |
| `DELETE` | `/collections/{name}` | Remove uma coleção (ou equivalente) |
| `GET` | `/health` | Verificação de liveness |

## Itens de stretch (opcionais)

Diferenciais que ajudam a evidenciar senioridade. Nenhum é obrigatório para uma submissão completa.

- Recuperação híbrida ou de outra forma aprimorada
- Respostas de query em streaming
- Testes automatizados e CI
- Observabilidade (tracing ou logs de execução estruturados)
- Multi-tenancy ou escopo de query por coleção
- Outras melhorias com foco em produção que você consiga justificar

## Entregáveis (repositório)

1. **Sistema funcional** — `docker compose up` sobe a aplicação (e o seu vector store, se for separado); ingestão e query funcionam de ponta a ponta
2. **`ARCHITECTURE.md`** — diagrama do *seu* grafo, escolha de vector store, decisões de ingestão/recuperação, tools, como você lida com recuperação ou respostas fracas e os limites dos loops, trade-offs de embedding/chunking, e como você escalaria a solução
3. **`README.md`** próprio desta pasta com instruções de execução (cold start a partir do `.env`)
4. **Vídeo curto** (Loom ou similar) mostrando o cold start e uma ingestão e query bem-sucedidas

**Ferramentas de IA:** Você pode usar assistentes de IA durante a construção do desafio.

## Sessão ao vivo

Submissões que passarem na revisão inicial incluirão uma **sessão de acompanhamento ao vivo** (tipicamente 60 minutos) no seu repositório — explicando seu design e fazendo alterações pontuais no código. Assistentes de código não são usados nessa sessão; consultar documentação de bibliotecas é permitido.

## O que estamos avaliando

- Um fluxo de trabalho multi-agente real em LangGraph (não um script linear de retrieve → generate, sem roteamento ou retries)
- Tools que o grafo de fato utiliza
- Evidência de reflexão sobre qualidade de recuperação/geração e modos de falha
- Empacotamento Python limpo e uma configuração Docker reprodutível
- Trade-offs honestos no `ARCHITECTURE.md`

Boa sorte — e projete algo que você seja capaz de defender.
