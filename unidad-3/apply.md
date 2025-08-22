<img width="1452" height="530" alt="image" src="https://github.com/user-attachments/assets/61221245-8b1c-4581-bd1e-b7d07e65a355" /># Unidad 3


## 🛠 Fase: Apply

### Actividad 06

<img width="1452" height="530" alt="image" src="https://github.com/user-attachments/assets/1095f861-c882-4eca-b747-d1e73d52b957" />



```javascript
let CLAVE = ['A', 'B', 'A'];
let intento = [];
let posClave = 0;
let cuentaAtras = 20;
let inicioTiempo;
let estado = "CONFIG";

let btnA, btnB, btnShake, btnReiniciar;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
  inicioTiempo = millis();

  btnA = createButton('A');
  btnA.position(50, height + 20);
  btnA.mousePressed(() => presionarBoton('A'));

  btnB = createButton('B');
  btnB.position(100, height + 20);
  btnB.mousePressed(() => presionarBoton('B'));

  btnShake = createButton('Shake');
  btnShake.position(150, height + 20);
  btnShake.mousePressed(() => presionarShake());

  btnReiniciar = createButton('Reiniciar');
  btnReiniciar.position(220, height + 20);
  btnReiniciar.mousePressed(() => presionarReiniciar());
}

function draw() {
  background(30);
  fill(255);

  if (estado === "CONFIG") {
    text("CONFIGURACIÓN", width / 2, 50);
    text(cuentaAtras, width / 2, height / 2);

  } else if (estado === "ARMED") {
    text("ACTIVADA", width / 2, 50);
    text(cuentaAtras, width / 2, height / 2);

    if (millis() - inicioTiempo >= 1000) {
      inicioTiempo = millis();
      cuentaAtras--;
      if (cuentaAtras <= 0) {
        estado = "EXPLODED";
      }
    }

  } else if (estado === "EXPLODED") {
    fill(255, 0, 0);
    text("💣 Has perdido, chaval 😁", width / 2, height / 2);
  }
}

function presionarBoton(boton) {
  if (estado === "CONFIG") {
    if (boton === 'A') {
      cuentaAtras = min(cuentaAtras + 1, 60);
    } else if (boton === 'B') {
      cuentaAtras = max(10, cuentaAtras - 1);
    }
  } else if (estado === "ARMED") {
    intento[posClave] = boton;
    posClave++;

    if (posClave === CLAVE.length) {
      let correcto = true;
      for (let i = 0; i < CLAVE.length; i++) {
        if (intento[i] !== CLAVE[i]) {
          correcto = false;
          break;
        }
      }

      if (correcto) {
        cuentaAtras = 20;
        estado = "CONFIG";
      }
      posClave = 0;
      intento = [];
    }
  }
}

function presionarShake() {
  if (estado === "CONFIG") {
    inicioTiempo = millis();
    estado = "ARMED";
  }
}

function presionarReiniciar() {
  if (estado === "EXPLODED") {
    cuentaAtras = 20;
    inicioTiempo = millis();
    estado = "CONFIG";
  }
}
```

### Acitivad 07


