# OpenSearch MCP Integration Guide

This guide provides step-by-step instructions for setting up OpenSearch with Amazon Q using MCP servers for natural language interactions.

[Previous sections remain the same...]

## Available MCP Tools

### Listing Available Tools

To see what tools an MCP server provides, you can ask Amazon Q:
```
What tools does the OpenSearch MCP server provide?
```
or
```
List all available tools from the OpenSearch MCP server
```

### OpenSearch MCP Server Tools

The OpenSearch MCP server currently provides the following tools:

1. **Index Management**
   - `create_index`: Create a new index with optional mappings and settings
   - `delete_index`: Remove an existing index
   - `get_index`: Get information about an index (mappings, settings, aliases)
   - `list_indices`: List all indices in the cluster

2. **Document Operations**
   - `index_document`: Add or update a document in an index
   - `get_document`: Retrieve a document by ID
   - `delete_document`: Remove a document by ID
   - `delete_by_query`: Delete documents matching a query

3. **Search Operations**
   - `search_documents`: Search for documents using query DSL
   
4. **Alias Management**
   - `list_aliases`: List all aliases
   - `get_alias`: Get alias information for a specific index
   - `put_alias`: Create or update an alias
   - `delete_alias`: Remove an alias

5. **Cluster Operations**
   - `get_cluster_health`: Check cluster health status
   - `get_cluster_stats`: Get cluster statistics

6. **General Operations**
   - `general_api_request`: Make custom API requests to OpenSearch

### Example Tool Usage

1. **Creating an Index**
   ```
   Create an index named "products" with the following fields:
   - name (text)
   - price (number)
   - description (text)
   - category (keyword)
   ```

2. **Searching Documents**
   ```
   Search the products index for items in the "electronics" category with price less than $500
   ```

3. **Managing Aliases**
   ```
   Create an alias named "current_products" for the products index
   ```

4. **Checking Cluster Health**
   ```
   Show me the current health status of the OpenSearch cluster
   ```

### Tool Parameters

Each tool accepts specific parameters. Here are some common ones:

1. **create_index**
   - `index`: Name of the index
   - `body`: Optional index configuration including mappings and settings

2. **search_documents**
   - `index`: Name of the index to search
   - `body`: Search query in OpenSearch Query DSL format

3. **index_document**
   - `index`: Target index name
   - `document`: Document data
   - `id`: Optional document ID

4. **general_api_request**
   - `method`: HTTP method (GET, POST, PUT, DELETE)
   - `path`: API endpoint path
   - `params`: Query parameters
   - `body`: Request body

### Tool Response Format

Tools typically return responses in JSON format, containing:
- Operation status
- Relevant data
- Error messages (if any)

Example response for a search operation:
```json
{
  "took": 5,
  "hits": {
    "total": {
      "value": 10,
      "relation": "eq"
    },
    "hits": [
      {
        "_index": "products",
        "_id": "1",
        "_score": 1.0,
        "_source": {
          "name": "Laptop",
          "price": 499.99
        }
      }
    ]
  }
}
```

[Rest of the README remains the same...]