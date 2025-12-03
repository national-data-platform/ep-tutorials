# Getting Started with the NDP-EP REST API

This tutorial covers how to interact directly with the NDP-EP REST API using HTTP requests.

## Prerequisites

- Access to an NDP-EP API instance
- curl, Postman, or any HTTP client
- Valid authentication token (for write operations)

## Base URL

All API requests are made to your NDP-EP instance URL:

```
https://your-api-endpoint.com
```

## Authentication

Most read operations are public, but write operations require authentication using a Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" https://your-api-endpoint.com/endpoint
```

## Read Operations (No Authentication Required)

### Get System Status

Check the API status and configuration:

```bash
curl https://your-api-endpoint.com/status
```

**Response:**
```json
{
  "api_version": "0.4.0",
  "organization": "ORGANIZATION-NAME",
  "ep_name": "EP-NAME",
  "kafka_enabled": true,
  "s3_enabled": true,
  "backend_connected": true
}
```

### List Organizations

Get all available organizations:

```bash
curl https://your-api-endpoint.com/organization
```

**Response:**
```json
["org1", "org2", "org3"]
```

Filter by name:

```bash
curl "https://your-api-endpoint.com/organization?name=nasa"
```

### Search Datasets

Search for datasets by terms:

```bash
curl "https://your-api-endpoint.com/search?terms=climate"
```

Search with multiple terms:

```bash
curl "https://your-api-endpoint.com/search?terms=climate&terms=temperature"
```

**Response:**
```json
[
  {
    "id": "12345678-abcd-efgh-ijkl-1234567890ab",
    "name": "climate_dataset",
    "title": "Climate Dataset",
    "owner_org": "noaa",
    "description": "Climate data from NOAA",
    "resources": [
      {
        "id": "resource-id",
        "url": "https://example.com/data.csv",
        "name": "Main Data",
        "format": "CSV"
      }
    ]
  }
]
```

### Advanced Search (POST)

For more complex searches, use the POST endpoint:

```bash
curl -X POST https://your-api-endpoint.com/search \
  -H "Content-Type: application/json" \
  -d '{
    "dataset_name": "climate",
    "owner_org": "noaa",
    "resource_format": "CSV"
  }'
```

## Write Operations (Authentication Required)

### Create an Organization

```bash
curl -X POST https://your-api-endpoint.com/organization \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my-organization",
    "title": "My Organization",
    "description": "Organization description"
  }'
```

**Response:**
```json
{
  "id": "305284e6-6338-4e13-b39b-e6efe9f1c45a",
  "message": "Organization created successfully"
}
```

### Register a URL Resource

```bash
curl -X POST https://your-api-endpoint.com/url \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "resource_name": "my-dataset",
    "resource_title": "My Dataset",
    "owner_org": "my-organization",
    "resource_url": "https://example.com/data.csv",
    "file_type": "CSV",
    "notes": "Dataset description"
  }'
```

### Register an S3 Resource

```bash
curl -X POST https://your-api-endpoint.com/s3 \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "resource_name": "s3-dataset",
    "resource_title": "S3 Dataset",
    "owner_org": "my-organization",
    "s3_bucket": "my-bucket",
    "s3_key": "data/file.csv"
  }'
```

### Register a Service

```bash
curl -X POST https://your-api-endpoint.com/services \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "service_name": "my-api",
    "service_title": "My API Service",
    "owner_org": "services",
    "service_url": "https://api.example.com",
    "service_type": "API",
    "notes": "RESTful API service"
  }'
```

## Delete Operations

### Delete a Resource by ID

```bash
curl -X DELETE "https://your-api-endpoint.com/resource/RESOURCE_ID" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Delete a Resource by Name

```bash
curl -X DELETE "https://your-api-endpoint.com/resource?name=my-dataset" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Delete an Organization

```bash
curl -X DELETE "https://your-api-endpoint.com/organization/my-organization" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Error Handling

The API returns standard HTTP status codes:

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid or missing token |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found |
| 409 | Conflict - Duplicate resource |
| 422 | Validation Error |

**Error Response Format:**
```json
{
  "detail": "Error message describing the problem"
}
```

## API Documentation

For the complete API reference, access the OpenAPI documentation:

- **Swagger UI**: `https://your-api-endpoint.com/docs`
- **OpenAPI JSON**: `https://your-api-endpoint.com/openapi.json`

## Next Steps

- Explore the [Python tutorials](../python/) for a higher-level interface
- Check out the [complete examples](../examples/) for end-to-end workflows

## Related Resources

- [ndp-ep Python Library](https://github.com/sci-ndp/ndp-ep-py)
