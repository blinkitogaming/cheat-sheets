# YAML validation
Para validar archivos yaml en la terminal de Linux podemos usar `yamllint`.

Pasos a seguir:

### Paso 1
Instalar `yamllint`

```shell
sudo apt-get install yamllint
```

### Paso 2
Para ponerlo en marcha sólo tenemos que usar su comando seguido de la ruta al archivo que queremos comprobar.

```shell
yamllint /ruta/al/archivo.yaml
```

Si cuando lo ejecutes encuentra algún error verás un resumen con el resultado. Algo similar a esto:

```shell
/ruta/al/archivo.yaml
8:1       error    too many blank lines (1 > 0)  (empty-lines)
```

Si no ha encontrado ningún error en el archivo devolverá una línea de terminal.