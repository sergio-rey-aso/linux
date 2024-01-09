---
title: Linux
description: samba
permalink: /samba/
---

<h1>Samba</h1>

<h3>Tabla de contenidos</h3>


# 1. Samba

`Samba` es una implementación libre del protocolo de archivos compartidos de *Microsoft Windows* (antiguamente llamado ***SMB***, renombrado posteriormente a ***CIFS***) para sistemas de tipo *UNIX*. De esta forma, es posible que computadoras con *GNU/Linux*, *Mac OS X* o *Unix* en general se vean como servidores o actúen como clientes en redes de Windows. En otras palabras, `Samba` permite compartir archivos y recursos entre sistemas operativos diferentes, como *Windows* y *Linux*, en una red local

Algunos de los usos comunes de `Samba` en *GNU/Linux* son:

- **Compartir archivos**: Samba permite compartir archivos y directorios entre sistemas operativos diferentes, lo que facilita la transferencia de archivos entre ellos.
- **Compartir impresoras**: Samba también puede utilizarse para compartir impresoras en una red local.
- **Autenticación de usuarios**: Samba puede utilizarse para autenticar usuarios en una red local.
- **Controlador de dominio**: Samba puede actuar como un controlador de dominio en una red local1.


Para introducir de forma rápida `samba` tenemos el siguiente tutorial de Ubuntu: 

- [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview)


En este tutorial se hace una instalación sencilla de `samba` y se puede ver su uso.

Otros enlaces desde donde extraer recursos:

- [Instalar y configurar Samba Share en Ubuntu 22.04|20.04|18.04](https://es.linux-console.net/?p=21480). Este enlace es bueno, se podría copiar todo el contenido por que hace configuración para todo el mundo, para usuarios logueados y después faltaría hacer para Active Directory

- [Samba](https://pfc.upnfm.edu.hn/cursos/redes/REDES_LINUX/samba/Que_es_samba.html). De aquí ha cogido Ximo parte de sus apuntes.

# puntos

- Compartir anónimo
- Compartir usuarios
- Crear usuarios para compartir
- Compartir por otros usuarios **no** `root`
- webadmin


**Fuentes:**

- [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview)
- [Instalar y configurar Samba Share en Ubuntu 22.04, 20.04 y 18.04](https://es.linux-console.net/?p=21480)
- [YouTube: Uso de Samba para compartir carpetas con equipos Windows](https://www.youtube.com/watch?v=2fF1etV7iYY)
