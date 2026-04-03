# Kiavi Senior DevOps Take-Home Challenge - Dockerized Spina CMS

**Goal**: Provide a working local developer experience for the Spina CMS engine on a Ruby on Rails application.

**Status**: Fully functional and tested.

## Quick Start

```powershell
# 1. Build the Docker image
docker compose build

# 2. Start Postgres in background
docker compose up -d db

# Wait 10-15 seconds

# 3. Create database
docker compose run --rm web rails db:create

# 4. Install Active Storage
docker compose run --rm web rails active_storage:install

# 5. Install Spina (interactive)
docker compose run -it --rm web rails spina:install

# 6. Start the application
docker compose up
