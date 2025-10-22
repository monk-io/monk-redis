# monk-redis

Monk templates for Redis deployments and Redis MCP Server integration.

## Available Templates

### Redis
- **`redis/redis`** - Single Redis instance
- **`redis-mcp/mcp`** - Redis MCP Server for AI-driven data management

### Redis Cluster
- **`redis-cluster/stack`** - Redis cluster with leader-follower replication and sentinel
- **`redis-cluster/redis-leader`** - Redis leader node
- **`redis-cluster/redis-follower-1`** - Redis follower node 1
- **`redis-cluster/redis-follower-2`** - Redis follower node 2
- **`redis-cluster/sentinel`** - Redis sentinel for high availability

### Redis Cluster with HAProxy
- **`redis-cluster-haproxy/stack`** - Redis cluster with HAProxy load balancer

### Redis MCP Server
The Redis MCP Server provides a natural language interface for AI applications to interact with Redis data.

#### Main Template
- **`redis-mcp/mcp`** - Flexible Redis MCP Server with HTTP transport

#### Example Stacks
- **`example.simple.yaml`** - Simple Redis instance with MCP server
- **`example.cluster.yaml`** - Redis cluster with sentinel and MCP server
- **`example.cloud.yaml`** - Redis Cloud with MCP server

## Quick Start

### Basic Redis
```bash
monk run redis/redis
```

### Redis MCP Server
```bash
# Run with default settings
monk run redis-mcp/mcp

# Run with custom Redis connection
monk run redis-mcp/mcp --set redis_host=your-redis-host --set redis_port=6379
```

### Redis Cluster
```bash
monk run redis-cluster/stack
```

### Example Stacks
```bash
# Simple Redis + MCP
monk load redis-mcp/example.simple.yaml
monk run redis-mcp-simple/redis-stack

# Cluster + MCP
monk load redis-mcp/example.cluster.yaml
monk run redis-mcp-cluster/cluster-stack
```

## Features

- **Single Redis Instance** - Basic Redis deployment
- **Redis Cluster** - High availability with leader-follower replication and sentinel
- **Redis MCP Server** - AI-powered natural language interface for Redis
- **Multiple Deployment Options** - Local, cluster, and cloud configurations
- **HTTP Transport** - MCP server accessible on port 8000 for external tools

## Integration

The Redis MCP Server integrates with:
- **Cursor/VS Code** - Via MCP extension
- **Claude Desktop** - Via MCP configuration
- **GitHub Copilot** - Via HTTP transport
- **OpenAI Agents SDK** - Via HTTP transport

## Documentation

- [Redis MCP Server Documentation](redis-mcp/README.md)
- [Monk Documentation](https://docs.monk.io/)

