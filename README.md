# Docker en Desarrollo
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build --force-recreate

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
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build mysql

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

scp .env dgonzalez@172.24.0.146:/home/dgonzalez/Projects/ops-node/ops-node-api

# Optimize Project

php artisan optimize:clear
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan event:cache
php artisan permission:cache-reset
php artisan optimize

# MYSQL

Acces server DB
docker compose exec mysql mysql -u root -p
