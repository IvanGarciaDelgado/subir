# DOCKER-COMPOSE

En esta práctica veremos la instalación de Mediawiki, Wordpress, Adminer, Guestbook y un servidor Apache.
Todos estos despliegues los haremos a través de docker compose, recomiendo dejar el servidor apache el último para que no altere el funcionamiento de los demás.

## Wordpress 📋

Creamos un directorio por despliegue, y un fichero docker-compose.yml, dónde añadiremos el código necesario para su funcionamiento.

_Obtenemos el código desde Docker Hub, asegurándonos que es la imagen oficial_.

```
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

Se puede **personalizar** las variables de entorno para que sea más personal.

Abriremos un terminal (es importante que la terminal se abra en la ubicación del archivo, y sino situarte con el comando "cd") y escribiremos el comando _docker compose up -d_

Tras tener los dos contenedores corriendo abrimos la aplicación en el servidor ("Open in browser").


## Adminer 📋

_Obtenemos el código desde Docker Hub, asegurándonos que es la imagen oficial_.

```
version: '3.1'

services:

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

  db:
    image: mysql:5.6
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Se puede **personalizar** las variables de entorno para que sea más personal.

## Servidor Web Apache 📋

```
version: '3.9'
services:
  apache:
    image: httpd:latest
    container_name: apacheIvan
    ports:
    - '8080:80'
    volumes:
    - ./website:/usr/local/apache2/htdocs

```

## Guestbook 📋

```
version: '3.1'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    ports:
      - 80:5000
  db:
    container_name: redis
    image: redis
    restart: always

```
