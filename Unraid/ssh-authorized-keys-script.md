# Mantener ssh authorized_keys después de reiniciar
Con esto conseguiremos que las claves ssh almacenadas en el fichero `.ssh/authorized_keys` se mantengan intactas después de reiniciar el servidor.

### Paso 1
En Linux, usa el comando `ssh-keygen` con los parámetros necesarios para generar el par de llaves ssh.
*Nota: puedes darle el nombre que quieras al par de claves para identificarlas*
Se generarán dos archivos:
- Una clave privada: id_rsa o nombre_clave_elegido
- Una clave pública: id_rsa.pub o nombre_clave_elegido.pub

### Paso 2
En el servidor unRAID, ve al fichero `/root/.ssh` (crea el fichero si no existe).

Edita el fichero:

`/root/.ssh/authorized_keys` (crea el archivo si no existe).

Copia el contenido de la clave pública (`id_rsa.pub` o `nombre_clave_elegido.pub`) dentro del fichero `authorized_keys`.

Si necesitas añadir varias claves puedes hacerlo copiando cada una de ellas en una línea nueva.

### Paso 3
Cambia los permisos del archivo `authorized_keys` a 600:

```shell
chmod 600 /root/.ssh/authorized_keys
```

Ahora puedes hacer ssh a tu servidor unRAID sin necesidad de introducir la contraseña.

### Paso 4
Para hacer que estos cambios sean permanentes incluso después de un reinicio del servidor nos valdremos del plugin `User Scripts`.

Ve a `Settings -> User Scripts` y crea un nuevo script con el nombre que desees.

Edita el script y añade lo siguiente:

```shell
#!/bin/bash

SSH_DIR=/root/.ssh

mkdir ${SSH_DIR}
chmod 755 ${SSH_DIR}
cp /boot/config/ssh/authorized_keys ${SSH_DIR}/authorized_keys
chmod 600 ${SSH_DIR}/authorized_keys
```

Guarda los cambios.

Sólo falta establecer la programación del script para que se ejecute con cada primer inicio del Array.

`At First Array Start Only`

Ahora debería seguir funcionando incluso después de reniciar el servidor.