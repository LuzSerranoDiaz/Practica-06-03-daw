# Practica-06-03-daw

## docker-compose

```yml
version: '3.4'

services:
  mysql:
    image: mysql:8.0
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
```
#### Servicio mysql
* Image: mysql:8.0, indica la imagen que se usara para este contenedor y su versión.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
  * MYSQL_ROOT_PASSWORD indica la contraseña del usuario root.
  * MYSQL_DATABASE indica el nombre de la base de datos.
  * MYSQL_USER indica el usuario.
  * MYSQL_PASSWORD indica la contraseña del usuario.
* Volumes: indica donde se localiza el volumen dentro del contenedor.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
```yml
  phpmyadmin:
    image: phpmyadmin:5.2
    ports:
      - 8080:80
    environment:
      - PMA_HOST=mysql
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql
```
* Image: phpmyadmin:8.0, indica la imagen que se usara para este contenedor y su versión.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
  * PMA_HOST indica el host donde se encuentra mysql, al poner `mysql` indicamos que ese contenedor es el host.
* Volumes: indica donde se localiza el volumen dentro del contenedor.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
* depends_on: indica de que servicios depende.
```yml
  prestashop:
    image: prestashop/prestashop:8
    environment: 
      - PRESTASHOP_DATABASE_HOST=mysql
      - PRESTASHOP_DATABASE_NAME=${MYSQL_DATABASE}
      - PRESTASHOP_DATABASE_USER=${MYSQL_USER}
      - PRESTASHOP_DATABASE_PASSWORD=${MYSQL_PASSWORD}
    volumes:
      - prestashop_data:/var/www/html
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql

```
* Image: bitnami/prestashop:8, indica la imagen que se usara para este contenedor y su versión.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
      * PRESTASHOP_DATABASE_HOST indica el host de la base de datos.
      * PRESTASHOP_DATABASE_NAME indica el nombre de la base de datos.
      * PRESTASHOP_DATABASE_USER indica el usuario de la base de datos.
      * PRESTASHOP_DATABASE_PASSWORD indica la contraseña del usuario de la base de datos.
* Volumes: indica donde se localiza el volumen dentro del contenedor.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
* depends_on: indica de que servicios depende.
```yml
  https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    restart: always
    environment:
      DOMAINS: '${DOMAIN} -> http://wordpress:8080'
      STAGE: 'production' 
    networks:
      - frontend-network
```
* Image: steveltn/https-portal:1, indica la imagen que se usara para este contenedor y su versión.
* Ports: indica los puertos por los que se puede acceder al contenedor en especifico.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
  * DOMAINS indica el dominio por el que se va acceder y el contenedor al que redirije.
  * STAGE indica en que etapa o modo se encuentra el servicio.
* Networks: indica a que conexión a la que pertenece este contenedor.
```yml

volumes:
  mysql_data:
  prestashop_data:

networks: 
  backend-network:
  frontend-network:
```
