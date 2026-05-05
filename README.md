# Docker en Desarrollo
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build

# docker en Producción
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build

# Bajar desarrollo
docker compose -f docker-compose.yml -f docker-compose.dev.yml down

# Bajar produccion
docker compose -f docker-compose.yml -f docker-compose.prod.yml down

# Ver los containers
docker ps --format "table {{.Names}}\t{{.Ports}}"


# Para QA, si ops-node-front corre con Docker Compose, reinícialo así desde la raíz del proyecto:

docker compose -f docker-compose.yml -f docker-compose.dev.yml restart ops-node-front

# Los cambios incluyen nuevas dependencias o cambios en package.json, reconstruir el contenedor:

docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build ops-node-front

# Ambiente productivo:

docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build ops-node-front

# Para revisar logs:

docker compose logs -f --tail=100 ops-node-front
Si el problema es cache del navegador o Vite, prueba:

docker compose exec ops-node-front npm install --ignore-scripts
docker compose restart ops-node-front

en el navegador hard refresh: Ctrl + F5.

# Nginx delante del front, también reiniciar Nginx:

docker compose restart ops-node-nginx