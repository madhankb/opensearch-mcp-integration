# OpenSearch MCP Integration Guide

This guide provides step-by-step instructions for setting up OpenSearch with Amazon Q using MCP servers for natural language interactions.

## Prerequisites

- Docker and Docker Compose installed
- Python 3.x installed
- pipx installed (`pip install pipx`)
- Amazon Q subscription

## 1. Setting up Local OpenSearch Instance

1. Create a new directory for your project:
   ```bash
   mkdir opensearch-mcp && cd opensearch-mcp
   ```

2. Create a `docker-compose.yml` file with the following content:
   ```yaml
   version: '3'
   services:
     opensearch:
       image: opensearchproject/opensearch:latest
       environment:
         - discovery.type=single-node
         - bootstrap.memory_lock=true
         - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m"
         - "OPENSEARCH_INITIAL_ADMIN_PASSWORD=myStrongPassword123!"
         - "plugins.security.ssl.http.enabled=false"
       ulimits:
         memlock:
           soft: -1
           hard: -1
         nofile:
           soft: 65536
           hard: 65536
       volumes:
         - opensearch-data:/usr/share/opensearch/data
       ports:
         - 9200:9200
         - 9600:9600
       networks:
         - opensearch-net

     opensearch-dashboards:
       image: opensearchproject/opensearch-dashboards:latest
       environment:
         - 'OPENSEARCH_HOSTS=["http://opensearch:9200"]'
         - "DISABLE_SECURITY_DASHBOARDS_PLUGIN=true"
       ports:
         - 5601:5601
       networks:
         - opensearch-net

   volumes:
     opensearch-data:

   networks:
     opensearch-net:
   ```

3. Start the containers:
   ```bash
   docker compose up -d
   ```

## 2. Installing OpenSearch MCP Server

1. Install the OpenSearch MCP server using pipx:
   ```bash
   pipx install opensearch-mcp-server
   ```

## 3. Configuring Amazon Q MCP

1. Create a `mcp_config.json` file in your Amazon Q configuration directory (refer to Amazon Q documentation for the exact location):
   ```json
   {
     "mcpServers": {
       "opensearch-mcp-server": {
         "command": "/Users/YOUR_USERNAME/.local/bin/opensearch-mcp-server",
         "args": [],
         "env": {
           "OPENSEARCH_URL": "http://localhost:9200",
           "OPENSEARCH_USERNAME": "admin",
           "OPENSEARCH_PASSWORD": "myStrongPassword123!"
         }
       }
     }
   }
   ```

   Replace `YOUR_USERNAME` with your system username.

## 4. Verifying the Setup

1. Check OpenSearch is running:
   ```bash
   curl -X GET -u admin:myStrongPassword123! http://localhost:9200
   ```

2. Verify OpenSearch Dashboards:
   - Open http://localhost:5601 in your browser

3. Check indices:
   ```bash
   curl -X GET -u admin:myStrongPassword123! http://localhost:9200/_cat/indices
   ```

## 5. Using Natural Language with OpenSearch

Here are some example commands you can try in Amazon Q:

1. Create an index:
   ```
   Create a new index called "books" with title and author fields
   ```

2. Add documents:
   ```
   Add a document to the books index with title "The Great Gatsby" and author "F. Scott Fitzgerald"
   ```

3. Search documents:
   ```
   Search for books by F. Scott Fitzgerald
   ```

4. Get index information:
   ```
   Show me the mapping of the books index
   ```

## Troubleshooting

1. If MCP server fails to initialize:
   - Check if OpenSearch is running (`docker ps`)
   - Verify credentials in `mcp_config.json`
   - Check OpenSearch logs (`docker compose logs opensearch`)

2. If tools are not available:
   - Restart Amazon Q
   - Check MCP server logs
   - Verify the `mcp_config.json` syntax

## Available MCP Tools

The OpenSearch MCP server provides several tools for interacting with OpenSearch:

- `create_index`: Create new indices
- `delete_index`: Remove indices
- `index_document`: Add documents to an index
- `search_documents`: Search for documents
- `get_index`: Get index information
- And more...

## Security Notes

1. Always use strong passwords in production
2. Enable SSL in production environments
3. Regularly rotate credentials

## Additional Resources

- [OpenSearch Documentation](https://opensearch.org/docs/latest/)
- [Amazon Q Documentation](https://docs.aws.amazon.com/amazonq/)
- [MCP Protocol Documentation](https://github.com/aws/model-context-protocol)