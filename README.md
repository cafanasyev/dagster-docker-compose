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

3. Add a `.dockerignore` to your project so the image build skips local junk —
   especially a stale local `.venv`, which would otherwise overwrite the one
   built inside the image and break it:

   ```bash
   cat > .dockerignore <<'EOF'
   .venv
   .git
   .gitignore
   .idea
   .dg
   .pytest_cache
   .ruff_cache
   .tmp_dagster_home_*
   .import_linter_cache
   __pycache__/
   **/__pycache__/
   *.pyc
   *.egg-info
   Makefile
   EOF
   ```

4. Configure this template: `cp .env.example .env` and edit the values
   (project path, module name, credentials, image name, port).

5. Build and start the stack:

   ```bash
   docker compose up --build
   ```

6. Open the UI at `http://localhost:<DAGSTER_WEBSERVER_PORT>` (default 3000).

7. To re-build user code container run:

   ```bash
   docker compose up -d --build user_code
   ```

## Files

- `docker-compose.yml` — postgres, user_code (gRPC), webserver, daemon.
- `Dockerfile_user_code` — builds your project; installs deps and source in separate layers so code edits don't trigger a full dependency reinstall.
- `Dockerfile_dagster` — webserver + daemon image.
- `dagster.yaml` / `workspace.yaml` — Dagster instance + code location config.
- `.env.example` — required variables.