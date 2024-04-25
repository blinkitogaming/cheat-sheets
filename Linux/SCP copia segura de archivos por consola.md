# Comando scp
Con este comando podemos copiar archivos de forma segura a través de la terminal.

El formato del comando es el siguiente:

    scp /RUTA/ORIGEN/ARCHIVO_A_COPIAR <IP_SERVIDOR_REMOTO>:/RUTA/DESTINO/NOMBRE_ARCHIVO

Como el comando usa ssh, se recomienda previamente haber hecho conexión por SSH con el servidor remoto para evitar la pregunta de si confiamos en el host a la que tenemos que responder con *yes/no*.

Si no hemos realizado conexión previa por ssh y queremos evitar esta comprobación y el correspondiente mensaje con el prompt debemos añadir este flag al comando anterior:

    -o StrictHostKeyChecking=no

Quendando así:

    scp -o StrictHostKeyChecking=no /RUTA/ORIGEN/ARCHIVO_A_COPIAR <IP_SERVIDOR_REMOTO>:/RUTA/DESTINO/NOMBRE_ARCHIVO

*NOTA: esto no es recomendable en un entorno de producción ya que es una práctica poco segura. Evitando que podamos darnos cuenta de un ataque como "man in the middle"*