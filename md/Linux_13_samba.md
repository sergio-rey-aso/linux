---
title: Linux
description: samba
permalink: /Linux_samba/
---

<h1>Samba</h1>

<h3>Tabla de contenidos</h3>


- [1. ¿Qué es un script?](#1-qué-es-un-script)
  - [1.1. Ejemplo: Script que borra los ficheros log en /var/log](#11-ejemplo-script-que-borra-los-ficheros-log-en-varlog)
  - [1.2. Empezando el script con el sha-­bang (`#!`)](#12-empezando-el-script-con-el-sha-bang-)
- [2. Comentarios](#2-comentarios)
- [3. Variables](#3-variables)
  - [3.1. Variables de usuario/script](#31-variables-de-usuarioscript)
  - [3.2. Variables de entorno](#32-variables-de-entorno)
  - [3.3. Variables especiales de bash](#33-variables-especiales-de-bash)
- [4. `echo`, `read` y `printf`](#4-echo-read-y-printf)
  - [4.1. `echo`](#41-echo)
  - [4.2. `printf`](#42-printf)
  - [4.3. `read`](#43-read)
- [5. Parámetros](#5-parámetros)
- [6. Comillas](#6-comillas)
- [7. Valores devueltos por programas](#7-valores-devueltos-por-programas)
- [8. Condiciones en shell script](#8-condiciones-en-shell-script)
  - [8.1. Condiciones al ejecutar comandos](#81-condiciones-al-ejecutar-comandos)
  - [8.2. Evaluación de expresiones. La orden `test`.](#82-evaluación-de-expresiones-la-orden-test)
  - [8.3. Evaluación de condiciones booleanas: verdadero (0) y falso (1)](#83-evaluación-de-condiciones-booleanas-verdadero-0-y-falso-1)
- [9. Control de flujo de programa. Estructura `if`.](#9-control-de-flujo-de-programa-estructura-if)
- [10. Bloques dentro de un programa](#10-bloques-dentro-de-un-programa)
- [11. Recordando el Redireccionamiento](#11-recordando-el-redireccionamiento)
- [12. Expresiones matemáticas](#12-expresiones-matemáticas)
- [13. Metacaracteres y algunos caracteres especiales](#13-metacaracteres-y-algunos-caracteres-especiales)
- [14. Estructura `case`](#14-estructura-case)
- [15. Bucles](#15-bucles)
  - [15.1. Estructura `While` (mientras)](#151-estructura-while-mientras)
  - [15.2. Estructura `for`](#152-estructura-for)
  - [15.3. Ruptura de bucles: `break` y `continue`](#153-ruptura-de-bucles-break-y-continue)
- [16. Saltos de línea en los scripts: '`;`' y '`\`'](#16-saltos-de-línea-en-los-scripts--y-)
- [17. Vectores (Arrays)](#17-vectores-arrays)
- [18. Funciones](#18-funciones)
- [19. ANEXOS](#19-anexos)
  - [19.1. El doble paréntesis `(( .. ))`](#191-el-doble-paréntesis---)
  - [19.2. Expresiones regulares](#192-expresiones-regulares)
  - [19.3. Número aleatorios](#193-número-aleatorios)
  - [19.4. Comando `awk`](#194-comando-awk)
  - [19.5. Definir color y posición del texto en la consola](#195-definir-color-y-posición-del-texto-en-la-consola)
  - [19.6. Definir la posición](#196-definir-la-posición)
  - [19.7. `Zenity`](#197-zenity)
  - [19.8. Comandos para mejorar la salida por consola.](#198-comandos-para-mejorar-la-salida-por-consola)
  - [19.9. Reflexión final](#199-reflexión-final)





# 1. Samba

Para introducir de forma rápida `samba` tenemos el siguiente tutorial de Ubuntu: 

- [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview)


En este tutorial se hace una instalación sencilla de `samba` y se puede ver su uso.

Otros enlaces desde donde extraer recursos:

- [Instalar y configurar Samba Share en Ubuntu 22.04|20.04|18.04](https://es.linux-console.net/?p=21480). Este enlace es bueno, se podría copiar todo el contenido por que hace configuración para todo el mundo, para usuarios logados y después faltaría hacer para Active Directory

- [Samba](https://pfc.upnfm.edu.hn/cursos/redes/REDES_LINUX/samba/Que_es_samba.html). De aquí ha cogido Ximo parte de sus apuntes.

# puntos

- Compartir anonimo
- Compartir usuarios
- Crear usuarios para compartir
- Compartir por otros usuarios no root
- webadmin