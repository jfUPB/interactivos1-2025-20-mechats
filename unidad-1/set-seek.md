# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

¿Qué es un sidstema fisico interactivo?

Es un sistema que permite la interaccion entre usuario y programa, se caracteriza por tener una inmersion profunda y que cada accion que haga un usuario va a tener una respuesta o una salida por parte del programa en tiempo real


¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

El entretenimiento digital es una profesion que exige, valga la redundancia, entretener al usuario. Siento que esta herramienta es muy util para este fin, pues puede brindar una experiencia a un publico que, s esta bien hecho, va a disfrutar al interactuar con el producto

### Actividad 2

¿Qué es el diseño/arte generativo?

es una forma de arte creada en colaboración con sistemas autónomos como algoritmos o inteligencia artificial, en lugar de que el artista controle directamente cada detalle de la obra, crea un sistema o conjunto de reglas que genera el arte por sí mismo, aunque el artista es el que establece los parametros hay cierta autonomia en la maquina, o incluso hay una cooperacion entre artista y maquina



¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

la inegnieria en diseño de entretenimiento digital es una carrera que fluctua junto con el desarrollo tecnologico, yo siento que para mi futuro profesional el diseño generativo permitiria generar aplicaciones que vayan mas alla de una experiencia por la cual un usuario puedra aprender si no que una maquina pueda obtener informacion y utilizar esta misma para generar sus propios comportamientos y tener potestad de elegir la mejor ruta para que el usuario disfrute al maximo la experiencia


### Actividad 3

En este sistemas físico interactivo identifica los inputs, outputs y el proceso.

#### Inputs

. Botones del microbit

. Codigo que cargamos al microbit

. Mouse y teclado

. Cable USB

#### outputs

. Cambio de color en la pantalla

. Cara feliz en el microbit

#### procesos

. El computador compila y transfiere el código al Micro:bit

. El Micro:bit interpreta y ejecuta el código


### Actividad 4

 https://editor.p5js.org/ <img width="1907" height="922" alt="Captura de pantalla 2025-07-23 131059" src="https://github.com/user-attachments/assets/734a5da3-fece-4fc3-b0e5-1421c18efc05" />


``` js
function setup() {
  createCanvas(600, 400);
  background(220);
}

function draw() {
  let x = random(width);
  let y = random(height);
  let size = random(20, 60);
  let shapeType = floor(random(3));

  fill(random(255), random(255), random(255));

  if (shapeType === 0) {
    ellipse(x, y, size, size);
  } else if (shapeType === 1) {
    rect(x, y, size, size);
  } else {
    triangle(x, y, x + size, y, x + size / 2, y - size);
  }
}
```


<img width="1907" height="922" alt="sistemas" src="https://github.com/user-attachments/assets/9587c415-c8cf-4f0d-9a98-0d6268a30b5e" />









