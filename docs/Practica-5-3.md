# Practica5.2-iaw

## Script docker-compose.yml
```
version: '3.9'

services:
### Instalacion de wordpress
  wordpress:
    image: bitnami/wordpress:latest
    environment:
      - WORDPRESS_DATABASE_HOST=mariadb
      - WORDPRESS_DATABASE_USER=bn_wordpress
      - WORDPRESS_DATABASE_PASSWORD=bitnami
      - WORDPRESS_DATABASE_NAME=bitnami_wordpress
      - WORDPRESS_BLOG_NAME=User's blog
      - WORDPRESS_USERNAME=user
      - WORDPRESS_PASSWORD=bitnami
      - WORDPRESS_EMAIL=user@example.com
    volumes:
      - wordpress_data:/bitnami/wordpress
    depends_on:
      - mariadb
    restart: always

### Instalacion de mariadb
  mariadb:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=bitnami
      - MYSQL_DATABASE=bitnami_wordpress
      - MYSQL_USER=bn_wordpress
      - MYSQL_PASSWORD=bitnami
    volumes:
      - mariadb_data:/var/lib/mysql
    restart: always

### Instalacion de phpmyadmin
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8080:80"
    environment:
      - PMA_HOST=mariadb
      - PMA_USER=bn_wordpress
      - PMA_PASSWORD=bitnami
    depends_on:
      - mariadb
    restart: always

### Instalacion de https-portal (lets encrypt)
  https-portal:
    image: steveltn/https-portal:latest
    ports:
      - "80:80"
      - "443:443"
    environment:
      DOMAINS: "${DOMAIN} -> http://wordpress:8080"
      STAGE: "production"
    volumes:
      - https_portal_data:/var/lib/https-portal
    depends_on:
      - wordpress
    restart: always

volumes:
  wordpress_data:
  mariadb_data:
  https_portal_data:
```

## Ejecucion de docker compose up
![](imagenes/ejecucion%20.png)

## Comprobacion de docker compose ps
![](imagenes/ps.png)

## Visualizacion de wordpress
![](imagenes/wordpress.png)

## Hellow World
![](imagenes/hellow_world.png)

## Let's Encrypt
![](imagenes/lets.png)


Dominio= iawmarcos.ddnsking.com

Repositorio= https://github.com/Marcos-Ramos-Martinez/Practica5.2-iaw.git