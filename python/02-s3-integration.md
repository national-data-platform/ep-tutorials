# S3 Integration with ndp-ep

This tutorial covers how to work with S3-compatible storage using the `ndp-ep` Python library.

## Prerequisites

- `ndp-ep` library installed (`pip install ndp-ep`)
- Access to an NDP-EP API instance with S3 enabled
- Valid authentication token

## Setup

```python
from ndp_ep import Client

# Initialize client with authentication
client = Client(
    base_url="https://your-api-endpoint.com",
    token="your-api-token"
)
```

## Bucket Operations

### List Buckets

```python
# Get all available buckets
result = client.list_buckets()
buckets = result['buckets']

for bucket in buckets:
    print(f"Bucket: {bucket['name']}")
    print(f"  Created: {bucket['creation_date']}")
```

### Create a Bucket

```python
# Create a new bucket
result = client.create_bucket('my-data-bucket')
print(result['message'])  # "Bucket 'my-data-bucket' created successfully"
```

### Get Bucket Info

```python
# Get information about a specific bucket
info = client.get_bucket_info('my-data-bucket')
print(info)
```

### Delete a Bucket

```python
# Delete an empty bucket
result = client.delete_bucket('my-data-bucket')
```

> **Note**: A bucket must be empty before it can be deleted.

## Object Operations

### Upload an Object

```python
# Upload from bytes
data = b'Hello, World!'
result = client.upload_object(
    bucket_name='my-data-bucket',
    object_key='hello.txt',
    file_data=data,
    content_type='text/plain'
)
print(f"Uploaded: {result['key']} ({result['size']} bytes)")
```

```python
# Upload a file from disk
with open('local_file.csv', 'rb') as f:
    file_data = f.read()

result = client.upload_object(
    bucket_name='my-data-bucket',
    object_key='data/my_file.csv',
    file_data=file_data,
    content_type='text/csv'
)
```

### List Objects in a Bucket

```python
# List all objects
result = client.list_objects('my-data-bucket')
objects = result['objects']

for obj in objects:
    print(f"{obj['key']} - {obj['size']} bytes")
```

```python
# List objects with a prefix (like a folder)
result = client.list_objects('my-data-bucket', prefix='data/')
```

### Download an Object

```python
# Download object as bytes
data = client.download_object('my-data-bucket', 'hello.txt')
print(data.decode('utf-8'))  # "Hello, World!"
```

```python
# Download and save to file
data = client.download_object('my-data-bucket', 'data/my_file.csv')
with open('downloaded_file.csv', 'wb') as f:
    f.write(data)
```

### Get Object Metadata

```python
# Get detailed metadata about an object
metadata = client.get_object_metadata('my-data-bucket', 'hello.txt')
print(f"Size: {metadata['size']} bytes")
print(f"Content-Type: {metadata['content_type']}")
print(f"Last Modified: {metadata['last_modified']}")
print(f"ETag: {metadata['etag']}")
```

### Delete an Object

```python
# Delete a single object
result = client.delete_object('my-data-bucket', 'hello.txt')
```

## Presigned URLs

Presigned URLs allow temporary access to objects without sharing credentials.

### Generate Download URL

```python
# Generate a presigned URL for downloading (valid for 1 hour)
result = client.generate_presigned_download_url(
    bucket_name='my-data-bucket',
    object_key='hello.txt',
    expiration=3600  # seconds
)
print(f"Download URL: {result['url']}")
print(f"Expires in: {result['expires_in']} seconds")
```

### Generate Upload URL

```python
# Generate a presigned URL for uploading
result = client.generate_presigned_upload_url(
    bucket_name='my-data-bucket',
    object_key='uploads/new_file.txt',
    expiration=3600
)
print(f"Upload URL: {result['url']}")
```

## Complete Example

Here's a complete workflow example:

```python
from ndp_ep import Client

# Initialize
client = Client(base_url="https://your-api-endpoint.com", token="your-token")

# Create a bucket for our project
client.create_bucket('project-data')

# Upload some data
dataset = b'id,name,value\n1,alpha,100\n2,beta,200\n3,gamma,300'
client.upload_object(
    bucket_name='project-data',
    object_key='datasets/sample.csv',
    file_data=dataset,
    content_type='text/csv'
)

# List what we have
result = client.list_objects('project-data')
for obj in result['objects']:
    print(f"  {obj['key']}")

# Generate a shareable link
url_result = client.generate_presigned_download_url(
    bucket_name='project-data',
    object_key='datasets/sample.csv',
    expiration=86400  # 24 hours
)
print(f"Share this link: {url_result['url']}")

# Cleanup when done
client.delete_object('project-data', 'datasets/sample.csv')
client.delete_bucket('project-data')
```

## Next Steps

- Learn about [Kafka streaming](./03-kafka-streaming.md) for real-time data
- Check out the [complete examples](../examples/) for end-to-end workflows

## Related Resources

- [ndp-ep-py GitHub Repository](https://github.com/sci-ndp/ndp-ep-py)
