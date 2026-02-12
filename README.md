# 🔗 LangChain-RAG-Projects

A collection of hands-on projects demonstrating **LangChain** fundamentals, **Retrieval-Augmented Generation (RAG)** patterns, and **LLM-powered** data workflows using OpenAI models.

---

## 📁 Repository Structure

```
LangChain-RAG-Projects/
│
├── 01_langchain_basics/        # LangChain fundamentals & output parsers
│   ├── Langchain_API_usage.ipynb
│   └── langchain_output_parsers.ipynb
│
├── 02_rag_faiss/                # RAG with FAISS vector store (PDF Q&A)
│   ├── rag_faiss_load_documents.ipynb
│   └── rag_faiss_query.ipynb
│
├── 03_rag_sql/                  # RAG with SQL databases (Text-to-SQL)
│   ├── RagSql27.ipynb
│   └── rag_sql.ipynb
│
├── 04_document_search/          # Document search & resume analysis
│   └── document_search_Langchain.ipynb
│
└── README.md
```

---

## 🚀 Projects Overview

### 1. LangChain Basics (`01_langchain_basics/`)
Introduction to LangChain's core components — LLMs, Chat Models, Prompt Templates, chains (LCEL), and Output Parsers (JSON & CSV).

### 2. RAG with FAISS (`02_rag_faiss/`)
Build a PDF-based question-answering system using FAISS vector store, OpenAI embeddings, and a retrieval chain. Demonstrates document loading, chunking, embedding, and querying with the Bhagavad Gita PDF.

### 3. RAG with SQL (`03_rag_sql/`)
Natural language to SQL querying using LangChain's `SQLDatabaseChain`. Connect to a MySQL database and ask analytical questions in plain English — the LLM generates and executes SQL queries automatically.

### 4. Document Search (`04_document_search/`)
Resume search and analysis pipeline using LangChain's `InMemoryVectorStore`. Load a directory of PDF resumes, create embeddings, and perform semantic search to extract skills, names, and generate interview questions.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **LangChain** | LLM orchestration framework |
| **OpenAI GPT-3.5 / GPT-4** | Language model for generation |
| **FAISS** | Vector similarity search |
| **MySQL** | SQL database for Text-to-SQL |
| **PyPDF** | PDF document loading |
| **Python** | Programming language |

---

## ⚙️ Prerequisites

- Python 3.10+
- OpenAI API Key
- MySQL Server (for `03_rag_sql` projects)

### Installation

```bash
pip install langchain langchain-openai langchain-community langchain-experimental
pip install openai faiss-cpu pypdf tiktoken
pip install pymysql sqlalchemy
```

### Environment Setup

```python
import os
os.environ["OPENAI_API_KEY"] = "your-api-key-here"
```

---

## 📖 How to Use

1. Clone the repository
2. Install the required dependencies
3. Set your OpenAI API key as an environment variable
4. Navigate to any project folder and open the Jupyter notebooks
5. Run the cells sequentially

---

## 👤 Author

**Shaiksameenasulthana**

---

## 📄 License

This project is for educational purposes.
