# Media Planner Workspace

A comprehensive AI-powered media planning platform with multi-tenant architecture, real-time analytics, and durable workflow orchestration.

## 🏗️ Architecture Overview

This workspace contains the complete infrastructure for a modern media planning platform:

- **FastAPI Backend** (`media-planner-infra/`): Python web service with multi-tenant architecture
- **AI-Powered Planning**: LangGraph integration for intelligent campaign optimization  
- **Durable Workflows**: Temporal.io integration for reliable background processing
- **Google API Integration**: Native support for Google Ads, Drive, and Sheets
- **Real-time Features**: WebSocket support for live updates

## 🚀 Quick Start

### Prerequisites

- Python 3.11+ with Poetry
- Temporal CLI (replaces Docker setup)
- Redis (for caching)
- PostgreSQL (optional, for production)

### Installation

1. **Install Temporal CLI** (one-time setup):
```bash
# macOS
brew install temporal

# Other platforms: https://docs.temporal.io/cli#install
```

2. **Clone and setup the backend**:
```bash
cd media-planner-infra
poetry install
```

3. **Start Temporal server**:
```bash
# Start Temporal development server (replaces Docker setup)
temporal server start-dev --ui-port 8088
```

4. **Run the application**:
```bash
# In a new terminal
cd media-planner-infra  
poetry run uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

5. **Access the services**:
- API Documentation: http://localhost:8000/api/docs
- Health Check: http://localhost:8000/health  
- Temporal Web UI: http://localhost:8088
- Temporal gRPC: localhost:7233

## 📁 Project Structure

```
media-planner-workspace/
├── media-planner-infra/           # FastAPI backend service
│   ├── app/                       # Application code
│   ├── docs/                      # Technical documentation
│   ├── scripts/                   # Development scripts
│   └── tests/                     # Test suite
├── docs/                          # Project documentation
└── README.md                      # This file
```

## 🔧 Development

### Temporal Development Server

The new Temporal CLI approach provides several advantages over Docker:

- ✅ **Native Apple Silicon Support**: No platform emulation
- ✅ **Zero Configuration**: No docker-compose complexity  
- ✅ **Instant Startup**: Seconds vs minutes
- ✅ **Built-in Persistence**: Optional SQLite with `--db-filename`
- ✅ **Official Recommendation**: Temporal's preferred local setup

```bash
# Basic development server (in-memory, clean slate each restart)
temporal server start-dev --ui-port 8088

# With persistence (retains data between restarts)  
temporal server start-dev --ui-port 8088 --db-filename temporal.db
```

### Code Quality

```bash
cd media-planner-infra
poetry run black . && poetry run isort . && poetry run flake8 . && poetry run mypy .
poetry run pytest --cov=app
```

## 📚 Documentation

- **[Infrastructure README](media-planner-infra/README.md)**: Detailed backend setup
- **[Temporal Setup](media-planner-infra/TEMPORAL_SETUP.md)**: Workflow engine configuration
- **[Project Overview](docs/project-overview.md)**: High-level architecture
- **[Media Planner Architecture](docs/media-planner-architecture.md)**: Technical design
- **[Workflow Orchestration](docs/workflow-orchestration-plan.md)**: Temporal workflows

## 🚢 Production Deployment

For production environments:
- Use Temporal Cloud or self-hosted Temporal cluster
- Configure PostgreSQL database
- Set up proper authentication and TLS
- See [Infrastructure README](media-planner-infra/README.md) for details

## 🤝 Contributing

1. Follow the code quality guidelines in [.cursor/rules/](.cursor/rules/)
2. Write tests for new features
3. Update documentation for any changes
4. Use conventional commit messages
