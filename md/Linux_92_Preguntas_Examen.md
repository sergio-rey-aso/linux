Fichero /etc/export donde tiene que poner cómo compartir con una red en concreto. Dada la siguiente configuración, qué se debe cambiar para compartir únicamente con la red del aula (192.168.13.0/24


¡Por supuesto! Aquí están las preguntas de nuevo, esta vez con la respuesta correcta marcada:

1. **¿Cuál es el propósito principal de LDAP en un entorno Linux?**
   - a. Gestión de paquetes
   - b. Autenticación de usuarios
   - c. Configuración del firewall
   - d. Compilación de kernel
   - **Respuesta correcta: b. Autenticación de usuarios**

2. **¿Qué significa la sigla DN en el contexto de LDAP?**
    - a. 
    - b. Directory Name
    - c. 
    - d. 
    - **Respuesta correcta: d. Distinguished Name**

3. **¿Cuál es la función de un DN en LDAP?**
   - a. 
   - b. 
   - c. Establecer una nueva contraseña
   - d. 
   - **Respuesta correcta: b. Identificar de manera única una entrada en el directorio**

25. **¿Qué comando se utiliza para buscar información en un servidor LDAP desde la línea de comandos en Linux?**
   - a. `ldapsearch`
   - b. ``
   - c. ``
   - d. ``
   - **Respuesta correcta: a. ldapsearch**

26. ****
   - a. 
   - b. 
   - c.  
   - d. CSV
   - **Respuesta correcta: c. LDIF (LDAP Data Interchange Format)**

27. **¿Qué comando se utiliza para agregar una nueva entrada al directorio LDAP desde la línea de comandos en Linux?**
   - a. ``
   - b. `ldapadd`
   - c. `ldifadd`
   - d. `ldapcreate`
   - **Respuesta correcta: b. ldapadd**


17. **¿Cuál es el propósito de la entrada `dc` en un DN LDAP?**
    - a. 
    - b. 
    - c. Representar el dominio de directorio
    - d. 
    - **Respuesta correcta: c. Representar el dominio de directorio**


20. **¿Cuál es el comando utilizado para iniciar y detener el servicio OpenLDAP en Linux?**
    - a. `startslapd` y `stopslapd`
    - b. 
    - c. 
    - d. `systemctl start slapd` y `systemctl stop slapd`
    - **Respuesta correcta: d. `systemctl start slapd` y `systemctl stop slapd`**


21. **¿Qué comando se utiliza para comprobar que el servicio LDAP esta configurado y todas sus entradas?**
    - a. `ldapstatus`
    - b. `ldapquery`
    - c. `ldapcat`
    - d. `statusldap`
    - **Respuesta correcta: c. `ldapcat`**


28. **¿Qué comando se utiliza para agregar un nuevo usuario al directorio LDAP desde la línea de comandos en Linux?**
    - a. `adduserldap`
    - b. `ldapadduser`
    - c. `ldifadduser`
    - d. `ldapadd`
    - **Respuesta correcta: d. `ldapadd`**







Script en powerShell


```powershell
$dominio = "dc=DOM-SERGIO,dc=local"
$pathOUExamen = "ou=ouExamen,$dominio"
$ficheroCSV = Import-Csv -Path "./DatosExamen01.csv" -Delimiter ',';

ForEach($linea in $ficheroCSV){
    $variable_1 = $linea.columna_1
    $variable_2 = $linea.columna_2
    $variable_3 = $linea.columna_3
    $variable_4 = $linea.columna_4

    switch ($variable_1) {
        "nuevo" { 
            if (-not (Get-ADOrganizationalUnit -Filter "Name -eq '$variable_3'")) {
                New-ADOrganizationalUnit -Name $variable_3 -Path $pathOUExamen -ProtectedFromAccidentalDeletion $false -ErrorAction Stop
            }
            
            $pathLinea = "ou=$variable_3,$pathOUExamen"
            $contraseña = ConvertTo-SecureString -String "Contraseña123" -AsPlainText -Force
            New-ADUser -Name $variable_2 -AccountPassword $contraseña -Path $pathLinea -Enabled $true -ErrorAction Stop

            if (-not (Get-ADGroup -Filter "Name -eq '$variable_4'")) {
                New-ADGroup -Name $variable_4 -GroupScope Global -Path $pathLinea -ErrorAction Stop
            }
            Add-ADGroupMember -Identity $variable_4 -Members $variable_2 -ErrorAction Stop
         }

        "eliminar" { 
            Remove-ADUser -Identity $variable_2 -Confirm:$false 
        }

        Default { 
            Write-Host "Accion no válida: $accion" -ForegroundColor Yellow 
        }
    }
}
```

Preguntas 
1. Nuestro dominio se llama DOM-EXAMEN.local. Completa el valor de la variable $domino (1)
2. En la variable pathOUExamen queremos indicar una unidad organizativa llamada ouExamen que esta directamente dentro del dominio. Completa el valor necesario utilizando la variable $domino (2)
3. Completa el comando necesario en la línea 4 (3)
4. Explica de forma esquemática y detallada qué hace el script.
5. Si en nuestro dominio tenemos los siguientes usuarios 
   1. ISO: Isabel, Ignacio
   2. ASO: Antonio, Ana y tu (tu nombre)
6. Escribe un fichero para que se realice con estos usuarios el primer caso del switch (nuevo)
7. Escribe un fichero para que se realice con estos usuarios el segundo caso del switch (eliminar)

