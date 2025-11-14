# Desafio Técnico — Sistema Agente + Vetor + MCP (Model Context Protocol)

## Objetivo

Implementar um **sistema agêntico** que:

1. **Receba** um **artigo científico como entrada** (PDF, URL ou texto bruto).  
2. **Classifique** o artigo em **uma** das **três áreas científicas** previamente definidas.  
3. **Extraia** informações do artigo no **formato JSON** abaixo:

```json
{
  "what problem does the article propose to solve?": "",
  "step by step on how to solve it": ["", "", ""],
  "conclusion": ""
}
```

4. **Gere uma resenha crítica** do artigo, **apontando aspectos positivos** e **possíveis falhas**.

Para que o sistema realize a classificação, o candidato deve **construir** uma *vector store* contendo **9 artigos** (3 áreas científicas × 3 artigos por área), e **export** o acesso a esse *vector store ao sistema agêntico* via um **servidor MCP**.

---

## Escopo e Restrições

- **Áreas científicas**: escolha **3** entre (exemplos) *Física*, *Biologia*, *Ciência da Computação*, *Química*, *Economia*, *Medicina*, *Engenharia*, etc.  
- **Artigos do vector store**: podem ser de domínio público; se usar PDFs grandes, faça chunking.  
- **Framework do agente**: livre (ex.: **CrewAI**, **AutoGen**, **LangGraph**, **LangChain Agents**, etc.).  
- **Servidor MCP**: implemente um servidor que exponha **pelo menos duas ferramentas**:  
  1. `search_articles(query: string) -> [{id, title, area, score}]`  
  2. `get_article_content(id: string) -> {id, title, area, content}`  
- **Extração**: preencha **exatamente** as chaves do JSON acima (note os *typos* em “article”), mantendo o **idioma do artigo** na resposta extraída.  
- **Resenha**: texto em **português**, citando **pontos fortes** e **fragilidades/metodologia/ameaças à validade**.  
- **Sistema agêntico**: sistema deve ser multi-agêntico

---

## Entregáveis (repositório)

1. `README.md` (instruções de execução e arquitetura)  
2. Código do **servidor MCP**  
3. Código do **agente**  
4. Apresentação dos frameworks avaliados com justificativa do motivo de escolha, privilegiando flexibilidade,  abrangência e escalabilidade.  
5. Script/notebook para **montar e popular o vector store** (9 artigos)  
6. **Testes automatizados** mínimos (ver ”Como o Candidato Deve Apresentar”)  
7. **Amostras**:  
   - `samples/input_article_1.(pdf|txt|md|url)`  
   - `samples/output_1.json` (extração)  
   - `samples/review_1.md` (resenha)  
8. `requirements.txt` ou `pyproject.toml` (ou equivalente)  
9. (Opcional) `docker-compose.yml` para *one-click run*

---

## Requisitos Técnicos

- **Vector store**: livre (FAISS, Chroma, Milvus, Weaviate, Qdrant, etc.).  
- **Embeddings**: livre (OpenAI, local, HuggingFace, etc.).  
- **Chunking**: justificar *window size* e *overlap*.  
- **MCP**: servidor com **manifest** e **tools** claras; documente como o agente consome as ferramentas MCP.  
- **Classificação**: combine `similarity search` ao *centroid* por área ou *few-shot* com *retrieval*.  
- **Extractor**: projetar *prompt* para preencher **estritamente** o JSON alvo.  
- **Resenhista**: prompt com *rubrica* (novidade, método, validade, ameaças, replicabilidade).  
- **Testes** e *hardening*: lidar com PDF ruim, timeouts, ausência de metadados.

---

## Fluxo Esperado (alto nível)

1. **Ingestão**: baixar/ler 9 artigos, normalizar metadados (título, autores, área), chunking e indexação no vector-store.  
2. **Servidor MCP**: publicar `search_articles` e `get_article_content`.  
3. **Agente**:  
   - Recebe um artigo novo (entrada do usuário).  
   - Se o artigo vier como URL/PDF, faz *fetch \+ OCR* (se necessário) e normalização.  
   - Gera *embedding* e consulta o vector store via MCP para contexto.  
   - **Classifica** o artigo em uma das 3 áreas.  
   - **Extrai** o JSON no formato exigido.  
   - **Produz** a **resenha crítica**.  
   - Devolve: `{area, extraction_json, review_md}`.

