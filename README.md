# Insurance-Policy-PDF-Retrieval-Structured-Extraction
Upload an insurance policy PDF and get back a clean, structured summary as **JSON** and **CSV**. The pipeline extracts the policy name, coverage benefits, exclusions, waiting periods and claim settlement requirements, with page references for every item.

It combines **retrieval-augmented generation (RAG)** with a local open-source LLM, so no paid API is needed.

---

## Features

- Reads any text-based insurance PDF
- Semantic search over the document using vector embeddings
- Extracts five fields into a fixed schema
- Cites the source page numbers for every field
- Handles messy LLM output (code fences, trailing commas, truncated JSON)
- Regex cross-check for waiting periods as a safety net
- Exports to JSON (full detail) and CSV (flat table for Excel / Sheets)
- Built-in quality check that flags empty or failed fields

---

## How It Works

```
PDF -> pages -> chunks -> embeddings -> Chroma vector DB
                                              |
Field queries -> vector search -> relevant chunks -> LLM (JSON-only prompt)
                                              |
                                  parse + validate -> JSON / CSV
```

| Part | Steps | What happens |
|---|---|---|
| A. Ingestion and retrieval | 1 to 8 | The PDF is read, cut into chunks, converted to vectors, stored in a vector database and searched by meaning. |
| B. Structured extraction | 9 to 16 | For each field, the best-matching chunks are fetched and an LLM turns them into structured JSON. |

---

## Tech Stack

| Component | Tool |
|---|---|
| PDF loading | `pypdf` via LangChain `PyPDFLoader` |
| Chunking | LangChain `RecursiveCharacterTextSplitter` |
| Embeddings | `BAAI/bge-small-en-v1.5` (sentence-transformers) |
| Vector database | ChromaDB |
| LLM | `Qwen/Qwen2.5-3B-Instruct` (Hugging Face Transformers, fp16) |
| Data handling | pandas |
| Environment | Google Colab (T4 GPU) |

---

## Requirements

- Google Colab (the upload and download steps use `google.colab.files`)
