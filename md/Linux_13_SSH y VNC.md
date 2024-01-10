---
title: Linux
description: SSH y VNC
permalink: /ssh/
---

Se trata de conectar desde el HOST a dos equipos virtualizados: un Ubuntu Desktop y un Ubuntu Server.

### 1. En primer lugar, para **conectar por `ssh`**:

- En el caso del `Ubuntu Server` será transparente, pues al instalar el equipo se ha seleccionado la opción de instalar `openssh`
- En el caso del `Ubuntu-Desktop`, el `openssh` no esta instalado, por lo que se debe instalar y configurar. Así pues los pasos a seguir serán:

```bash
#instalamos openssh
sudo apt install openssh-server

# Comprobamos estado del servicio y lo habilitamos y activamos
sudo systemctl status ssh
sudo systemctl enable ssh
sudo systemctl start ssh

#nos aseguramos que el puerto SSH esta abierto:
sudo ufw allow ssh

# Ya es accesible, no obstante si queremos modificar algún parámetro:
sudo nano /etc/ssh/sshd_config
```

> Fuente: [ionos: SSH en Ubuntu: instalación y activación](https://www.ionos.es/digitalguide/servidores/configuracion/ubuntu-ssh/)

Ya tenemos los dos equipos conectados por ssh

### 2. Ahora procedemos a conectar los dos equipos por `VNC`

En este caso, el proceso es al reves que en el anterior:

- En el `Desktop` lo podemos activar simplemente mediante la opción de `configuracion`->`compartir` y activamos opciones.de `Escritorio remoto`
- En el `Server` debemos realizar todo los pasos para la instalación de un entorno gráfico y el servidor vnc

Fuente: [DevOps Tutorials: How to Install and Configure VNC on Ubuntu](https://vegastack.com/tutorials/how-to-install-and-configure-vnc-on-ubuntu-22-04/)

