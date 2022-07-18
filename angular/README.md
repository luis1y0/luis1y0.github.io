# Angular


## Creación de Proyecto

Para generar un proyecto, primero se necesitan la herramienta NVM que se instala
con:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm --version
```

Con NVM se puede instalar NodeJS y su herramienta de gestión de paquetes `npm`
directamente desde la terminal:

```bash
nvm install 16
node --version
npm --version
```

Una vez teniendo `npm` se puede instalar la herramienta cli de Angular:

```bash
npm install -g @angular/cli
```

Y para iniciar un proyecto se ejecuta:

```bash
ng new my-app
```

Con el comando serve se puede compilar el proyecto y con el parametro --open
se abrira el navegador automaticamente:

```bash
ng serve --open
```

