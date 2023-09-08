# Cheat Sheet

## git

Git stash: para cambios sin confirmar cuando se cambia de rama, se actualiza
de un remoto, acciones que entran en conflicto con los cambios, el espacio
usado por git stash le llama `WIP` (work in progress).

```bash
git stash push # agrega los cambios como WIP al stack; "push" es opcional
git stash pop # elimina el ultimo WIP y lo aplica;
git stash apply # aplica el WIP pero no lo elimina del stack
git stash push -- <filename> # agregar archivo especifico; "push" es opcional
git stash show stash@{index}
git checkout stash@{<index>} -- <filename> # unstash opcion 1
git restore --source=stash@{<index>} -- <filename> # unstash opcion 2
git stash apply "stash@{2}"
```

## Backup/Restore de una base de datos MySQL

Creacion de una base de datos:

```sql
CREATE DATABASE <name> CHARACTER SET utf8mb4;
CREATE USER '<user_name>'@'%' IDENTIFIED BY '<password_value>';
GRANT ALL PRIVILEGES ON *.* TO '<user_name>'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

Backup:

```bash
mysqldump -u [username] -p [database_name] [tbl_name ...] > [backup_name].sql
```

Restore:

```bash
mysql -u [username] -p < [backup_name].sql
```

## Instalacion de Docker y Docker Compose

```bash
sudo apt update
sudo apt install docker.io
apt install docker-compose
# para poder usar el comando sin permisos root
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker # se usa para efectuar los cambios de grupos sin cerrar sesion en el sistema
```

## Creacion de claves ssh para Github/Gitlab

```bash
# github
ssh-keygen -t rsa -b 4096 -C "email"
# gitlab
ssh-keygen -t rsa -b 2048 -C "email"
```
