# Advanced Q&A Bot with LangChain Agents and Multi-Source Retrieval

This project showcases how to build an **intelligent agent-powered Q&A bot** that can answer questions using data from multiple sources such as:

- 🌐 Wikipedia
- 📚 ArXiv research papers
- 📄 LangSmith documentation via web scraping + vector search (RAG)

LangChain's **agent** system, paired with tool integration (`QueryRun`, `RetrieverTool`, etc.), allows dynamic tool selection and natural-language-driven orchestration.

---

## 🧠 Core Concepts Explored

| Concept | Description |
|--------|-------------|
| 🧰 `QueryRun` | Tool to fetch answers using APIs like Wikipedia or ArXiv |
| 🧩 `APIWrapper` | Wraps external APIs for integration with LangChain tools |
| 📚 `RetrieverTool` | Wraps a vector retriever (RAG) into a tool usable by agents |
| 🧠 `AgentExecutor` | Coordinates between tools and the LLM to generate answers |
| ⚙️ `create_openai_tools_agent` | Combines LLM, tools, and prompt into an autonomous agent |
| 🔍 `FAISS` + `HuggingFaceEmbeddings` | Used for RAG over LangSmith docs |

---

## 📁 Project Structure

```

.
├── .env                             # API Keys
├── agents.ipynb        # Full notebook implementation
├── Colon.pdf, other docs           # (Optional) supporting PDFs
└── README.md

````

---

## 🔧 Setup Instructions

### 1. Install Dependencies

```bash
pip install langchain langchain-community langchainhub faiss-cpu sentence-transformers python-dotenv
````

### 2. Add `.env` File

```env
GROQ_API_KEY=your_groq_api_key
```

---

## 🔌 Data Sources & Tool Setup

### 📚 LangSmith Docs (RAG)

* Loaded with `WebBaseLoader`
* Chunked using `RecursiveCharacterTextSplitter`
* Embedded using `HuggingFaceEmbeddings`
* Stored in `FAISS` and exposed via `RetrieverTool`

### 🌐 Wikipedia & ArXiv Integration

```python
from langchain_community.utilities import WikipediaAPIWrapper, ArxivAPIWrapper
from langchain_community.tools import WikipediaQueryRun, ArxivQueryRun
```

Both are converted to tools for the agent to dynamically call.

---

## 🤖 LLM & Agent Setup

### LLM Configuration (Groq - LLaMA3)

```python
from langchain_openai import ChatOpenAI
llm = ChatOpenAI()
```

### Prompt & Agent Creation

```python
from langchain import hub
prompt = hub.pull("hwchase17/openai-functions-agent")

from langchain.agents import create_openai_tools_agent
agent = create_openai_tools_agent(llm, tools, prompt)
```

### Final Agent Executor

```python
from langchain.agents import AgentExecutor
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
```

---

## 🧪 Example Queries

```python
agent_executor.invoke({"input": "Tell me about Langsmith"})
agent_executor.invoke({"input": "What's the paper 1605.08386 about?"})
agent_executor.invoke({"input": "What is React?"})
```

Agent will:

* Use the correct tool (Wikipedia, ArXiv, LangSmith retriever) based on input
* Retrieve relevant data
* Generate coherent answers

---

## ✅ What You Learned

* ✅ How to build a **multi-source retrieval system** for Q\&A
* ✅ Use of `APIWrapper` and `QueryRun` for external data integration
* ✅ Vectorization + FAISS for RAG-based document QA
* ✅ Creating LangChain agents with dynamic tool execution
* ✅ Prompt-driven tool selection and multi-hop reasoning

---

## 🔮 Next Steps

* Add caching and persistent vector DBs (e.g., Chroma, Weaviate)
* Integrate memory for multi-turn conversations
* Deploy via `FastAPI` or `LangServe`
* Connect to frontend with Streamlit or React

---

## 🙌 Credits

* [LangChain Docs](https://docs.langchain.com/)
* [Groq API](https://console.groq.com/)
* [ArXiv API](https://arxiv.org/help/api/)
* [Wikipedia API](https://www.mediawiki.org/wiki/API:Main_page)

---

## 🚀 Build Your Own Knowledge-Integrated Agents with LangChain Today!
