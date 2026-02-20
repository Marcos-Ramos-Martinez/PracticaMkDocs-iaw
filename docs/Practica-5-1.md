# Practica-5.1-iaw
Repositorio para la practica 5.1 de IAW del curso 25/26

## docker-compose.yml
```
version: '3.4'

services:
### Instalamos mysql
  mysql:
    image: mysql:9.1
    ports: 
      - 3306:3306
    environment: 
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    volumes: 
      - mysql_data:/var/lib/mysql
    networks: 
      - backend-network
    restart: always
### Instalamos phpmyadmin
  phpmyadmin:
    image: phpmyadmin:5.2.1
    ports:
      - 8080:80
    environment: 
      - PMA_ARBITRARY=1
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql
### Instalamos prestashop
  prestashop:
    image: prestashop/prestashop:8
    environment: 
      - DB_SERVER=mysql
    volumes:
      - prestashop_data:/var/www/html
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql

  https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    restart: always
    environment:
      DOMAINS: "${DOMAIN} -> http://prestashop:80"
      STAGE: 'production' # Don't use production until staging works
      # FORCE_RENEW: 'true'
    networks:
      - frontend-network

volumes:
  mysql_data:
  prestashop_data:

networks: 
  backend-network:
  frontend-network:
```
## Lanzamos el contenedor
![](imagenes/creacion_de_docker.png)

## Comprobamos el estado del contenedor
![](imagenes/docker_ps.png)

## Let`s Encrypt
![](imagenes/lets.png)

## Creacion de prestashop
![](imagenes/prestashop.png)

### Comando para eliminar la carpeta install
docker exec practica-51-iaw-prestashop-1 rm -rf /var/www/html/install

## Inicio de sesion
![](imagenes/inicio%20de%20sesion.png)

## Panel de control
![](imagenes/panel%20de%20admin.png)

Dominio= iawmarcos.ddnsking.com

Repositorio= https://github.com/Marcos-Ramos-Martinez/Practica-5.1-iaw.git