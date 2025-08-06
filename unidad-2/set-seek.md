# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

#### ¿Cómo funciona este ejemplo?

El programa hace que dos puntos en la pantalla del Micro:bit parpadeen. Uno cambia cada 1 segundo y el otro cada medio segundo. Para eso se usa una clase que guarda la posición del punto, su brillo y cuánto tiempo debe esperar antes de cambiar. El programa revisa todo el tiempo si ya pasó ese tiempo, y si es así, cambia el brillo del punto.

#### ¿Cuáles son los estados en el programa?

Hay dos estados:

Init: se guarda el tiempo y se muestra el punto.

WaitTimeout: se espera hasta que pase el tiempo, y luego se cambia el brillo del punto.

#### ¿Cuáles son los eventos/inputs en el programa?

El único evento es que pase el tiempo que se indicó que son 0.5 y 1 seg.

#### ¿Cuáles son las acciones en el programa?

Mostrar el punto en pantalla con un brillo.

Cambiar el brillo, encender o apagar en el punto.

Guardar el nuevo tiempo para volver a esperar.


### Actividad 02

<img width="1365" height="778" alt="image" src="https://github.com/user-attachments/assets/1acb1f83-027c-4cb4-bd41-6def3f4a1c5b" />



```.py
from microbit import *
import utime

state = "ROJO"
start_time = utime.ticks_ms()

while True:
    current_time = utime.ticks_ms()

    if state == "ROJO":
        display.clear()
        display.set_pixel(2, 0, 9)
        if utime.ticks_diff(current_time, start_time) > 3000:
            state = "VERDE"
            start_time = current_time

    elif state == "VERDE":
        display.clear()
        display.set_pixel(2, 2, 9)
        if utime.ticks_diff(current_time, start_time) > 3000:
            state = "AMARILLO"
            start_time = current_time

    elif state == "AMARILLO":
        display.clear()
        display.set_pixel(2, 4, 9)
        if utime.ticks_diff(current_time, start_time) > 1000:
            state = "ROJO"
            start_time = current_time
```

En este código hay tres estados principales que representan las luces del semáforo: rojo, verde  y amarillo. Cada uno indica cuál luz debe estar encendida en ese momento.

El evento que hace que el semáforo cambie de estado es el paso del tiempo. Cuando han pasado 3 segundos en el estado rojo, el semáforo cambia a "verde. Después de otros 3 segundos, cambia a amarillo, Finalmente, tras 1 segundo, vuelve al estado rojo, y así el ciclo se repite.

Las acciones que realiza el programa son limpiar la pantalla, encender un LED en una posición específica para mostrar la luz correspondiente, cambiar al siguiente estado y actualizar el tiempo en el que comenzó ese estado. Cada luz está ubicada en la misma columna (x = 2), pero en una fila diferente: (2, 0) para rojo, (2, 2) para verde y (2, 4) para amarillo.

### Actividad 03

Decimos que el programa hace varias cosas “al tiempo”  porque en cada repetición del ciclo while True, revisa si se presionó el botón A y también si ya pasó cierto tiempo.

En este programa los eventos son las cosas que pueden suceder y que hacen que el sistema cambie de estado El primer evento es cuando se presiona el boton A del microbit Este evento puede ocurrir en cualquier momento y hace que el sistema cambie de estado inmediatamente El otro evento importante es el paso del tiempo Cada estado tiene un tiempo asignado y cuando ese tiempo se cumple el sistema tambien cambia de estado Por lo tanto los dos eventos que controlan el comportamiento del programa son presionar el boton y que se acabe el tiempo de espera.

#### Vector de prueba 1

 la condicion inicial es que estamos en state_happy se espera un tiempo sin tocar ningun boton el evento es que se cumple el tiempo definido y el resultado es que el sistema cambia al estado state_smile mostrando la carita sonriente

 #### Vector de prueba 2

 en el segundo caso la condicion inicial es que estamos en state_happy el evento es que se presiona el boton a antes de que pase el tiempo y como resultado el sistema cambia al estado state_sad mostrando la carita triste

 #### Vector de prueba 3

 en el tercer caso la condicion inicial es que estamos en state_smile no se toca ningun boton el evento es que pasa el tiempo asignado y el resultado es que el sistema cambia al estado state_sad mostrando la carita triste

 



