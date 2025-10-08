# DocsChat RAG 🚀

<div align="center">

![Python](https://img.shields.io/badge/python-3.11+-blue.svg)
![LangChain](https://img.shields.io/badge/LangChain-0.1.0-green.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![CI](https://github.com/RomanRosa/docschat-rag/workflows/CI/badge.svg)
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
