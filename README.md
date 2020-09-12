# Acerca del sitio

## Construccion del sitio

Este sitio es generado a traves de [docsify](https://docsify.js.org), una herramienta
para la generacion de sitios estaticos como otros que se pueden encontrar en
[staticgen.com](https://www.staticgen.com/), herramienta que funciona a traves de
NodeJS; la instalacion de estas herramientas es con los siguientes comandos:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
source ~/.bashrc
nvm --version
nvm install 12
node --version
npm i docsify-cli -g
```

La inicializacion del sitio base es la siguiente:

```bash
mkdir sitio && cd sitio
docsify init .
docsify serve .
```

El resaltado de sintaxis es a traves de [prism](https://prismjs.com), se puede acceder las librerias
especificas del lenguaje en su [CDN](https://cdn.jsdelivr.net/npm/prismjs@1/components/).

