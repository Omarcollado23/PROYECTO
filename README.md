# Practica de ESP32 con Sensor de Distancia HC-SR04 y LCD
Este repositorio muestra el proyecto en el cual podemos programar una ESP32 con el sensor de distancia HC-SR04 con un LCD para simular una banda de llenado de botellas.


### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un sensor (```HC-SR04```) para medir distancia. Cabe aclarar que esta practica se usara en el simulador llamado [WOKWI](https://https://wokwi.com/).


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Sensor HC-SR04
- LCD 16X2 (l2C)

## Instrucciones


### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
const int DHT_PIN = 15;
const int Trigger = 26;   //Pin digital 2 para el Trigger del sensor
const int Echo = 25;   //Pin digital 3 para el Echo del sensor

// defines variables
long duration;
int distance;
int safetyDistance;

DHTesp dhtSensor;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0


}

void loop() {

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");
  delay(1000);
  lcd.setCursor(0, 0);
  lcd.print("DIPLOMADO AIyM");
  lcd.setCursor(0, 1);
  lcd.print("SABADO 6 D JUNIO");
  delay(1000);

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);

  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(distance);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms

  lcd.setCursor(0, 0);
  lcd.print("  Distancia: " + String(d) +"cm  ");
  lcd.setCursor(0, 1);
  lcd.print("    Ing. OMAR    ");
  delay(1000);
}

```
2. Instalamos la libreria de  **LiquidCrystal I2C** como se muestra en la siguente imagen, dando clic en (Library Manager) y despues en el simbolo de (+)

![]()

3. Hacemos las conexiones en el **ESP32** del sensor **HC-SR04** y **LCD** como se muestra en la siguiente imagen.

![]()

### Instrucciónes de operación

1. Iniciamos el simulador.
2. Visualizar los datos en el monitor serial.
3. Colocar la distancia dando *doble click* al sensor **HC-SR04** 

  

## Resultados

Cuando haya funcionado, la información obtenida del sensor y **HC-SR04** se arrojara en el LCD como se muestra en las siguentes imagenes.

![]()
![]()
![]()



# Créditos

Desarrollado por Ing. Omar Alejandro Collado Carriola

- [GitHub](https://github.com/Omarcollado23)