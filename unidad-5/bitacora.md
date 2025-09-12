
# Evidencias de la unidad 5

## Actividad 01

### Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

 Mediante el puerto USB el microbit envia datos a p5.js, en una parte del codigo del microbit hay una linea de texto que dice uart.write(data) y por mera intuicion digo que eso permite enviar los daoa a traves del puerto serial, por otro lado en el codigo de p5.js se usa create.serial para conectarse con el microbit, en el codigo aparece que se estan mandando 4 datos del microbit a p5.js, que son. El acelerometro en, en y, el boton A y el B

### Cómo es la estructura del protocolo ASCII usado?

No tengo ni la menor idea 

### Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

desde la linea que dice 
```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
  if (data) {
    data = data.trim();
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    } else {
      print("No se están recibiendo 4 datos del micro:bit");
    }
  }
}

```
desde ahi empieza a leer todos los caracteres que llegan y se detiene cuando esta el \n, tambien se para los datos que llegan por comas y verifica que si lleguen los 4 datos que el microbit envia (sensorx sensory boton A boton B) despues transforma esos datos, pero no se porque se le suma windowWidth / 2 a los sensores Y y X y tambien verifica si se presionan los botones A o B


### ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

Para poder generar el evento keypressed es necesario que el nuevo estado actual de A sea igual a true y el estado previo a este haya sido igual a false, este true queda guardado en el estado previo, leyendolo una y otra vez hasta que la condición cambie, es decir hasta que se vuelva a soltar el boton


