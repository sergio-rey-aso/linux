---
title: Linux
description: Proyecto ISO-ASO
permalink: /Proyecto/
---


# Proyecto. Listado de ideas

## Servidores del año pasado
- Servidores
  - Servidor Dominio
  - Servidor Dominio Secundario 
  - Servidor de aplicaciones
  - FreeNAS
  - Servidor de datos
  - Servidor Web ( Linux )
- Clientes:
  - Gerente
  - Administración/Contabilidad
  - I+D+I / Laboratorio --> Físico
  - Almacén / Producción --> Físico
  - Administración de sistemas 

## Linux

- Añadir servidor de Base de Datos (Linux)
- Cambiar equipos clientes de producción a Linux, dentro de dominio.
- 

- Script que compruebe cada minuto el estado de la conexión a internet.
- Script para copia de seguridad. 
- Comprobar la posibilidad de hacer copia de seguridad por `rsync`. A Google Drive o OneDrive se puede mediante `rClone`
- Redirección de syslog a un equipo
- Script que lea los equipos que envían syslog, y pregunte de cuál se quiere visualizar, si de uno en concreto o de todos. Pasado un tiempo, si no se elige nada, que muestre todos.
- Script para conectar con equipo. Si el equipo tiene VNC pregunta si quieres conectarte por VNC y o por SSH y si solo tiene SSH se conecta directamente por SSH.
- Crear un servicio de algo (base de datos o lo que sea). Si es posible que se ejecute al iniciar y al apagar el servicio. Por ejemplo podría ser para registrar el arranque y el apagado de los equipos, mínimo.



 - **Nueva clase de formación**
   - Tiene un servidor en Linux con LDAP, NFS y SAMBA
   - Se comparte una carpeta SAMBA llamada entradas. Se dejan ficheros en esta carpeta de entradas con listados de alumnos nuevos o grupos que finalizan
   - Hay un script que se inicia por un servicio de tipo path, que cogerá los ficheros del la carpeta y dará de alta usuarios o eliminará. según corresponda. Estos alumnos se crean en grupos y dentro de LDAP
   - Se utilizará una carpeta compartida en NFS para crear los perfiles móviles de los alumnos
   - Desde un par de equipos cliente se accedera indistitanmente a las difernetes cuentas creadas en LDAP
   - 

## Windows

- Crear un tenant  -->  Se necesita tarjeta
- Sincronizar dominio local con tenant
- Relación de confianza entre los diferentes dominios.
- Scripts para que gestión automática de usuarios. Se dejan ficheros en una carpeta y a partir de ahí debe pasar todo.

## Hibrido

- Crea un server con Samba


# Puntos a tratar del proyecto

- Material:
  - Podemos tener equipo potente para proxmox
  - El año pasado se utilizaron equipos de los alumnos, pero este año no se puede porque están los de ASIX1

- Reparto de los que no estaban el año pasado: en mi caso  alumnos

- Reparto de tareas y documentación dentro de los grupos


