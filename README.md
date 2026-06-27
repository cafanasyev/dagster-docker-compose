# Dagster docker-compose deployment template

Local **deployment** for a standard `create-dagster` / `uv` project.
(For the dev inner loop use `dg dev` in your project, not this stack.)

## Setup

1. Create a project with the expected layout (skip if you already have one):

   ```bash
   uvx create-dagster@latest project my-project
   cd my-project
   ```

2. Add the deployment dependencies (updates `pyproject.toml` + `uv.lock`):

   ```bash
   uv add dagster-postgres dagster-docker
   ```

3. Configure this template: `cp .env.example .env` and edit the values
   (project path, module name, credentials, image name, port).

4. Build and start the stack:

   ```bash
   docker compose up --build
   ```

5. Open the UI at `http://localhost:<DAGSTER_WEBSERVER_PORT>` (default 3000).

## Files

- `docker-compose.yml` — postgres, user_code (gRPC), webserver, daemon.
- `Dockerfile_user_code` — builds your project with `uv sync --frozen`.
- `Dockerfile_dagster` — webserver + daemon image.
- `dagster.yaml` / `workspace.yaml` — Dagster instance + code location config.
- `.env.example` — required variables.