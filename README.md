# Docker-Compose
## Enabling profiles
'''''
version: "3.9"
services:
  frontend:
    image: frontend
    profiles: ["frontend"]

  phpmyadmin:
    image: phpmyadmin
    depends_on:
      - db
    profiles:
      - debug

  backend:
    image: backend

  db:
    image: mysql
'''''
docker-compose --profile debug up
COMPOSE_PROFILES=debug docker-compose up

docker-compose --profile frontend --profile debug up
COMPOSE_PROFILES=frontend,debug docker-compose up


## Auto-enabling profiles and dependency resolution
'''''
version: "3.9"
services:
  backend:
    image: backend

  db:
    image: mysql

  db-migrations:
    image: backend
    command: myapp migrate
    depends_on:
      - db
    profiles:
      - tools
'''''
# will only start backend and db
docker-compose up -d

# this will run db-migrations (and - if necessary - start db)
# by implicitly enabling profile `tools`
docker-compose run db-migrations


