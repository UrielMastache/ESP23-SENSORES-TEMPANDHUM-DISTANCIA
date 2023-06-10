# Practica ESP32 con HC-SR04 Ultrasonic Distance Sensor y pantalla LED pasa detectar la temperatura, humedad y la distancia
Este repositorio muestra como podemos programar una ESP32 con el sensor Ultrasonico de distancia y un sensor DHT22 para saber la temperatura y la humedad.

## Introducción

### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un sensor (```HC-SR04```) y la pantalla  (```LCD I2C```) y un sensor (```DHT22```) Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/).


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Sensor HC-SR04 Ultra Distance 
- Pantalla Led I2C
- Sensor DHT22
- Y muchas ganas de chambear :D


## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:


```
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
#include "DHTesp.h"


const int DHT_PIN = 15;
DHTesp dhtSensor;

const int Trigger = 13;   //Pin digital 2 para el Trigger del sensor
const int Echo = 12;   //Pin digital 3 para el Echo del sensor

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
Serial.begin(9600);//iniciailzamos la comunicación
  lcd.init();
  lcd.backlight();
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0


  // put your setup code here, to run once:
  Serial.begin(115200);
  Serial.println("Hello, ESP32!");
}

void loop()
{
  
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");
  delay(1500);

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms

 lcd.setCursor(0, 0);
  lcd.print("Distancia: " + String(d) + "cm");
  lcd.setCursor(0, 1);
  lcd.print("                 " );
  delay (1500);
  lcd.setCursor(0, 0);
  lcd.print("Tenga buen dia   ");
lcd.setCursor(0, 1);
  lcd.print("  Nos vemos      ");
  delay (1500);
  lcd.setCursor(0, 0);
lcd.print("    Made by      ");
lcd.setCursor(0, 1);
  lcd.print("    Uriel M         ");
  delay(1000);


}

```

2. Hacer la conexion de **HC-SR04** con la **ESP32** como se muestra en la siguente imagen.



![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/conexion%20sensor%20nivel.png?raw=true)

3. Hacer la conexion de **LCD I2C** con la **ESP32** como se muestra en la siguiente imagen.

![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/conexion%20pantalla%20led.png?raw=true)

4. Hacer la conexion de **DHT22** con la **ESP32** como se muestra en la siguiente imagen

![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/Conexion%20sensor%20tempandhum.png?raw=true)

### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial y con la pantalla LCD.

## Resultados

Cuando haya funcionado, verás los valores dentro del monitor serial (parte blanca de abajo, así como también en la pantalla LCD) como se muestra en las siguentes imagenes.

Temperatura y humedad 
![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/humedas%20y%20temp.png?raw=true)

Distancia 

![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/distancia.png?raw=true)

Mensaje 1

![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/mensaje%201.png?raw=true)

Mensaje 2 

![](https://github.com/UrielMastache/ESP23-SENSORES-TEMPANDHUM-DISTANCIA/blob/main/mensaje%202.png?raw=true)

## Comentarios

Recuerda que esto es demostrativo, y puedes modificar todo a como mejor te adaptes y lo necesites.


Gracias por ver :D 

Ciao.