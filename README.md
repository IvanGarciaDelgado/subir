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

Creamos un directo y un fichero docker-compose.yml.

_Obtenemos el código desde Docker Hub, asegurándonos que es la imagen oficial_.

```
version: '3.1'

services:

  adminer:
    image: adminer
    restart: always
    ports:
      - 8081:8080

  db:
    image: mysql:5.6
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Se puede **personalizar** la variable de entorno para que sea más personal.

* Recomiendo cambiar el puerto del 8080, para no tener el mismo puerto en distintas aplicaciones o servidores web.

Abriremos un terminal (es importante que la terminal se abra en la ubicación del archivo, y sino situarte con el comando "cd") y escribiremos el comando _docker compose up -d_

Tras tener los dos contenedores corriendo abrimos la aplicación en el servidor ("Open in browser").

## Guestbook 📋

Creamos un directo y un fichero docker-compose.yml.

Copiaremos este codigo en el fichero.

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

Abriremos un terminal (es importante que la terminal se abra en la ubicación del archivo, y sino situarte con el comando "cd") y escribiremos el comando _docker compose up -d_

Tras tener los dos contenedores corriendo abrimos la aplicación en el servidor ("Open in browser").

## Mediawiki 📋

Creamos un directo y un fichero docker-compose.yml.

Copiaremos este codigo en el fichero.

```
# MediaWiki with MariaDB
#
# Access via "http://localhost:8080"
#   (or "http://$(docker-machine ip):8080" if using docker-machine)
version: '3'
services:
  mediawiki:
    container_name: miwiki
    image: mediawiki
    restart: always
    ports:
      - 8081:80
    links:
      - database
    volumes:
      - images:/var/www/html/images
      # After initial setup, download LocalSettings.php to the same directory as
      # this yaml and uncomment the following line and use compose to restart
      # the mediawiki service
      # - ./LocalSettings.php:/var/www/html/LocalSettings.php
  # This key also defines the name of the database host used during setup instead of the default "localhost"
  database:
    image: mariadb
    restart: always
    environment:
      # @see https://phabricator.wikimedia.org/source/mediawiki/browse/master/includes/DefaultSettings.php
      MYSQL_DATABASE: my_wiki
      MYSQL_USER: wikiuser
      MYSQL_PASSWORD: example
      MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
    volumes:
      - db:/var/lib/mysql

volumes:
  images:
  db:

```

Se puede **personalizar** las variables de entorno para que sea más personal.

* Recomiendo cambiar el puerto del 8080, para no tener el mismo puerto en distintas aplicaciones o servidores web.

Abriremos un terminal (es importante que la terminal se abra en la ubicación del archivo, y sino situarte con el comando "cd") y escribiremos el comando _docker compose up -d_

Tras tener los dos contenedores corriendo abrimos la aplicación en el servidor ("Open in browser").

* Tras arrancar la aplicación y rellenar todos los datos, para terminar la instalación deberás de copiar en el contenedor el fichero LocalSetting.php, se puede hacer a través de comandos (de local a contenedor) o descomentar una línea del código y guardar los cambios.
* Tras esto, _docker compose down_ y repetimos el comando _docker compose up -d_

```
# MediaWiki with MariaDB
#
# Access via "http://localhost:8080"
#   (or "http://$(docker-machine ip):8080" if using docker-machine)
version: '3'
services:
  mediawiki:
    container_name: miwiki
    image: mediawiki
    restart: always
    ports:
      - 8081:80
    links:
      - database
    volumes:
      - images:/var/www/html/images
      # After initial setup, download LocalSettings.php to the same directory as
      # this yaml and uncomment the following line and use compose to restart
      # the mediawiki service
      # - ./LocalSettings.php:/var/www/html/LocalSettings.php
  # This key also defines the name of the database host used during setup instead of the default "localhost"
  database:
    image: mariadb
    restart: always
    environment:
      # @see https://phabricator.wikimedia.org/source/mediawiki/browse/master/includes/DefaultSettings.php
      MYSQL_DATABASE: my_wiki
      MYSQL_USER: wikiuser
      MYSQL_PASSWORD: example
      MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
    volumes:
      - db:/var/lib/mysql

volumes:
  images:
  db:

```

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


