# 🧠 Nestlé HR Policy Chatbot
A document-aware conversational assistant that allows users to query Nestlé’s HR policy using natural language. The chatbot uses a **Retrieval-Augmented Generation (RAG)** architecture to ensure answers are accurate, explainable, and grounded strictly in the policy document.

---

## 🚀 Features
- Ask HR policy questions in plain English
- Semantic search across the HR policy PDF
- Answers grounded only in document context (no hallucinations)
- Persistent vector database for fast reuse
- Interactive chatbot UI built with Gradio

---

## 🎯 Problem Statement
HR policy documents are often long, dense, and difficult to navigate. Employees may not know the exact terminology used in policies, making keyword search ineffective.

Using a standalone LLM to answer HR questions can lead to **hallucinated or incorrect responses**, which is unacceptable for policy-related information.

---

## ✅ Solution
This project solves the problem by combining:
- **Semantic search** using embeddings
- **Vector-based retrieval** using ChromaDB
- **LLM-based answer generation** constrained by retrieved context

The result is a chatbot that answers questions **only when the information exists in the document**.

---

## 🧩 How It Works

1. **Document Loading**  
   The Nestlé HR policy PDF is loaded using `PyPDFLoader`.

2. **Text Chunking**  
   The document is split into overlapping chunks to preserve context.

3. **Embedding Generation**  
   Each chunk is converted into a vector using OpenAI’s embedding model.

4. **Vector Storage**  
   Vectors, text, and metadata (page numbers) are stored in **ChromaDB** with persistence enabled.

5. **Retrieval-Augmented Generation (RAG)**  
   - User queries are embedded  
   - Relevant chunks are retrieved from ChromaDB  
   - Retrieved context is injected into the LLM prompt  
   - The model answers **only using the provided context**

6. **Chat Interface**  
   A Gradio-based chatbot UI sits on top of the RAG pipeline for interactive querying.

---

## 🛠️ Tech Stack
- Python  
- LangChain (Runnable-based API)  
- OpenAI Embeddings & Chat Models  
- ChromaDB  
- Gradio  
- SQLite (via `pysqlite3-binary`)  

---

## ⚠️ Challenges & Solutions

### SQLite Version Compatibility
ChromaDB requires SQLite ≥ 3.35, which was not available by default.

**Solution:**  
Used `pysqlite3-binary` and patched Python’s `sqlite3` module at runtime.

---

### LangChain API Changes
Older tutorials relied on deprecated imports.

**Solution:**  
Migrated to LangChain’s modular packages:
- `langchain-core`
- `langchain-community`
- `langchain-openai`
- Runnable-based APIs (`invoke`, `ChatPromptTemplate.from_messages`)

---

### UI Integration Without Refactoring
The requirement was to add a chatbot UI without changing core RAG logic.

**Solution:**  
A lightweight Gradio layer was added that simply calls `rag_chain.invoke(user_query)`.

---

## ▶️ Running the App

1. Install dependencies:
```bash
pip install -r requirements.txt
```
2. Open and run the jupyter notebook-
app.ipynb


