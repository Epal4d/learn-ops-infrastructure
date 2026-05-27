# Tech Stack (AI)

## 1. Run Questions

### 1a. Config Files

| Config File | Location | Config Value | What it's for | How it's used |
|---|---|---|---|---|
| learn-ops-api.yaml | learn-ops-api/config/learn-ops-api.yaml | DATABASE_URL = ${learnops.DATABASE_URL} | Database connection URL for the app | Injected as an environment variable at runtime (DigitalOcean app config) so Django connects to Postgres |
| learn-ops-api.yaml | learn-ops-api/config/learn-ops-api.yaml | http_port = 8080 | HTTP port mapped by the DigitalOcean service | Used by platform to route incoming HTTP traffic to the service |
| learn-ops-api.yaml | learn-ops-api/config/learn-ops-api.yaml | github.repo = stevebrownlee/learn-ops-api | GitHub repo used for automated deploys | DigitalOcean/GitHub integration pulls code from this repo/branch for deploys |
| docker-compose.yml | learn-ops-infrastructure/docker-compose.yml | database.image = postgres:16 | Postgres image and version for local/infrastructure DB service | Docker Compose starts this image as the `database` service for local dev/tests |
| docker-compose.yml | learn-ops-infrastructure/docker-compose.yml | database.ports = "5432:5432" | Host<->container port mapping for Postgres | Exposes Postgres to the host for local connections and tooling |
| docker-compose.yml | learn-ops-infrastructure/docker-compose.yml | api.env_file = "../learn-ops-api/.env" | Path to environment variables for the API service | Docker Compose loads these env vars into the API container at start |
| .env (api) | learn-ops-api/.env | LEARN_OPS_DJANGO_SECRET_KEY = <REDACTED> | Django SECRET_KEY used by the application | Read by Django settings to sign cookies/secure sessions at runtime |
| .env (api) | learn-ops-api/.env | LEARN_OPS_DB = learningplatform | Database name used by the application | Used by Django/DB client to select the database name when connecting |
| .env (api) | learn-ops-api/.env | LEARN_OPS_PASSWORD = <REDACTED> | Database password for the DB user | Loaded into container env and used by Django to authenticate to Postgres |

### 1b. How to Start It

Start the application from the `learn-ops-infrastructure` folder using Docker Compose.

Primary command:

```bash
cd learn-ops-infrastructure
make up
```

This runs `docker compose up --build -d` using `learn-ops-infrastructure/docker-compose.yml`.

Key startup behavior from the config files:
- `docker-compose.yml` loads API environment variables from `../learn-ops-api/.env` and client variables from `../learn-ops-client/.env`.
- The `api` service is built from `../learn-ops-api/Dockerfile` and starts via `entrypoint.sh`, which runs Django migrations and then `python3 manage.py runserver 0.0.0.0:8000`.
- The `client` service uses `REACT_APP_API_URI=http://localhost:8000` from `learn-ops-client/.env`, so the React frontend connects to the local API.

Alternative commands:

```bash
cd learn-ops-infrastructure
make up-api       # Start only the API service
make up-client-api # Start the API and client services only
```

If `make` is unavailable, use:

```bash
cd learn-ops-infrastructure
docker compose up --build -d
```

### 1c. Where to Access It

| Service | Port | URL |
|---|---|---|
| API (Django dev server) | 8000 | http://localhost:8000 |
| API debug (debugpy) | 5678 | localhost:5678 (debugger port) |
| Client (React) | 3000 | http://localhost:3000 |
| Prometheus | 9090 | http://localhost:9090 |
| Grafana (host) | 3001 | http://localhost:3001 |
| Postgres (DB) | 5432 | postgres://learnops:<REDACTED>@localhost:5432/learningplatform |
| Postgres Exporter | 9187 | http://localhost:9187 |
| Valkey (message broker) | 6379 | redis://localhost:6379/0 (Valkey port) |

### 1d. Service Dependencies

| Service | Depends On | Why |
|---|---|---|
| | | |
| | | |
| | | |

### 1e. Main Entry Points

| Service | Startup File | Routes / URL Config File |
|---|---|---|
| | | |
| | | |
| | | |

## 2. Services

| Service Name | Tech Stack (including version) | Purpose |
|---|---|---|
| | | |

## 3. System Overview