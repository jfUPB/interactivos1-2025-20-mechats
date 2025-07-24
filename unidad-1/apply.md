# Unidad 1

## 🛠 Fase: Apply

### Actividad 05
En esta actividad pudimos ver que mediante una conexion serial y  la pulsacion de el boton A del micro bit  se pudo alterar con ayuda de un codigo el color de un cuadrado, a su vez cuando el boton A dejaba de ser precionado volvia a cambiar de color, sin embargo nos topamos con un problema, y era que cuando se presionaba el boton solo se iluminaba por un corto tiempo y despues se volvia a apagar, para solucionarlo se modifico el codigo del micro bit y de p5.js, haciendo que, mientras el boton A no estuviese presionado, el microbit enviara a la computadora la tecla N, que según el codigo de p5.js, cuando este tecla se presione, el cuadrado se pintara de verde otra vez. Esto permite que la pantalla muestre el cuadro de color rojo hasta que soltemos el boton A

### Actuvudad 06

https://editor.p5js.org/mechats/sketches/9AV_dcjYL

```
let port;
let x = 200;

function setup() {
  createCanvas(400, 400);
  background(220);
  port = createSerial();

  port.on('data', serialEvent);
  port.on('error', err => console.error(err));

  
  let connectBtn = createButton("Conectar micro:bit");
  connectBtn.position(10, height + 10);
  connectBtn.mousePressed(() => {
    port.requestPort().then(() => port.open());
  });
}

function draw() {
  background(220);
  ellipse(x, height / 2, 50, 50);
}

function serialEvent() {
  let data = port.readLine().trim();
  if (data === 'A') {
    x -= 5;
  } else if (data === 'B') {
    x += 5;
  }
  x = constrain(x, 25, width - 25);
}
```

```from microbit import *

uart.init(baudrate=115200)

while True:
    if button_a.is_pressed():
        uart.write('A')
    elif button_b.is_pressed():
        uart.write('B')
    else:
        uart.write('N')
    sleep(100)
```

