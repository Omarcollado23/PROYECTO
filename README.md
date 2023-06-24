# PROYECTO DE LLENADO DE BOTELLAS con ESP32, Sensor de Distancia HC-SR04 y LCD
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
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

// defines pins numbers
const int Trigger = 26;
const int Echo = 25;
const int led1 = 2;
const int led2 = 4;
const int led3 = 19;
const int led4 = 18;

// defines variables
long duration;
int distance;
int safetyDistance;

void setup() {
pinMode(Trigger, OUTPUT); // Sets the trigPin as an Output
pinMode(Echo, INPUT); // Sets the echoPin as an Input
pinMode(led1, OUTPUT);
pinMode(led2, OUTPUT);
pinMode(led3, OUTPUT);
pinMode(led4, OUTPUT);
Serial.begin(9600); // Starts the serial communication


  lcd.init();
  lcd.backlight();
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
}


void loop() {
// Clears the trigPin
digitalWrite(Trigger, LOW);
delayMicroseconds(10);

// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(Trigger, HIGH);
delayMicroseconds(10);
digitalWrite(Trigger, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(Echo, HIGH);

// Calculating the distance
distance= duration*0.034/2;

safetyDistance = distance;

if (safetyDistance>=2 && safetyDistance<=100)
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
}
else if(safetyDistance>=100 && safetyDistance<=200) 
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
}
else if(safetyDistance>=200 && safetyDistance<=300) 
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, LOW);
}
else if(safetyDistance>=300 && distance <=320) 
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, HIGH);
}
else
{
 digitalWrite(led1,  LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
}

Serial.print("Distance: ");
Serial.println(distance);
delay (2000);

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);

  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm



  lcd.setCursor(0, 0);
  lcd.print("LLENANDO : " + String(d) +"  ml  ");
  lcd.setCursor(0, 1);

}

```

2. Instalamos la libreria de  **LiquidCrystal I2C** como se muestra en la siguente imagen, dando clic en (Library Manager) y despues en el simbolo de (+)

![]()

3. Hacemos las conexiones en el **ESP32** del sensor **HC-SR04** y **LCD** como se muestra en la siguiente imagen.

![](https://github.com/Omarcollado23/PROYECTO/blob/main/conexiones.png?raw=true)

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