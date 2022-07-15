# Spring Boot

## Dependencias principales

La principal dependencia para generar un servidor http usando
tomcat es:

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Para conectar a mysql:

```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```

Para manipular la base de datos sin usar codigo sql directamente:

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

## Variables en tiempo de ejecucion

En Spring Boot el archivo en `src/main/resources/application.properties`
se usa para configurar variables que spring lee en tiempo de ejecucion,
por ejemplo para establecer un puerto http especifico de escucha para
tomcat se usa esta linea: `server.port=8080`. Tambien se puede configurar
que conector de base de datos se usara y las credenciales de conexion.
Ademas usando una convencion de nombre como application.dev.properties
se pueden establecer los valores a usar dependiendo de si se trabaja en
en local, produccion u otros entornos.

## Configurar los CORS

## DevTools

DevTools ayuda a poder probar el codigo mas rapido al hacer modificaciones
sin tener la necesidad de compilar de nuevo y volver a iniciar el servidor.

