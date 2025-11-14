# Codebase Indexer

A production-ready tool for indexing and searching GitHub repositories using advanced AST-based chunking, vector embeddings, and semantic search capabilities. This system enables intelligent code search and retrieval by creating vector embeddings of code chunks and storing them in FAISS vector databases.

## Overview

Codebase Indexer is a standalone tool designed for developers and teams who need to efficiently search and understand large codebases. It uses tree-sitter for AST-based code parsing and supports incremental updates through Merkle tree-based change detection, making it ideal for maintaining up-to-date indexes of rapidly evolving projects.

## Key Features

- **Multi-language Support**: Supports 25+ programming languages including Python, Java, JavaScript, TypeScript, C/C++, Go, Rust, Ruby, Bash, and more
- **AST-based Code Chunking**: Intelligent code chunking that respects language syntax and structure using tree-sitter
- **Incremental Updates**: Only re-indexes changed files using Merkle tree-based change detection for efficient operations
- **Semantic Search**: Find relevant code snippets using natural language queries powered by vector embeddings
- **Efficient Storage**: FAISS vector databases optimized for fast similarity search at scale
- **Multi-repository Management**: Index and search across multiple repositories with isolated namespaces
- **Rich CLI Interface**: User-friendly command-line interface with progress tracking and detailed feedback
- **Rate Limiting**: Built-in rate limiting for API calls to ensure reliable operation

## Installation

### Prerequisites

- Python 3.8 or higher
- GitHub Personal Access Token
- 2GB+ RAM recommended for large repositories

### Setup

1. Clone the repository:
```bash
git clone https://github.com/yourusername/CodebaseIndexer.git
cd CodebaseIndexer
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Configure environment variables by creating a `.env` file:
```bash
GITHUB_TOKEN=your_github_personal_access_token
VECTOR_STORE_DIR=./VectorStore  # Optional: defaults to ./VectorStore
MERKLE_DB_PATH=./merkle_trees.db  # Optional: defaults to ./merkle_trees.db
```

To obtain a GitHub Personal Access Token, visit [GitHub Settings > Developer Settings > Personal Access Tokens](https://github.com/settings/tokens).

## Usage

### Indexing a Repository

Index a new repository or update an existing one:

```bash
python main.py --repo owner/repo
```

You can also use the full GitHub URL:

```bash
python main.py --repo https://github.com/owner/repo
```

Exclude specific directories from indexing:

```bash
python main.py --repo owner/repo --exclude node_modules,dist,build,.git
```

### Searching a Repository

Search for code snippets using natural language queries:

```bash
python main.py --repo owner/repo --search "function to parse JSON files"
```

Search results include:
- File path and line numbers
- Similarity score
- Chunk type (function, class, etc.) and language
- Preview of the code snippet

### Deleting an Index

Remove a repository index to free up disk space:

```bash
python main.py --repo owner/repo --delete
```

### Verbose Logging

Enable detailed logging for debugging or monitoring:

```bash
python main.py --repo owner/repo --verbose
```

## Code Chunking Strategy

The system employs a multi-tiered chunking approach to handle diverse file types:

1. **AST-based Chunking** (Primary): Uses tree-sitter to parse code into syntax trees and extract semantic units like functions, classes, and methods. This preserves code context and structure.

2. **Recursive Text Splitting** (Fallback): For languages without tree-sitter support or when AST parsing fails, uses language-aware recursive splitting with configurable separators.

3. **Simple Chunking** (Final Fallback): For markdown, text files, and other non-code content, splits by paragraphs and sentences.

All chunks respect the configured token limits (default: 1000 tokens per chunk with 200 token overlap) to ensure optimal embedding quality.

## Supported Languages

The indexer supports the following languages with tree-sitter AST parsing:

**Compiled Languages**: C, C++, Java, Go, Rust
**Scripting Languages**: Python, JavaScript, TypeScript, Ruby, Bash, Perl, PHP, Groovy
**Build Systems**: Make, Gradle
**Markup/Data**: HTML, XML, YAML, JSON, TOML, INI, SQL
**Documentation**: Markdown, Plain Text

## Configuration

Default settings can be customized via environment variables:

- **Embedding Model**: `openai/text-embedding-3-large` (via GitHub Models)
- **Max Tokens per Chunk**: 1000 tokens
- **Overlap Tokens**: 200 tokens
- **Max File Size**: 1MB
- **Vector Similarity**: Cosine similarity
- **Rate Limits**: 15 requests/minute, 5 concurrent, 64000 tokens/request

## Contributing

Contributions are welcome. Please ensure your code follows the existing style and includes appropriate tests.

## License

This project is licensed under the Apache-2.0 License License. See the LICENSE file for details.

## Acknowledgments

The codebase indexing approach is inspired by techniques described in [How Cursor Indexes Codebases Fast](https://read.engineerscodex.com/p/how-cursor-indexes-codebases-fast), which provides insights into efficient codebase indexing for AI-powered development tools.
