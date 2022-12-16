# DOCKER-COMPOSE

Instalación de servidor apache, mediawiki, wordpress, adminer y guestbook.

## Wordpress 📋

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