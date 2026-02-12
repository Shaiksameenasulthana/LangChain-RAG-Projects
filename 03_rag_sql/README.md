# 📕 03 — RAG with SQL (Natural Language to SQL)

A **Text-to-SQL** pipeline using LangChain's `SQLDatabaseChain` that connects to a **MySQL database** and lets you ask analytical questions in plain English. The LLM automatically generates SQL queries, executes them, and returns human-readable answers.

---

## 🏗️ Architecture

```
User Question (Natural Language)
    │
    ▼
SQLDatabaseChain (LangChain)
    │
    ├── Inspects table schema & sample rows
    ├── Generates SQL query via LLM
    ├── Executes SQL on MySQL database
    └── Returns raw result
    │
    ▼
System Prompt + Context + Question → ChatOpenAI
    │
    ▼
Human-readable Answer
```

---

## 📓 Notebooks

### 1. `RagSql27.ipynb` — UK Bank Customers Dataset

**Database:** MySQL → `p6_uk_bank_customers` table

**Dataset Columns:**
| Column | Description |
|---|---|
| Customer ID | Unique identifier |
| Name / Surname | Customer name |
| Gender | Male / Female |
| Age | Customer age |
| Region | England, Wales, Scotland, Northern Ireland |
| Job Classification | White Collar / Blue Collar |
| Date Joined | Account opening date |
| Balance | Account balance |

**Example queries tested:**
- "Provide names of all people from England"
- "Provide count of all Male people"
- "Provide count of all female who are from Wales"
- "Which region has the highest average balance?"
- "List the top 5 customers with the highest balances in England"
- "How has the average balance changed over time for customers who joined before and after 2018?"
- "What percentage of customers have a balance above the median balance?"
- "How would you identify outliers in the balance column?"

---

### 2. `rag_sql.ipynb` — Person Data / Risk Analysis Dataset

**Database:** MySQL → `person_data` table

**Dataset Columns:**
| Column | Description |
|---|---|
| ID | Unique identifier |
| Income | Annual income |
| Age | Age of individual |
| Experience | Professional experience (years) |
| Married | Marital status (single/married) |
| House_Ownership | own / rented / norent_noown |
| Car_Ownership | yes / no |
| Profession | Individual's profession |
| CITY / STATE | Location |
| CURRENT_JOB_YRS | Years in current job |
| CURRENT_HOUSE_YRS | Years in current house |
| Risk_Flag | 0 = low risk, 1 = high risk |

**Example queries tested:**
- "How many rows and columns present in this training data?"
- "List the top 5 professions with the highest incomes"
- "Compare the average income of car owners vs. non-car owners"
- "Are individuals with higher incomes less likely to be classified as high-risk?"
- "Which cities or states have the highest proportion of high-risk individuals?"
- "Person whose is architect & salary above 50,000 in tabular format"
- "How many people are from Kerala who are married and own house and car?"

---

## 💡 How It Works

The pipeline uses two functions:

1. **`retrieve_from_db(query)`** — Sends the natural language query through `SQLDatabaseChain`, which auto-generates and executes a SQL query, returning the raw result.

2. **`generate(query)`** — Takes the raw DB result and passes it through a system prompt with dataset context to produce a polished, human-readable answer via `ChatOpenAI`.

```python
def generate(query: str) -> str:
    db_context = retrieve_from_db(query)
    messages = [
        SystemMessage(content=system_message),
        human_qry_template.format(human_input=query, db_context=db_context)
    ]
    response = llm(messages).content
    return response
```

---

## ⚙️ MySQL Setup

```python
host = 'localhost'
port = '3306'
username = 'root'
database_schema = 'database'
mysql_uri = f"mysql+pymysql://{username}@{host}:{port}/{database_schema}"
db = SQLDatabase.from_uri(mysql_uri, include_tables=['table_name'], sample_rows_in_table_info=2)
```

---

## 🔑 Key Concepts

| Concept | Description |
|---|---|
| **Text-to-SQL** | LLM converts natural language to SQL queries |
| **SQLDatabaseChain** | LangChain chain that auto-inspects schema and generates SQL |
| **System Prompt Engineering** | Provides dataset column descriptions for better answers |
| **Two-step Pipeline** | Retrieve raw data → Generate polished response |

---

## 📦 Dependencies

```bash
pip install langchain langchain-openai langchain-experimental langchain-community
pip install pymysql sqlalchemy openai
```

> **Note:** Requires a running MySQL server with the datasets imported.
