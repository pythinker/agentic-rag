# Agentic RAG

## Overview
Agentic RAG (Retrieval Augmented Generation) is an AI-powered system that combines the capabilities of RAG with autonomous agent behaviors. This system:

- Retrieves relevant information from a knowledge base using vector similarity search
- Augments LLM responses with this retrieved context for more accurate and informed answers
- Incorporates agentic behaviors allowing the system to take actions based on user requests
- Uses n8n workflows to orchestrate the RAG pipeline and agent actions
- Stores conversations in PostgreSQL and vector embeddings in Qdrant
- Provides a simple API endpoint for integration with other applications

This project demonstrates how to build and deploy a self-hosted AI solution with document retrieval capabilities and autonomous behaviors.

## Installation

### Cloning the Repository

```bash
git clone https://github.com/pythinker/ryan-ai-agent.git
cd self-hosted-ai-starter-kit
```

### Running n8n using Docker Compose on Mac
```
git clone https://github.com/n8n-io/self-hosted-ai-starter-kit.git
cd self-hosted-ai-starter-kit
docker compose up
```

If you're running OLLAMA locally on your Mac (not in Docker), you need to modify the OLLAMA_HOST environment variable
in the n8n service configuration. Update the x-n8n section in your Docker Compose file as follows:

```yaml
x-n8n: &service-n8n
  # ... other configurations ...
  environment:
    # ... other environment variables ...
    - OLLAMA_HOST=host.docker.internal:11434
```

Additionally, after you see "Editor is now accessible via: <http://localhost:5678/>":

1. Head to <http://localhost:5678/home/credentials>
2. Click on "Local Ollama service"
3. Change the base URL to "http://host.docker.internal:11434/"
4. Set the Host of the PostgreSQL widget on n8n to "host.docker.internal"


## Usage
### Use the AgenticRAG API Endpoint
```bash
curl -X POST http://localhost:5678/webhook-test/invoke_n8n_agent -H "Content-Type: application/json" -d '{"chatInput": "What are the ingredients of Apple Berry Crisp?", "sessionId": "c324038d8b2944a0855c2e40441038e3"}'
```

### Accessing PostgreSQL and Qdrant DBs
You can access PostgreSQL and Qdrant DBs in these addresses, respectively:
- "http://localhost:5050"
- "http://localhost:6333/dashboard"

### Deleting the Vector DB
```bash
curl -X DELETE 'http://localhost:6333/collections/documents'
```

### Deleting the Chat History
```bash
psql -h localhost -p 5432 -U root -d postgres
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'n8n'; DROP DATABASE n8n;
```
Note that you need to execute `docker compose down` and `docker compose up` again after you delete the chat history
