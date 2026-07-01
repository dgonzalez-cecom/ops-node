# Docker en Desarrollo
Ejecutar Docker Compose siempre desde la raiz del proyecto. Los archivos oficiales son `docker-compose.yml`, `docker-compose.dev.yml` y `docker-compose.prod.yml`.

docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build --force-recreate

# docker en Producción
docker compose -f docker-compose.yml -f docker-compose.apache-proxy.yml up -d --build
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build

# Bajar desarrollo
docker compose -f docker-compose.yml -f docker-compose.dev.yml down

# Bajar produccion
docker compose -f docker-compose.yml -f docker-compose.prod.yml down
docker compose -f docker-compose.yml -f docker-compose.apache-proxy.yml down

# Levantar ops-node-scheduler
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build ops-node-scheduler
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build mysql
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build ops-node-api-nginx
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build redis
docker compose -f docker-compose.yml -f docker-compose.apache-proxy.yml up -d --build mysql
docker compose -f docker-compose.yml -f docker-compose.apache-proxy.yml up -d --build ops-node-api-php
docker compose -f docker-compose.yml -f docker-compose.apache-proxy.yml up -d --build ops-node-api-nginx
# Ver los containers
docker ps --format "table {{.Names}}\t{{.Ports}}"


# Para QA, si ops-node-front corre con Docker Compose, reinícialo así desde la raíz del proyecto:

docker compose -f docker-compose.yml -f docker-compose.dev.yml restart ops-node-front
docker compose -f docker-compose.yml -f docker-compose.dev.yml restart ops-node-api
docker compose -f docker-compose.yml -f docker-compose.dev.yml restart redis
docker compose -f docker-compose.yml -f docker-compose.dev.yml restart ops-node-horizon-worker
docker compose -f docker-compose.yml -f docker-compose.apache-proxy.yml restart mysql


# Los cambios incluyen nuevas dependencias o cambios en package.json, reconstruir el contenedor:

docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build ops-node-front
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build mysql

# Ambiente productivo:

docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build ops-node-front

# Conectarse al servicio php
docker compose exec ops-node-api-php sh

# Para revisar logs

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
php artisan event:cache
php artisan permission:cache-reset
php artisan optimize

# MYSQL

Acces server DB
docker compose exec mysql mysql -u root -p

2. Desde server anfitrion
mysql -u root -p

# LOAD DATABASE
docker compose exec -T service_name_mysql mysql -u root -pMipass namedb < C:\path\file.sql

export COMPOSE_FILE=docker-compose.yml:docker-compose.dev.yml

# From QA
git checkout main
git fetch origin
git reset --hard origin/main
git clean -fd

# Evitar falsos cambios en Linux
git config core.fileMode false

# Server DEV
  API: http://localhost:8000
 FRONT: http://localhost:5173
# Server QA UBUNTU SERVER
  FQDN: intranet-lab.goc.mx
   API: http://172.24.0.146:8000
 FRONT: http://172.24.0.146:5173
COURSE: http://172.24.0.146:5173/courses/login
COURSE_ENROLLED: http://172.24.0.146:5173/courses/enrollments?payroll_id=1234&source=fingerprint

# Server QA DEBIAN
  API: http://172.16.0.243:8000
FRONT: http://172.16.0.243:5173

ssh doroteo@172.16.0.243
# Server PROD


# Arquitecura with Apache
ops-node/
├── docker-compose.yml
├── docker-compose.dev.yml
├── docker-compose.apache-proxy.yml
├── docker/
│   └── nginx/
│       └── default.conf              ← reemplaza o ajusta este archivo
└── apache-ops-node-vhost.conf        ← NO va dentro de Docker

# TIPS GIT
Descargar repositorio remoto y cambiarte a la rama
$ git switch -c feature/reverse-proxy origin/feature/reverse-proxy