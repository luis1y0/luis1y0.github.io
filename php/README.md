# PHP

# Instalacion desde docker para desarrollo

Para uso en desarrollo con alpine se puede descargar la imagen con docker pull
y al listarla el espacio ocupado es de menos de 100 MB;

```shell
docker pull php:alpine
```

Para probar la ejecucion de una pagina simple en php que solo muestra hola mundo
en el navegador, en una carpeta vacia coloco una archivo de cualquier nombre
por ejemplo `index.php` con el siguiente código:

```php
<?php

echo 'Hola mundo';
```

Antes de ejecutar el codigo desde docker, es preferible crear una red de docker
para facilitar la conexion entre diferentes contenedores de docker, aunque no
se recomienda usar una red bridge, en mi entorno de desarrollo creare una de
este tipo con el siguiente comando:

```shell
docker network create --driver bridge red-backend
```

Para ejecutar ese codigo desde la terminal en la misma carpeta donde se
encuentra el archivo `index.php` ejecuto el siguiente comando de docker:

```shell
docker run -d -p 8000:8000 --name backend-php -v "$PWD":/usr/src/myapp -w /usr/src/myapp --network red-backend php:alpine php -S 0.0.0.0:8000 index.php
```

En el comando anterior, los parametros hacen lo siguiente:

 * `-d`: Esto hace que el contenedor se ejecute en segundo plano y no se
 bloque la terminal.
 * `-p 8000:8000`: Mapea los puertos \<puerto-contenedor:puerto-host> lo que
 significa que si el servidor funciona en el puerto 8000 dentro del contenedor,
 en mi navegador tambien lo puedo ver desde el puerto 8000.
 * `--name backend-php`: Le da un nombre al contenedor y tambien permite usarse
 como host dentro de la red `red-backend` creada anteriormente para conectar
 desde otro contenedor sin saber la direccion ip que se le asigna, es util
 cuando se conecta a un contenedor donde se encuentra la base de datos u otros
 servicios.
 * `-v "$PWD":/usr/src/myapp`: En este parametro es para crear
 un volumen para sincronizar datos entre mi computadora y el contenedor, de
 esta manera ejecuto el codigo php que yo escribo, antes de los dos puntos, $PWD
 es para indicar la carpeta de mi computadora, con $PWD digo que es la carpeta
 en la que me encuentro, por eso el comando se ejecuta en la carpeta donde tengo
 mi codigo. Despues de los dos punto indico la carpeta dentro del contenedor
 donde se sincronizara el codigo que escriba.
 * `-w /usr/src/myapp`: Este parametro le indica al contendor donde se
 ejecutara el comando que indique al final del comando docker run, lo cual
 usare para ejecutar facilmente el comando de php que inicia un servidor http.
 * `--network red-backend`: Esto indica que el contenedor estara enlazado a la
 red de docker `red-backend` que se creo anteriormente.
 * `php:alpine`: Indica que el nuevo contenedor se construya con la imagen
 `php:alpine`.
 * `php -S 0.0.0.0:8000 index.php`: Al final se encuentra el comando que
 ejecutara el servidor http de pruebas, este comando escuchara solicitudes
 http en el puerto 8000 y ese servidor manejara las solicitudes con el archivo
 `index.php` dentro de la carpeta `/usr/src/myapp` que contiene una copia
 actualizada con el codigo que editemos en el archivo `index.php` de nuestra
 computadora.

