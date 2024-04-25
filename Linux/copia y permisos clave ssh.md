# Conexión a servidor remoto con clave SSH
Con este comando copiaremos una clave SSH pública y la copiaremos a un servidor remoto dentro del archivo *~/.ssh/authorized_keys*.

Nos será útil para posteriormente poder conectarnos por SSH al servidor remoto sin necesidad de introducir la contraseña.

En mi caso lo uso para poder hacer deploy de archivos mediante CI/CD en Gitlab.

## Pasos a seguir

#### Paso 1
Genera una clave SSH con el siguiente código:

    ssh-keygen -m PEM -t rsa

Sigue los pasos estableciendo un nombre para la pareja de archivos de la clave (*NOMBRE_CLAVE* y *NOMBRE_CLAVE.pub*).

#### Paso 2
Ahora con el siguiente comando copiaremos el contenido del archivo de clave pública (*.pub*) al servidor remoto, dentro del archivo *authorized_keys* en la ruta *~/.ssh/*

    sudo cat ~/.ssh/<NOMBRE_CLAVE>.pub | ssh <USUARIO>@<IP_SERVIDOR_REMOTO> "touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"

Después de esto ya podremos conectarnos por ssh al servidor remoto sin necesidad de introducir la contraseña.

Para ello sólo tendremos que usar el comando ssh de la siguiente forma:

    ssh -i ~/.ssh/<NOMBRE_CLAVE> <USUARIO>@<IP_SERVIDOR_REMOTO>

Un ejemplo del comando sería:

    ssh -i ~/.ssh/miclave root@192.168.10.10