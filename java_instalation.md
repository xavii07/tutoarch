---
title: Instalación y configuración de Java jdk
description: Instalación y configuración de Java jdk
keywords: java, instalación, configuración
---

# Instalación y configuración de Java jdk
- `java -version` para ver la versión de java instalada.
- `sudo pacman -sS java | grep jre` para ver las versiones de java jre disponibles.
- `sudo pacman -S jre-openjdk` para instalar java.
- `sudo pacman -sS java | grep jdk` para ver las versiones de java jdk disponibles.
- `sudo pacman -S jdk-openjdk` para instalar java.

## Instalar netbeans
- `git clone https://aur.archlinux.org/trizen.git` para clonar el repositorio de trizen.
- `cd trizen` para entrar a la carpeta.
- `makepkg -sri` para instalar trizen.
- `trizen -S apache-netbeans` para instalar netbeans.

## Instalar postgresql
- `sudo pacman -Ss postgresql` para ver las versiones de postgresql disponibles.
- `sudo pacman -Ss postgresql | grep extra/postgresql` para ver las versiones de postgresql disponibles en extra.
- `yay -S postgresql-12` para instalar postgresql versiones anteriores.
- `sudo pacman -S postgresql` para instalar postgresql.
- `postgres --version` para ver la versión de postgresql instalada.
- `sudo systemctl start postgresql` para iniciar postgresql.
- `sudo systemctl status postgresql` para ver el estado de postgresql.
- `sudo -iu postgres` para ingresar como usuario postgres.
- `initdb -D /var/lib/postgres/data` para inicializar la base de datos.
- `pg_ctl -D /var/lib/postgres/data -l logfile start` para iniciar el servicio de postgresql.
- `psql` para ingresar a postgresql.
- `sudo nvim /var/lib/postgres/data/postgresql.conf` buscamos la linea de `listen_addresses` y cambiamos `localhost` por `*`.
- `sudo systemctl restart postgresql` para reiniciar postgresql.
- `sudo systemctl enable postgresql` para que postgresql se inicie al iniciar el sistema.
- `sudo nvim /var/lib/postgres/data/pg_hba.conf` para configurar el acceso a postgresql agregamos al final (ip a para consultar ip) `host all all 192.168.101.7/24 trust`.
- `sudo systemctl restart postgresql`

## Instalar pgadmin4
- Debemos instalar por python
- [Instalar_pgadmin](https://www.pgadmin.org/download/pgadmin-4-python/)