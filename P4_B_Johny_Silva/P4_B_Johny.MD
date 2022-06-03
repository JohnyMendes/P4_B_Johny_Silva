# Johny Silva Mendes
# PRACTICA 4: SISTEMAS OPERATIVOS EN TIEMPO REAL

Objetivo: observar el funcionamiento de un sistema operativo en tiempo Real

## EJERCICIO PRÁCTICO 2 

Para este ejercicio se ha utilizado un semaforo mutex (mutual exclusion) para cooridnar el encendido y apagado del led. 

Se puede observar el código a continuación:
```cpp
#include <Arduino.h>

void ledON (void * pvParameters);
void ledOFF(void * pvParameters);
int LED =2; 

SemaphoreHandle_t semafor;

void setup(){

Serial.begin(9600);
pinMode(LED, OUTPUT);
semafor= xSemaphoreCreateMutex();

xTaskCreate(
    ledON,
    "LED ON",
    10000,
    NULL,
    1,
    NULL);

xTaskCreate(
    ledOFF,
    "LED OFF",
    10000,
    NULL,
    1,
    NULL);
}

void loop(){}

void ledON (void * pvParameters){
    for(;;){
        xSemaphoreTake(semafor, portMAX_DELAY);
        digitalWrite(LED, HIGH);
        delay(3000);
        xSemaphoreGive(semafor);
    }
}

void ledOFF (void * pvParameters){
    for(;;){
        xSemaphoreTake(semafor, portMAX_DELAY);
        digitalWrite(LED, LOW);
        delay(1500);
        xSemaphoreGive(semafor);
    }
}
```