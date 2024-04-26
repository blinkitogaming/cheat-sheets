# Habilitar registro de contenedores vía HTTP

Necesario si queremos que funcione en un servidor local de Gitlab a través de HTTP.

Pasos a seguir:

### Paso 1
Editar el archivo `/etc/docker/daemon.json`

```shell
{
    "insecure-registries" : [ "IP_o_DOMINIO_GITLAB_SERVER:5000" ]
}
```

### Paso 2
Reiniciar el daemon de docker:

```shell
sudo service docker restart
```

Si ejecutamos el comando `docker info` veremos que la línea que hemos añadido aparece listada de la siguiente forma:

```shell
Insecure Registries:
  IP_o_DOMINIO_GITLAB_SERVER:5000
```