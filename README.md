# 🤖 DocAI-RAG

**DocAI-RAG** is a Retrieval-Augmented Generation (RAG) application that allows users to interact with documents and source-code files using natural language.

The application extracts text from multiple file formats, splits the content into smaller chunks, generates vector embeddings, retrieves relevant information using FAISS, and uses a locally hosted Large Language Model (LLM) through Ollama to generate responses.

The project is designed as a foundation for building more advanced document intelligence and AI-powered knowledge systems.

---

## ✨ Features

* 📄 **Multi-format document support**
* 🔎 **Semantic similarity search**
* 🧠 **Retrieval-Augmented Generation (RAG)**
* 💬 **Conversational question answering**
* 🤖 **Local LLM inference using Ollama**
* 🔐 **Local document processing**
* ⚡ **FAISS vector search**
* 💻 **Source-code file analysis**
* 🧩 **Extensible RAG architecture**

---

## 📚 Supported File Formats

### Documents

* PDF — `.pdf`
* Text — `.txt`
* Markdown — `.md`
* Microsoft Word — `.docx`
* OpenDocument Text — `.odt`

### Structured Data

* JSON — `.json`

### Source Code

* Python — `.py`
* C — `.c`
* C++ — `.cpp`
* Java — `.java`
* JavaScript — `.js`
* TypeScript — `.ts`
* HTML — `.html`
* C# — `.cs`
* Shell — `.sh`

---

# 🏗️ Architecture

DocAI-RAG currently follows a straightforward RAG pipeline:

```text
                    ┌──────────────────┐
                    │    Input File    │
                    │ PDF / DOCX / JSON│
                    │ TXT / MD / Code  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Text Extraction  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Text Chunking    │
                    │ Recursive        │
                    │ Character Split  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Embedding Model  │
                    │  all-MiniLM      │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │   FAISS Index    │
                    │ Vector Retrieval │
                    └────────┬─────────┘
                             │
                       User Question
                             │
                             ▼
                    ┌──────────────────┐
                    │ Relevant Context │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    Ollama LLM    │
                    │    Gemma 3:1B    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ AI-Generated     │
                    │ Response         │
                    └──────────────────┘
```

---

# 🔍 How It Works

DocAI-RAG consists of several stages.

### 1. File Ingestion

The application accepts a supported file through the command line.

```bash
python main.py --path ./sample.pdf
```

The file extension is detected automatically and the appropriate text extraction method is used.

---

### 2. Text Extraction

DocAI-RAG extracts textual content from:

* PDF documents
* Word documents
* OpenDocument files
* JSON files
* Text and Markdown files
* Source-code files

For example:

```text
document.pdf
      ↓
PDF text extraction
      ↓
Raw document text
```

---

### 3. Text Chunking

Large documents are divided into smaller chunks using LangChain's `RecursiveCharacterTextSplitter`.

Current configuration:

```text
Chunk size:    500 characters
Overlap:        50 characters
```

Chunk overlap helps preserve context between neighboring chunks.

---

### 4. Embedding Generation

Each text chunk is converted into a vector representation using the `all-MiniLM` embedding model.

```text
Text
 ↓
Embedding Model
 ↓
Numerical Vector
```

These vectors allow semantically similar content to be retrieved even when the user's wording does not exactly match the source document.

---

### 5. Vector Search

The generated embeddings are stored in a FAISS vector index.

When a user asks a question:

```text
User Question
      ↓
Question Embedding
      ↓
FAISS Similarity Search
      ↓
Relevant Document Chunks
```

The retrieved chunks provide contextual information for the LLM.

---

### 6. Response Generation

The retrieved context and user's question are passed to the local LLM running through Ollama.

The LLM then generates a conversational response based on the retrieved document context.

---

# 🛠️ Technology Stack

| Technology      | Purpose                    |
| --------------- | -------------------------- |
| **Python**      | Core programming language  |
| **LangChain**   | RAG pipeline orchestration |
| **Ollama**      | Local LLM inference        |
| **Gemma 3:1B**  | Default language model     |
| **all-MiniLM**  | Text embedding model       |
| **FAISS**       | Vector similarity search   |
| **PyMuPDF**     | PDF text extraction        |
| **python-docx** | DOCX processing            |
| **odfpy**       | ODT processing             |

---

# 📁 Project Structure

```text
DocAI-RAG/
│
├── main.py
├── requirements.txt
├── README.md
│
└── sample/
    ├── sample.pdf
    ├── sample.txt
    ├── sample.json
    └── sample.py
```

The current implementation is intentionally lightweight, with the RAG pipeline contained primarily within `main.py`.

The architecture can be progressively modularized as additional capabilities are introduced.

---

# ⚙️ Installation

## Prerequisites

Make sure the following are installed:

* Python 3.8+
* Ollama
* pip

Clone the repository:

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>

