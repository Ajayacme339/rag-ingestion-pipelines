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
