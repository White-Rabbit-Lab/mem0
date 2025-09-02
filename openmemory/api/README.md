# OpenMemory API

This directory contains the backend API for OpenMemory, built with FastAPI and SQLAlchemy. This also runs the Mem0 MCP Server that you can use with MCP clients to remember things.

## Quick Start with Docker (Recommended)

The easiest way to get started is using Docker. Make sure you have Docker and Docker Compose installed.

1. Build the containers:
```bash
make build
```

2. Create `.env` file:
```bash
make env
```

Once you run this command, edit the file `api/.env` and enter the `OPENAI_API_KEY`.

3. Start the services:
```bash
make up
```

The API will be available at `http://localhost:8765`

## Enable Neo4j Graph Store

OpenMemory can optionally use Neo4j as a graph store (in addition to the vector store). Set the following environment variables to enable it:

- `NEO4J_URL` (e.g., `neo4j://localhost:7687` or `neo4j://neo4j:7687` in Docker)
- `NEO4J_USERNAME` (e.g., `neo4j`)
- `NEO4J_PASSWORD` (e.g., `password`)
- `NEO4J_DATABASE` (default `neo4j`)
- `NEO4J_BASE_LABEL` (default `true`)

The provided Docker Compose (`openmemory/docker-compose.yml`) already includes a `neo4j` service and passes these env vars to the API container. To start everything:

```bash
docker compose up --build
```

You can also override graph settings via the persisted config by writing a `mem0.graph_store` block; it takes precedence over env defaults.

### Common Docker Commands

- View logs: `make logs`
- Open shell in container: `make shell`
- Run database migrations: `make migrate`
- Run tests: `make test`
- Run tests and clean up: `make test-clean`
- Stop containers: `make down`

## API Documentation

Once the server is running, you can access the API documentation at:
- Swagger UI: `http://localhost:8765/docs`
- ReDoc: `http://localhost:8765/redoc`

## Project Structure

- `app/`: Main application code
  - `models.py`: Database models
  - `database.py`: Database configuration
  - `routers/`: API route handlers
- `migrations/`: Database migration files
- `tests/`: Test files
- `alembic/`: Alembic migration configuration
- `main.py`: Application entry point

## Development Guidelines

- Follow PEP 8 style guide
- Use type hints
- Write tests for new features
- Update documentation when making changes
- Run migrations for database changes
