# Ubuntu Server: Añadir una nueva tarjeta de red.

Si hemos instalado Ubuntu server y despues añadimos una nueva tarjeta, para que se asigne IP, ncesitamos añadir la tarjeta a `netplan`, así que debemos editar el fichero `/etc/netplan/01.....`  y ahí añadimos por ejemplo que es dhcp

Ejemplo de fichero ```netplan```

```
network:
    ethernets:
        enp0s3:
            dhcp4: false
            addresses: [192.168.1.202/24]
            gateway4: 192.168.1.1
            nameservers:
              addresses: [8.8.8.8,8.8.4.4,192.168.1.1]
    ethernets:
        enp0s8:
            dhcp4: true
    version: 2
```

Para aplicar la nueva configuración 

```
 sudo netplan --debug apply
```

y comprobamos que ha funcionado:

```
ip .br a
```

### Mas operaciones interesantes con tarjetas de red y herramienta `ip`

#### Si no funciona una tarjeta y la queremos habilitar y añadir una `IP`

- Levantamos la conexión del intefaz:

```
ip link set vboxnet0 up
```

- Asignamos manualmente una IP a la interfaz:

```
ip addr add 192.168.56.1/24 dev vboxnet0
```

- Si todavía no conseguimos hacer ping, entonces añadimos la ruta:

```
ip route add 192.168.56.0/24 dev vboxnet0
```

- Si queremos deshabilitar `IPv6`.

```bash
ip -6 addr flush eth0
```

- Todo lo que hacemos con la herramienta `ip` desaparece al reiniciar el sistema.



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


