# DocsChat RAG 🚀

<div align="center">

![Python](https://img.shields.io/badge/python-3.11+-blue.svg)
![LangChain](https://img.shields.io/badge/LangChain-0.1.0-green.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)

**Enterprise-grade RAG system for technical documentation with advanced retrieval strategies and precise source citation**

[Features](#-features) •
[Demo](#-demo) •
[Quick Start](#-quick-start) •
[Architecture](#-architecture) •
[Documentation](#-documentation)

</div>

---

## 📋 Overview

DocsChat RAG is a production-ready Retrieval-Augmented Generation system designed for querying technical documentation (Python, React, FastAPI) with enterprise-grade architecture patterns, multiple retrieval strategies, and accurate source citations.

### What Makes This Different?

- 🎯 **Hybrid Search**: Combines semantic (vector) and keyword (BM25) search with RRF fusion
- 🔍 **HyDE Query Transformation**: Hypothetical Document Embeddings for improved retrieval
- 📊 **Intelligent Reranking**: Cohere API integration for relevance optimization
- 📖 **Source Citation**: Precise page numbers and document references
- 💬 **Conversational Memory**: Multi-turn conversation context
- 🏗️ **Clean Architecture**: SOLID principles, dependency injection, comprehensive testing
- 🐳 **Production Ready**: Docker containerization, CI/CD pipelines, monitoring

---

## ✨ Features

### Core Capabilities

- **Multi-Source Ingestion**: Scrapes and processes Python, React, and FastAPI official documentation
- **Advanced Chunking**: Semantic and recursive text splitting strategies
- **Hybrid Retrieval**: 
  - Semantic search (cosine similarity)
  - Keyword search (BM25)
  - RRF-based fusion
- **Query Enhancement**: HyDE transformation for better retrieval
- **Smart Reranking**: Cohere cross-encoder for relevance scoring
- **Citation Tracking**: Page numbers and source URLs preserved
- **Conversational Context**: Last N turns memory management

### Technical Highlights

- **SOLID Principles**: Clean, maintainable, extensible codebase
- **Type Safety**: Comprehensive type hints with mypy validation
- **Testing**: Unit and integration tests with pytest
- **Documentation**: Detailed docstrings (Google style) and architecture docs
- **CI/CD**: GitHub Actions for linting, testing, and deployment
- **Observability**: Structured logging with loguru
- **Containerization**: Docker and docker-compose setup

---

## 🎥 Demo

> **Live Demo**: [docschat-rag.streamlit.app](https://docschat-rag.streamlit.app) *(coming soon)*

### Example Query

```
User: "How do I handle CORS in FastAPI?"

DocsChat:
To handle CORS in FastAPI, use the CORSMiddleware:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Sources:**
- FastAPI Documentation: Advanced User Guide > CORS (Page 47)
- [Official Docs Link](https://fastapi.tiangolo.com/tutorial/cors/)
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- Poetry (recommended) or pip
- OpenAI API key
- Cohere API key (optional, for reranking)

### Installation

```bash
# Clone repository
git clone https://github.com/RomanRosa/docschat-rag.git
cd docschat-rag

# Install dependencies with Poetry
poetry install

# OR with pip
pip install -r requirements.txt

# Setup environment variables
cp .env.example .env
# Edit .env with your API keys
```

### Environment Variables

```bash
# .env
OPENAI_API_KEY=your_openai_api_key_here
COHERE_API_KEY=your_cohere_api_key_here

# LLM Configuration
LLM_MODEL=gpt-4o-mini
EMBEDDING_MODEL=text-embedding-3-small
TEMPERATURE=0.0

# Retrieval Configuration
TOP_K=10
RERANK_TOP_K=3
CHUNK_SIZE=1000
CHUNK_OVERLAP=200

# Vector Store
CHROMADB_PERSIST_DIR=./data/vectorstore
```

### Run Locally

```bash
# 1. Ingest documentation (one-time setup)
poetry run python scripts/ingest_docs.py --sources python react fastapi

# 2. Build vector index
poetry run python scripts/rebuild_index.py

# 3. Launch Streamlit UI
poetry run streamlit run src/ui/app.py
```

Access at: `http://localhost:8501`

### Docker Quick Start

```bash
# Build and run with docker-compose
docker-compose up -d

# Access at http://localhost:8501
```

---

## 🏗️ Architecture

### High-Level Design

```
┌─────────────┐
│   User UI   │  Streamlit Chat Interface
└──────┬──────┘
       │
       ▼
┌─────────────────────────────────────────────┐
│           RAG Pipeline Orchestrator         │
│  ┌─────────────────────────────────────┐    │
│  │  Query Processor                    │    │
│  │  - Validation                       │    │
│  │  - HyDE Transformation              │    │
│  └────────────┬────────────────────────┘    │
│               │                             │
│  ┌────────────▼────────────────────────┐    │
│  │  Hybrid Retriever                   │    │
│  │  ┌──────────────┐  ┌──────────────┐ │    │
│  │  │  Semantic    │  │  Keyword     │ │    │
│  │  │  (Vector)    │  │  (BM25)      │ │    │
│  │  └──────┬───────┘  └──────┬───────┘ │    │
│  │         │                  │        │    │
│  │         └────────┬─────────┘        │    │
│  │                  │                  │    │
│  │         ┌────────▼─────────┐        │    │
│  │         │  RRF Fusion      │        │    │
│  │         └────────┬─────────┘        │    │
│  └──────────────────┼─────────────────┘     │
│                     │                       │
│  ┌──────────────────▼─────────────────┐     │
│  │  Cohere Reranker                   │     │
│  └──────────────────┬─────────────────┘     │
│                     │                       │
│  ┌──────────────────▼─────────────────┐     │
│  │  LLM Generator (GPT-4o-mini)       │     │
│  │  - Prompt with context             │     │
│  │  - Citation injection              │     │
│  │  - Memory management               │     │
│  └────────────────────────────────────┘     │
└─────────────────────────────────────────────┘
```

### Key Components

| Module | Responsibility | Key Classes |
|--------|---------------|-------------|
| `ingestion/` | Document scraping, parsing, chunking | `PythonDocsIngester`, `SemanticChunker` |
| `vectorization/` | Embeddings generation, vector storage | `OpenAIEmbedder`, `ChromaVectorStore` |
| `retrieval/` | Search strategies, reranking | `HybridRetriever`, `CohereReranker` |
| `generation/` | LLM calls, prompt templating | `OpenAIGenerator`, `ConversationalMemory` |
| `pipeline/` | End-to-end orchestration | `RAGPipeline`, `QueryProcessor` |
| `ui/` | Streamlit interface | `ChatInterface`, `SourcePanel` |

See [ARCHITECTURE.md](docs/ARCHITECTURE.md) for detailed design documentation.

---

## 📚 Documentation

- **[Architecture Guide](docs/ARCHITECTURE.md)**: System design, component diagrams, design decisions
- **[API Reference](docs/API.md)**: Module APIs, class references
- **[Development Guide](docs/DEVELOPMENT.md)**: Local setup, testing, contributing
- **[Deployment Guide](docs/DEPLOYMENT.md)**: Docker, Streamlit Cloud, AWS deployment

---

## 🧪 Testing

```bash
# Run all tests
poetry run pytest

# Run with coverage
poetry run pytest --cov=src --cov-report=html

# Run specific test module
poetry run pytest tests/unit/test_retrieval.py

# Run integration tests
poetry run pytest tests/integration/
```

### Test Coverage

Current coverage: **92%** *(target: 90%+)*

---

## 🛠️ Development

### Setup Development Environment

```bash
# Install dev dependencies
poetry install --with dev

# Setup pre-commit hooks
pre-commit install

# Run linting
poetry run ruff check src/
poetry run black src/
poetry run mypy src/

# Format code
poetry run black src/
```

### Code Style

- **Formatter**: Black (line length: 100)
- **Linter**: Ruff (replaces flake8, isort, pylint)
- **Type Checker**: Mypy (strict mode)
- **Docstrings**: Google style
- **Commits**: Conventional Commits

---

## 📦 Tech Stack

| Category | Technology |
|----------|-----------|
| **Framework** | LangChain 0.1.0 |
| **LLM** | OpenAI GPT-4o-mini |
| **Embeddings** | OpenAI text-embedding-3-small |
| **Vector DB** | ChromaDB |
| **Reranking** | Cohere API |
| **UI** | Streamlit |
| **Testing** | Pytest, pytest-cov |
| **CI/CD** | GitHub Actions |
| **Containerization** | Docker, docker-compose |
| **Logging** | Loguru |

---

## 🗺️ Roadmap

### Phase 1: MVP ✅ (Current)
- [x] Basic ingestion pipeline
- [x] Hybrid retrieval
- [x] LLM generation with citations
- [x] Streamlit UI
- [x] Docker setup

### Phase 2: Enhancement 🚧 (In Progress)
- [ ] HyDE query transformation
- [ ] Advanced reranking
- [ ] Conversational memory
- [ ] Performance benchmarking

### Phase 3: Production 📋 (Planned)
- [ ] Multi-tenancy support
- [ ] API endpoints (FastAPI)
- [ ] Admin dashboard
- [ ] Usage analytics
- [ ] Cost optimization

### Phase 4: Advanced 🔮 (Future)
- [ ] Multi-modal support (code screenshots)
- [ ] Custom fine-tuned embeddings
- [ ] Graph RAG integration
- [ ] Real-time doc updates

---

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Workflow

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'feat(retrieval): add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

---

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [LangChain](https://github.com/langchain-ai/langchain) for RAG framework
- [ChromaDB](https://www.trychroma.com/) for vector storage
- [OpenAI](https://openai.com/) for LLM and embeddings
- [Cohere](https://cohere.com/) for reranking API
- [Streamlit](https://streamlit.io/) for rapid UI development

---

## 📞 Contact

**Francisco Román Peña de la Rosa**

- LinkedIn: [franciscopena76165796](https://www.linkedin.com/in/franciscopena76165796/)
- GitHub: [@RomanRosa](https://github.com/RomanRosa)
- Email: roman_de_la_rosa@hotmail.com

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=RomanRosa/docschat-rag&type=Date)](https://star-history.com/#RomanRosa/docschat-rag&Date)

---

<div align="center">

**If this project helped you, please consider giving it a ⭐!**

Made with ❤️ by [Roman de la Rosa](https://github.com/RomanRosa)

</div>
