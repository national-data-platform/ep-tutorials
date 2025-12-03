# Getting Started with ndp-ep-py

This guide provides a quick introduction to the `ndp-ep-py` Python library for interacting with the NDP-EP API.

## Installation

```bash
pip install ndp-ep-py
```

## Quick Start

### Initialize the Client

```python
from ndp_ep import Client

# Using token authentication
client = Client(
    api_url="https://your-api-endpoint.com",
    token="your-api-token"
)

# Or using username/password
client = Client(
    api_url="https://your-api-endpoint.com",
    username="your-username",
    password="your-password"
)
```

### Basic Operations

```python
# List organizations
organizations = client.list_organizations()

# Search datasets
datasets = client.search_datasets(query="climate")

# Get system status
status = client.get_status()
```

## Official Documentation

For comprehensive tutorials and API reference, see the official ndp-ep-py documentation:

| Resource | Description |
|----------|-------------|
| [Getting Started Tutorial](https://github.com/sci-ndp/ndp-ep-py/blob/main/docs/source/tutorials/getting_started.ipynb) | Complete walkthrough of basic operations |
| [Bulk Resource Management](https://github.com/sci-ndp/ndp-ep-py/blob/main/docs/source/tutorials/bulk_resource_management.ipynb) | Managing multiple resources efficiently |
| [API Reference](https://github.com/sci-ndp/ndp-ep-py#api-reference) | Complete list of available methods |

## Key Features

The library provides methods for:

- **Authentication**: Token-based and username/password authentication
- **Organizations**: Create, list, and manage organizations
- **Datasets**: Search, register, update, and delete datasets
- **Resources**: Register URL, S3, and Kafka topic resources
- **S3 Operations**: Bucket management, file upload/download, presigned URLs
- **Services**: Register and manage microservices
- **System Info**: Check status, get Kafka details, Jupyter configuration

## Next Steps

After getting familiar with the basics:

1. Explore the [official tutorials](https://github.com/sci-ndp/ndp-ep-py/tree/main/docs/source/tutorials)
2. Check out the [complete examples](../examples/) in this repository
3. Learn about [S3 integration](./02-s3-integration.md) (coming soon)
4. Explore [Kafka streaming](./03-kafka-streaming.md) (coming soon)

## Related Resources

- [ndp-ep-py GitHub Repository](https://github.com/sci-ndp/ndp-ep-py)
- [NDP-EP API Documentation](https://github.com/rbardaji/ndp-ep)
