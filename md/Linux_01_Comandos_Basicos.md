---
title: Linux
description: Comandos Linux Básicos
permalink: /basico/
---

<h1>Comandos Linux Básicos</h1>

<h3>Tabla de contenidos</h3>

# 1. Introducción


# 1. Comandos GNU/Linus Bash

- Recordatorio comandos carpetas y permisos
- Comandos gestión de gestión de información de ficheros
- Comandos administracion:
  - `ssh`
  - `vnc`
  - Automatizaciń con `cron`
  - Automaticación con `at` y `batch`
  - Demonios




# 2. Algunos comandos básicos de ***PowerShell***


1\. Introducción La línea de comandos de Linux es una herramienta muy potente que nos permite realizar cualquier acción en el sistema. En Linux el entorno gráfico es una opción y, de hecho, podemos instalar el sistema operativo sin entorno gráfico con todas sus funcionalidades (se hace en servidores para optimizar los recursos). Aunque podemos hacer cualquier cosa necesitamos los permisos necesarios para hacerlo. Por eso hay muchos comandos que sólo los puede ejecutar el superusuario root. Como se vió anteriormente el prompt de cualquier usuario normal termina en el carácter $ y el de root en #. Otra cuestión importante es que cuando el usuario root ejecuta un comando el sistema operativo considera que sabe qué está haciendo y no nos pedirá confirmaciones, simplemente lo hace. Por eso debemos tener mucho cuidado cuando somos root en un sistema Linux. La recomendación es que siempre trabajamos como un usuario normal y sólo cuando tenemos que ejecutar un comando que necesita permisos de superusuario nos convertimos en root. El comando para cambiar de usuario es su y se le pasa como parámetro al usuario en que nos queremos loguearte. Si no le pasamos ningún parámetro se supone que queremos ser root. ejemplo: super jmonllor - pasamos a ser el usuario jmonllor (después de escribir su contraseña) su - pasamos a ser el usuario root (si escribimos la contraseña de root) También es posible ejecutar un comando que necesita permisos de root desde nuestro usuario anteponiéndole el comando sudo. Para hacer esto nuestro usuario debe pertenecer al grupo de usuarios que pueden hacer sudo (sudoers). En el caso de Ubuntu esta es la manera de trabajar por defecto y el usuario con que instalamos el sistema pertenece al grupo de sudoers. De hecho durante el proceso de instalación no se nos pide la contraseña de administrador por lo que no nos podemos loguearte como root (podemos hacerlo con el comando sudo su). En el caso de Debian al usuario que se crea durante la instalación es un usuario nomal (no puede der sudo) pero si se nos pide la contraseña de root para pueden loguearte cómo root.