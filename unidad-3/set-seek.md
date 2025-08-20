# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5


![Untitled (1)](https://github.com/user-attachments/assets/7bb19bff-67ed-4ba1-a2a0-82608e1aa351)


#### Vectores de prueba

# Vectores de prueba – Máquina de estados

| Estado inicial | Evento disparador      | Acciones                                                         | Estado final        |
|----------------|------------------------|------------------------------------------------------------------|---------------------|
| CONFIG         | A                      | `count = min(count+1,60)` ; `display.show(count)`                | CONFIG              |
| CONFIG         | B                      | `count = max(10,count-1)` ; `display.show(count)`                | CONFIG              |
| CONFIG         | S                      | `startTime = utime.ticks_ms()`                                   | ARMED               |
| ARMED          | 1 segundo transcurrido | `count = count - 1` ; `display.show(count)`                      | ARMED  |
| ARMED          | count == 0             | `display.show(Image.SKULL)`                                      | EXPLODED            |
| ARMED          | A                      | `key[keyindex]='A'` ; `keyindex++`                               | ARMED               |
| ARMED          | B                      | `key[keyindex]='B'` ; `keyindex++`                               | ARMED               |
| ARMED          | key correcto           | `count=20` ; `display.show(count)` ; `keyindex=0`                | CONFIG              |
| ARMED          | key incorrecto         | `keyindex=0`                                                     | ARMED               |
| EXPLODED       | T                      | `count=20` ; `display.show(count)` ; `startTime=utime.ticks_ms()`| CONFIG              |