cd DocAI-RAG
```

Install the required Python packages:

```bash
pip install -r requirements.txt
```

---

# 🤖 Configure Ollama

Start the Ollama server:

```bash
ollama serve
```

Pull the default language model:

```bash
ollama pull gemma3:1b
```

Pull the embedding model:

```bash
ollama pull all-minilm
```

Verify the installed models:

```bash
ollama list
```

---

# ▶️ Usage

Run DocAI-RAG by providing the path to a supported file.

### PDF

```bash
python main.py --path ./sample.pdf
```

### JSON

```bash
python main.py --path ./sample.json
```

### Text

```bash
python main.py --path ./sample.txt
```

### Python source code

```bash
python main.py --path ./sample.py
```

---

# 💬 Interactive Chat

After the application starts, users can ask questions about the selected file.

Example:

```text
Active data repository: ./sample.pdf

Ask me anything! (Type /quit to exit) >>>
What is the main topic of this document?

>>> The document discusses ...

Ask me anything! (Type /quit to exit) >>>
Summarize the key points.

>>> The key points are ...

Ask me anything! (Type /quit to exit) >>>
/quit

Bye.
```

---

# 🔐 Local-First Architecture

DocAI-RAG uses Ollama for local LLM inference.

This provides a local-first workflow where document processing, embeddings, and language-model inference can be performed on the user's machine.

This architecture is useful for experimenting with RAG applications involving documents that users may prefer not to send to external AI APIs.

---

# 🎯 Example Use Cases

## 📖 Research Assistant

Ask questions about research papers, technical documentation, books, and other long-form documents.

## 💻 Code Assistant

Provide source-code files and ask questions about their implementation, logic, and structure.

## 📋 Document Analysis

Interact with business documents without manually searching through the entire document.

## 🧠 Personal Knowledge Assistant

Use a collection of personal notes, Markdown files, documentation, and other text-based resources as a searchable knowledge base.

---

# 🧪 Current RAG Pipeline

The current implementation provides the following pipeline:

```text
Document
   ↓
Text Extraction
   ↓
Recursive Chunking
   ↓
Embedding Generation
   ↓
FAISS Vector Store
   ↓
Similarity Retrieval
   ↓
Conversational RAG Chain
   ↓
Ollama LLM
   ↓
Response
```

---

# 🚧 Future Improvements

DocAI-RAG is intended to evolve incrementally from a basic RAG prototype into a more production-oriented document intelligence system.

### Retrieval Improvements

* [ ] Hybrid search using BM25 + vector retrieval
* [ ] Configurable chunk sizes and overlap
* [ ] Document-aware chunking
* [ ] Metadata-based filtering
* [ ] Cross-encoder reranking
* [ ] Improved retrieval strategies

### Response Quality

* [ ] Source citations
* [ ] Page-level references
* [ ] Retrieval confidence scoring
* [ ] "I don't know" behavior for insufficient context
* [ ] Prompt optimization
* [ ] Context compression

### Evaluation

* [ ] Build a RAG evaluation dataset
* [ ] Retrieval Precision@K
* [ ] Recall@K
* [ ] MRR
* [ ] NDCG
* [ ] Answer relevance
* [ ] Faithfulness evaluation
* [ ] Hallucination analysis

### Engineering

* [ ] Modular project structure
* [ ] FastAPI backend
* [ ] Web-based user interface
* [ ] PostgreSQL / pgvector support
* [ ] Docker
* [ ] Automated testing
* [ ] CI/CD
* [ ] Cloud deployment
* [ ] Authentication and document access control
* [ ] Monitoring and observability

---

# 📈 Development Roadmap

The project will be developed incrementally.

```text
              ┌───────────────────┐
              │     Version 1     │
              │   Basic RAG       │
              └─────────┬─────────┘
                        ↓
              ┌───────────────────┐
              │     Version 2     │
              │ Metadata +        │
              │ Citations         │
              └─────────┬─────────┘
                        ↓
              ┌───────────────────┐
              │     Version 3     │
              │ Hybrid Retrieval  │
              └─────────┬─────────┘
                        ↓
              ┌───────────────────┐
              │     Version 4     │
              │ Reranking +       │
              │ Retrieval Eval    │
              └─────────┬─────────┘
                        ↓
              ┌───────────────────┐
              │     Version 5     │
              │ FastAPI + Docker  │
              └─────────┬─────────┘
                        ↓
              ┌───────────────────┐
              │     Version 6     │
              │ Cloud + Monitoring│
              └───────────────────┘
```

---

# 📌 Project Goals

The long-term objective of DocAI-RAG is to explore how modern AI systems can combine:

* Large Language Models
* Semantic search
* Vector databases
* Natural Language Processing
* Document processing
* Information retrieval
* Evaluation
* AI application engineering

⭐ If you find this project useful, consider giving the repository a star.
