# AI Knowledge Assistant (RAG)

Upload PDF, TXT or Markdown documents and ask questions about them. Answers are generated only from the documents, with the retrieved chunks and their similarity scores shown.

**Tech:** Python, FastAPI, sentence-transformer embeddings (`all-MiniLM-L6-v2` via FastEmbed/ONNX), FAISS, Google Gemini.

## Assignment mapping
| Requirement | Where |
|---|---|
| Part 1: zero-shot, few-shot, role-based templates | `app/prompts.py` |
| Part 1: same 5 questions, compare and explain | `questions.json`, `scripts/compare_prompts.py`, `docs/REPORT.md` |
| Part 2: load 5+ docs, chunk, embed, store, top-3 retrieval with scores | `data/` (5 docs), `app/retrieval.py`, `scripts/semantic_search_demo.py` |
| Part 2: semantic vs keyword explanation | `docs/REPORT.md` |
| Upload PDF/TXT/MD + Q&A app | `app/main.py`, `static/index.html` |

## Setup
```bash
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Mac/Linux
pip install -r requirements.txt
copy .env.example .env         # Mac/Linux: cp .env.example .env
```
Put your Gemini API key in `.env` (free: https://aistudio.google.com/app/apikey).

## Run the web app
```bash
uvicorn app.main:app
```
Open http://127.0.0.1:8000, click **Load sample documents** (or upload your own files), choose a prompting technique and ask a question. **Compare all 3 techniques** shows the three answers side by side.

## Run the experiments (outputs go to `results/`)
```bash
python -m scripts.semantic_search_demo   # Part 2: top-3 chunks with scores + keyword comparison
python -m scripts.compare_prompts        # Part 1: 3 prompts x 5 questions, scored
```
The first run downloads the embedding model (about 90 MB), so it takes a little longer.

## Error handling
No document uploaded, empty question, unsupported file format, oversized or scanned (no text) files and a missing API key all return a clear message.
