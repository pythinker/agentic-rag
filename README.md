# Agentic RAG

![Agentic RAG Architecture](docs/agentic-rag.png)

## Table of Contents
- [Overview](#overview)
- [How to Build](#how-to-build)
  - [Cloning the Repository](#cloning-the-repository)
  - [Running n8n using Docker Compose on Mac](#running-n8n-using-docker-compose-on-mac)
- [How to Use](#how-to-use)
  - [Store Source Documents](#store-source-documents)
  - [Create Embeddings for Source Documents](#create-embeddings-for-source-documents)
  - [Ask Questions about the Source Documents](#ask-questions-about-the-source-documents)
- [How to Diagnose](#how-to-diagnose)
  - [Accessing PostgreSQL and Qdrant DBs](#accessing-postgresql-and-qdrant-dbs)
- [Project Structure](#project-structure)
  - [Key Components](#key-components)

## Overview
Agentic RAG (Retrieval Augmented Generation) is an AI-powered system that combines the capabilities of RAG with autonomous agent behaviors. This system:

- Retrieves relevant information from a knowledge base using vector similarity search
- Augments LLM responses with this retrieved context for more accurate and informed answers
- Incorporates agentic behaviors allowing the system to take actions based on user requests
- Uses n8n workflows to orchestrate the RAG pipeline and agent actions
- Stores conversations in PostgreSQL and vector embeddings in Qdrant
- Provides a simple API endpoint for integration with other applications

This project demonstrates how to build and deploy a self-hosted AI solution with document retrieval capabilities and autonomous behaviors.

## How to Build

### Cloning the Repository

```bash
git clone https://github.com/pythinker/agentic-rag.git
```

### Running n8n using Docker Compose on Mac
```
cd agentic-rag
rm -rf ./volumes
docker compose up
```
The n8n editor is now accessible via `http://localhost:5678/`

## How to Use
### Store Source Documents
Put all your PDF files into the `./shared` folder
### Create Embeddings for Source Documents
```bash
curl -X GET http://localhost:5678/webhook/create_source_embeddings -H "Content-Type: application/json"
```
### Ask Questions about the Source Documents
```bash
curl -X POST http://localhost:5678/webhook/invoke_n8n_agent -H "Content-Type: application/json" -d '{"chatInput": "What are the ingredients of Apple Berry Crisp?", "sessionId": "c324038d8b2944a0855c2e40441038e3"}'
```

## How to Diagnose
### Accessing PostgreSQL and Qdrant DBs
You can access PostgreSQL and Qdrant DBs in these addresses, respectively:
- `http://localhost:5050`
- `http://localhost:6333/dashboard`

## Project Structure
The project is organized into the following components:

### Key Components
- `docker-compose.yml` - Docker Compose configuration for running all services
- `n8n_workflows/` - Contains the n8n workflow definition for the agentic RAG system
- `n8n_credentials/` - Credential configurations for n8n
- `shared/` - Directory for storing source documents (PDFs)
- `volumes/` - Data persistence for the services (created at runtime)
- `docs/` - Project documentation and diagrams
