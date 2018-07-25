# Ract-Native / Expo.io / Android

Debugging remoto con Reat-Native, Expo.io y Android (Dispositivo físico)

### 1.  Configura tu móvil:
Si no lo has hecho [activa las opciones de programador y la depuración USB](https://github.com/maruos/maruos/wiki/USB-Debugging) en el móvil.
    
### 2.  Allow-Control-Allow-Origin:
    
Instalá  [Allow-Control-Allow-Origin](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi): * en Chrome o alguna extensión similar

*NOTA IMPORTANTE:* está opción puede causar conflictos con algunos sitios, se recomienda activar solo cuando se pretende hacer el debug, de lo contrario desactivar en el botón Enable cros-origin resorce sharing de la extensión en chrome.

### 3. Conecta tu dispositivo a través de USB

 ##### 3.1 Conecta tu dispositivo a través de USB a tu máquina de desarrollo.
 ##### 3.2 Verifica el código del fabricante usando en la terminal:
	 

> lsusb 
	 
     
    $ lsusb Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate
     Matching Hub Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0
    root hub Bus 001 Device 003: ID 22b8:2e76 Motorola PCS Bus 001 Device
    002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub Bus 001
    Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub Bus 004 Device
    001: ID 1d6b:0003 Linux Foundation 3.0 root hub Bus 003 Device 001: ID
    1d6b:0002 Linux Foundation 2.0 root hub

	

Estas líneas representan los dispositivos USB que actualmente están conectados a tu máquina. En esté caso el modelo del teléfono está indicado con Motorola PCS.

	`Bus 001 Device 003: ID 22b8:2e76 Motorola PCS`
De la linea anterior toma los primeros cuatro dígitos de la identificación del dispositivo. En teste ejemplo **22b8** de Motorola.
 
 

 ##### 3.3 Determina la regla udev:

Ingresa lo siguiente en la terminal, asegurándote de cambiar el 22b8 por el identificador de tu teléfono.

```
echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="22b8", MODE="0666", GROUP="plugdev"' | sudo tee /etc/udev/rules.d/51-android-usb.rules

```

Verifica que tu móvil se este conectando correctamente a  ADB con el comando `adb devices` en la terminal.

```
$ adb devices
List of devices attached
emulator-5554 offline   # Google emulator
14ed2fcc device         # Physical device

```
Si aparece **device** en la columna de la derecha todo esta listo.

### 4.  Developer menu:
    
 1. Ejecuta el siguiente comando desde la terminal del computador para abrir el menú de desarrollo en el móvil:
	>    adb shell input keyevent 82
 2. Selecciona '**Remote Js Debugging**' en el menú que despliega el móvil.
 3. Abre el [debugger](http://localhost:19001/debugger-ui/) en chrome preciona **Ctrl⇧J** (control/shift/J).
 4. Ejecuta tu proyecto con `exp start` (opcionalmente puedes ejecutar `exp start --lan`).
 5. Abre de forma manual la app de expo en tu dispositivo android.
 6. Listo! ahora deberías poder hacer debugging desde Chrome con JS.
