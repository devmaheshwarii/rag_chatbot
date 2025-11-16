---

# Retrieval-Augmented Generation (RAG) Chatbot

### Python | LangChain | HuggingFace | FAISS

This repository contains a fully functional Retrieval-Augmented Generation (RAG) chatbot built using Python, LangChain, HuggingFace Transformers, and FAISS. The system ingests unstructured documents, converts them into vector embeddings, stores them in a FAISS index, and performs real-time semantic retrieval to generate accurate, context-aware responses.

---

## Features

* RAG pipeline for context-grounded answers
* Document parsing and preprocessing (PDF, text files, etc.)
* Recursive chunking with optimized size and overlap
* Embeddings generated using Sentence Transformers or BGE models
* FAISS vector store for fast semantic search
* Retrieval + LLM generation based on top-k context
* Source-aware responses (shows which document and chunk were used)
* FastAPI backend for serving queries
* Scalable up to 100K+ documents
* Modular code for easy maintenance and extension

---

## Project Structure

```
rag-chatbot/
│
├── data/                     # Input documents
├── embeddings/               # Stored FAISS index and embeddings
├── app/
│   ├── ingestion.py          # Document loading and cleaning
│   ├── chunking.py           # Text splitter configuration
│   ├── embeddings.py         # Embedding model setup
│   ├── vectorstore.py        # FAISS index builder and saver
│   ├── retriever.py          # Semantic search logic
│   ├── rag_chain.py          # LangChain RAG pipeline
│   └── api.py                # FastAPI server
│
├── main.py                   # End-to-end execution script
└── requirements.txt
```

---

## Installation

```
git clone https://github.com/your-username/rag-chatbot.git
cd rag-chatbot
pip install -r requirements.txt
```

---

## Step-by-Step Pipeline

### 1. Document Ingestion

Documents are loaded from the `data/` directory.

Supports:

* PDF
* Text files
* Word documents (optional with loaders)

### 2. Text Cleaning

Basic preprocessing includes:

* Removing extra whitespace
* Normalizing text
* Ensuring consistent formatting

### 3. Chunking

Used: `RecursiveCharacterTextSplitter`

Configuration:

* Chunk size: 700–1000 tokens
* Overlap: 100–150 tokens
* Breaks at sentence boundaries when possible

This improves retrieval precision for long enterprise documents.

### 4. Embeddings

Default models:

* `sentence-transformers/all-mpnet-base-v2`, or
* `BAAI/bge-base-en`

Embeddings are generated for each chunk and stored for reuse.

### 5. Vector Database (FAISS)

A FAISS index is built and persisted under `embeddings/`.

Benefits:

* High-performance cosine similarity search
* Suitable for datasets with 100K+ chunks

### 6. Retrieval Pipeline

Given a user query:

1. Convert the query into embeddings
2. Perform a top-k vector search
3. Return the highest-scoring document chunks

### 7. Generation Pipeline (RAG)

The LLM is given:

* User query
* Retrieved context
* RAG-specific prompt template

The final answer is grounded strictly in document content.

---

## Running the Chatbot

```
python main.py
```


```
POST /ask
{
    "query": "your question"
}
```

---

## Prompt Template (Used for RAG)

```
You are a retrieval-augmented assistant. Use only the provided context to answer the question. 
Do not add information that is not present in the context. 
If the answer cannot be determined from the context, say so clearly.

Context:
{context}

Question:
{query}

Answer:
```

---

## Best Practices and Rules Followed

* Never generate answers without retrieved context
* Always return document sources with the answer
* Avoid hallucinations by strict prompting
* Normalize documents before chunking
* Cache embeddings to avoid duplicate computation
* Validate FAISS index dimensions for reliability
* Use batch inference for large document collections

---

## Scaling to Large Document Sets

To scale beyond 100K documents:

* Switch to HNSW FAISS index
* Use GPU-accelerated FAISS
* Apply hybrid retrieval (keyword + vector)
* Compress embeddings using Product Quantization (PQ)
* Stream document ingestion in batches

---

## Future Enhancements

* Web UI using Next.js or React
* Interactive chat history
* Multi-document file upload on the fly
* Reranking using cross-encoders
* Role-based access with authentication

---

## License

This project is distributed under the MIT License.

---

