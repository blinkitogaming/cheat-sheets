### /etc/wsl.conf

Crea un archivo *wsl.conf* en la siguiente ruta:

    /etc/wsl.conf

Y añade las siguientes líneas:

    [user]
    default=<nombre_usuario>

Cierra la terminal y ejecuta el siguiente comando en *PowerShell* o *CMD*:

    wsl --terminate <nombre_distro>

Cuando abras una nueva terminal de WSL el nuevo usuario por defecto será el que hayas especificado.