# Usar comandos de Docker en la Terminal sin usar "sudo"
Después de instalar Docker en nuestro servidor, con estos pasos podremos usar los comandos de Docker en la terminal sin la necesidad de estar escribiendo sudo todo el tiempo.

## Pasos a seguir

Usa los siguientes comandos:

    sudo groupadd docker
    sudo usermod -aG docker $USER
    newgrp docker

Para probar si ha funcionado podemos usar el siguiente comando para levantar un contenedor de "Hola mundo":

    docker run hello-world

Si todo ha ido podremos ver que el contenedor está levantado y funcionando con el comando:

    docker ps

Después de haber comprobado que funciona podemos eliminar este contenedor de prueba. Para ello usaremos los siguientes comandos:

Primero, paramos el contenedor:

    docker stop hello-world

Después lo eliminamos:

    docker kill hello-world