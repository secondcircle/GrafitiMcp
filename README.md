# Graphiti MCP Server

A knowledge graph memory system for AI agents using the Model Context Protocol (MCP).

Based on [Graphiti](https://github.com/getzep/graphiti), an open-source knowledge graph for AI agents.

## Overview

Graphiti MCP Server provides a temporal memory graph for AI agents, allowing them to store and retrieve information with rich context. It uses:

- [**Graphiti**](https://github.com/getzep/graphiti): Core graph functionality for managing entities and relationships
- **Neo4j**: Graph database for storing knowledge
- **Graphiti**: Core graph functionality for managing entities and relationships
- **MCP Server**: Model Context Protocol interface for AI agents to interact with the graph

## Prerequisites

- Docker and Docker Compose
- OpenAI API key

## Setup Instructions

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd GrafitiMcp
   ```

2. **Set up environment variables**

   ```bash
   cp .env.template .env
   ```

   Edit the `.env` file and add your OpenAI API key and other configuration:

   ```
   NEO4J_PASSWORD=your_secure_password_here
   OPENAI_API_KEY=your_openai_api_key_here
   MODEL_NAME=gpt-4o  # or another OpenAI model
   ```

3. **Start the services**

   ```bash
   docker-compose up
   ```

   This will start:
   - Neo4j database on port 8044
   - Graphiti API on port 8080
   - Graphiti MCP Server on port 8001

## Connecting to the MCP Server

The MCP server runs on port 8001 and uses Server-Sent Events (SSE) transport by default.

### In Roo Code

To connect to the MCP server in Roo Code:

1. Open Roo Code
2. Go to Settings > MCP Servers
3. Add a new MCP server with:
   - Name: `graphiti-mcp`
   - URL: `http://localhost:8001`
   - Transport: `sse`

### In Custom Applications

To connect to the MCP server in your own applications, use the MCP client library:

```python
from mcp.client import MCPClient

# Create an MCP client
client = MCPClient("http://localhost:8001", transport="sse")

# Connect to the server
await client.connect()

# Use the tools provided by the server
result = await client.use_tool("add_episode", {
    "name": "Test Episode",
    "episode_body": "This is a test episode",
    "source": "text"
})
```

## Basic Usage Examples

### Adding Information to the Graph

```python
# Add a text episode
await client.use_tool("add_episode", {
    "name": "Company News",
    "episode_body": "Acme Corp announced a new product line today.",
    "source": "text",
    "source_description": "news article"
})

# Add structured JSON data
await client.use_tool("add_episode", {
    "name": "Customer Profile",
    "episode_body": "{\"company\": {\"name\": \"Acme Technologies\"}, \"products\": [{\"id\": \"P001\", \"name\": \"CloudSync\"}, {\"id\": \"P002\", \"name\": \"DataMiner\"}]}",
    "source": "json",
    "source_description": "CRM data"
})
```

### Searching the Graph

```python
# Search for nodes
nodes = await client.use_tool("search_nodes", {
    "query": "What products does Acme Technologies offer?",
    "max_nodes": 5
})

# Search for facts
facts = await client.use_tool("search_facts", {
    "query": "Tell me about Acme Corp's product announcements",
    "max_facts": 5
})
```

## Troubleshooting

### Connection Issues

If you can't connect to the MCP server:

1. Ensure all services are running: `docker-compose ps`
2. Check the logs: `docker-compose logs graphiti-mcp`
3. Verify the server is accessible: `curl http://localhost:8001/status`

### Neo4j Connection Errors

If the MCP server can't connect to Neo4j:

1. Check Neo4j is running: `docker-compose logs neo4j`
2. Verify Neo4j credentials in the `.env` file
3. Ensure Neo4j is healthy: `docker-compose exec neo4j cypher-shell -u neo4j -p <your-password> "RETURN 1;"`

## License

[Specify your license here]