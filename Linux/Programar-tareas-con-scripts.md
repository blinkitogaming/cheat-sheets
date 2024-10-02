# Programar tareas en un servidor mediante el uso de scripts y crontab
Con estos pasos podremos crear un script en nuestro servidor y programar una tarea para que se repita en el tiempo.

## Pasos a seguir

#### Paso 1
Creamos un directorio donde almacenaremos nuestros scripts. En mi caso estará dentro de la carpeta de mi usuario. Para ello:

Creamos el directorio para los scripts:

    mkdir ~/scripts


#### Paso 2
Ahora creamos el script que queramos con el comando:

    touch ~/scripts/nombre_del_script.sh

Para que el script pueda ser ejecutable debe empezar en la primera lína con `#!/bin/bash`

Después abrimos el script con un editor de texto para poder editarlo:

    nano ~/scripts/nombre_del_script.sh


#### Paso 3
Cuando tengamos listo nuestro script lo guardamos con las teclas `CTRL+O` y cerramos el editor de texto con `CTRL+X`

#### Paso 4
Ahora haremos ejecutable nuestro script con el comando:

    chmod u+x ~/scripts/nombre_del_script.sh

#### Paso 5
En este paso añadiremos nuestro script a crontab para que se ejecute cuando le digamos, para ello:

Con este comando editaremos el archivo de crontab:

    crontab -e

Y aquí crearemos una línea con la siguiente sintaxis:

    * * * * * /ruta/al/script/nombre_del_script.sh

Donde los `*` representan datos que irán especificados por números y quedarán como `*` si no se requiere especificarlo. De forma que:

```
* * * * * /ruta/al/script/nombre_del_script.sh
- - - - -
| | | | |
| | | | ----- Día de la semana (0 - 7) (Domingo=0 or 7)
| | | ------- Mes (1 - 12)
| | --------- Día del mes (1 - 31)
| ----------- Hora (0 - 23)
------------- Minuto (0 - 59)
```
Por ejemplo, si quieres que tu script se ejecute cada día a las 3 AM sería así:

    0 3 * * * /root/backup.sh

Os dejo una [herramienta](https://crontab.guru/) para calcular la sintaxis.
