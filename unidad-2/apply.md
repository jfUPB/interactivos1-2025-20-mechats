# Unidad 2


## 🛠 Fase: Apply


### Actividad 04

<img width="540" height="706" alt="image" src="https://github.com/user-attachments/assets/ce92ef2d-7622-4b36-9f11-9bbe391a062a" />


los estados que se evidencian en mi diagrama son

#### Config_tiempo

que las acciones y eventos que realiza son:

Display: mostrar tiempo inicial
aumentar tiempo disminuir tiempo
Display: mostrar el tiempo actual


y la transicion que realiza es ir a ARMED 

#### ARMED

sus acciones son:

Display: mostrar cuenta regresiva en pantalla

y tiene una transicion a EXPLOSION 

#### EXPLOSION 

las acciones que tiene este estado son:

Speaker: hacer un sonido que indique que ya no hay tiempo
Display: mostrar el BOOM 

EXPLOSION tiene una transicion de nuevo a CONFIG

por ultimo, los eventos de mi diagrama son:

apretar los botones UP/DOWN (incrementan y decrementan el tiempo)
usar shake (activa la bomba)
oprimir touch, que en este caso lo puse como el pin1 (devuelve de nuevo a config_tiempo)


#### Actividad 05

```python
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_ARMED = 2
STATE_EXPLODE = 3

current_state = STATE_INIT
tiempo = 20
cuenta = 0
start_time = 0
interval = 1000

while True:
    if current_state == STATE_INIT:
        tiempo = 20
        display.show(tiempo)
        current_state = STATE_CONFIG

    elif current_state == STATE_CONFIG:
        if button_a.was_pressed() and tiempo < 60:
            tiempo += 1
            display.show(tiempo)
        if button_b.was_pressed() and tiempo > 10:
            tiempo -= 1
            display.show(tiempo)
        if accelerometer.was_gesture("shake"):
            cuenta = tiempo
            start_time = utime.ticks_ms()
            current_state = STATE_ARMED

    elif current_state == STATE_ARMED:
        display.show(cuenta)
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:
            cuenta -= 1
            start_time = utime.ticks_ms()
        if cuenta <= 0:
            display.show(Image.SKULL)
            music.play(['c4:2', 'g3:2', 'c3:4'])  
            current_state = STATE_EXPLODE

    elif current_state == STATE_EXPLODE:
        if pin1.is_touched():  
            current_state = STATE_CONFIG
            display.show(tiempo)
```
#### Vectores de prueba

1. Inicialización

   estado inicial: state:int

   entradas o condiciones: no hay

   resultado esperado: se muestre en la pantalla los 20 segundos predeterminados

2.  no incrementar y decrementar tiempo fuera del limite

   estado inicial: STATE_CONFIG, tiempo = 60/ STATE_CONFIG, tiempo = 10

   entradas o condiciones: UP(incrementar tiempo)/DOWN (decrementar tiempo)

   resultado esperado: no permite subir a mas de 60seg/ no permite bajas a menos de 10seg

  3. Cuenta regresiva correcta

     estado inicial: STATE_ARMED, cuenta > 0

     entradas o condiciones: Esperar > interval

     resultado esperado: la cuenta disminuye correctamente de 1 en 1

   4. Reinicio tras explosión

      estado inicial: STATE_EXPLODE

      entradas o condiciones: tocar pin1

      resultado esperado: Regresa a STATE_CONFIG, muestra tiempo en display



