---
title: Linux
description: Tips Redes
permalink: /redes/
---


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
ip -br a
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



