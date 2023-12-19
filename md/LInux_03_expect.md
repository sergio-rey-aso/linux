
El scripting en Linux con expect es una técnica que permite automatizar tareas repetitivas en sistemas, tanto de forma local como remota, que requieren introducir información manualmente. Expect es un programa que habla a otros programas a través de un script. Siguiendo este script, Expect sabe qué salida esperar del programa que ejecuta y responder en consecuencia y, si procede, es posible devolver el control al usuario o revocarlo. Resulta muy útil para automatizar tareas repetitivas en sistemas, tanto de forma local como remota, que requieren introducir información manualmente, más aún cuando trabajamos con instrucciones y protocolos como SSH, SCP, SFTP, TELNET o RLOGIN.

Para utilizar expect, es necesario escribir un script que describa la secuencia de comandos a introducir. En el script, se pueden definir variables, bucles, condiciones y otros elementos de programación para automatizar tareas complejas. Por ejemplo, se puede escribir un script que se conecte a una lista de equipos y muestre el listado de usuarios conectados en cada uno de los equipos. El listado de usuarios se puede guardar en un archivo ubicado en el equipo donde se ejecuta el script.

Aquí hay un ejemplo de script de expect que se conecta a una lista de equipos y muestra el listado de usuarios conectados en cada uno de los equipos. El listado de usuarios se guarda en un archivo ubicado en el equipo donde se ejecuta el script.

```bash
#!/usr/bin/expect -f

# Define the list of hosts
set hosts [list host1 host2 host3]

# Define the username and password
set username "your_username"
set password "your_password"

# Define the command to run on each host
set command "who"

# Define the output file
set output_file "output.txt"

# Loop through the list of hosts
foreach host $hosts {
    # Connect to the host
    spawn ssh $username@$host

    # Wait for the password prompt
    expect "password: "

    # Send the password
    send "$password\r"

    # Wait for the command prompt
    expect "$ "

    # Run the command and save the output to a file
    send "$command > $output_file\r"

    # Wait for the command to finish
    expect "$ "

    # Exit the SSH session
    send "exit\r"

    # Wait for the session to close
    expect eof
}
```

Este script se conectará a cada uno de los equipos en la lista hosts y ejecutará el comando who en cada uno de ellos. El listado de usuarios conectados se guardará en el archivo output.txt en el directorio local.




Aquí hay un ejemplo de script de expect que utiliza el comando scp para copiar archivos de un equipo remoto a un equipo local. Este script se conecta a un equipo remoto, copia un archivo y lo guarda en un directorio local.

```bash
#!/usr/bin/expect -f

# Define the remote host, username, and password
set remote_host "remote_host"
set remote_username "remote_username"
set remote_password "remote_password"

# Define the local directory to save the file
set local_directory "/path/to/local/directory/"

# Define the remote directory and file to copy
set remote_directory "/path/to/remote/directory/"
set remote_file "file.txt"

# Connect to the remote host
spawn ssh $remote_username@$remote_host

# Wait for the password prompt
expect "password: "

# Send the password
send "$remote_password\r"

# Wait for the command prompt
expect "$ "

# Copy the file to the local directory
send "scp $remote_username@$remote_host:$remote_directory$remote_file $local_directory\r"

# Wait for the password prompt
expect "password: "

# Send the password
send "$remote_password\r"

# Wait for the command prompt
expect "$ "

# Exit the SSH session
send "exit\r"

# Wait for the session to close
expect eof
```

Este script se conectará a un equipo remoto y copiará el archivo file.txt del directorio remoto /path/to/remote/directory/ al directorio local /path/to/local/directory/.



Ejercicio en el que debas conectarte con todos los equipos tu fila de compañeros, que ejecutes un comando generando un fichero con tu nombre y que posteiormente obtengas el fichero generado mediante scp