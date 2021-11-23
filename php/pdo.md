# Acceso a base de datos

# El objeto PDO

Segun la documentacion de PHP, la clase PDO es una abstraccion sin ninguna
funcionalidad, solo posee los metodos que definen la interfaz de comunicacion
a base de datos, no se puede usar, pero se pueden usar otras clases que
que extienden de esta clase para crear una implementacion especifica de un
motor de base de datos. Para este ejemplo se usara la implementacion de
`pdo_mysql`.

# Instalacion de Maria DB

Al parecer la imagen de bitnami para Maria DB es un poco mas ligera, asi que
es buena opcion para pruebas de desarrollo, se puede descargar con este comando:

```shell
docker pull bitnami/mariadb
```

Por defecto esta imagen contiene el usuario root al cual podemos establecer
por ejemplo de password la palabra `root` y como nombre de base de datos
`places_db` para probar con esste comando:

```shell
docker run -d --name mariadb -e MARIADB_ROOT_PASSWORD=root -e MARIADB_DATABASE=places_db --network red-backend bitnami/mariadb
```

El nombre de mariadb en este contenedor servira como host cuando se necesite
conectar desde codigo. Por eso se esta agregando a la misma red llamada
`red-backend` que se hizo en la creacion del contenedor para ejecutar php.

Como ejemplo de base de datos se puede usar este codigo en un archivo llamado
`inserts.sql`:

```sql
DROP TABLE IF EXISTS `places`;

CREATE TABLE `places` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `visited` tinyint(1) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=12 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `places` (name, visited) VALUES ('Berlin',0),('Budapest',0),('Cincinnati',1),('Denver',0),('Helsinki',0),('Lisbon',0),('Moscow',1),('Nairobi',0),('Oslo',1),('Rio',0),('Tokyo',0);
```

Para ejecutar el script dentro del contenedor de mariadb se puede copiar
el archivo sql dentro del contenedor de mariadb, con esto se accede al
contenedor y se ejecuta el script con los siguientes comandos:

```shell
docker cp inserts.sql mariadb:/inserts.sql
docker exec -it mariadb bash
mysql -u root -p places_db < inserts.sql
exit
```

## Extension de pod_mysql

Para poder conectar a mysql, se necesita agregar una extension en el contenedor
de php ya que por defecto no viene con el pdo especifico para conectar a mysql.
Para ello se utilizan comandos especiales dentro de la imagen de docker para
habilitarlo de la siguiente manera:

```
docker exec -it backend-php sh
apk update
# Este comando puede tardar un momento ya que realiza algunas descargas
docker-php-ext-install mysqli pdo pdo_mysql
exit
```

Para finalizar y probar que la conexion a base de datos esta funcionando, se
puede escribir el siguiente codigo desde el archivo `index.php`:

```php
<?php

$pdo = new PDO('mysql:host=mariadb;dbname=places_db', 'root', 'root');
$sql = 'SELECT * FROM places';
foreach ($pdo->query($sql) as $row) {
  echo $row['id'];
  echo ' - ';
  echo $row['name'];
  echo ' - ';
  echo $row['visited'];
  echo '<br>';
}
```

Se usa el constructor de PDO de php y en la cadena que se le pasa por
constructor se le dice que se usara la implementacion de mysql para
conectar a mariadb, se le indica en que el host es `mariadb`, RECORDAR
QUE EL NOMBRE DEL CONTENEDOR SIRVE COMO HOST DENTRO DE UNA RED DE DOCKER
Y ASI NO ES NECESARIO SABER SU IP. Se le indica enseguida que conectamos
a la base de datos `places_db` y tanto el usuario como password son root.

Una vez ya se tiene la conexion a la base de datos, podemo crear una cadena
con la sentencia sql de consulta, si estamos haciendo un select, se usa el
metodo `query()` que devuelve un arreglo con las filas que sean resultado de
la consulta. Ese arreglo es facil recorrerlo con un `foreach` y cada elemento
tiene claves asociadas a las columnas de la base de datos para facilitar su
acceso.
