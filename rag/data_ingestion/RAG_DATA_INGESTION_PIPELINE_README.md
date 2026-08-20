# RAG Data Ingestion Pipeline

A hands-on implementation of the **Retrieval-Augmented Generation (RAG) data ingestion pipeline** using Python, LangChain, Sentence Transformers, and ChromaDB.

The notebook demonstrates how raw PDF documents are transformed into searchable vector representations and how relevant chunks can later be retrieved for a RAG application.

## Pipeline

```text
                  DATA INGESTION

PDF Documents
      │
      ▼
┌─────────────────┐
│  PDF Loader     │
│  PyPDFLoader    │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────┐
│     Text Splitting          │
│ RecursiveCharacterSplitter  │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│      Embedding Model        │
│    all-MiniLM-L6-v2         │
│        384 dimensions       │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│         ChromaDB            │
│      Vector Store           │
└────────────┬────────────────┘
             │
             ▼
       Persistent Storage


                  RETRIEVAL

User Query
     │
     ▼
Query Embedding
     │
     ▼
ChromaDB Similarity Search
     │
     ▼
Top-K Relevant Chunks
     │
     ▼
Context for LLM
```

## What This Project Covers

This notebook walks through the core RAG data pipeline:

1. Loading PDF documents
2. Extracting document content and metadata
3. Splitting documents into smaller chunks
4. Generating vector embeddings
5. Persisting embeddings in ChromaDB
6. Converting user queries into embeddings
7. Performing vector similarity search
8. Filtering and ranking retrieved chunks

## Tech Stack

* **Python**
* **LangChain**
* **PyPDFLoader**
* **RecursiveCharacterTextSplitter**
* **Sentence Transformers**
* **all-MiniLM-L6-v2**
* **ChromaDB**
* **NumPy**
* **scikit-learn**

## Project Structure

```text
rag/
└── data_ingestion/
    ├── notebook/
    │   └── pdf_loader.ipynb
    │
    └── data/
        ├── pdf_files/
        │   ├── redis.pdf
        │   ├── maven.pdf
        │   └── ...
        │
        └── vector_store/
            └── ...
```

## 1. PDF Loading

The pipeline recursively scans the data directory for PDF files.

```python
pdf_files = list(pdf_dir.glob("**/*.pdf"))
```

Each PDF is loaded using `PyPDFLoader`.

The loader produces LangChain `Document` objects containing:

* page content
* source path
* page number
* total pages
* PDF metadata

Additional application-specific metadata is added:

```python
doc.metadata["source_file"] = pdf_file.name
doc.metadata["file_type"] = "pdf"
```

This metadata is useful later for identifying where retrieved information came from.

## 2. Text Chunking

Large documents are split into smaller chunks before generating embeddings.

```python
RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    length_function=len,
    separators=["\n\n", "\n", " ", ""]
)
```

### Chunk Size

`chunk_size=1000` defines the target maximum size of each chunk.

### Chunk Overlap

`chunk_overlap=200` allows neighboring chunks to share content.

For example:

```text
Chunk 1
┌────────────────────────────────────────────┐
│ A B C D E F G H I J K L M N O              │
└────────────────────────────────────────────┘
                            │
                            │ overlap
                            ▼
Chunk 2
                    ┌────────────────────────────────────────────┐
                    │ K L M N O P Q R S T U V W                  │
                    └────────────────────────────────────────────┘
```

The overlap helps preserve context when an important sentence or concept falls near a chunk boundary.

## 3. Generate Embeddings

Each chunk is converted into a numerical vector using the Sentence Transformers model:

```text
all-MiniLM-L6-v2
```

The current implementation produces **384-dimensional embeddings**.

```python
embedding_manager = EmbeddingManager()

embeddings = embedding_manager.generate_embeddings(texts)
```

For example:

```text
13 chunks
    ↓
13 embedding vectors
    ↓
(13, 384)
```

The important idea is:

```text
Text
 ↓
Embedding Model
 ↓
Vector
```

Semantically similar text should produce vectors that are close together in the embedding space.

## 4. Store Embeddings in ChromaDB

ChromaDB is used as the vector store.

```python
chromadb.PersistentClient(
    path="../data/vector_store"
)
```

A collection named:

```text
pdf_documents
```

is created or reused.

Each record contains:

```text
ID
Document
Embedding
Metadata
```

The vector store is persistent, meaning the embeddings are stored on disk and can be reused across notebook sessions.

## 5. Retrieval

The retrieval pipeline converts the user's query into an embedding and searches ChromaDB for similar vectors.

```python
rag_retriever.retrieve(
    "Base Model vs. AI Assistant"
)
```

The retriever supports:

```python
top_k=5
score_threshold=0.0
```

### Retrieval Flow

```text
User Query
    │
    ▼
Generate Query Embedding
    │
    ▼
ChromaDB Vector Search
    │
    ▼
Top-K Results
    │
    ▼
Similarity Score
    │
    ▼
Score Filtering
    │
    ▼
Ranked Documents
```

The retrieved result contains:

```python
{
    "id": "...",
    "content": "...",
    "metadata": {...},
    "similarity_score": 0.91,
    "distance": 0.09,
    "rank": 1
}
```

## Current Results

The notebook currently demonstrates:

```text
PDF files       → 3
Loaded pages    → 11
Generated chunks → 13
Embedding size  → 384
Vector store    → ChromaDB
Collection      → pdf_documents
Top-K retrieval → 5
```

The notebook successfully loads the PDFs, chunks their content, generates embeddings, stores them in ChromaDB, and performs semantic retrieval.

## Why This Is Useful for RAG

Traditional keyword search looks for matching words.

Vector search instead represents text as embeddings and searches based on **semantic similarity**.

For example:

```text
Query:
"How does Redis implement distributed locking?"

        ↓

Embedding

        ↓

Vector similarity search

        ↓

Relevant Redis chunks
```

Even when the exact words in the query don't match the document, semantically related content can still be retrieved.

This retrieved content can then be supplied to an LLM as context:

```text
User Question
      +
Retrieved Context
      ↓
     LLM
      ↓
Grounded Answer
```

> Note: The current notebook focuses on the ingestion and retrieval stages. The final LLM generation stage is not implemented in this notebook.

## Key Concepts Learned

### Document Loading

Converting raw files into structured documents that can be processed by the RAG pipeline.

### Chunking

Breaking large documents into smaller units that are more suitable for embedding and retrieval.

### Chunk Overlap

Maintaining context between neighboring chunks.

### Embeddings

Representing text as numerical vectors that capture semantic meaning.

### Vector Database

Persisting embeddings and supporting similarity-based retrieval.

### Semantic Search

Finding documents based on meaning rather than exact keyword matching.

### Top-K Retrieval

Returning the K most relevant chunks for a query.

### Similarity Threshold

Filtering retrieved results based on their similarity score.

## Next Steps

The natural next stage after this notebook is to build the complete RAG pipeline:

```text
Documents
   ↓
Chunking
   ↓
Embeddings
   ↓
ChromaDB
   ↓
Retriever
   ↓
Prompt Construction
   ↓
LLM
   ↓
Generated Answer
```

Future improvements can include:

* LLM integration
* RAG prompt templates
* Source citations
* Metadata filtering
* Hybrid search
* Reranking
* Retrieval evaluation
* Query rewriting
* Conversational memory
* Streaming responses
