# Automated Requirements Quality Assistant (ARQA)

> *Technical standards don't read themselves. This tool does.*

An AI-powered document intelligence suite built with LangChain and RAG (Retrieval-Augmented Generation), designed for systems engineers and technical program managers working with complex specifications and standards.

Instead of manually searching through hundreds of pages of guidebooks, ask questions in plain English and get answers pulled directly from the source documents — with page-level citations.

---

## Tools

### v1.0 — Standards & Guidebook Q&A (`v1_Query/`)

Natural language querying across a collection of systems engineering standards and technical guidebooks. Ask questions in plain English and get answers grounded in the source documents with page-level citations.

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

### v2.0 — Automated Requirements Quality Assistant (`v2_ARQA/`)

Automatically extracts SHALL statements from a performance specification and evaluates each one against a knowledge base of systems engineering standards — improving requirement quality before development begins.

**Example findings (PayCore360 synthetic audit — 10 requirements):**
- REQ-005 *"The system SHALL handle a lot of employees without slowing down"* → **NON-COMPLIANT** — "a lot" and "without slowing down" are not quantifiable and cannot be verified. Revised example provided.
- REQ-006 *"The system SHALL be secure and protect employee data"* → **NON-COMPLIANT** — "secure" is undefined; missing SSE principles, risk assessment basis, and operational resilience criteria.
- REQ-009 *"The system SHALL sometimes notify employees when their paycheck is ready"* → **NON-COMPLIANT** — "sometimes" fails to define triggering conditions or frequency.
- REQ-010 *"The system SHALL comply with all applicable laws and regulations"* → **NON-COMPLIANT** — blanket compliance statement; no specific statutes named, no V&V criteria, no flow-down to subcontractors.

**Validation Results (10-requirement synthetic audit):**

| Metric | Result |
|--------|--------|
| Requirements audited | 10 |
| NON-COMPLIANT findings | 5 |
| Substantive gaps flagged | 3 |
| Near-passes | 2 |
| Citations per finding | 3 (avg), page-level |
| Suggested rewrites provided | Yes — all non-compliant findings |

All findings include specific citations to source documents and pages, identified gaps mapped to named standards sections, and concrete suggested rewrites.

**Key Features:**
- Automatic SHALL statement extraction from any PDF
- Header/structural SHALL filtering — avoids auditing non-requirement statements
- LLM selects the most relevant standard(s) for each requirement
- FlashRank reranking for improved retrieval quality
- Citation tracking with source document and page number
- Temporary vector DB per audit session — no data persistence between runs

---

## Why Two LLMs?

v1 uses OpenAI for LLM and embeddings. v2 migrated to Google Gemini (`gemini-flash-latest` + `gemini-embedding-001`) to reduce cost and remove the OpenAI dependency for Colab users, making the tool free to run for anyone with a Google API key.

---

## Document Sources

Both tools use the same knowledge base. The pipeline is document-agnostic.  Swap in your own standards and the tool adapts.

**Current knowledge base:**
- `MIL-STD-882E.pdf` — System Safety
- `DOD SysEng Guidebook.pdf` — Systems Engineering best practices
- `Scrum-Guide-US-2020.pdf` — Agile/Scrum methodology
- `DOD_DevSecOps Fundamentals.pdf` — DevSecOps practices

---

## Tech Stack

| Component | Tool |
|-----------|------|
| RAG orchestration | LangChain |
| LLM (v1) | OpenAI |
| LLM (v2) | Google Gemini (`gemini-flash-latest`) |
| Embeddings (v1) | OpenAI |
| Embeddings (v2) | Google Gemini (`gemini-embedding-001`) |
| Vector store | ChromaDB |
| Reranker | FlashRank (`ms-marco-MiniLM-L-12-v2`) |
| PDF loading | PyPDF |
| Runtime | Google Colab |

---

## Setup

### 1. Open in Google Colab

Both notebooks are designed to run in Google Colab, so no local installation is required.

### 2. Add your API keys

In Colab, go to the 🔑 Secrets tab in the left sidebar and add:

- **v1 (Query):** `OPENAI_API_KEY`
- **v2 (ARQA):** `GOOGLE_API_KEY`

### 3. Upload your PDF documents

Upload your technical standards or requirements PDFs directly to the Colab session storage.

### 4. Run all cells in order

> **Note:** The first run embeds your documents into a ChromaDB vector store. Subsequent runs load from the saved store to optimize performance and reduce API costs.

---

## Project Roadmap

| Version | Tool | Status |
|---------|------|--------|
| v1.0 | Standards & Guidebook Q&A | ✅ Complete |
| v2.0 | Automated Requirements Quality Assistant (ARQA) | ✅ Complete |
| v3.0 | Verification Method Generator — RAG-grounded generation of IEEE/MIL-STD-compliant verification methods for each SHALL requirement | 📋 Planned |

---

## Security

- Never commit your API key
- Add `.env` to your `.gitignore` if running locally
- Anyone cloning this repo needs their own API key to run it

---

## Known Limitations

- **No ground truth oracle:** The tool flags requirements against standards language, not against a definitive pass/fail ruleset. Findings are grounded in the source documents but require human review before action.
- **SHALL extraction is regex-based:** The current extractor catches most requirement statements but may miss SHALL statements with unusual formatting or split across lines.
- **Header filtering is heuristic:** The structural SHALL filter (e.g., "The following table SHALL...") reduces false positives but is not exhaustive.
- **Colab session storage only:** Uploaded PDFs and the vector store do not persist between Colab sessions. Re-upload and re-embed on each new session.

---

*Built to solve real problems encountered managing large-scale technical programs — and to explore what's possible when AI meets program management workflows.*
