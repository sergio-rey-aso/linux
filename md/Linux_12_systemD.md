---
title: Linux
description: Procesos servidores y arranque del sistema con SystemD
permalink: /Linux_SystemD/
---

<h1>Gestión de procesos: SystemD</h1>

<h3>Tabla de contenidos</h3>

# Introducción

Si ejecutamos el siguiente comando: 

```bash
ps -eo pid,ppid,euser,stat,comm
```

Obtenemos un listado de los procesos del sistema con los siguientes datos:

- PID del proceso,
- PID del padre del proceso,
- usuario
- Estado del proceso
- línea de comandos que originó el proceso

En este listado, podemos ver que todo proceso tiene un padre (PPID), y que todo padre tiene a su vez otro, hasta llegar a los procesos 1 y 2. El mas importantes es el 1, el systemd.


# Servicios o procesos demonio

Veamos qué es un proceso o servicio y cómo funciona

## Funcionamiento interno de un proceso servidor.

Se hace la demo de como funciona un servidor


# El arranque del sistema linux

Una forma excelente de representar el arranque del sistema es mediante las siguientes figuras:

En primer lugar, es la BIOS la que inicia el sistema hardware y busca un disco duro que pueda inicializar el Sistema Operativo

<div align="center">
    <img src="../img/SystemD_Arranque1.png" alt="Ejemplo logger" width="80%" />
</div>

Seguimos todos los graficos de arranque y llegamos hasta el comando 

```bash
ls -l /sbin/init
```

y el **EJERCICIO**
> Ejercicio: Intenta razonar qué de pasos y acciones que deberá dar el proceso init u otros procesos
en el arranque para dejar el computador listo para ser usado por un usuario.

# SystemD

## Objetivos del systemd

### `Units`. Tipos

Una ​unit es un fichero de configuración​ que controla algún aspecto relevante para el funcionamiento y arranque de los servicios. Entenderemos mejor esta idea viendo los tipos de unit existentes

Los 7 tipos más importantes:

- **service**: Demonios que pueden ser iniciados, detenidos, reiniciados o recargados.
- **socket**: Esta unidad encapsula un socket en el sistema de archivos o en Internet. Cada unidad socket tiene una unidad de servicio correspondiente.
- **device**: Esta unidad encapsula un dispositivo en el árbol de dispositivos de Linux.
- **mount**: Esta unidad encapsula un punto de montaje en la jerarquía del sistema de archivos.
- **automount**: Encapsula un punto de montaje automático. Cada unidad automount tiene una unidad mount correspondiente, que se inicia al acceder al directorio de automontaje.
- **target**: Utilizada para la agrupación lógica de unidades. Referencia a otras unidades, que pueden ser controladas conjuntamente, un ejemplo sería multi-user.target, que básicamente desempeña el papel de nivel de ejecución 3 en el sistema clásico SysV.
- **snapshot**: Similar a las unidades target.


### Units de servicio: 

Son las unit que describen aspectos del funcionamiento de un servicio​. En concreto se pueden especificar las siguientes informaciones:
- Comando a ejecutar para arrancar el servicio
- Comando para detener el servicio
- Dependencias previas
- Servicios que dependen de esta unit (creo)
- Destino de la salida de error o estándar.

#### ficheros .service

```
[Unit]
Description=Foo

[Service]
ExecStart=/usr/sbin/foo-daemon

[Install]
WantedBy=multi-user.target
```

Donde 
- **Unit**: características generales
- **Service**: Indica como iniciar y parar el servicio
- **Target**: BAjo que condiciones o ***target*** se ejecuta el servicio.


#### Targets

Significan estado a alcanzar por una situación concreta del sistema, por ejemplo `multi-user.target` es el principal de inicio.

Ver los ficheros en `/usr/lib/systemd/system/multi-user.target` y la carpeta `/usr/lib/systemd/system/multi-user.target.wants` y analizar 

#### Carpetas importantes

