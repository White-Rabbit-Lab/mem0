.PHONY: help up up-d down logs shell migrate test test-clean env ui-install ui-start ui-dev ui-build ui-dev-start

# Ports (host-side) and UI envs
OPENMEMORY_PORT ?= 8765
OPENMEMORY_UI_PORT ?= 3000
QDRANT_PORT ?= 6333

NEXT_PUBLIC_USER_ID ?= $(USER)
NEXT_PUBLIC_API_URL ?= http://localhost:$(OPENMEMORY_PORT)

# Default target
help:
	@echo "Available commands:"
	@echo "  make env          - Copy .env.example to .env"
	@echo "  make up           - Start with env + ports"
	@echo "  make up-d         - Start detached with env + ports"
	@echo "  make down         - Stop the containers (and volumes)"
	@echo "  make logs         - Tail logs"
	@echo "  make shell        - Open a shell in a container"
	@echo "  make migrate      - Run database migrations"
	@echo "  make test         - Run tests in a new container"
	@echo "  make test-clean   - Run tests and clean up volumes"
	@echo "  make ui-dev       - Run UI in dev mode"
	@echo
	@echo "Environment variables (override as needed):"
	@echo "  OPENMEMORY_PORT     (default: 8765)"
	@echo "  OPENMEMORY_UI_PORT  (default: 3000)"
	@echo "  QDRANT_PORT         (default: 6333)"
	@echo "  NEXT_PUBLIC_API_URL (default: http://localhost:$$OPENMEMORY_PORT)"

env:
	cd api && cp .env.example .env
	cd ui && cp .env.example .env

build:
	docker compose build

up:
	OPENMEMORY_PORT=$(OPENMEMORY_PORT) OPENMEMORY_UI_PORT=$(OPENMEMORY_UI_PORT) QDRANT_PORT=$(QDRANT_PORT) \
	NEXT_PUBLIC_USER_ID=$(NEXT_PUBLIC_USER_ID) NEXT_PUBLIC_API_URL=$(NEXT_PUBLIC_API_URL) \
	docker compose up

up-d:
	OPENMEMORY_PORT=$(OPENMEMORY_PORT) OPENMEMORY_UI_PORT=$(OPENMEMORY_UI_PORT) QDRANT_PORT=$(QDRANT_PORT) \
	NEXT_PUBLIC_USER_ID=$(NEXT_PUBLIC_USER_ID) NEXT_PUBLIC_API_URL=$(NEXT_PUBLIC_API_URL) \
	docker compose up -d

down:
	docker compose down -v
	rm -f api/openmemory.db

logs:
	docker compose logs -f

shell:
	docker compose exec api bash

upgrade:
	docker compose exec api alembic upgrade head

migrate:
	docker compose exec api alembic upgrade head

downgrade:
	docker compose exec api alembic downgrade -1

ui-dev:
	cd ui && NEXT_PUBLIC_USER_ID=$(USER) NEXT_PUBLIC_API_URL=$(NEXT_PUBLIC_API_URL) pnpm install && pnpm dev
