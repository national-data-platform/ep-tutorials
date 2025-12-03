# EP-Tutorials Repository Guidelines

## Purpose
This repository serves as a library of tutorials and demos for NDP-EP (National Data Platform - EndPoint), covering:
- Direct REST API usage (ndp-ep API)
- Python library integration (ndp-ep-py)
- Frontend/Dashboard usage (ep-frontend)

## What is NDP-EP?
NDP-EP is part of the National Data Platform ecosystem. It provides:
- **API**: RESTful backend for managing datasets, organizations, resources (S3, Kafka topics, URLs), and services across CKAN environments
- **Python Client** (ndp-ep-py): Simplified interface for API interactions with token/password auth, dataset search, S3 operations, Kafka integration
- **Frontend**: React-based admin console for managing NDP-EP API instances, datasets, organizations, and monitoring system health

### Key Resources
- API: https://github.com/rbardaji/ndp-factory-api and https://github.com/rbardaji/ndp-ep
- Python Library: https://github.com/sci-ndp/ndp-ep-py
- Frontend: https://github.com/national-data-platform/ep-frontend

## Language Rules
- **Tutorials and documentation**: Always in English
- **Code comments**: Always in English
- **Commit messages**: Always in English

## Git Commit Rules
**CRITICAL**: Never include any reference to AI assistance in:
- Commit messages
- Code comments
- Documentation
- PR descriptions

Write commits as if authored entirely by a human developer. Use standard commit messages without any AI attribution.

## Tutorial Structure
When creating tutorials, follow this format:
1. Clear title and objective
2. Prerequisites section
3. Step-by-step instructions
4. Code examples with explanations
5. Expected outputs/results
6. Troubleshooting section (when applicable)

## Content Categories
- `api/` - Direct REST API tutorials
- `python/` - Python library (ndp-ep-py) tutorials
- `frontend/` - Dashboard/UI tutorials
- `examples/` - Complete demo applications
