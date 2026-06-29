# Sistema de Control de Proceso IoT - Evaluación Sumativa 3

## 📝 Explicación del Proceso
Este proyecto consiste en un sistema IoT bidireccional diseñado para el monitoreo y control de variables físicas (Temperatura) en tiempo real. El sistema integra hardware embebido, protocolos de comunicación industrial, un backend en Python y persistencia relacional en la nube.

El flujo de información opera bajo dos mecánicas principales:
1. **Telemetría (ESP32 -> Nube -> Flask):** El nodo periférico ESP32 recopila de forma constante variables físicas simuladas y las transmite de forma inalámbrica mediante el protocolo MQTT hacia un Broker central (`broker.hivemq.com`). El servidor Flask actúa como suscriptor, recibe las tramas y las registra en una base de datos relacional (Supabase), capturando automáticamente marcas de tiempo precisas (año, mes, día, hora, minuto y segundo).
2. **Control Centralizado (Flask -> ESP32):** Desde la interfaz web del backend, el usuario puede conmutar remotamente entre 3 estados mediante un panel de control nativo. Esta orden viaja por MQTT y es interpretada por el ESP32 para activar actuadores físicos (LEDs de estado).

## 🎛️ Justificación del Rol de la Tarjeta
* **ESP32 (Nodo Periférico de Adquisición y Actuación):** Cumple un rol crítico y dedicado como la interfaz física del sistema. Está encargado de establecer la conectividad Wi-Fi nativa, formatear las cargas de datos con la identidad del operador ("Ignacio Henseleit"), publicar la telemetría cada 5 segundos y ejecutar una función callback asíncrona para controlar las salidas digitales GPIO asociadas a los 3 LEDs indicadores de estado.

## 📊 Diagrama del Sistema
[Interfaz Web Flask] <--- (HTTP) ---> [Backend Python (Flask)]
^              |
(MQTT Sub)     (MQTT Pub)
|              v
[Base de Datos Supabase] <--- (PostgreSQL) --+      [Broker MQTT HiveMQ]
^
|
(Bidireccional)
v
[Hardware ESP32]
|
(Salidas Digitales)
v
[LEDs Verde/Amarillo/Rojo]


## 🚀 Instrucciones para Ejecutar el Proyecto

### Requisitos previos en Windows:
Contar con Python 3 instalado de forma local y añadir las dependencias obligatorias mediante la consola:
```cmd
pip install flask paho-mqtt supabase
Pasos de Lanzamiento:
Conectar el hardware ESP32 y realizar el despliegue del código correspondiente mediante Arduino IDE.

Configurar las credenciales SUPABASE_URL y SUPABASE_KEY dentro del script app.py.

Abrir una terminal de comandos (cmd) en Windows, navegar hasta la raíz de la carpeta y ejecutar el comando:

DOS
python app.py
Acceder al monitor centralizado abriendo un navegador web en la dirección local: http://127.0.0.1:5000/.


---

¡Y listo!