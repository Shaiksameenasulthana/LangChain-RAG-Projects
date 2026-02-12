
# 📘 01 — LangChain Basics

This module covers the foundational building blocks of **LangChain** — from making simple LLM calls to building chains with prompt templates and structured output parsers.

---

## 📓 Notebooks

### 1. `Langchain_API_usage.ipynb`
Covers the basics of interacting with OpenAI models through LangChain.

**Topics covered:**
- Setting up the OpenAI API key
- Using `OpenAI` LLM for text completion
- Using `ChatOpenAI` for chat-based interactions
- Working with `SystemMessage` and `HumanMessage`
- Streaming responses with `chat.stream()`
- Creating reusable `PromptTemplate` with variables (`{role}`, `{content}`)
- Building chains using **LangChain Expression Language (LCEL)** — `prompt | model`
- Invoking chains with dynamic inputs

**Example chain:**
```python
from langchain.prompts import PromptTemplate
from langchain.llms import OpenAI

prompt_template = PromptTemplate.from_template("as a {role} tell me about {content}.")
chain = prompt_template | OpenAI()
output = chain.invoke({"role": "Astrologer", "content": "galaxy"})
```

---

### 2. `langchain_output_parsers.ipynb`
Demonstrates how to parse LLM output into structured formats using LangChain's output parsers.

**Topics covered:**

- **JSON Output Parsing** — Using `SimpleJsonOutputParser` to extract structured JSON from LLM responses
  - Birth date/place lookups (e.g., M.K. Gandhi, Narendra Modi, A.P.J. Abdul Kalam)
  - Sentiment analysis with reason extraction
  - Place and speciality lookups (e.g., best hiking spots, yoga retreats)
  - Role-based skills extraction (e.g., Cloud Architect, DevOps Engineer, UX Designer)
  - Data Science skills extraction

- **CSV Output Parsing** — Using `CommaSeparatedListOutputParser` to get comma-separated lists
  - IPL teams listing
  - Top populated cities in India and the world
  - Most visited tourist places
  - Top snowfall places, entrance exams, educated cities
  - Power BI details

**Example — JSON parser chain:**
```python
from langchain.output_parsers.json import SimpleJsonOutputParser

json_prompt = PromptTemplate.from_template(
    "Return a JSON object with birthdate and birthplace key that answers: {question}"
)
chain = json_prompt | model | SimpleJsonOutputParser()
result = chain.invoke("where did M.K. Gandhi born")
# Output: {'birthdate': 'October 2, 1869', 'birthplace': 'Porbandar, Gujarat'}
```

**Example — CSV parser chain:**
```python
from langchain.output_parsers import CommaSeparatedListOutputParser

output_parser = CommaSeparatedListOutputParser()
prompt = PromptTemplate(
    template="List five {subject}.\n{format_instructions}",
    input_variables=["subject"],
    partial_variables={"format_instructions": output_parser.get_format_instructions()}
)
output = model(prompt.format(subject="Indian Premier League Teams"))
parsed = output_parser.parse(output)
# Output: ['Chennai Super Kings', 'Mumbai Indians', ...]
```

---

## 🔑 Key Concepts Learned

| Concept | Description |
|---|---|
| **LLM vs ChatModel** | `OpenAI()` for completions, `ChatOpenAI()` for chat |
| **PromptTemplate** | Reusable prompts with dynamic variables |
| **LCEL Chains** | `prompt \| model \| parser` pipeline syntax |
| **SimpleJsonOutputParser** | Parses LLM text output into Python dict |
| **CommaSeparatedListOutputParser** | Parses LLM text output into Python list |
| **Streaming** | Real-time token-by-token response via `chat.stream()` |

---

## 📦 Dependencies

```bash
pip install langchain langchain-openai openai
```
