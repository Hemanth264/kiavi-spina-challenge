# Rails + Spina CMS — Docker Dev Environment

## Setup Instructions

# 1. Build the Docker image
docker compose build

# 2. Start Postgres in background
docker compose up -d db

# Wait 10-15 seconds for Postgres to be ready

# 3. Create database
docker compose run --rm web rails db:create

# 4. Install Active Storage
docker compose run --rm web rails active_storage:install

# 5. Install Spina (interactive — follow the prompts)
docker compose run -it --rm web rails spina:install

# 6. Start the application
docker compose up

## Key Design Choices

**gem_cache named volume** — Caching the bundle in a named volume means `bundle install` 
is fast on subsequent runs without rebuilding the entire image every time.

**Bind mount for app code** — Live file changes reflect inside the container instantly.

**Postgres in trust mode** — No passwords needed locally. Keeps onboarding simple.

**HTTP only, no TLS** — Permitted for local development per the spec.

**Added `libyaml-dev` to Dockerfile** — Fixes a `psych` gem compilation failure during build.

**Removed auto-generated CI workflow** — It was failing on irrelevant defaults.

## Challenges Faced

- Windows Docker volume sync delays caused some confusion early on
- A stray `end` in the Gemfile caused the initial build to fail
- The `psych` gem wouldn't compile until `libyaml-dev` was added to the Dockerfile

## Known Caveats

- First run requires the interactive `spina:install` step — can't be skipped
- On Windows, file changes may take a few seconds to appear inside the container
- If the DB fails to connect: `docker compose down -v` then retry from step 2
- This is a local dev environment only — no production hardening included

## References
- Spina CMS: https://github.com/SpinaCMS/Spina
