# 📗 02 — RAG with FAISS (PDF Question Answering)

A **Retrieval-Augmented Generation (RAG)** pipeline that loads a PDF document, creates vector embeddings using FAISS, and answers questions based on the document's content using OpenAI's GPT model.

**Use Case:** Question answering on the **Bhagavad Gita** PDF — ask questions about chapters, characters, teachings, and philosophy.

---

## 🏗️ Architecture

```
PDF Document
    │
    ▼
PyPDFLoader (load pages)
    │
    ▼
RecursiveCharacterTextSplitter (chunk: 1000 chars, overlap: 200)
    │
    ▼
OpenAI Embeddings → FAISS Vector Store (saved locally)
    │
    ▼
User Query → Similarity Search → Top-K Chunks Retrieved
    │
    ▼
Prompt + Context + Question → ChatOpenAI (GPT-3.5-Turbo)
    │
    ▼
Answer (grounded in document content)
```

---

## 📓 Notebooks

### 1. `rag_faiss_load_documents.ipynb`
Handles the **ingestion pipeline** — loading, splitting, embedding, and saving the vector store.

**Steps:**
1. Load PDF using `PyPDFLoader`
2. Split into chunks with `RecursiveCharacterTextSplitter` (chunk_size=1000, overlap=200)
3. Generate embeddings using `OpenAIEmbeddings`
4. Store in FAISS vector store and save locally to `vectorstore/db_faiss`

```python
loader = PyPDFLoader("Bhagavad-gita.pdf")
docs = loader.load()
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
splits = text_splitter.split_documents(docs)
vectorstore = FAISS.from_documents(documents=splits, embedding=OpenAIEmbeddings())
vectorstore.save_local('vectorstore/db_faiss')
```

---

### 2. `rag_faiss_query.ipynb`
Handles the **retrieval and generation pipeline** — querying the vector store and generating answers.

**Components:**
- **`load_knowledgeBase()`** — Loads the saved FAISS index from disk
- **`load_llm()`** — Initializes `ChatOpenAI` (GPT-3.5-Turbo, temperature=0)
- **`load_prompt()`** — Creates a prompt template that injects retrieved context
- **RAG Chain** — Built using LCEL: `retriever | prompt | llm | StrOutputParser`

**RAG Chain Construction:**
```python
rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
response = rag_chain.invoke("How many chapters in Bhagavad Gita?")
```

**Example queries tested:**
- "How many chapters in Bhagavad Gita?"
- "Who is Krishna?"
- "What is the moral of Bhagavad Gita?"
- "Explain the concept of Karma Yoga as described in the Gita"
- "What does Krishna say about how to handle failure and success?"
- "How does Krishna describe the three gunas — Sattva, Rajas, and Tamas?"
- "What are the four paths of Yoga described in the Bhagavad Gita?"
- "How does Krishna explain Moksha (liberation) and its attainment?"

---

## 🔑 Key Concepts

| Concept | Description |
|---|---|
| **RAG** | Retrieve relevant context from a knowledge base before generating answers |
| **FAISS** | Facebook AI Similarity Search — fast vector similarity lookup |
| **Chunking** | Breaking documents into smaller overlapping pieces for better retrieval |
| **Embeddings** | Dense vector representations of text for semantic search |
| **LCEL Chain** | Composable chain: `retriever \| prompt \| llm \| parser` |

---

## 📦 Dependencies

```bash
pip install langchain langchain-openai langchain-community
pip install faiss-cpu pypdf tiktoken openai
```

---

## 📂 Output

After running the load notebook, a local FAISS index is saved at:
```
vectorstore/
└── db_faiss/
    ├── index.faiss
    └── index.pkl
```
