# Docker en Desarrollo
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build

# docker en Producción
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build

# Bajar desarrollo
docker compose -f docker-compose.yml -f docker-compose.dev.yml down

# Bajar produccion
docker compose -f docker-compose.yml -f docker-compose.prod.yml down


