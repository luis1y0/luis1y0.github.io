# FastAPI

## Instalacion de administrador entorno Python

Instalar dependencias previas:

```bash
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl
```

Instalar **pyenv**:

```bash
curl https://pyenv.run | bash
```

Cargar comando **pyenv** en archivo `.bashrc` o `.bash_profile` o `.profile`
ejecutado al abrir la terminal:

```bash
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

Instalar y establecer una version global de python:

```bash
pyenv install 3.9
pyenv versions
pyenv global 3.9
python --version
pip --version
```

## Generar un entorno virtual e instalar FastAPI

```bash
mkdir api-project && cd api-project
python -m venv venv
source venv/bin/activate
pip install fastapi
pip install "uvicorn[standard]"
pip freeze > requirements.txt
```

## Ejecutar un ejemplo

Archivo `main.py`:

```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}

```

Iniciar servidor:

```bash
uvicorn main:app --reload
```

Se puede acceder a la api en la direccion `http://localhost:8000` y a la documentacion interactiva en `http://localhost:8000/docs` o `http://localhost:8000/redoc`.

Para evitar problemas con CORS en javascript:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost.tiangolo.com",
    "https://localhost.tiangolo.com",
    "http://localhost",
    "http://localhost:8080",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```