---

## Critérios de Avaliação

1. **Aderência Funcional**   
   - Constrói e popula o vector store (9/9)   
   - MCP funcional com as tools pedidas   
   - Pipeline completo (entrada → classificação → extração JSON → resenha)   
2. **Qualidade Técnica**   
   - Arquitetura clara e multi-agêntica  
   - Desacoplamento MCP/Agente/Index   
   - Boas práticas (tipagem, testes, logs, tratamento de erros)   
   - Eficiência de retrieval (top-k, score, filtros)   
   - Classificador: avaliar edge case (processamento de artigo que não pertence à um dos três armazenados na vector-store)  
3. **Qualidade da Extração & Resenha**   
   - JSON **exato** (chaves iguais às especificadas) e conteúdo coerente   
   - Resenha crítica e equilibrada (pontos positivos e falhas)   
4. **DX & Documentação**   
   - `README` reprodutível, comandos de *setup* e *run* claros 

**Penalidades**

- MCP inexistente ou não consumido pelo agente  
- Sistema mono-agêntico  
- Rodar o teste em plataforma online ao invés de usar SDK local

---

## Como o Candidato Deve Apresentar (mínimo)

- **Setup**:

```shell
# Exemplo (ajuste aos seus scripts):
make setup
make index   # constrói e popula o vector store
make mcp     # sobe o servidor MCP
make agent   # inicia o agente
```

- **Teste 1**: classificar e extrair de `samples/input_article_1.pdf`  
  - Espera-se um dos rótulos entre suas 3 áreas.  
  - Gera `out/extraction_1.json` (estrutura idêntica à especificada) e `out/review_1.md`.  
- **Teste 2**: enviar **URL** de artigo curto (abstract) e repetir.  
- **Teste 3**: *edge case* — artigo fora das 3 áreas → comportar-se com a **melhor aproximação** (classificar na mais próxima) e justificar na resenha.

---

## Dicas

1. **Selecionar áreas** e artigos (9 no total); salvar metadados.  
2. **Normalizar** texto (limpeza, segmentação, chunking) e **indexar** com embeddings.  
3. **Implementar MCP** (manifest \+ tools) com respostas simples e consistentes.  
4. **Construir os Agentes** (CrewAI/AutoGen/LangGraph) com *tools* MCP plugadas.  
     
5. **ExtratorS e Resenhista**: explorar arquiteturas multi-agênticas para obter os resultados, como hand-offs, debates, etc  
6. **Rastreabilidade**: guarde `source_id`, página, `chunk_id` no retorno do MCP.  
7. **Validação de JSON**: use *pydantic*/*jsonschema* para garantir chaves exatas (inclusive o *typo* “article”).  
   

---

## Template de Saída do Agente

```json
{
  "area": "Biologia",
  "extraction": {
    "what problem does the artcle propose to solve?": "…",
    "step by step on how to solve it": ["Passo 1 …", "Passo 2 …"],
    "conclusion": "…"
  },
  "review_markdown": "## Resenha
**Pontos positivos:** …
**Possíveis falhas:** …
**Comentários finais:** …"
}
```

---

## Bônus

* **Uso do Linux**

---

## Prazo/Entrega

1. **Prazo sugerido**: 5 dias corridos.  
2. **Compartilhamento do repositório:** Ao finalizar envie o link do repositório (público) com instruções claras no [`README.md`](http://README.md) para [**rh@most.com.br**](mailto:rh@most.com.br).

**Observação:** Aceitaremos **também** a versão do JSON com as chaves corrigidas (“article”, “positive”), **mas a versão oficial de avaliação usa exatamente as chaves** conforme o enunciado.

3. **Vídeo de demonstração**  
   * Grave um **vídeo da sua tela (com áudio)** apresentando a **execução completa do fluxo do sistema** e destacando as principais funcionalidades;  
   * Inclua também um **breve tour explicativo pelo código-fonte** e, se desejar, mencione qualquer documento técnico adicional (ex: resumo técnico ou instruções de uso);  
   * Envie o **link do vídeo** (YouTube não listado, Google Drive, etc.) para [**rh@most.com.br**](mailto:rh@most.com.br). Esse vídeo será utilizado na **etapa de pré-seleção**.

   * **Os candidatos pré-selecionados** serão convidados para uma **apresentação técnica** com o time da MOST.

     
