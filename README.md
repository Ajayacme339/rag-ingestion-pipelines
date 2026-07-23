# RAG Ingestion Pipelines

**Retrieval-Augmented Generation** pipelines with ingestion support for multiple source formats — documents, JSON, Word, CSV/Excel, and SQL databases.

## What's inside

| Module | Description |
|---|---|
| `DOCUMENT_RAG` | Full RAG application: document processor, LangGraph graph builder + nodes, RAG state management, ReAct node, and a **Streamlit UI** (`streamlit_app.py`). Handles PDF, CSV, and Excel corpora |
| `Json_file_ingestion` | Ingesting JSON files into the vector pipeline |
| `Word_document_Ingestion` | Ingesting Word documents |
| `csv_excel_ingestion` | Ingesting tabular CSV/Excel sources |
| `sqldatabaseingestion` | Ingesting from SQL databases |

## Architecture (DOCUMENT_RAG)

```
Source files → document_processor → chunks → embeddings/vector store
                                                   ↓
User question → LangGraph (nodes + ReAct) → retrieval → grounded answer
                                                   ↓
                                            Streamlit UI
```

## Tech stack

Python · LangChain · LangGraph · Streamlit · vector stores

## Run the document RAG app

```bash
cd DOCUMENT_RAG
pip install -r requirements.txt
streamlit run streamlit_app.py
```

Part of my ongoing AI engineering learning journey.

## 🧠 Key Engineering Decisions & Tradeoffs

- **Multi-format ingestion as first-class citizens.** Separate ingestion paths for PDF, CSV, Excel, JSON, Word, and SQL sources — because real enterprise knowledge is never one clean format. This is the data-engineering discipline most "chat-with-your-PDF" demos skip.
- **`RecursiveCharacterTextSplitter`, chunk size 500 / overlap 50.** Small chunks favor retrieval precision over broad context; the 50-token overlap preserves continuity across boundaries. Tradeoff: more chunks means more embedding cost, accepted here for answer precision.
- **FAISS for the vector store.** In-memory, zero-infra, fast to iterate locally. Tradeoff: not persistent or distributed — a deliberate choice for a single-node app; production scale would swap to a managed vector DB (e.g. pgvector / Pinecone).
- **LangGraph state machine (retrieve → generate) with a ReAct node.** An explicit graph makes the flow inspectable and extensible versus a monolithic chain; the ReAct node allows tool-augmented reasoning when a straight lookup isn't enough.
- **Retriever `k=4`.** Balances recall against context-window cost and answer focus.
- **GPT-4o + OpenAI embeddings, isolated in `config.py`.** Model choice is centralized so swapping providers is a one-line change.

## 📊 Evaluation & Results

Evaluation is currently manual and qualitative. A quantitative harness — retrieval hit-rate (relevant chunk in top-*k*) plus answer groundedness on a labeled question set — is the next planned addition, following the reusable `llm-eval-harness` pattern in my portfolio.
