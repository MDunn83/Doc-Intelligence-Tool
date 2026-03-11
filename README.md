
Readme · MD
Copy

# Doc-Intelligence-Tool

Document intelligence tool that enables natural language querying across systems engineering standards and technical guidebooks using LangChain and Retrieval-Augmented Generation (RAG).

Instead of manually searching through hundreds of pages of standards and guidebooks, ask questions in plain English and get answers pulled directly from the source documents — with citations.

---

## What It Does

- Loads and indexes a collection of systems engineering and DevSecOps reference documents
- Accepts natural language questions
- Retrieves the most relevant document chunks using semantic search + reranking
- Returns answers grounded in the source documents with page-level citations

**Example queries:**
- *"What are the consequences of poor requirements?"*
- *"Summarize the systems engineering process"*

---

## Document Sources

You can use your own, but chose the following for this pilot project:

- `MIL-STD-882E.pdf`
- `DOD SysEng Guidebook.pdf`
- `Scrum-Guide-US-2020.pdf`
- `DOD_DevSecOps Fundamentals.pdf`

---

## Tech Stack

- **LangChain** — RAG pipeline orchestration
- **OpenAI** — Embeddings and LLM (gpt-3.5-turbo)
- **ChromaDB** — Vector store
- **FlashRank** — Reranking for improved retrieval quality
- **PyPDF** — PDF document loading

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/MDunn83/Doc-Intelligence-Tool.git
cd Doc-Intelligence-Tool
```

### 2. Install dependencies
```bash
pip install langchain langchain-community langchain-openai langchain-chroma flashrank "numpy<2"
```

### 3. Add your OpenAI API key
Create a `.env` file in the root directory:
```
OPENAI_API_KEY=your-key-here
```

### 4. Add your PDF documents
Place your PDFs in the project root directory.

### 5. Run the notebook
Open `Doc-Intelligence-Tool.ipynb` in Jupyter and run all cells.

> **Note:** The first run will embed your documents (~$0.50 one-time cost). Subsequent runs should load from the saved vector store at no additional cost.

---

## Project Roadmap

| Version | Tool | Status |
|---------|------|--------|
| v1.0 | MIL-STD Q&A Tool — natural language querying of SE standards | ✅ Complete |
| v2.0 | Requirements Quality Checker — evaluates SHALL statements against SE best practices | 🔄 In progress |
| v3.0 | Verification Method Generator — auto-generates Section 4 verification methods from requirements | 📋 Planned |

---

## Security

- Never commit your `.env` file
- Add `.env` and `docs/chroma/` to your `.gitignore`
- Anyone cloning this repo needs their own OpenAI API key

---

*Built as part of a Defense PM → AI Product Manager transition portfolio*
