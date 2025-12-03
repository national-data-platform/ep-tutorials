# NDP-EP Tutorials

A comprehensive library of tutorials and demos for the National Data Platform - EndPoint (NDP-EP).

## Overview

NDP-EP is a data management platform that enables researchers and developers to work with datasets, streaming data, and cloud resources through a unified interface. This repository provides hands-on tutorials covering all aspects of the platform.

## What is NDP-EP?

NDP-EP consists of three main components:

- **REST API**: Backend service for managing datasets, organizations, resources (S3, Kafka topics, URLs), and services across CKAN environments
- **Python Library** ([ndp-ep](https://github.com/sci-ndp/ndp-ep-py)): A Python client providing a simplified interface for API interactions
- **Frontend** ([ep-frontend](https://github.com/national-data-platform/ep-frontend)): React-based administrative console for managing NDP-EP instances

## Repository Structure

```
ep-tutorials/
├── api/          # Direct REST API tutorials
├── python/       # Python library (ndp-ep) tutorials
├── frontend/     # Dashboard/UI tutorials
└── examples/     # Complete demo applications
```

## Getting Started

### Prerequisites

- Python 3.8+ (for Python tutorials)
- Access to an NDP-EP API instance
- ndp-ep library: `pip install ndp-ep`

### Quick Links

| Section | Description |
|---------|-------------|
| [API Tutorials](./api/) | Learn to interact directly with the REST API |
| [Python Tutorials](./python/) | Use the ndp-ep library for programmatic access |
| [Frontend Tutorials](./frontend/) | Navigate and use the administrative console |
| [Examples](./examples/) | End-to-end demo applications |

## Related Resources

- [ndp-ep Python Library](https://github.com/sci-ndp/ndp-ep-py)
- [EP Frontend](https://github.com/national-data-platform/ep-frontend)
- [National Data Platform](https://nationaldataplatform.org/)

## Contributing

Contributions are welcome! When adding new tutorials:

1. Place tutorials in the appropriate folder based on topic
2. Include a clear title and objective
3. List prerequisites
4. Provide step-by-step instructions with code examples
5. Show expected outputs/results
6. Add troubleshooting tips when applicable

## License

MIT License
