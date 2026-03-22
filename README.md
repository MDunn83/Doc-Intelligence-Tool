# Doc-Intelligence-Tool

A suite of AI-powered document intelligence tools built with LangChain 
and Retrieval-Augmented Generation (RAG), designed for systems engineers 
and technical program managers working with complex technical 
specifications and standards.

Instead of manually searching through hundreds of pages of standards 
and guidebooks, ask questions in plain English and get answers pulled 
directly from the source documents — with citations.

---

## Tools

### v1.0 — Standards & Guidebook Q&A (`v1_QA/`)
Natural language querying across a collection of systems engineering 
standards and technical guidebooks. Ask questions in plain English and 
get answers grounded in the source documents with page-level citations.

**Example queries:**
- *"What are the consequences of poor requirements?"*
- *"Summarize the systems engineering process"*
- *"What does the DevSecOps guide say about continuous integration?"*

**Key Features:**
- Semantic search + FlashRank reranking for improved retrieval quality
- Self-query retrieval — filter results by specific document
- Page-level citations with every answer
- Conversational memory across a session

---

### v2.0 — SHALL Requirements Auditor (`v2_shall_auditor/`)
Automatically extracts SHALL statements from a performance specification 
and evaluates each one against a knowledge base of systems engineering 
standards — improving requirement quality before development begins.

**Example findings:**
- REQ-005 *"The system SHALL handle a lot of employees without slowing 
  down"* → **NON-COMPLIANT** — "a lot" and "without slowing down" are 
  not quantifiable and cannot be verified
- REQ-009 *"The system SHALL sometimes notify employees"* → 
  **NON-COMPLIANT** — "sometimes" fails to define triggering conditions

**Key Features:**
- Automatic SHALL statement extraction from any PDF
- LLM selects the most relevant standard(s) for each requirement
- FlashRank reranking for improved retrieval quality
- Citation tracking with source document and page number
- Temporary vector DB per audit session — no data persistence

---

## Document Sources

Both tools use the same knowledge base. You can swap in your own 
documents — the pipeline is document-agnostic.

Current knowledge base:
- `MIL-STD-882E.pdf` — System Safety
- `DOD SysEng Guidebook.pdf` — Systems Engineering best practices
- `Scrum-Guide-US-2020.pdf` — Agile/Scrum methodology
- `DOD_DevSecOps Fundamentals.pdf` — DevSecOps practices

---

## Tech Stack

- **LangChain** — RAG pipeline orchestration
- **Google Gemini** (gemini-flash-latest + gemini-embedding-001) — LLM 
  and embeddings
- **ChromaDB** — Vector store
- **FlashRank** (ms-marco-MiniLM-L-12-v2) — Cross-encoder reranking
- **PyPDF** — PDF document loading
- **Google Colab** — Runtime environment

---

## Setup

### 1. Open in Google Colab
Both notebooks are designed to run in Google Colab — no local 
installation required.

### 2. Add your Google API key
In Colab, go to the 🔑 Secrets tab in the left sidebar and add:
```
GOOGLE_API_KEY = your-key-here
```

### 3. Upload your PDF documents
Upload your standards PDFs and (for v2) your requirements PDF directly 
to the Colab session.

### 4. Run all cells in order

> **Note:** The first run will embed your documents. Subsequent runs 
> load from the saved vector store at no additional cost.

---

## Project Roadmap

| Version | Tool | Status |
|---------|------|--------|
| v1.0 | Standards & Guidebook Q&A | ✅ Complete |
| v2.0 | SHALL Requirements Auditor | ✅ Complete |
| v3.0 | Verification Method Generator — auto-generates Section 4 verification methods from Section 3 requirements using Graph RAG | 📋 Planned |

---

## Security

- Never commit your API key
- Add `.env` to your `.gitignore` if running locally
- Anyone cloning this repo needs their own Google API key to run it

---

*Built as part of a Defense PM → AI Product Manager transition portfolio*
