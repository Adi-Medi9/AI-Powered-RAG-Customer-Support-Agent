# AI-Powered RAG Customer Support Agent

A lightweight Retrieval-Augmented Generation (RAG) application for answering questions from uploaded documents. The app reads PDFs, text files, and Markdown files, splits them into chunks, embeds them with OpenAI embeddings, retrieves the most relevant sections, and uses a deep reasoning agent to produce grounded responses.

This project is designed as a customer support-style Q&A assistant: upload documents, ask a question, and the system searches the knowledge base before answering.

## Features

- Upload multiple documents in one session
- Supports PDF, TXT, and Markdown inputs
- Extracts text from uploaded files using `pypdf`
- Splits long documents into overlapping chunks for better retrieval
- Builds an in-memory vector index with `OpenAIEmbeddings`
- Uses semantic similarity search to find relevant passages
- Saves retrieved chunks under `/retrieved/` for evidence-based analysis
- Uses a Deep Agent with subagents to inspect chunks and synthesize answers
- Provides a Streamlit-based chat interface for end-user interaction

## Tech Stack

- Python 3.13
- Streamlit
- LangChain
- DeepAgents
- OpenAI embeddings and chat models
- `pypdf` for PDF parsing
- `python-dotenv` for environment configuration

## Repository Structure

```text
AI-Powered-RAG-Customer-Support-Agent/
├── .vscode/
├── pdfs/
│   ├── climate_change.pdf
│   └── global_warming.pdf
├── src/
│   └── rag/
│       ├── __init__.py
│       ├── agent.py
│       ├── filereader.py
│       ├── indexing.py
│       ├── prompts.py
│       └── tools.py
├── .gitignore
├── .python-version
├── pyproject.toml
├── README.md
├── uv.lock
└── ...
```

### Main files

- `src/rag/__init__.py` — main Streamlit app and document Q&A workflow
- `src/rag/agent.py` — example Deep Agent setup using a static search flow
- `src/rag/indexing.py` — prototype indexing script for local PDF documents
- `src/rag/filereader.py` — PDF text extraction helper
- `src/rag/tools.py` — search tool that retrieves relevant chunks
- `src/rag/prompts.py` — RAG workflow and subagent instructions for the reasoning process

## How it Works

1. The user uploads one or more files through the Streamlit UI.
2. Each file is converted to a LangChain `Document` object.
3. The document is split into text chunks using `RecursiveCharacterTextSplitter`.
4. Chunks are embedded and indexed in an in-memory vector store.
5. The user asks a question.
6. The agent performs a similarity search against the vector store.
7. Top matching chunks are saved to the backend filesystem.
8. Subagents inspect each chunk and summarize relevant facts.
9. The final answer is composed from the gathered evidence.

## Environment Setup

Create a `.env` file in the project root with your OpenAI API key:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

## Installation

Using `uv` (recommended):

```bash
uv sync
```

Or with `pip`:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

## Run the Application

Launch the Streamlit app:

```bash
streamlit run src/rag/__init__.py
```

This opens the chat interface where you can upload documents and ask questions.

## Example Workflow

1. Start the app.
2. Upload `pdfs/climate_change.pdf` or `pdfs/global_warming.pdf`.
3. Click `Process documents`.
4. Ask questions like:

```text
What are the causes of climate change?
How does global warming affect the environment?
```

The system searches relevant document chunks and responds with a grounded answer.

## Notes

- The vector store is kept in memory, so indexing is session-based.
- The app currently uses OpenAI embeddings and chat models, so an API key is required.
- This is an experimental project and serves as a strong starting point for a customer support knowledge assistant.

## Future Enhancements

- Persistent vector database storage
- Multi-document source citations in answers
- Authentication and user management
- Support for larger enterprise document collections
- Deployment as a web app or internal support portal

## License

This project is provided as-is for learning and experimentation. Add a license file if you intend to publish or distribute it broadly.
