# Redis MCP Server

The Redis MCP Server is a natural language interface designed for agentic applications to efficiently manage and search data in Redis. It integrates seamlessly with MCP (Model Content Protocol) clients, enabling AI-driven workflows to interact with structured and unstructured data in Redis.

## Features

- **Natural Language Queries**: Enables AI agents to query and update Redis using natural language
- **Seamless MCP Integration**: Works with any MCP client for smooth communication
- **Full Redis Support**: Handles hashes, lists, sets, sorted sets, streams, and more
- **Search & Filtering**: Supports efficient data retrieval and searching in Redis
- **Scalable & Lightweight**: Designed for high-performance data operations
- **HTTP Transport**: Exposes MCP server on port 8000 for external access

## Main Template

### `redis-mcp/mcp`
The main flexible Redis MCP Server that can connect to any Redis instance.

```bash
# Run with default settings (connects to localhost:6379)
monk run redis-mcp/mcp

# Run with custom Redis connection
monk run redis-mcp/mcp --set redis_host=your-redis-host --set redis_port=6379 --set redis_password=your-password
```

## Example Stacks

### Simple Redis Stack
Redis instance with MCP server using Monk connections.

```bash
monk load redis-mcp/example.simple.yaml
monk run redis-mcp-simple/redis-stack
```

### Redis Cluster Stack
Redis cluster with sentinel and MCP server.

```bash
monk load redis-mcp/example.cluster.yaml
monk run redis-mcp-cluster/cluster-stack
```

### Redis Cloud Stack
Redis Cloud instance with MCP server.

```bash
monk load redis-mcp/example.cloud.yaml
monk run redis-mcp-cloud/cloud-stack
```

## Configuration

The Redis MCP Server supports various configuration options through environment variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `MCP_TRANSPORT` | Transport type (stdio, streamable-http, sse) | streamable-http |
| `MCP_HOST` | Server host for HTTP transport | 0.0.0.0 |
| `MCP_PORT` | Server port for HTTP transport | 8000 |
| `REDIS_HOST` | Redis server hostname | localhost |
| `REDIS_PORT` | Redis server port | 6379 |
| `REDIS_USERNAME` | Redis username (if applicable) | "" |
| `REDIS_PWD` | Redis password (if applicable) | "" |
| `REDIS_SSL` | Enable SSL/TLS connection | false |
| `REDIS_CA_PATH` | Path to CA certificate | "" |
| `REDIS_CLUSTER_MODE` | Enable cluster mode | false |

## Integration Examples



### With VS Code

Add to your `.vscode/mcp.json`:

```json
{
  "servers": {
    "redis": {
      "type": "http",
      "url": "http://127.0.0.1:8000/mcp/"
    }
  }
}
```

### With GitHub Copilot

Configure in your settings:

```json
{
  "mcp": {
    "servers": {
      "redis-mcp": {
        "type": "http",
        "url": "http://127.0.0.1:8000/mcp/"
      }
    }
  }
}
```

## Use Cases

- **AI Assistants**: Enable LLMs to fetch, store, and process data in Redis
- **Chatbots & Virtual Agents**: Retrieve session data, manage queues, and personalize responses
- **Data Search & Analytics**: Query Redis for real-time insights and fast lookups
- **Event Processing**: Manage event streams with Redis Streams

## Example Queries

Once connected, you can ask natural language questions like:

- "Store the entire conversation in a stream"
- "Cache this item"
- "Store the session with an expiration time"
- "Index and search this vector"
- "Get all users with premium status"
- "Add this item to the shopping cart"

## Dependencies

The Redis MCP Server uses the official `mcp/redis` Docker image, which is based on the [Redis MCP Server](https://github.com/redis/mcp-redis) project.

## Troubleshooting

### Connection Issues
- Ensure the Redis instance is running and accessible
- Check firewall settings if connecting to remote Redis instances
- Verify credentials for authenticated Redis instances

### Transport Issues
- The MCP server runs on port 8000 by default
- Check that the MCP client supports HTTP transport
- Ensure port 8000 is available and not blocked by firewall

### Cluster Mode
- When using cluster mode, ensure all cluster nodes are healthy
- Verify that the sentinel configuration is correct
