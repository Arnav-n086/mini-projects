# RAG Chatbot Progression

A set of Jupyter notebooks that build up a Retrieval-Augmented Generation (RAG) chatbot step by step, from a plain dictionary lookup to a multi-database routing RAG system. The running example throughout is an e-commerce FAQ assistant for a store that sells e-books and courses for IT professionals.

Each notebook is self-contained and adds one new idea on top of the concepts introduced before it.

## Notebooks

| # | Notebook | What it introduces |
|---|----------|---------------------|
| 01 | [01_dictionary_faq_chatbot.ipynb](01_dictionary_faq_chatbot.ipynb) | Baseline: a hardcoded `dict` of FAQ question → answer, looked up with an exact string match. No AI involved — establishes the problem RAG solves (it only works for exact-match questions). |
| 02 | [02_simple_openai_chatbot.ipynb](02_simple_openai_chatbot.ipynb) | A plain LLM chatbot using the OpenAI Chat Completions API with a system prompt, no retrieval — the model answers from its own knowledge only. |
| 03 | [03_first_rag_chatbot.ipynb](03_first_rag_chatbot.ipynb) | First real RAG pipeline: embeds the FAQ questions with `text-embedding-3-small`, stores the vectors in an in-memory `dict`, and retrieves the best match by manual cosine similarity before answering. |
| 04 | [04_rag_chatbot_pinecone.ipynb](04_rag_chatbot_pinecone.ipynb) | Swaps the in-memory vector store for a real vector database (Pinecone). Upserts FAQ embeddings into a `faq-database` index and queries it for the top match. Ends with an interactive `while True` chat loop. |
| 05 | [05_multi_query_rag_chatbot.ipynb](05_multi_query_rag_chatbot.ipynb) | Multi-query retrieval: an LLM call rewrites the user's question into several alternative phrasings (as a JSON list), each is embedded and retrieved separately, and the results are combined before answering. |
| 06 | [06_fusion_rag_chatbot.ipynb](06_fusion_rag_chatbot.ipynb) | Extends multi-query retrieval with **Reciprocal Rank Fusion (RRF)**: each generated query retrieves its own top-N documents, and the per-query rankings are merged into a single ranked list before building the context. |
| 07 | [07_hyde_rag_chatbot.ipynb](07_hyde_rag_chatbot.ipynb) | **HyDE** (Hypothetical Document Embeddings): instead of embedding the user's question directly, the LLM first writes a hypothetical answer document, and *that* is embedded and used for retrieval. |
| 08 | [08_prompt_routing_rag_chatbot.ipynb](08_prompt_routing_rag_chatbot.ipynb) | Prompt routing: classifies the query's intent (`factual` / `explanation` / `guidance`) and picks a different prompt template per intent before calling the LLM. No retrieval yet — sets up the routing idea used next. |
| 09 | [09_database_routing_rag_chatbot.ipynb](09_database_routing_rag_chatbot.ipynb) | Database routing: three separate Pinecone indexes (`product-faq`, `finance-faq`, `tech-faq`), each seeded with its own FAQ set. The query's intent is classified and routed to the matching index before retrieval and answering. |

## Setup

1. **Python environment** — each notebook installs its own extra dependencies inline (`%pip install openai`, `%pip install pinecone`), but at minimum you'll need:
   - `openai`
   - `pinecone`
   - `python-dotenv`
   - `numpy` (used for manual cosine similarity in notebook 03)

2. **Environment variables** — create a `.env` file in this folder (already git-ignored) with:
   ```
   OPENAI_API_KEY=sk-...
   PINECONE_API_KEY=pcsk_...
   ```

3. **Pinecone indexes** — notebooks 04–07 expect a Pinecone index named `faq-database` (namespace `ns1`) populated by running notebook 04's upsert cell first. Notebook 09 creates and seeds three of its own indexes (`product-faq`, `finance-faq`, `tech-faq`) directly in its own cells. All indexes use `text-embedding-3-small` embeddings (1536 dimensions).

## Suggested order

Run the notebooks in numeric order (01 → 09) — each one assumes the concepts (and sometimes the Pinecone data) from the previous notebooks are already in place. Notebook 04 must be run at least once before 05, 06, or 07 so the `faq-database` index is populated.

## Notes

- The FAQ content and system prompts are intentionally simple/repeated across notebooks so each one can be run independently once its Pinecone index exists.
- A few notebooks (03, 09) contain known rough edges left over from experimentation (e.g. variable shadowing, a routing typo) — they're kept as-is since the point of this series is exploring RAG techniques, not shipping production code.
