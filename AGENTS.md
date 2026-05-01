# AGENTS.md

## Stack
- **`ops-node-api/`** - Laravel 13 (PHP 8.3), MySQL, Redis, Horizon, Sanctum, Spatie Laravel Data
- **`ops-node-front/`** - Vue 3, Vite, Pinia, TypeScript (Node ^20.19.0 || >=22.12.0)

## Commands

```bash
# Docker (from root)
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build  # Dev
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build # Prod

# Backend (from ops-node-api/)
composer run dev          # serve + queue:listen (high,default,employee-sync) + pail + vite
composer run test         # config:clear + phpunit
php artisan test --filter=TestName  # single test
./vendor/bin/pint        # lint

# Frontend (from ops-node-front/)
npm run dev              # vite
npm run build            # type-check + vite build
npm run type-check       # vue-tsc

# Setup
composer run setup       # install + migrate + npm build
```

## Docker Services (dev)
| Service | Port | Notes |
|---------|------|-------|
| Nginx | 8000 | API entry |
| Frontend | 5173 | Vite HMR |
| MySQL | 3306 | db=ops_node, user=ops_node |
| Redis | 6379 | phpredis client |
| RedisInsight | 5540 | |
| Horizon worker | - | `php artisan horizon` |

## Tests
- SQLite `:memory:` (phpunit.xml)
- SESSION/QUEUE/CACHE overridden to `array`/`sync`
- `DB_DATABASE=:memory:`, `DB_CONNECTION=sqlite`

## Quirks
- `.npmrc`: `ignore-scripts=true` → use `npm install --ignore-scripts`
- `.env` gitignored — copy from `ops-node-api/.env.example`
- Queue worker listens on: `high,default,employee-sync`
- Packages: `spatie/laravel-data`, `spatie/laravel-permission`, `laravel/horizon`

## Multi-tenancy
- `BranchScope` global scope filters queries by `branch_id`
- Bypass: `withoutBranchScope()` / `forBranch($id)`

## Outbox Pattern
- Table `outbox`: events (pending/sent/failed), exponential backoff retry
- Use `DispatchesWithOutbox` trait or `Model::createWithOutbox()` / `->updateWithOutbox()`
- Failed events: `storage/logs/sync-failed.log`

## API
- Routes: `/api/v1/*`
- Response: `{ success, code, message, errors, results }`
- Use `ApiResponse::success()`, `ApiResponse::created()`, `ApiResponse::error()`
