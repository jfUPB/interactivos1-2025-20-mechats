# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](https://editor.p5js.org/generative-design/sketches/P_2_1_5_03)

Código a modificar:

``` js

// P_2_1_5_03
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * Drawing tool for creating moire effect compositions using
 * rectangles of various widths, lengths, angles and colours.
 *
 * MOUSE
 * mouse               : place moire effect rectangle
 *
 * KEYS
 * 1                   : set color to red
 * 2                   : set color to green
 * 3                   : set color to blue
 * 4                   : set color to black
 * arrow up            : increase rectangle width
 * arrow down          : decrease rectangle width
 * s                   : save png
 *
 * CONTRIBUTED BY
 * [Niels Poldervaart](http://NielsPoldervaart.nl)
 */
'use strict';

var shapes = [];
var density = 2.5;
var shapeHeight = 64;
var shapeColor;

var newShape;

function setup() {
  createCanvas(800, 800);
  noFill();
  shapeColor = color(0);
}

function draw() {
  background(255);

  shapes.forEach(function(shape) {
    shape.draw();
  });

  if (newShape) {
    newShape.x2 = mouseX;
    newShape.y2 = mouseY;
    newShape.h = shapeHeight;
    newShape.c = shapeColor;
    newShape.draw();
  }
}

function Shape(x1, y1, x2, y2, h, c) {
  this.x1 = x1;
  this.y1 = y1;
  this.x2 = x2;
  this.y2 = y2;
  this.h = h;
  this.c = c;

  Shape.prototype.draw = function() {
    var w = dist(this.x1, this.y1, this.x2, this.y2);
    var a = atan2(this.y2 - this.y1, this.x2 - this.x1);
    stroke(this.c);
    push();
    translate(this.x1, this.y1);
    rotate(a);
    translate(0, -this.h / 2);
    for (var i = 0; i < this.h; i += density) {
      line(0, i, w, i);
    }
    pop();
  };
}

function mousePressed() {
  newShape = new Shape(pmouseX, pmouseY, mouseX, mouseY, shapeHeight, shapeColor);
}

function mouseReleased() {
  shapes.push(newShape);
  newShape = undefined;
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (key == '1') shapeColor = color(255, 0, 0);
  if (key == '2') shapeColor = color(0, 255, 0);
  if (key == '3') shapeColor = color(0, 0, 255);
  if (key == '4') shapeColor = color(0);

  if (keyCode == UP_ARROW) shapeHeight += density;
  if (keyCode == DOWN_ARROW) shapeHeight -= density;
}


```

[Enlace a la aplicación modificada](https://editor.p5js.org/mechats/sketches/bGrhdTWrs)

Código modificado:

``` js
let shapes = [];
let density = 2.5;
let shapeHeight = 64;
let shapeColor;
let newShape;

let port;
let connectBtn;
let connectionInitialized = false;
let microBitConnected = false;

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};
let appState = STATES.WAIT_MICROBIT_CONNECTION;

let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevmicroBitAState = false;
let prevmicroBitBState = false;

let colors = [ [255,0,0], [0,255,0], [0,0,255], [0,0,0] ];
let colorIndex = 0;

function setup() {
  createCanvas(800, 800);
  noFill();
  shapeColor = color(0);

  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(10, 10);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(255);

  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");

    if (port.opened() && !connectionInitialized) {
      port.clear();
      connectionInitialized = true;
    }

    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length === 4) {
          microBitX = int(values[0]) + width / 2;
          microBitY = int(values[1]) + height / 2;
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        }
      }
    }
  }

  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected) {
        appState = STATES.RUNNING;
      }
      break;

    case STATES.RUNNING:
      if (!microBitConnected) {
        appState = STATES.WAIT_MICROBIT_CONNECTION;
      }

      if (microBitAState) {
        if (!newShape) {
          newShape = new Shape(microBitX, microBitY, microBitX + 10, microBitY + 10, shapeHeight, shapeColor);
        } else {
          newShape.x2 = microBitX;
          newShape.y2 = microBitY;
          newShape.h = shapeHeight;
          newShape.c = shapeColor;
          newShape.draw();
        }
      }

      shapes.forEach(function (shape) {
        shape.draw();
      });

      if (newShape && microBitAState) {
        newShape.draw();
      }

      break;
  }
}

function Shape(x1, y1, x2, y2, h, c) {
  this.x1 = x1;
  this.y1 = y1;
  this.x2 = x2;
  this.y2 = y2;
  this.h = h;
  this.c = c;

  this.draw = function () {
    let w = dist(this.x1, this.y1, this.x2, this.y2);
    let a = atan2(this.y2 - this.y1, this.x2 - this.x1);
    stroke(this.c);
    push();
    translate(this.x1, this.y1);
    rotate(a);
    translate(0, -this.h / 2);
    for (let i = 0; i < this.h; i += density) {
      line(0, i, w, i);
    }
    pop();
  };
}

function updateButtonStates(newAState, newBState) {
  if (newAState && !prevmicroBitAState) {
    newShape = new Shape(microBitX, microBitY, microBitX, microBitY, shapeHeight, shapeColor);
  }

  if (!newAState && prevmicroBitAState) {
    if (newShape) {
      shapes.push(newShape);
      newShape = undefined;
    }
  }

  if (newBState && !prevmicroBitBState) {
    colorIndex = (colorIndex + 1) % colors.length;
    let c = colors[colorIndex];
    shapeColor = color(c[0], c[1], c[2]);
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

function mousePressed() {
  newShape = new Shape(pmouseX, pmouseY, mouseX, mouseY, shapeHeight, shapeColor);
}

function mouseReleased() {
  if (newShape) {
    shapes.push(newShape);
    newShape = undefined;
  }
}

function keyPressed() {
  if (key == "s" || key == "S") saveCanvas("mi_dibujo", "png");
  if (key == "1") shapeColor = color(255, 0, 0);
  if (key == "2") shapeColor = color(0, 255, 0);
  if (key == "3") shapeColor = color(0, 0, 255);
  if (key == "4") shapeColor = color(0);
  if (keyCode == UP_ARROW) shapeHeight += density;
  if (keyCode == DOWN_ARROW) shapeHeight -= density;
}

```

## Video

[Video demostratativo](https://youtu.be/q8HUxTPsx2M)


