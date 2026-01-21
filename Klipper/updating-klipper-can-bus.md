# Actualizar Klipper con conexión CANBUS
Aquí partimos de la base de que tenemos ya un sistema que está conectado por CANBUS de forma estable. No aplica para nuevas instalaciones.

Primero, necesitamos actualizar todos los componentes del sistema desde la interfaz web, ya sea Mainsail o Fluidd.
Una vez realizado este paso, es posible que el sistema muestre un error de comunicación. Esto es debido a que la versión de Klipper de nuestro host es distinta de la del resto de componentes (placa del cabezal, probe, Raspberry o CB1/CB2, placa base principal...).

Ahora pasaremos a actualizar la versión de Klipper para cada componente.

Para ello, necesitaremos conectarnos por `ssh` a nuestra Raspberry/CB1/CB2.

## 1. Placa del cabezal - Toolhead board (en mi caso EBB36 v1.2)
### Paso 1
Vamos al directorio klipper:
```shell
cd ~/klipper
```

Accedemos al menú de configuración de klipper:
```shell
make menuconfig
```

Podemos encontrar los parámetros necesarios dependiendo de nuestra placa [aquí](https://canbus.esoterical.online/toolhead_flashing/common_hardware).

Aquí necesitamos establecer los parámetros recomendados para el modelo de placa que estemos usando en nuestro cabezal.
Estableceremos la interfaz de comunicación como 'CAN bus' con los pins que se especifique en cada caso. También estableceremos la velocidad del CAN bus a la misma que hayamos configurado en nuestro archivo can0 (en mi caso es 1000000).
Si no sabes de qué estoy hablando, revisa la esta [guía](https://canbus.esoterical.online/).

Una vez que tengamos lista la configuración del firmware, pulsaremos Q para salir de este menú. Pulsaremos Y para guardar los cambios.

Ahora escribiremos el siguiente comando para asegurarnos de que hemos borrado cualquier archivo compilado anteriormente y, por tanto, antiguo. El nuevo archivo compilado se guardará en `~/klipper/out/klipper.bin`
```shell
make clean
make
```

### Paso 2
Primero, pararemos el servicio de Klipper con este comando:
```shell
sudo service klipper stop
```

Ahora rescataremos el canbus_uuid de nuestro archivo de configuración de la placa del cabezal, en el apartado [mcu NOMBRE_QUE_HAYAMOS_ELEGIDO]. Y una vez lo tengamos, forzaremos su reinicio en modo Katapult con el comando:
```shell
python3 ~/katapult/scripts/flashtool.py -i can0 -r -u yourtoolheaduuid
```

En este punto seguramente nos de un mensaje de `Flash Success` o `Bootloader Request Complete`, pero *ESTO NO HA FLASHEADO NADA, NECESITAMOS CONTINUAR CON LOS SIGUIENTES PASOS*.

### Paso 3
Podemos verificar que la placa se encuentra en el modo correcto con el siguiente comando. Si nos devuleve `Detected UUID: xxxxxxxxx, Application: Katapult` el dispositivo estará listo para continuar:
```shell
python3 ~/katapult/scripts/flashtool.py -q

```

### Paso 4
Ahora podemos usar el siguiente comando para flashear Klipper:
```shell
python3 ~/katapult/scripts/flashtool.py -i can0 -f ~/klipper/out/klipper.bin -u yourtooolheaduuid
```

Una vez que se haya completado, podremos ver que al usar este comando de nuevo, esta vez nos devolverá el mismo UUID, pero con `Application: Klipper` en lugar de `Application: Katapult`:
```shell
python3 ~/katapult/scripts/flashtool.py -i can0 -q

```

Si en este punto ves que no existe conexión con la placa del cabezal, entonces deberás comprobar que las opciones seleccionadas a la hora de compilar el firmware de klipper.bin eran correctas. Vuelve al Paso 1, comprueba todas las opciones después del comando `make menuconfig` y vuelve a compilar con `make clean` y `make`, después vuelve a poner la placa en modo katapult como en el Paso 3.

Si todo ha ido bien y te ha devuelto `Application: Klipper`, es hora de iniciar el servicio de Klipper de nuevo en nuestra Raspberry/CB1/CB2 con el comando `sudo service klipper start` y luego hacer un firmware_restart para confirmar que Klipper se inicia sin ningún tipo de error.
```shell
sudo service klipper start
```


## 2. Probe (en mi caso Eddy Duo)
Repite los pasos, pero con el canbus_uuid de este dispositivo.


Una vez que todos los dispositivos estén actualizados, podrás ver desde la interfaz de Mainsail o Fluidd, en el apartado "Machine" que todos los mcu tienen la misma versión de Klipper. Por lo tanto, todo estará bien comunicado y funcionará sin errores.