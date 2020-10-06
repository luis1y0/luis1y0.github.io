# Instalacion y Configuracion de entorno

## Intrucciones para Ubuntu

Para trabajar con C necesitamos instalar `gcc` que incluye el compilador para C,
pero aparte del compilador al instalar `cmake` se podra automatizar la compilacion de
dependencias; entre sus funciones principales se encuentra registrar todos los archivos
de codigo fuente y las librerias a utilizar en el proyecto, todo desde un archivo sin
necesidad de especificarlo en la llamada al comando gcc.

```bash
sudo apt update
sudo apt install gcc cmake
```

## Instrucciones para Windows

En Windows se haria uso de [MinGW](http://mingw-w64.org/doku.php/download) que significa "Minimalist GNU for Windows" y
funciona nativamente en windows siendo no compatible con el entorno de ejecucion POSIX; otra opción es
[Cygwin](https://cygwin.com/install.html) que pretende proveer una capa POSIX permitiendo generar un ejecutable en
windows con el mismo codigo fuente para sistemas UNIX. 