- `/lib/systemd/system` : Repositorio principal de ***units***, gestionado por el gestor de paquetes.
- `/usr/lib/sytemd/system` : Copia del principal de ***units*** al que se le añaden las ***units*** los administradores 
- `/etc/systemd/system` : Configuración del sistema **real** aplicada, o sea, las que se ejecutan. Observer que todas enlaces a la carpeta anterior

### Gestión de Servicios. `systemd`

`systemd` es un sistema **init** y un administrador del sistema que se ha convertido en el nuevo estándar para las distribuciones Linux. Debido a su gran adopción, merece la pena familiarizarse con `systemd`, ya que hará que administrar servidores sea mucho más fácil. Conocer y utilizar las herramientas y daemons que componen `systemd` le ayudará a apreciar mejor la potencia, la flexibilidad y las capacidades que proporciona, o al menos a simplificar su trabajo.

La finalidad principal de un sistema **init** es inicializar los componentes que deben iniciarse tras arrancar el kernel Linux. El sistema **init** también se utiliza para administrar servicios y daemons para el servidor en cualquier momento mientras se ejecuta el sistema. Teniendo eso en cuenta, comenzaremos con algunas operaciones básicas de administración de servicio.

En `systemd`, el destino de la mayoría de las acciones son “unidades”, que son recursos que `systemd` sabe cómo administrar. Las unidades se categorizan por el tipo de recurso al que representan y se definen con archivos conocidos como archivos de unidad. El tipo de cada unidad puede deducirse del sufijo al final del archivo.

Para las tareas de administración de servicio, la unidad de destino será unidades de servicio, que tienen archivos de unidad con un sufijo `.service`. Sin embargo, para la mayoría de los comandos de administración de servicio, puede dejar fuera el sufijo `.service`, ya que `systemd` es lo suficientemente inteligente para saber que probablemente quiere operar sobre un servicio cuando utiliza comandos de administración de servicio.


### El comando `systemctl`

Sintetizando: 

- Iniciar y apagar un servicio:

```bash
sudo systemctl start application.service
sudo systemctl stop application.service
```

- Reiniciar o volver a cargar:

```bash
sudo systemctl restart application.service
sudo systemctl reload application.service  # solo recarga configuración
```

- Habilitar y deshabilitar servicios

```bash
sudo systemctl enable application.service
sudo systemctl disable application.service
```

- Comprobar estado del servicio:

```bash
systemctl status application.service
```

- Enmascarar y desenmascarar unidades

```bash
sudo systemctl mask nginx.service
sudo systemctl unmask nginx.service
```

- Enumerar units del sistema:

```bash
systemctl                                       # lista las units cargadas
systemctl list-units                            # ídem anterior 
systemctl list-units --all                      # lista incluso las no activas
systemctl list-units --all --state=inactive     # filtrando por las inactivas
systemctl list-units --type=service             # filtrando por servicios solamente.
```

Tenemos muchos tutoriales buenos por internet, por ejemplo 

[DigitalOcean: Cómo usar Systemctl para gestionar servicios y unidades de Systemd](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units-es)



Demo de cómo se `status` y `mask`




# Ejercicio de iniciar servicio:

Puntos a tener en cuenta

- Si metemos nuevos ficheros de 
- Los archivos deben ser del root
- El archivo servHoraMult.service se puede copiar a `/usr/lib/systemd/system` o a `/lib/systemd/system`, es indiferente, se replica.
- Una vez hecho el paso anterior es aconsejable recargar el demonio de systemctl

```bash
sudo systemctl daemon-reload
```

- Se debe copiar el ejecutable en donde pone en el archivo de configuración: `/usr/bin/servHoraMult`
- Cuidado, porque el servicio lo mete en `/etc/systemd/system/multi-user.target.wants/`

Para ejecutar la prueba: 
```bash
sernetcat localhost 5000
```


Ejercicio python

El ejercicio consta de:
- Servidor en python (`socketEchoServer.py`) que se ejecuta como un servicio.
- Aplicación que llama al servicio (`socketEchoClient.py`) que se ejecuta cada minuto entre las 8 y las 14 horas, los días lectivos. Las respuestas la dejan un un fichero en la carpeta `/tmp`