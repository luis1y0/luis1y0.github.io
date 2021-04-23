# Node JS

## Instalación en Linux

Usando [NVM](https://github.com/nvm-sh/nvm) es muy sencillo instalar y
administrar diferentes versiones de Node a la vez desde la linea de comandos
como se muestra a continuación:

```bash
# Tambien se puede usar wget:
# wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
source ~/.bashrc
nvm --version
nvm install 14 --lts
node --version
```

