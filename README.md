# Agentic AI using Python 

Hands-on implementations of **Agentic AI applications using Python, LangChain, and modern LLM tooling**.

This repository is focused on learning by building practical AI systems — starting from core LLM concepts and gradually moving toward **RAG, AI agents, tool calling, memory, orchestration, and production-oriented Agentic AI workflows**.

## 📂 Project Structure

```text
agentic_ai_hands_on/
│
├── rag/
│   └── data_ingestion/
│       ├── data/
│       │   └── pdf_files/
│       │
│       ├── notebook/
│       │   └── pdf_loader.ipynb
│       │
│       ├── src/
│       │
│       └── RAG_DATA_INGESTION_PIPELINE_README.md
│
├── .idea/
├── .gitignore
├── .python-version
├── main.py
├── pyproject.toml
├── requirements.txt
└── uv.lock
```

## 🚀 Topics Covered

The repository will progressively cover:

* **LLMs & Prompt Engineering**
* **LangChain**
* **Retrieval-Augmented Generation (RAG)**
* **Document ingestion and chunking**
* **Embeddings & Vector Databases**
* **Semantic Search & Retrieval**
* **Tool Calling**
* **AI Agents**
* **Agentic AI workflows**
* **Memory**
* **Multi-agent systems**
* **LLM evaluation**
* **Production-oriented AI architectures**

## 📚 Implementations

### RAG — Data Ingestion Pipeline [`rag/data_ingestion/RAG_DATA_INGESTION_PIPELINE_README.md`](rag/data_ingestion/RAG_DATA_INGESTION_PIPELINE_README.md)

The first implementation focuses on building the **data ingestion layer of a RAG pipeline** using Python, LangChain, embeddings, and ChromaDB.

It covers:

```text
PDF Documents
      ↓
Document Loading
      ↓
Text Splitting
      ↓
Chunking
      ↓
Embedding Generation
      ↓
Vector Store
      ↓
ChromaDB
```

Detailed documentation:

**[`rag/data_ingestion/RAG_DATA_INGESTION_PIPELINE_README.md`](rag/data_ingestion/RAG_DATA_INGESTION_PIPELINE_README.md)**

Notebook implementation:

**[`rag/data_ingestion/notebook/pdf_loader.ipynb`](rag/data_ingestion/notebook/pdf_loader.ipynb)**

## 🐍 Python Environment & Dependency Management

This project uses **uv** for Python version management, dependency management, virtual environments, and reproducible builds.

### Install / Initialize

```bash
uv init
uv sync
```

### Manage Dependencies

```bash
uv add <package>
uv add --dev <package>
uv remove <package>
uv lock
```

### Run the Application

```bash
uv run python main.py
uv run <command>
```

### Python Versions

```bash
uv python install 3.12
uv python list
```

### Development

```bash
uv run pytest
uv run ruff check .
uv run ruff format .
```

### Build & Publish

```bash
uv build
uv publish
```

## 🛠️ Technology Stack

* **Python**
* **LangChain**
* **LangChain integrations**
* **LLMs**
* **Sentence Transformers**
* **ChromaDB**
* **Vector Embeddings**
* **uv**
* **Jupyter Notebook**

## 🎯 Goal

The goal of this repository is to build a strong practical understanding of **Agentic AI from the ground up**, rather than relying only on theoretical concepts.

Each module is intended to be a small, working implementation that can later be combined into larger **production-oriented AI systems**.
