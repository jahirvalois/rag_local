# Local RAG + Chat Assistant (Cross-Platform)

> Build and run **Retrieval-Augmented Generation (RAG)** + **Chat** assistant locally on **Windows** or **Linux**, using open-weight models.

---

## Overview

This project lets you:
- 🗂️ Ingest PDFs, TXTs, or Markdown files.
- 💬 Chat naturally with your own documents.
- 🧩 Run everything locally — even on CPU.
- ⚙️ Upgrade to GPU or multi-GPU servers easily.

---

## Tech Stack

| Component | Tool | Purpose |
|------------|------|----------|
| **Model Runner** | [Ollama](https://ollama.ai) / [vLLM](https://github.com/vllm-project/vllm) | Local inference (CPU/GPU) |
| **Framework** | [LlamaIndex](https://github.com/run-llama/llama_index) | RAG orchestration and chat engine |
| **Vector DB** | [Chroma](https://www.trychroma.com) | Vector search and similarity |
| **Embeddings** | [Sentence-Transformers](https://www.sbert.net) | Text & image embeddings |
| **UI** | [Streamlit](https://streamlit.io) | Web-based Chat interface |

---

## Windows Setup

```powershell
# 1️⃣ Install dependencies
winget install Python.Python.3.11
winget install Ollama.Ollama

# 2️⃣ Pull starter models
ollama pull phi3:mini
ollama pull nomic-embed-text

# 3️⃣ Clone and setup project
git clone https://github.com/<your-repo>/rag_local.git
cd rag_local
python -m venv .venv
. .\.venv\Scripts\Activate.ps1
pip install -U -r requirements.txt

# 4️⃣ Build index and start chat
python .\app\build_index.py
streamlit run .\app\chat_ui.py
```

Then open **http://localhost:8501**

---

## Linux Setup

```bash
# 1️⃣ System setup
sudo apt update && sudo apt install -y python3 python3-venv python3-pip git curl

# 2️⃣ Install Ollama (model runner)
curl -fsSL https://ollama.com/install.sh | sh
ollama serve &

# 3️⃣ Pull models
ollama pull phi3:mini
ollama pull nomic-embed-text

# 4️⃣ Clone and configure
git clone https://github.com/<your-repo>/rag_local.git
cd rag_local
python3 -m venv .venv && source .venv/bin/activate
pip install -U -r requirements.txt

# 5️⃣ Index and run chat
python app/build_index.py
streamlit run app/chat_ui.py
```

---

## Environment Configuration (`app/.env`)

```env
LLM_MODEL=phi3:mini
EMBED_MODEL=BAAI/bge-small-en-v1.5
CHROMA_PATH=./chroma_db
DATA_DIR=./data
PERSIST_DIR=./storage
TOP_K=5
```

---

## Project Structure

```
rag-chat-local/
├── app/
│   ├── build_index.py      # Builds document embeddings
│   ├── chat_ui.py          # Streamlit multi-turn chat UI
│   └── .env                # Model and config
├── data/                   # PDFs, TXT, or MD files
├── chroma_db/              # Persistent vector DB
├── storage/                # LlamaIndex storage context
├── requirements.txt
└── README.md
```

---

## 🔄 Updating Existing Builds

You can easily swap models in your `.env` to upgrade quality or speed.

### Example – Chat model update
```env
# Old
LLM_MODEL=phi3:mini
# New
LLM_MODEL=llama3.1:8b
```

```bash
ollama pull llama3.1:8b
python app/build_index.py
streamlit run app/chat_ui.py
```

### Example – RAG embedding model update
```env
# Old
EMBED_MODEL=BAAI/bge-small-en-v1.5
# New
EMBED_MODEL=BAAI/bge-large-en-v1.5
```

```bash
pip install -U sentence-transformers
python app/build_index.py
```

---

> 💡 Tip: Start with `phi3:mini` for rapid CPU experimentation.  
> Upgrade to `llama3.1:8b` or `qwen2.5:7b` once your base workflow is stable.
