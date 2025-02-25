# Recuperacion-1

# Curso de Python 2.
## Examen 1.-
<img src="https://i.postimg.cc/pTpsRN0s/Imagen-de-Whats-App-2025-02-23-a-las-22-09-32-11abdce0.jpg" width="250"/>

## Examen 2.-
<img src="https://i.postimg.cc/vTvgKTWk/Imagen-de-Whats-App-2025-02-23-a-las-22-15-18-1fce7539.jpg" width="250"/>

## Examen 3.-
<img src="https://i.postimg.cc/kMHkVsKV/Imagen-de-Whats-App-2025-02-23-a-las-22-22-55-009a3594.jpg" width="250"/>

## Examen 4.-
<img src="https://i.postimg.cc/YSVs4qL5/Imagen-de-Whats-App-2025-02-23-a-las-22-28-37-ed2d9a7d.jpg" width="250"/>

## Examen Final.-
<img src="https://i.postimg.cc/7PpJBvSq/Imagen-de-Whats-App-2025-02-23-a-las-23-07-40-27dada0b.jpg" width="250"/>

# Actividades:

## Actividad 2:
Video: https://drive.google.com/file/d/1ixuEjj98WUoAgGLEfUQD3hF9MufNr3dv/view?usp=drive_link
Codigo:
import network
from umqtt.simple import MQTTClient
from machine import Pin, time_pulse_us
import time

# Configuración WiFi
WIFI_SSID = "MANUELIFTER"
WIFI_PASSWORD = "deadlift"

# Configuración MQTT
MQTT_BROKER = "192.168.137.167"
MQTT_USER = ""
MQTT_PASSWORD = ""
MQTT_CLIENT_ID = ""
MQTT_TOPIC = "utng/sensor"
MQTT_PORT = 1883

# Configuración del sensor ultrasónico
TRIG_PIN = 5
ECHO_PIN = 18

trig = Pin(TRIG_PIN, Pin.OUT)
echo = Pin(ECHO_PIN, Pin.IN)

# Configuración del LED RGB
RED_PIN = 15
GREEN_PIN = 2
BLUE_PIN = 4

led_red = Pin(RED_PIN, Pin.OUT)
led_green = Pin(GREEN_PIN, Pin.OUT)
led_blue = Pin(BLUE_PIN, Pin.OUT)

# Función para conectar a WiFi
def conectar_wifi():
    print("Conectando a WiFi...", end="")
    sta_if = network.WLAN(network.STA_IF)
    sta_if.active(True)
    sta_if.connect(WIFI_SSID, WIFI_PASSWORD)
    while not sta_if.isconnected():
        print(".", end="")
        time.sleep(0.3)
    print("\nWiFi Conectada!")

# Función para conectar al broker MQTT
def conectar_broker():
    client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER, port=MQTT_PORT, user=MQTT_USER, password=MQTT_PASSWORD)
    client.connect()
    print(f"Conectado a MQTT Broker: {MQTT_BROKER}, Topic: {MQTT_TOPIC}")
    return client

# Función para encender el LED RGB con un color específico
def set_color(red, green, blue):
    led_red.value(red)
    led_green.value(green)
    led_blue.value(blue)

# Función para medir distancia con HC-SR04
def medir_distancia():
    trig.off()
    time.sleep_us(2)
    trig.on()
    time.sleep_us(10)
    trig.off()
    duracion = time_pulse_us(echo, 1, 30000)  # Máximo 30 ms de espera
    if duracion < 0:
        return -1  # Error en la medición
    distancia = (duracion * 0.0343) / 2  # Convertir a cm
    return distancia

# Conectar a WiFi y MQTT
conectar_wifi()
client = conectar_broker()

distancia_anterior = -1  # Inicializamos con un valor inválido

# Bucle principal
while True:
    distancia = medir_distancia()
    print(f"Distancia: {distancia} cm")
    # Cambiar el color del LED RGB según la distancia
    if distancia == -1:
        set_color(0, 0, 0)  # Apagar LED si hay error
    elif distancia > 20:
        set_color(0, 1, 0)  # Verde
    elif distancia > 10:
        set_color(1, 1, 0)  # Amarillo
    else:
        set_color(1, 0, 0)  # Rojo
  # Publicar la distancia en MQTT solo si cambia
  if distancia != distancia_anterior:
        client.publish(MQTT_TOPIC, str(distancia))
        distancia_anterior = distancia  # Actualizar la distancia anterior
    time.sleep(2)  # Esperar 2 segundos antes de la siguiente medición

## Actividad 3:
<img src="https://i.postimg.cc/1tGxCkrv/IMG-20250225-WA0059.jpg" width="250"/>
<img src="https://i.postimg.cc/Prm4Ltr3/IMG-20250225-WA0060.jpg" width="250"/>
<img src="https://i.postimg.cc/pr0PkNPg/IMG-20250225-WA0061.jpg" width="250"/>
<img src="https://i.postimg.cc/fWHsGNhP/IMG-20250225-WA0062.jpg" width="250"/>

## Actividad 4:
Video: https://drive.google.com/file/d/1W--JtCVg1CHI2KMle4SguFdh-Q-EvLXH/view?usp=sharing
imagenes:
<img src="https://i.postimg.cc/7LS6yY14/Imagen-de-Whats-App-2025-02-25-a-las-17-37-08-9042821d.jpg" width="250"/>
<img src="https://i.postimg.cc/d09YB7mh/Imagen-de-Whats-App-2025-02-25-a-las-17-37-08-c64c3f31.jpg" width="250"/>
<img src="https://i.postimg.cc/L4LrBg92/Imagen-de-Whats-App-2025-02-25-a-las-17-37-08-e1a98222.jpg" width="250"/>
<img src="https://i.postimg.cc/d0hf7LNM/Imagen-de-Whats-App-2025-02-25-a-las-17-37-09-cb7b3f14.jpg" width="250"/>
