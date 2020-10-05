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