# Agentic Assistant

**Grounded Multi-Agent Workflow with Citations + Verifier**

An end-to-end agentic AI system that performs structured planning, retrieval-grounded research, drafting, and automated verification — using only documented facts from a user-provided knowledge base.

Built with **LangGraph**, **LangChain**, **Chroma**, **OpenAI models**, and **Streamlit**.

---

## 🚀 Overview

Agentic Assistant is a multi-agent Retrieval-Augmented Generation (RAG) system designed to:

- Plan task execution  
- Retrieve relevant documents  
- Extract only grounded, cited facts  
- Draft a client-ready deliverable  
- Verify that all claims are supported  
- Retry if unsupported claims are detected  

It enforces **strict grounding rules** and prevents hallucinated content.

---

## 🧠 Architecture

The system follows a structured agent workflow:

```
Planner → Researcher → Writer → Verifier
                         ↑         ↓
                         ←--- Retry Loop ---
```

### Agents

#### 1️⃣ Planner Agent
- Creates a structured execution plan (3–6 steps)
- Does **NOT** perform research or drafting
- Outputs structured JSON

#### 2️⃣ Research Agent
- Retrieves documents from Chroma vector store
- Extracts only facts supported by retrieved sources
- Every fact must include citations
- If no evidence exists → returns `"Not found in sources"`

#### 3️⃣ Writer Agent
- Produces a client-ready Markdown deliverable
- Uses only extracted research notes
- Never introduces outside knowledge
- If research insufficient → clearly states so

#### 4️⃣ Verifier Agent (Final Authority)
- Ensures every claim is supported by research notes
- Fails if unsupported claims are found
- Retries research if needed
- Stops after configurable max retries

---

## 📂 Project Structure

```
.
├── agents/
│   ├── planner.py
│   ├── researcher.py
│   ├── writer.py
│   ├── verifier.py
│   └── graph.py
│
├── schemas/
│   └── state.py
│
├── tools/
│   └── retriever.py
│
├── streamlit_app.py
├── test_cases.json
└── README.md
```

---

## 🔍 Knowledge Base & Retrieval

- Uses **Chroma** as a persistent vector store
- Supports:
  - `.txt`
  - `.pdf` (if `PyPDFLoader` available)
- Documents are chunked with overlap
- Each chunk includes:
  - `doc_id`
  - `location`
  - snippet preview

Index rebuild is triggered only when the document fingerprint changes.

---

## 🖥️ User Interface (Streamlit)

Features:

- Chat-style interface  
- File upload (`.txt`, `.pdf`)  
- Auto-indexing  
- Clean citation display  
- Execution plan tab  
- Full agent trace log  
- Verifier retry counter  

---

## 🛠️ Installation

### 1️⃣ Clone the repository

```bash
git clone <your-repo-url>
cd agentic-assistant
```

### 2️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

### 3️⃣ Configure environment variables

Create a `.env` file:

```env
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-4o-mini
```

---

## ▶️ Running the Application

```bash
streamlit run streamlit_app.py
```

Then:

1. Upload documents  
2. Click **Save**  
3. Enter a task  
4. Review grounded output + citations  

---

## 🧪 Example Tasks

Included in `test_cases.json`:

- Summarize top 5 risks across docs  
- Compare two approaches and recommend one  
- Extract deadlines + owners  
- Draft a client update email  
- Create a Confluence-style internal page  

---

## 🔐 Grounding & Safety Design

This system enforces:

- No outside knowledge usage  
- Citation-required facts  
- Prompt injection resistance  
- Verifier-controlled output  
- Retry loop with max cap  
- Safe failure mode if unsupported  

If claims cannot be supported, the system explicitly states:

> "Not found in sources."

---

## ⚙️ Configuration

Configurable via `AppState.meta`:

- Model name  
- Chroma persist directory  
- Verifier max retries  

---

## 🧩 Key Design Decisions

- Structured outputs via **Pydantic models**
- **LangGraph** for explicit agent control flow
- Persistent vector store for efficiency
- Fingerprint-based index refresh
- Clear separation of planning, research, writing, and verification
- LLM-as-judge verification stage

---

## 📊 Strengths

- Clear agent separation  
- Deterministic control flow  
- Strong grounding enforcement  
- Transparent traceability  
- Clean UI for evaluation and demonstration  

---

## ⚠️ Limitations

- Verifier is LLM-based (non-deterministic)  
- Retrieval is single-query (no multi-hop reasoning yet)  
- No automatic reranking  
- Citation granularity at chunk level  

---

## 🔮 Future Improvements

- Deterministic grounding checker  
- Multi-query retrieval  
- Reranking layer  
- Stable document hashing IDs  
- Automated test harness for evaluation  
- Sentence-level citation enforcement  
- Cost tracking and token budgeting  

---

## 📜 License

MIT License (or specify your own)

---

## 👤 Author

Merjeme
Multi-Agent Systems Project  
