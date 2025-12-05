# NDP-EP Python Library: Complete API Reference

This guide covers all functions available in the `ndp-ep` Python library, organized by category.

## Table of Contents

1. [Connection & Authentication](#1-connection--authentication)
2. [Dataset Registration](#2-dataset-registration)
3. [Dataset Search & Management](#3-dataset-search--management)
4. [Organization Management](#4-organization-management)
5. [Service Management](#5-service-management)
6. [S3 Storage Operations](#6-s3-storage-operations)
7. [Kafka Integration](#7-kafka-integration)
8. [Pelican Federation](#8-pelican-federation)
9. [Jupyter Integration](#9-jupyter-integration)

---

## 1. Connection & Authentication

### Client Initialization

```python
from ndp_ep import Client

# With token authentication
client = Client(base_url="http://localhost:8002", token="your-token")

# With password authentication
client = Client(base_url="http://localhost:8002")
client.get_token(username="user", password="pass")
```

### get_system_status()

Check the system status and available services.

```python
status = client.get_system_status()
print(status)
```

**Returns:**
```python
{
    'api_version': '0.4.0',
    'ep_name': 'EP-DEMO',
    'organization': 'ORGANIZATION-DEMO',
    'backend_connected': True,      # Local catalog (CKAN or other)
    's3_connected': True,           # MinIO/S3 storage
    'kafka_enabled': True,          # Kafka streaming
    'jupyterlab_enabled': True,     # JupyterLab integration
    'is_public': True
}
```

### get_system_metrics()

Retrieve system metrics.

```python
metrics = client.get_system_metrics()
print(metrics)
```

### get_user_info()

Get information about the current authenticated user.

```python
user = client.get_user_info()
print(user)
```

### get_token(username, password)

Obtain an authentication token using username and password.

```python
client = Client(base_url="http://localhost:8002")
client.get_token(username="admin", password="secret")
# Token is now stored in the client
```

---

## 2. Dataset Registration

### register_general_dataset(data, server='local')

Register a new general dataset.

```python
result = client.register_general_dataset({
    "resource_name": "my-dataset",
    "resource_title": "My Dataset Title",
    "owner_org": "my-organization",
    "notes": "Description of the dataset"
})
print(result)
```

**Parameters:**
- `data`: Dictionary with dataset metadata
- `server`: `'local'` (default) or `'global'`

### register_url(data, server='local')

Register a URL resource pointing to external data.

```python
result = client.register_url({
    "resource_name": "external-data",
    "resource_title": "External Data Source",
    "owner_org": "my-organization",
    "resource_url": "https://example.com/data.csv",
    "file_type": "CSV",
    "notes": "Data from external source"
})
print(result)
```

### register_s3_link(data, server='local')

Register a resource linked to S3 storage.

```python
result = client.register_s3_link({
    "resource_name": "s3-data",
    "resource_title": "S3 Stored Data",
    "owner_org": "my-organization",
    "s3_bucket": "my-bucket",
    "s3_key": "path/to/file.nc",
    "notes": "Data stored in S3"
})
print(result)
```

### register_kafka_topic(data, server='local')

Register a Kafka topic as a streaming data source.

```python
result = client.register_kafka_topic({
    "resource_name": "sensor-stream",
    "resource_title": "Real-time Sensor Data",
    "owner_org": "my-organization",
    "topic": "sensors.temperature",
    "notes": "Live temperature readings"
})
print(result)
```

---

## 3. Dataset Search & Management

### search_datasets(terms, keys=None, server='global')

Search for datasets by terms.

```python
# Simple search
results = client.search_datasets(terms=["temperature"])

# Search with specific keys
results = client.search_datasets(
    terms=["utah", "satellite"],
    keys=["title", "notes"],
    server="local"
)

for ds in results:
    print(f"{ds['name']}: {ds.get('title', 'N/A')}")
```

**Parameters:**
- `terms`: List of search terms
- `keys`: Optional list of fields to search in
- `server`: `'global'` (default) or `'local'`

### advanced_search(search_data)

Perform advanced search with complex queries.

```python
results = client.advanced_search({
    "query": "temperature AND utah",
    "filters": {
        "organization": "noaa"
    },
    "limit": 10,
    "offset": 0
})
```

### update_general_dataset(dataset_id, data, server='local')

Full update of a dataset (replaces all fields).

```python
result = client.update_general_dataset(
    dataset_id="my-dataset-id",
    data={
        "resource_name": "my-dataset",
        "resource_title": "Updated Title",
        "owner_org": "my-organization",
        "notes": "Updated description"
    }
)
```

### patch_general_dataset(dataset_id, data, server='local')

Partial update of a dataset (only specified fields).

```python
result = client.patch_general_dataset(
    dataset_id="my-dataset-id",
    data={
        "notes": "Only updating the description"
    }
)
```

### update_url_resource(resource_id, data, server='local')

Update a URL resource.

```python
result = client.update_url_resource(
    resource_id="resource-id",
    data={
        "resource_url": "https://new-url.com/data.csv"
    }
)
```

### update_s3_resource(resource_id, data, server='local')

Update an S3 resource.

```python
result = client.update_s3_resource(
    resource_id="resource-id",
    data={
        "s3_key": "new/path/to/file.nc"
    }
)
```

### patch_s3_resource(resource_id, data, server='local')

Partial update of an S3 resource.

```python
result = client.patch_s3_resource(
    resource_id="resource-id",
    data={
        "notes": "Updated notes only"
    }
)
```

### patch_dataset_resource(dataset_id, resource_id, data, server='local')

Partial update of a specific resource within a dataset.

```python
result = client.patch_dataset_resource(
    dataset_id="dataset-id",
    resource_id="resource-id",
    data={
        "description": "Updated resource description"
    }
)
```

### delete_resource_by_id(resource_id, server='local')

Delete a resource by its ID.

```python
result = client.delete_resource_by_id(resource_id="resource-id")
```

### delete_resource_by_name(resource_name, server='local')

Delete a resource by its name.

```python
result = client.delete_resource_by_name(resource_name="my-dataset")
```

### delete_dataset_resource(dataset_id, resource_id, server='local')

Delete a specific resource from a dataset.

```python
result = client.delete_dataset_resource(
    dataset_id="dataset-id",
    resource_id="resource-id"
)
```

---

## 4. Organization Management

### list_organizations(name=None, server='global')

List all organizations.

```python
# List all organizations
orgs = client.list_organizations()
for org in orgs:
    print(org)

# Filter by name
orgs = client.list_organizations(name="noaa")

# From local server
orgs = client.list_organizations(server="local")
```

### register_organization(data, server='local')

Register a new organization.

```python
result = client.register_organization({
    "name": "my-org",
    "title": "My Organization",
    "description": "Organization description"
})
```

### delete_organization(organization_name, server='local')

Delete an organization.

```python
result = client.delete_organization(organization_name="my-org")
```

---

## 5. Service Management

### register_service(data, server='local')

Register a new service.

```python
result = client.register_service({
    "name": "data-processing-api",
    "title": "Data Processing API",
    "url": "https://api.example.com",
    "description": "API for processing datasets"
})
```

### update_service(service_id, data, server='local')

Full update of a service.

```python
result = client.update_service(
    service_id="service-id",
    data={
        "name": "data-processing-api",
        "title": "Updated API Title",
        "url": "https://new-api.example.com"
    }
)
```

### patch_service(service_id, data, server='local')

Partial update of a service.

```python
result = client.patch_service(
    service_id="service-id",
    data={
        "description": "Updated description only"
    }
)
```

---

## 6. S3 Storage Operations

### list_buckets()

List all available S3 buckets.

```python
result = client.list_buckets()
for bucket in result['buckets']:
    print(f"{bucket['name']} - Created: {bucket.get('creation_date')}")
```

### create_bucket(bucket_name)

Create a new S3 bucket.

```python
result = client.create_bucket("my-new-bucket")
print(result)
```

### get_bucket_info(bucket_name)

Get information about a specific bucket.

```python
info = client.get_bucket_info("my-bucket")
print(info)
```

### delete_bucket(bucket_name)

Delete an S3 bucket (must be empty).

```python
result = client.delete_bucket("my-bucket")
```

### list_objects(bucket_name, prefix=None)

List objects in a bucket.

```python
# List all objects
result = client.list_objects("my-bucket")
for obj in result['objects']:
    print(f"{obj['key']} - {obj['size']} bytes")

# List with prefix filter
result = client.list_objects("my-bucket", prefix="data/2024/")
```

### upload_object(bucket_name, object_key, file_data, content_type=None)

Upload an object to S3.

```python
# Upload from bytes
with open("local_file.csv", "rb") as f:
    data = f.read()

result = client.upload_object(
    bucket_name="my-bucket",
    object_key="path/to/file.csv",
    file_data=data,
    content_type="text/csv"
)
print(result)
```

### download_object(bucket_name, object_key)

Download an object from S3.

```python
data = client.download_object(
    bucket_name="my-bucket",
    object_key="path/to/file.csv"
)

# Save to local file
with open("downloaded_file.csv", "wb") as f:
    f.write(data)
```

### get_object_metadata(bucket_name, object_key)

Get metadata for an S3 object.

```python
metadata = client.get_object_metadata(
    bucket_name="my-bucket",
    object_key="path/to/file.csv"
)
print(metadata)
```

### delete_object(bucket_name, object_key)

Delete an object from S3.

```python
result = client.delete_object(
    bucket_name="my-bucket",
    object_key="path/to/file.csv"
)
```

### generate_presigned_download_url(bucket_name, object_key, expiration=None)

Generate a temporary URL for downloading an object.

```python
result = client.generate_presigned_download_url(
    bucket_name="my-bucket",
    object_key="path/to/file.csv",
    expiration=3600  # 1 hour in seconds
)
print(f"Download URL: {result['url']}")
```

### generate_presigned_upload_url(bucket_name, object_key, expiration=None)

Generate a temporary URL for uploading an object.

```python
result = client.generate_presigned_upload_url(
    bucket_name="my-bucket",
    object_key="uploads/new-file.csv",
    expiration=3600
)
print(f"Upload URL: {result['url']}")

# Use the URL to upload directly
import requests
with open("local_file.csv", "rb") as f:
    requests.put(result['url'], data=f)
```

---

## 7. Kafka Integration

### get_kafka_details()

Get Kafka connection details.

```python
kafka = client.get_kafka_details()
print(f"Host: {kafka['host']}")
print(f"Port: {kafka['port']}")
```

### register_kafka_topic(data, server='local')

Register a Kafka topic (see [Dataset Registration](#2-dataset-registration)).

### update_kafka_topic(dataset_id, data, server='local')

Update a Kafka topic resource.

```python
result = client.update_kafka_topic(
    dataset_id="topic-id",
    data={
        "topic": "new.topic.name",
        "notes": "Updated topic"
    }
)
```

---

## 8. Pelican Federation

Pelican is a federated data distribution system. These functions allow interaction with Pelican namespaces.

### list_federations()

List available Pelican federations.

```python
federations = client.list_federations()
print(federations)
```

### browse_pelican(path, federation='osdf', detail=False)

Browse files in a Pelican namespace.

```python
# Browse a directory
contents = client.browse_pelican(
    path="/osg-htc/public/",
    federation="osdf"
)
for item in contents:
    print(item)

# With detailed information
contents = client.browse_pelican(
    path="/osg-htc/public/",
    federation="osdf",
    detail=True
)
```

### get_pelican_info(path, federation='osdf')

Get metadata for a file without downloading.

```python
info = client.get_pelican_info(
    path="/osg-htc/public/data/file.csv",
    federation="osdf"
)
print(f"Size: {info['size']}")
print(f"Modified: {info['modified']}")
```

### download_pelican(path, federation='osdf', stream=False)

Download a file from Pelican.

```python
# Download entire file
data = client.download_pelican(
    path="/osg-htc/public/data/file.csv",
    federation="osdf"
)

# Stream large files
for chunk in client.download_pelican(
    path="/osg-htc/public/data/large_file.nc",
    federation="osdf",
    stream=True
):
    process(chunk)
```

### import_pelican_metadata(pelican_url, package_id, resource_name=None, resource_description=None)

Import a Pelican file as a resource in the local catalog.

```python
result = client.import_pelican_metadata(
    pelican_url="pelican://osg-htc/public/data/file.csv",
    package_id="my-dataset-id",
    resource_name="pelican-data",
    resource_description="Data imported from Pelican"
)
```

---

## 9. Jupyter Integration

### get_jupyter_details()

Get JupyterLab connection details.

```python
jupyter = client.get_jupyter_details()
print(f"URL: {jupyter['url']}")
```

---

## Error Handling

All methods may raise exceptions on errors. Use try/except for robust code:

```python
try:
    result = client.create_bucket("my-bucket")
    print(f"Created: {result}")
except Exception as e:
    print(f"Error: {e}")
```

## Common Patterns

### Check Services Before Operations

```python
status = client.get_system_status()

if not status.get('backend_connected'):
    raise RuntimeError("Local catalog not available")

if not status.get('s3_connected'):
    raise RuntimeError("S3 storage not available")

# Proceed with operations...
```

### Upload and Register Data

```python
# 1. Create bucket
client.create_bucket("data-bucket")

# 2. Upload file
with open("data.csv", "rb") as f:
    client.upload_object(
        bucket_name="data-bucket",
        object_key="datasets/data.csv",
        file_data=f.read(),
        content_type="text/csv"
    )

# 3. Register in catalog
client.register_s3_link({
    "resource_name": "my-csv-data",
    "resource_title": "My CSV Dataset",
    "owner_org": "my-org",
    "s3_bucket": "data-bucket",
    "s3_key": "datasets/data.csv"
})
```

### Search and Download

```python
# Search for datasets
results = client.search_datasets(terms=["temperature"])

# Get first result with S3 data
for ds in results:
    if 's3_bucket' in ds and 's3_key' in ds:
        data = client.download_object(
            bucket_name=ds['s3_bucket'],
            object_key=ds['s3_key']
        )
        break
```
