# Redis RIOT Data Replicator

Production-ready [Redis RIOT](https://redis.io/docs/latest/integrate/riot/) template for Redis-to-Redis data replication with built-in test environment.

## Overview

RIOT (Redis Input/Output Tools) is a powerful data replication tool for Redis instances. This template provides a complete Redis replication demonstration stack with:

- **Source Redis**: Redis instance with test data (port 6380)
- **Target Redis**: Clean Redis instance for replication (port 6379)  
- **RIOT Replicator**: Automated data migration between instances

This template is designed for testing and demonstrating Redis data replication capabilities.

## Components

### Redis Source (redis-target)
- **Port**: 6380
- **Purpose**: Contains pre-populated test data
- **Test Data**: 
  - `user:1` hash with user information
  - `product:1` hash with product details
- **Configuration**: No authentication, optimized for I/O threads

### Redis Target (redis)  
- **Port**: 6379
- **Purpose**: Clean target for data replication
- **Configuration**: No authentication, optimized for I/O threads

### RIOT Replicator
- **Image**: `riotx/riot:latest`
- **Function**: Replicates data from source (6380) to target (6379)
- **Batch Size**: 50 items per batch (configurable)

## Configuration Variables

- `batch_size`: Number of items to process in each batch (default: 50)

## Usage

### Deploy Complete Stack

Deploy the complete Redis replication demonstration:

```bash
monk run monk-redis/redis-stack
```

This deploys:
- **Source Redis** (redis-target) on port 6380 with test data
- **Target Redis** (redis) on port 6379 (empty)
- **RIOT replicator** that migrates data from source to target

### Deploy Individual Components

Run only the replicator (requires external Redis instances):

```bash
monk run monk-redis/riot-replicator
```

### Test the Replication

After deployment, verify the data migration:

```bash
# Check source data (port 6380)
redis-cli -p 6380 HGETALL user:1
redis-cli -p 6380 HGETALL product:1

# Check replicated data (port 6379)  
redis-cli -p 6379 HGETALL user:1
redis-cli -p 6379 HGETALL product:1
```

Expected output:
```
# user:1 hash
1) "id"
2) "1"
3) "name" 
4) "John Doe"
5) "email"
6) "john@example.com"
7) "age"
8) "30"

# product:1 hash
1) "id"
2) "1"
3) "name"
4) "Laptop"
5) "price"
6) "999.99"
7) "category"
8) "Electronics"
```

## Features

### Template Features
- **Ready-to-Use**: Complete demonstration environment with test data
- **No Authentication**: Simplified setup for testing and development
- **Optimized Configuration**: I/O threads enabled for better performance
- **Automatic Dependencies**: RIOT waits for Redis instances to be ready
- **Batch Processing**: Configurable batch size for optimal performance

### RIOT Capabilities
- **Redis-to-Redis Replication**: Efficient data migration between instances
- **Batch Processing**: Processes data in configurable batches (default: 50)
- **Dependency Management**: Automatically waits for source and target to be ready
- **Container-based**: Uses official RIOT Docker image

## Architecture

```
┌─────────────────┐    RIOT     ┌─────────────────┐
│   redis-target  │ ──────────► │      redis      │
│   (port 6380)   │ Replicator  │   (port 6379)   │
│   + test data   │             │     (empty)     │
└─────────────────┘             └─────────────────┘
```

## Troubleshooting

1. **Connection Issues**: Ensure both Redis instances are running and accessible
2. **Data Not Replicated**: Check RIOT container logs for error messages
3. **Port Conflicts**: Make sure ports 6379 and 6380 are available
4. **Container Issues**: Verify Docker/container runtime is working properly

## Customization

You can customize the template by modifying:
- `batch_size`: Adjust replication batch size for performance
- Redis configuration: Modify I/O threads, memory settings
- Test data: Change the sample data created in redis-target

For more information about RIOT capabilities, visit the [official documentation](https://redis.io/docs/latest/integrate/riot/).

