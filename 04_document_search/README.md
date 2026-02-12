# 📙 04 — Document Search (Resume Analysis with LangChain)

A **document search and analysis pipeline** that loads a directory of PDF resumes, creates vector embeddings using `InMemoryVectorStore`, and performs semantic search to extract information like skills, names, and generate interview questions.

---

## 🏗️ Architecture

```
PDF Resume Directory
    │
    ▼
PyPDFDirectoryLoader (load all PDFs from folder)
    │
    ▼
RecursiveCharacterTextSplitter (chunk: 1000 chars, overlap: 200)
    │
    ▼
OpenAI Embeddings → InMemoryVectorStore
    │
    ▼
Retriever (semantic search)
    │
    ▼
User Query → Retrieved Chunks → LLM (OpenAI) → Structured Answer
```

---

## 📓 Notebook

### `document_search_Langchain.ipynb`

**Step-by-step workflow:**

1. **Load Documents** — Uses `PyPDFDirectoryLoader` to load all PDF resumes from a directory
2. **Split & Embed** — Chunks documents and creates embeddings using `OpenAIEmbeddings`
3. **Create Retriever** — Stores embeddings in `InMemoryVectorStore` and creates a retriever
4. **Semantic Search** — Queries the retriever for relevant resume chunks
5. **LLM Analysis** — Passes retrieved content to OpenAI for structured analysis

**Key code:**
```python
from langchain.document_loaders import PyPDFDirectoryLoader
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

documents = PyPDFDirectoryLoader("path/to/resumes/").load()
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
splits = text_splitter.split_documents(documents)
vectorstore = InMemoryVectorStore.from_documents(documents=splits, embedding=OpenAIEmbeddings())
retriever = vectorstore.as_retriever()
```

---

## 🔍 Example Queries

**Information Extraction:**
- "Provide names of all people"
- "Provide me data science skills of all people"
- "Provide me college names based on resumes"
- "Provide me experience persons name for Data Analyst based on resumes"

**Interview Question Generation:**
- "Provide me interview questions based on resumes"
- "Provide me interview questions for Data Science based on resumes"
- "Provide me interview questions for Data Analyst based on resumes"

**Structured Output:**
- "Provide me developer names from this text in CSV format"
- "Provide me skills based on these resumes in JSON format"
- "Provide me machine learning names from this text in CSV format"

**Analytical Queries:**
- "How can vector embeddings help in searching and matching resumes with job descriptions?"
- "What key features in a resume should be prioritized for embedding?"
- "Provide me not career gap people names from this text in CSV format"

---

## 🔑 Key Concepts

| Concept | Description |
|---|---|
| **PyPDFDirectoryLoader** | Loads all PDFs from a directory at once |
| **InMemoryVectorStore** | Lightweight in-memory vector store (no persistence needed) |
| **Semantic Search** | Find resumes by meaning, not just keywords |
| **Retriever** | LangChain abstraction for fetching relevant documents |
| **LLM Post-processing** | Use OpenAI to extract structured info from retrieved chunks |

---

## 📦 Dependencies

```bash
pip install langchain langchain-openai langchain-community
pip install openai pypdf tiktoken
```
