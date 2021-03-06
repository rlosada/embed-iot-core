# Remote light system

Aplicaci贸n para ESP32 o similar basada en Arduino y compilada con PlatformIO.

## Detalles de la aplicaci贸n 馃攳

Este proyecto es una demostraci贸n abarcativa de las capacidades que tiene un dispositivo embebido para comunicarse por MQTT. Es capaz de enviar y recibir topics, enviar un topic al iniciar para dar aviso al sistema, as铆 como tambi茅n avisar autom谩ticamente si sufre una desconexi贸n (mensaje conocido como LWT). 

Su funcionalidad principal es actuar como un dispositivo de iluminaci贸n dentro de un sistema integral de luces que se controla de manera remota. Puede recibir un topic para controlar individualmente el LED de cada dispositivo asi como tambi茅n se pueden controlar un grupo de dispositivos al mismo tiempo haciendo uso de la capacidad de broadcast de MQTT. Tambi茅n es capaz de informar el estado del dispositivo general, y el estado del LED. Esto puede permitir a sistemas remotos administrar y tener un control sobre el estado de cada dispositivo dentro de la red.

Para probar el c贸digo copia el contenido del archivo `remote_light_system.cpp` y pegalo dentro del archivo `src/main.cpp`. Luego configura el archivo `src/secrets.h` cargando con los valores adecuados los siguientes datos:

```cpp
// ID del dispositivo
#define DEVICE_ID            "YOUR_ID"
// Acceso a WiFi
const char WIFI_SSID[]     = "YOUR_WIFI_SSID";
const char WIFI_PASS[]     = "YOUR_WIFI_PASS";
// Conexion al host MQTT
const char     MQTT_HOST[] = "YOUR_MQTT_HOST"; // i.e: 192.168.0.106
const uint16_t MQTT_PORT   = 1883;
const char     MQTT_USER[] = "YOUR_USER";
const char     MQTT_PASS[] = "YOUR_PASS";
```

En caso que te conectes a un broker con usuario y contrase帽a sera necesario que configures `MQTT_USER` y `MQTT_PASS`. El resto de los secrets dejalos tal cual est谩n.

Cuando tengas los datos cargados, abri una terminal con PlatformIO (como se indica en el README principal del proyecto) y ejecuta el comando `pio run -e default -t upload && pio device monitor` para cargar el programa en la placa y visualizarlo en la terminal serie. Deberias ver una salida como la siguiente:

```
Welcome to Remote light system - www.gotoiot.com

Trying connection to Wifi SSID 'WiFi Secret'.
Wifi connected. Assigned device IP: '192.168.0.103'
Trying connection to MQTT broker '192.168.0.106:1883'
Connected to MQTT broker successfully
Subscribed to topic: /led/DEVICE_ID
Subscribed to topic: /status/DEVICE_ID
Subscribed to topic: /led/status/DEVICE_ID
Subscribed to topic: /all/led
Subscribed to topic: /all/led/status
Sending MQTT Topic '/up' -> '{"device_id":"DEVICE_ID","ip":"192.168.0.103","status":"running"}'
```

Una vez que el dispositivo inicializa env铆a al broker el topic `up` con un payload como este `'{"device_id":"DEVICE_ID","ip":"192.168.0.103","status":"running"}'`. Una vez enviado tal topic esta disponible para ser controlado o encuestado remotamente bajo las siguientes condiciones:

* Si recibe el topic `/led/DEVICE_ID` o bien el topic `/all/led` con un payload `on` u `off` encender谩/apagar谩 el LED respectivamente.
* Si recibe el topic `/status/DEVICE_ID` env铆a un nuevo topic `/DEVICE_ID/status` con un payload similar a este `'{"device_id":"DEVICE_ID","ip":"192.168.0.103","status":"running"}'`.
* Si recibe el topic `/led/status/DEVICE_ID` o bien el topic `/all/led/status` env铆a un nuevo topic `/DEVICE_ID/led/status` con un payload similar a este `{"status":"off","time":85003}`.

En caso de desconectarse de manera inesperada env铆a el topic el topic `down` con un payload como este `'{"device_id":"DEVICE_ID"'`. Esto puede permitir a un sistema de control externo tener un control preciso del estado de funcionamiento de los dispositivos.

Para poder probar la funcionalidad completa del ejemplo, es necesario que tengas corriendo un broker MQTT y un cliente adicional. Si no sabes como hacerlo, podes ver nuestro proyecto [Connection MQTT](https://github.com/gotoiot/connection-mqtt), que se compone de un broker y distintos servicios relacionados que conforman un ecosistema MQTT completo.


## Autores 馃懃

Los autores de esta aplicaci贸n son: 

* **[Agustin Bassi](https://github.com/agustinBassi)**


## Licencia 馃搫

Este proyecto est谩 bajo Licencia ([MIT](https://choosealicense.com/licenses/mit/)). Pod茅s ver el archivo [LICENSE.md](LICENSE.md) para m谩s detalles sobre el uso de este material.

---

**Copyright 漏 Goto IoT 2021** - [**Website**](https://www.gotoiot.com) - [**Group**](https://groups.google.com/g/gotoiot) - [**Github**](https://www.github.com/gotoiot) - [**Twitter**](https://www.twitter.com/gotoiot) - [**Wiki**](https://github.com/gotoiot/doc/wiki)