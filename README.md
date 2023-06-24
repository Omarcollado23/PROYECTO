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
- 4 LED´s
- NODE-red (http://localhost:1880/#flow/214928d358894902)

## Instrucciones

### Requisitos previos

# Instalar Node-Red

## Pasos para instalación

1. Entrar a la pagina  https://nodejs.org/en
2. Descargar el archivo **18.16.0 LTS** como se muestra en la siguente imagen.

![](https://github.com/DiegoJm10/Node-red-instalcacion/blob/main/Node.js%20-%20Google%20Chrome%2014_06_2023%2005_04_00%20p.%20m..png?raw=true)

3. Abrir el archivo e instalar el programa [node.js](https://nodejs.org/en)


4. Nos vamos al inicio y buscamos ```cmd``` cOmo se muestra en la imagen y damos clic en "ejecutar como administrador" y escribir lo siguente:
![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/CMD.png?raw=true)

```
npm install -g --unsafe-perm node-red
```
![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/npm%20instal.png?raw=true)

5. Despues comprobamos que funcione node-red con el siguente codigo: (con este mismo codigo podemos arrancar el programa siempre que lo necesitemos)

```
node-red
```
 ![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/node-red.png?raw=true)


### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).
y escoger el apartado donde se encuentra el ESP32 unicamente solo.

![](https://github.com/Omarcollado23/PROYECTO/blob/main/esp32.png?raw=true)


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

![](https://github.com/Omarcollado23/PROYECTO/blob/main/LIBRERIA.png?raw=true)

3. Hacemos las conexiones en el **ESP32** del sensor **HC-SR04** y **LCD** como se muestra en la siguiente imagen.

![](https://github.com/Omarcollado23/PROYECTO/blob/main/conexiones.png?raw=true)

### Instrucciónes de operación

1. Iniciamos el simulador.
2. Visualizar los datos en el monitor serial.
3. Colocar la distancia dando *doble click* al sensor **HC-SR04** 
4. Visualizamos en la pantalla LCD



# Instrucciones para hacer la conexión con NODE-RED

 
## Arranque de programa

Para abrir la aplicación nos vamos algun explorador y colocamos el siguente link:    ```localhost:1880```


## Instalación de Dashboard

1. Abrimos la pestaña de opciones y elegimos ```Manage palette``` 

![](https://github.com/DiegoJm10/Node-red-instalcacion/blob/main/Node.js%20-%20Google%20Chrome%2014_06_2023%2005_06_26%20p.%20m..png?raw=true)

2. Seleccionamos **Install* y buscamos la libreria ```node-red-dashboard```.
3. Seleccionamos ```node-red-dashboard```.
![](https://github.com/DiegoJm10/Node-red-instalcacion/blob/main/Node.js%20-%20Google%20Chrome%2014_06_2023%2005_06_17%20p.%20m..png?raw=true)

4. Una ves instalda la librería nos queda de la siguiente manera.

![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/localhost1.png?raw=true)


5. Colocamos bloque ```mqqtt in```.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/bloquemqtt.png?raw=true)

6.  Configurar el bloque con el puerto mqtt con el ip ```44.195.202.69``` como se muestra en la imagen.

![](https://github.com/Omarcollado23/PROYECTO/blob/main/mqtt%20conf.png?raw=true)

7.  Colocar el bloque json y configurarlo como se muestra en la imagen.

![](https://github.com/Omarcollado23/PROYECTO/blob/main/json%20conf.png?raw=true)

8. Colocamos un bloque ```function``` para obtener los datos con el siguente codigo.

```
msg.payload = msg.payload.LLENADO;
msg.topic = "LLENADO";
return msg;
```

![](https://github.com/Omarcollado23/PROYECTO/blob/main/function%20conf.png?raw=true)

9. Colocamos un bloque chart y un bloque gauge.
Los configuramos de la siguiente manera como se muestra en las siguientes imagenes.

![](https://github.com/Omarcollado23/PROYECTO/blob/main/llenado%20datos.png?raw=true)
![](https://github.com/Omarcollado23/PROYECTO/blob/main/grafica.png?raw=true)

10. Y node debe quedar nuestro esquema de la siguiente manera
![]()


## Resultados

Cuando haya funcionado, la información obtenida del sensor y **HC-SR04** se arrojara en el LCD como se muestra en las siguentes imagenes. 
Y los manda al servidor de NODE-RED

![]()
![]()
![]()
![]()



# Créditos

Desarrollado por 
Ing. Omar Alejandro Collado Carriola
Ing. Sergio Rivera Barrera
Ing. Luis Daniel Mendoza 
Bahena
- [GitHub](https://github.com/Omarcollado23)