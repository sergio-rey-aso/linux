---
title: Linux
description: samba
permalink: /samba/
---

<h1>Samba</h1>

<h3>Tabla de contenidos</h3>

- [1. Introducción a Samba](#1-introducción-a-samba)
- [2. Instalación y configuración básica](#2-instalación-y-configuración-básica)
- [3. El fichero `/etc/samba/smb.conf`](#3-el-fichero-etcsambasmbconf)
  - [3.1. Sección `[global]`](#31-sección-global)
    - [3.1.1. Configuración del tipo de servidor : `server role`](#311-configuración-del-tipo-de-servidor--server-role)
    - [3.1.2. Opciones de configuración principales](#312-opciones-de-configuración-principales)
  - [3.2. Resto de secciones](#32-resto-de-secciones)
  - [3.3. Variables SAMBA](#33-variables-samba)
- [4. Crear, editar o eliminar usuarios en Samba](#4-crear-editar-o-eliminar-usuarios-en-samba)
- [5. Verificando la configuración: `testparm`](#5-verificando-la-configuración-testparm)
- [6. Conectando con el Servidor Samba](#6-conectando-con-el-servidor-samba)
  - [6.1. Accediendo desde un cliente GNU/Linux](#61-accediendo-desde-un-cliente-gnulinux)
    - [6.1.1. Usando la Línea de Comandos](#611-usando-la-línea-de-comandos)
    - [6.1.2. Desde el explorador de archivos](#612-desde-el-explorador-de-archivos)
    - [6.1.3. Montando el recurso compartido con `mount`](#613-montando-el-recurso-compartido-con-mount)
    - [6.1.4. Montando el recurso compartido permanentemente](#614-montando-el-recurso-compartido-permanentemente)
      - [6.1.4.1. usando `.smbcredentials`](#6141-usando-smbcredentials)
  - [6.2. Accediendo desde un cliente Windows](#62-accediendo-desde-un-cliente-windows)
  - [6.3. Otras formas de acceso](#63-otras-formas-de-acceso)
- [7. **Webmin**: Herramienta de Administración Web](#7-webmin-herramienta-de-administración-web)



# 1. Introducción a Samba

`Samba` es una implementación libre del protocolo de archivos compartidos de *Microsoft Windows* (antiguamente llamado ***SMB***, renombrado posteriormente a ***CIFS***) para sistemas de tipo *UNIX*. De esta forma, es posible que computadoras con *GNU/Linux*, *Mac OS X* o *Unix* en general se vean como servidores o actúen como clientes en redes de Windows. En otras palabras, `Samba` permite compartir archivos y recursos entre sistemas operativos diferentes, como *Windows* y *Linux*, en una red local

Algunos de los usos comunes de `Samba` en *GNU/Linux* son:

- **Compartir archivos**: Samba permite compartir archivos y directorios entre sistemas operativos diferentes, lo que facilita la transferencia de archivos entre ellos.
- **Compartir impresoras**: Samba también puede utilizarse para compartir impresoras en una red local.
- **Autenticación de usuarios**: Samba puede utilizarse para autenticar usuarios en una red local.
- **Controlador de dominio**: Samba puede actuar como un controlador de dominio en una red local1.

# 2. Instalación y configuración básica

La instalación de `Samba` es muy sencilla, y como siempre consta de la instalación de un paquete, en este caso llamado `samba` y posteriormente la configuración se realiza en el fichero `/etc/samba/smb.conf`. 

Para más información de instalación y configuración básica de `Samba`, aquí tienes el siguiente enlace: [Ubuntu.com: Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview).

En siguiente enlace puedes ver la configuración básica de dos carpetas compartidas, una accesible por todos los usuarios con permisos de lectura y otra crea una carpeta por usuario conectado, solo accesible por el propio usuario. [Instalar y configurar un servidor SAMBA en Debian y Ubuntu](https://medium.com/marcsanchezg/instalar-y-configurar-un-servidor-samba-en-debian-y-ubuntu-d05bb23644a1)


# 3. El fichero `/etc/samba/smb.conf`

La configuración de SAMBA, tanto servidor como cliente, se realiza en el fichero `/etc/samba/smb.conf`. Este fichero establece las características de SAMBA y los recursos que se compartirán en la red. Aunque este fichero posee un gran número de opciones, la configuración del mismo suele ser sencilla, dado que el valor por defecto es apropiado para casi todas las opciones y casos posibles.

Como punto importante, resaltar que se consideran comentarios todas las líneas que comienzan por `#` y por `;` (punto y coma), utilizándose generalmente el símbolo `#` para los comentarios formales y el punto y coma para comentar opciones de configuración. Además, el símbolo `\` indica que la línea continúa en la línea siguiente,
por lo que debe ignorarse el retorno de carro insertado a continuación.

De forma general, podemos considerar el fichero de configuración dividido en diferentes secciones. El comienzo de cada sección se indica mediante la etiqueta ***[nombre de sección]***, siendo el final de una sección el comienzo de la siguiente. 

Por defecto, el fichero se puede dividir en dos partes, una con la sección `[global]` y otra para el resto de recursos compartidos.

<div align="center">
    <img src="../img/samba/samba-02.png" alt="Samba" width="50%" />
</div>

Antes de nada, recordad que antes de manipular este tipo de ficheros, es aconsejable realizar una copia:

```bash
cp /etc/samba/smb.conf /etc/samba/smb_copia.conf
``` 

## 3.1. Sección `[global]`

La sección `[global]` define los parámetros de SAMBA del nivel global, así como los valores por defecto para el resto de parámetros en otras secciones si no se especifican en las mismas.

<div align="center">
    <img src="../img/samba/samba-03.png" alt="Samba" width="50%" />
</div>

### 3.1.1. Configuración del tipo de servidor : `server role`

Es importante antes de nada tener en cuenta el tipo de servidor que vamos a configurar con samba: el parámetro `server role` se utiliza para especificar el papel que el servidor **Samba** desempeñará en la red. Este parámetro determina si el servidor actuará como un ***controlador de dominio***, un ***servidor autónomo*** o en algún ***otro*** rol específico.

En la siguiente tabla se muestran los diferentes opciones para este parámetro fundamental: 

| Valor                                 | Descripción                                              |
|---------------------------------------|----------------------------------------------------------|
| **`standalone server`** (predeterminado)  | Servidor independiente sin funciones de controlador de dominio.|
| **`member server`**                      | Servidor que forma parte de un dominio existente.        |
| **`classic primary domain controller`**  | Controlador de dominio principal en un entorno Samba.    |
| **`classic backup domain controller`**    | Controlador de dominio de respaldo en un entorno Samba.  |
| `active directory domain controller`  | Controlador de dominio de Active Directory.              |
| `active directory domain member`      | Miembro de dominio de Active Directory.                   |
| `active directory domain controller for the domain` | Controlador de dominio específico de un dominio en Active Directory. |
| `active directory domain controller for a new domain` | Controlador de dominio para la creación de un nuevo dominio en Active Directory. |

Estos son algunos de los valores comunes para el parámetro "`server role`". Ten en cuenta que las opciones exactas pueden variar según la versión específica de Samba que estés utilizando, y siempre es recomendable consultar la documentación oficial correspondiente a tu versión específica para obtener la información más precisa. En todo caso los más utilizados son los 4 primeros, el resto son variaciones de estos.

```
server role = standalone server
server role = member server
server role = classic primary domain controller
server role = classic backup domain controller
```

Es importante elegir el valor correcto para "`server role`" según el papel que desees que el servidor **Samba** desempeñe en tu red. 

### 3.1.2. Opciones de configuración principales

Las principales opciones de configuración de la sección global de SAMBA son:

| Opción | Descripción | Valor por defecto | 
| --- | --- | --- |  
| workgroup | Nombre del grupo de trabajo o dominio de SAMBA. | Ninguno.  | 
| server string | Descripción del equipo en el dominio o grupo de trabajo. | Ninguno. | 
| netbios name | Nombre del ordenador SAMBA. | Nombre DNS. | 
| interfaces | Interfaces que utilizará SAMBA. Puede ser un interface (eth0), una dirección IP, una  dirección IP/mascara o una dirección broadcast/mascara. | Todos los interfaces excepto el de loopback. | 
| security | Nivel de seguridad. | user | 
| passdb backend | Modo de almacenamiento de las contraseñas. | tdbsam
| smb passwd file | Fichero con las contraseñas almacenadas. | En el ejecutable. | 
| encrypt passwords |  Utilizar contraseñas cifradas de Windows. | yes | 
| password server | Servidores Windows para la autentificación. | Ninguno.
| null password | Permitir el acceso de usuarios con contraseña nula. | no
| map to guest | Indica cuando un acceso debe considerarse como invitado. | Never | 
| hosts allow | Permite restringir los ordenadores y/o redes que pueden acceder al servidor. | Permiso a todos los ordenadores. | 
| log file | Fichero de log | /var/log/samba/log.%m | 
| max log file | Tamaño máximo del fichero de log | 50 kBytes | 

Un sencillo ejemplo de sección [global] es el siguiente:

```
[global]
workgroup = WORKGROUP
server string = SAMBA %v en %L
server role = standalone server
netbios name = glup
security = user
passdb backend = tdbsam
smb passwd file = /var/lib/samba/private/passdb.tdb
encrypt passwords = yes
map to guest = Never
hosts allow = 192.168.13. 192.168.56.
```

Donde `%v` se sustituye por la versión de SAMBA y `%L` por el nombre del ordenador SAMBA. Mas adelante tenemos una tabla de equivalencia de estas variables.

## 3.2. Resto de secciones 

En el resto de secciones podemos diferenciar en tres tipos: 

Por una parte, la sección `[homes]` define un recurso de red para cada usuario conocido por SAMBA y lo asocia al directorio raíz de cada usuario del ordenador servidor de SAMBA. Esta sección funciona como opción por defecto, pues un servidor SAMBA intenta primero comprobar si existe un servicio con ese nombre. Si no se
encuentra, la solicitud se trata como un nombre de usuario y se busca en el fichero local de contraseñas. Si el nombre existe y la clave es correcta, se crea un servicio, cuyo nombre se cambia de [homes] al nombre local del usuario y, si no se especifica otro valor, se utiliza el directorio raíz del usuario.

Por otra parte, la sección `[printers]` define un recurso compartido para cada nombre de impresora conocida por SAMBA, que suelen ser las impresoras especificadas en el fichero `/etc/printcap` del ordenador. Su funcionamiento es similar al de la sección [homes].

Por último, cualquier otro recurso (directorio o impresora) que se quiera compartir, debe especificarse creando una sección adicional en el fichero de configuración, donde el nombre de la sección se corresponderá con el nombre con el que el recurso será conocido en la red. **Aqui es donde nosotros trabajaremos habitualmente**

<div align="center">
    <img src="../img/samba/samba-04.png" alt="Samba" width="50%" />
</div>

Las principales opciones de configuración de estas secciones son:

| Opción | Descripción | Valor por defecto |
| --- | --- | --- |  
| **read only** | Exportación del recurso en solo lectura. | yes |  
| **writeable** | Exportación del recurso en modo escritura | no |
| **browseable** | Define si se permitirá mostrar este recurso en las listas de recursos compartidos de Windows | yes |
| **path** | Ruta absoluta al recurso | Ninguno. |
| **comment** | Descripción del servicio. | Ninguno. |
| **guest ok** | Define si se permitirá el acceso como usuario invitado. El valor puede ser Yes o No. | no |
| guest account | Usuario que identifica el acceso como invitado. | nobody |
| guest only | Todos los accesos se realizan como invitado | no |
| public | Es un equivalente de guest ok, es decir define si se permitirá el acceso como usuario invitado. El valor puede ser Yes o No. | no |
| force user | Fuerza a que el acceso al recurso se realice como el usuario especificado. | Ninguno |
| force group | Fuerza a que el acceso al recurso se realice como el grupo especificado. | Ninguno. |
| **hosts allow** | Lista de ordenadores desde el que se permite el acceso | Lista vacía (todos los ordenadores). |
| hosts deny | Lista de ordenadores a los que se les deniega el acceso. | Lista vacía (ningún ordenador). |
| printable | Indica si un dispositivo compartido es una impresora. | no |
| **valid users** | Define los usuarios o grupos, que podrán acceder al recurso compartido. Los valores pueden ser nombres de usuarios separados por comas o bien nombres de grupo antecedidos por una @. Ejemplo: fulano, mengano, @administradores | Lista vacía (todos los usuarios). |
| write list | Define los usuarios o grupos, que podrán acceder con permiso de escritura. Los valores pueden ser nombres de usuarios separados por comas o bien nombres de grupo antecedidos por una @. Ejemplo: fulano, mengano, @administradores | Lista vacía (todos los usuarios). |
| **admin users** | Define los usuarios o grupos, que podrán acceder con permisos administrativos para el recurso. Es decir, podrán acceder hacia el recurso realizando todas las operaciones como super-usuarios. | Lista vacía (ningún usuario). |
| **directory mask** | Es lo mismo que directory mode. Define qué permiso en el sistema tendrán los subdirectorios creados dentro del recurso. Ejemplos: 1777 | Definido por sistema | 
| **create mask** | Define que permiso en el sistema tendrán los nuevos archivos creados dentro del recurso. Ejemplo: 0644 | Definido por sistema | 
| follow symlinks | Permite el seguimiento de enlaces simbólicos | Yes |

`read only` y `writeable` se refieren al modo de exportación del recurso, solo que de forma inversa, así el
equivalente de `read only = yes` es `writeable = no`.

> **IMPORTANTE**: A la hora de compartir no solo debemos tener en cuenta los parámetros `read only` y `writeable` para establecer permisos, también **debemos tener en cuenta los permisos de las carpetas compartidas**, de forma similar a lo visto con **NFS**

En el siguiente ejemplo, en el cual se exportan los directorios raíz de los usuarios:
```
[homes]
comment = Directorios raiz de los usuarios
browseable = yes
writeable = yes
```

O en este otro ejemplo, en el que se exportan las impresoras conectadas al servidor de SAMBA:

```
[printers]
comment = Impresoras
path = /var/spool/samba
browseable = no
guest ok = no
writeable = no
printable = yes
```

Otro ejemplo, en el que se exporta un directorio temporal para que los usuarios puedan escribir en el mismo, es el siguiente:

```
[tmp]
comment = Espacio temporal de disco
path = /tmp
browseable = yes
read only = no
guest ok = yes
```

Otro ejemplo de carpeta compartida en el que se permite acceso total a todo el mundo.

```
[compartido]
comment = Archivos Compartidos
path = /home/compartido
guest ok = yes
browseable = yes
writeable = yes
create mask = 0777
directory mask = 0777
```

## 3.3. Variables SAMBA

Comentar que SAMBA posee una serie de variables, que comienzan por el símbolo `%`, y que permiten especificar de forma variable distintos valores, como pueden ser un conjunto de directorios, etc. Las principales variables son:

| Variable | Definición |
| --- | --- |
| `%a` | Arquitectura del cliente (SAMBA, WinNT, UNKNOWN, etc.) |
| `%I` | Dirección IP del cliente. |
| `%m` | Nombre NetBIOS del cliente. |
| `%M` | Nombre DNS del cliente. |
| `%g` | Grupo primario del usuario en Linux. |
| `%G` | Grupo primario del usuario que requiere el acceso. |
| `%H` | Directorio raíz del usuario en Linux. | 
| `%u` | Usuario en Linux. | 
| `%U` | Usuario que requiere el acceso. | 
| `%p` | Directorio donde montar el recurso compartido. | 
| `%P` | Directorio raíz compartido. | 
| `%S` | Nombre del recurso compartido. | 
| `%d` | Identificador del proceso. | 
| `%h` | Nombre DNS del servidor | 
| `%L` | Nombre NetBIOS del servidor. | 
| `%N` | Directorio raíz del servidor. | 
| `%v` | Versión de SAMBA. | 
| `%R` | Versión del protocolo SMB. | 
| `%T` | Día y hora actual del servidor. | 


# 4. Crear, editar o eliminar usuarios en Samba

Los usuarios que queremos que tengan acceso al servidor Samba deben estar creados como usuarios en nuestro servidor linux, podemos crear un grupo samba y agregar a ese grupo todos los usuarios que tendrán acceso al servidor samba:

Supongamos que tenemos un usuarios ya creado llamado *sergio* y que lo queremos agregar a los usuarios de Samba, para esto ejecutamos el siguiente comando:

```bash
sudo smbpasswd -a sergio
```
Nos aparecerá algo como:

<div align="center">
    <img src="../img/samba/samba-01.png" alt="Samba" width="50%" />
</div>


Además : 

```bash
smbpasswd -d nombre_usuario     # Para deshabilitar un usuario
smbpasswd -e nombre_usuario     # Para habilitar un usuario deshabilitado
smbpasswd -s nombre_usuario     # Para pregunta por el password del usuario
smbpasswd -n nombre_usuario     # Para resetear o variar el password
smbpasswd -x nombre_usuario     # Para borrar un usuario ejecutamos:
```

Si queremos realizar una correspondencia entre usuarios de un sistema Microsoft Windows y usuarios de Linux para el uso de recurso compartidos por *samba*, creamos un nuevo archivo donde estarán todos los usuarios autorizados para conectarse al Servidor de Samba, para esto ejecutamos:

```bash
sudo nano /etc/samba/smbusers
```

En el nuevo archivo copiamos la siguiente línea:

```bash
nombre_en_linux = "Nombre en Windows"
```

Donde `nombre_en_linux` es el nombre del usuario que tenemos en linux en este caso *sergio* y `Nombre en Windows` es el nombre del usuario de red en Windows. Tenemos que agregar una nueva línea por cada usuario que creemos para Samba.

Ahora que ya tenemos a los usuarios creados procedamos a ver como se comparten archivos y directorios.

Para aplicar el uso de estos usuarios en SAMBA, volvemos de nuevo a modificar el archivo `/etc/samba/smb.conf`

Busca la línea que dice:

```
; security = user
```

Y la modificamos por:

```
security = user
username map = /etc/samba/smbusers
```

Con esto lo que estamos haciendo es diciéndole a Samba que vamos a autenticar por usuario y donde está la lista de los usuarios permitidos que fue la que creamos anteriormente.

Para darle acceso a los usuario a sus respectivos directorios home o personales, hacemos lo siguiente:

Buscamos las línea donde dice

```
;[homes]
; comment = Home Directories;
;browseable = no
;valid users = %S
;writable = no
```

Y le quitamos el `;` para descomentarlos, y en `writable` le cambiamos *no* por *yes* para que el usuario pueda escribir en el directorio.

# 5. Verificando la configuración: `testparm`

Siempre que cambiemos la configuración del archivo `smb.conf` debemos ejecutar el siguiente comando:

```
testparm
```

lo que hace este parámetro es verificar que los parámetros del archivo `smb.conf` estén correctos. 

<div align="center">
    <img src="../img/samba/samba-05.png" alt="Samba" width="60%" />
</div>

Una vez sepamos que nuestro fichero de configuración esta correcto, podemos aplicar cambios mediante el reinicio del servicio: 

```bash
sudo systemctl restart smbd.service 
```
<div align="center">
    <img src="../img/samba/samba-06.png" alt="Samba" width="60%" />
</div>

# 6. Conectando con el Servidor Samba

Existen varias formas de acceder a un servidor Samba, dependiendo de la plataforma y los requisitos específicos de acceso. A continuación, se describen algunas de las formas comunes de acceder a un servidor Samba:


## 6.1. Accediendo desde un cliente GNU/Linux

Veamos diferentes forma de acceso

### 6.1.1. Usando la Línea de Comandos

Utiliza el comando `smbclient` en la terminal de Linux para acceder al servidor **Samba**. 

Para ello, en primer lugar debes instalar el paquete ***smblient*** en el cliente

Para su uso: 

```bash 
smbclient //nombre_del_servidor/nombre_del_recurso -U usuario
```
Luego, se te pedirá que ingreses la contraseña del usuario.

Debemos tener en cuenta que si utilizamos en esta forma de acceder, entraremos en la shell smb, que no es ni bash ni sh. Para más información sobre los comandos disponibles, consulta la documentación de [samba](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)

<div align="center">
    <img src="../img/samba/samba-07.png" alt="Samba" width="60%" />
</div>

### 6.1.2. Desde el explorador de archivos

Si tenemos el paquete ***smbclient*** instalado, podemos acceder desde el explorador de archivos: 

En este caso accedemos desde el explorador de archivos, pulsamos en `+ Otras Ubicaciones` e insertamos la ruta del servidor precedido de `smb://`

<div align="center">
    <img src="../img/samba/samba-08.png" alt="Samba" width="60%" />
</div>

nos pedirá la autentificación 

<div align="center">
    <img src="../img/samba/samba-09.png" alt="Samba" width="60%" />
</div>

y accedemos al recurso: 

<div align="center">
    <img src="../img/samba/samba-10.png" alt="Samba" width="60%" />
</div>


### 6.1.3. Montando el recurso compartido con `mount`

Podemos utilizar el comando `mount` para montar un recurso compartido **Samba** en un directorio local. Por ejemplo:

```bash
sudo mount -t cifs //nombre_del_servidor/nombre_del_recurso /ruta_de_montaje -o username=usuario
```
Luego, se te pedirá que ingreses la contraseña del usuario.

<div align="center">
    <img src="../img/samba/samba-11.png" alt="Samba" width="80%" />
</div>

De esta forma tenemos el recurso accesible *mientras no cerremos nuestra sesión* 

> **Cuidado**: para poder montar unidades **Samba**, debemos tener instalado el paquete `cifs-utils`, en caso contrarío nos dará un error que nos puede llevar a engaño: 

<div align="center">
    <img src="../img/samba/samba-12.png" alt="Samba" width="80%" />
</div>


### 6.1.4. Montando el recurso compartido permanentemente

Para montar permanentemente, debemos incluir en `/etc/fstab` los puntos de montajes.

En este caso, tenemos una gran variedad de opciones a la hora de montar:

```
//192.168.56.254/compartido  /mnt/compartido  cifs  guest,uid=1000  0  0
```

En este caso: 
- `guest`: se monta como usuarios invitado, no se necesita password
- `ùid=1000`: se accede como si se tratara del uid 1000

```
//192.168.56.254/compartido  /mnt/compartido  cifs  user=sergio,password=sergi@,noexec,user,rw,nounix,iocharset=utf8 0 0
```

En este caso, incluimos las credenciales en el propio, fichero, accesibles a todo el mundo.

Si queremos proteger los datos del usuario, usaremos el archivo `.smbcredentials`

#### 6.1.4.1. usando `.smbcredentials`

Se puede crear un archivo para almacenar las credenciales de acceso al recurso compartido Samba de forma segura. 

Creamos un archivo, por ejemplo, ~/.smbcredentials, con el siguiente contenido:

```
username=sergio
password=sergi@
```
y le aplicamos permisos restrictivos a este fichero:

```bash
chmod 600 ~/.smbcredentials.
```

En en archivo `/etc/fstab`:

```
//192.168.56.254/compartido  /mnt/compartido  cifs  credentials=/home/sergio/.smbcredentials,uid=1000,gid=1000  0  0
```

y ya podemos montar 

```bash
sudo mount -a
```

Si has configurado correctamente todo, el recurso compartido debería montarse en el punto especificado.

<div align="center">
    <img src="../img/samba/samba-13.png" alt="Samba" width="50%" />
</div>


## 6.2. Accediendo desde un cliente Windows

Desde un Explorador de Archivos en Windows:
- Abre el Explorador de Archivos en Windows.
- En la barra de direcciones, escribe la dirección del servidor Samba precedida por `\\` (por ejemplo, \\nombre_del_servidor o \\dirección_ip_del_servidor).
- Se te pedirá que introduzcas las credenciales de usuario y contraseña del servidor Samba.


## 6.3. Otras formas de acceso

- *A través de Aplicaciones de Cliente*:

Puedes utilizar aplicaciones específicas para acceder a recursos compartidos de Samba. Por ejemplo, en entornos Windows, puedes usar el "Mapa de Unidades de Red" para asignar una letra de unidad a un recurso compartido.

- *Desde Dispositivos Móviles*:

Utiliza aplicaciones de gestión de archivos en dispositivos móviles que admitan el protocolo SMB para acceder a los recursos compartidos Samba. Algunas aplicaciones permiten explorar y transferir archivos de manera similar a como lo harías en un Explorador de Archivos.

Es importante tener en cuenta que el acceso a un servidor Samba requiere configurar adecuadamente los recursos compartidos y establecer permisos de acceso. Además, la seguridad del acceso se puede gestionar mediante autenticación de usuarios y configuración de contraseñas en el servidor Samba.


https://www.blai.blog/2021/08/acceder-carpeta-samba-linux-active-directory.html



# 7. **Webmin**: Herramienta de Administración Web

**Webmin** es una interfaz web de administración de sistemas diseñada para simplificar y facilitar la gestión de servidores Unix-like, incluyendo sistemas basados en Linux como Ubuntu Server. Proporciona una interfaz gráfica intuitiva que permite a los administradores de sistemas configurar, controlar y supervisar diversos aspectos del servidor a través de un navegador web.

Características Principales:

1. **Configuración del Sistema:**
   - Ajuste de configuraciones del sistema, como la red, el almacenamiento y la hora del sistema.

2. **Usuarios y Grupos:**
   - Gestión de usuarios y grupos, creación y eliminación de cuentas, y asignación de permisos.

3. **Administración de Paquetes:**
   - Instalación, actualización y gestión de paquetes de software en el sistema.

4. **Configuración de Servicios:**
   - Control y configuración de servicios del sistema, como Apache, MySQL, BIND, etc.

5. **Firewall y Configuración de Red:**
   - Configuración del firewall y ajustes relacionados con la red.


**Instalación en Ubuntu Server:**

Para la instalación de Webmin, se recomienda utilizar los pasos indicados en la propia web de [webmin](https://webmin.com/download/)

Segùn podemos comprobar, los pasos serán los siguientes (en ubuntu):

```bash
# descarga de script que añade repositorios de webmin
curl -o setup-repos.sh https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh

# ejecución de script de repositorios
sudo sh setup-repos.sh

# procedemos con la instalación.
sudo apt-get install webmin --install-recommends

```


Para cceder a la Interfaz Web
  - Abre tu navegador web y visita `https://tu_direccion_ip_o_nombre_de_domino:10000/`
  - Inicia sesión con las credenciales de administrador del sistema.

<div align="center">
    <img src="../img/samba/webmin-01.png" alt="Samba" width="60%" />
</div>

> Notas Importantes:
> - Webmin utiliza el puerto 10000 por defecto y requiere una conexión segura (HTTPS).
> - Asegúrate de que el tráfico hacia el puerto 10000 esté permitido en el firewall.
> - Protege adecuadamente el acceso a la interfaz web de Webmin.

**Webmin** proporciona una forma conveniente de administrar servidores Linux a través de una interfaz gráfica, lo que puede ser especialmente útil para aquellos que prefieren una experiencia de administración basada en la web.

<div align="center">
    <img src="../img/samba/webmin-02.png" alt="Samba" width="80%" />
</div>

Recursos Adicionales:
- [Sitio web oficial de Webmin](http://www.webmin.com/)
- [Documentación de Webmin](http://doxfer.webmin.com/Webmin/Main_Page)




---

**Fuentes:**

- [Samba. Página oficial](https://www.samba.org/samba/docs/)
- [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview)
- [Instalar y configurar Samba Share en Ubuntu 22.04, 20.04 y 18.04](https://es.linux-console.net/?p=21480)
- [Teycen: Configurar Samba en Ubuntu para compartir archivos e impresoras en redes Windows](http://www.teycen.com/pages/linux/linux_samba.php)
- [Guía Ubuntu: Samba](https://www.guia-ubuntu.com/index.php/Samba#Para_compartir_una_carpeta)
- [Jose Castillo: Samba](https://castilloinformatica.es/wiki/index.php?title=Samba)
- [YouTube: Uso de Samba para compartir carpetas con equipos Windows](https://www.youtube.com/watch?v=2fF1etV7iYY)
- [carpetas compartidas con samba para usuarios de AD ](https://linux-l.listas.softwarelibre.narkive.com/iXaaYzcN/carpetas-compartidas-con-samba-para-usuarios-de-ad)

- [DigitalOcean: How To Install Webmin on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-webmin-on-ubuntu-22-04)
- [HowtoForge; Cómo instalar Webmin con el certificado SSL gratuito Let’s Encrypt en Ubuntu 22.04](https://howtoforge.es/como-instalar-webmin-con-el-certificado-ssl-gratuito-let-s-encrypt-en-ubuntu-22-04/)