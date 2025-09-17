
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

### Capturas

<img width="1916" height="755" alt="image" src="https://github.com/user-attachments/assets/16605f1e-6e7b-4801-bf89-0e41c32eb5ea" />


Como dato interesante trate de cambiar el color mientras mantenia A pulsado y dejo de dibujar por un breve momento para despues empezar a dibujar de nuevo, interesting...

## Actividad 02

Una vez cambiado el codgi del micro:bit este es el resultado de los datos que envia el micro:bit convertidos a texto

<img width="982" height="167" alt="image" src="https://github.com/user-attachments/assets/161d91bd-0397-4e4c-9be9-b64efb14c073" />

Donde aparecen un monton de signos de interrogación es porque estaba viendo que pasaba si presionaba A o si lo movia y asi.

Muestra un monton de simbolos raros debido principalmente a que con el cambio que hicimos en el micro:bit ahora este no esta enviando un texto plano que podamos leer nosotros ni el programa, antes enviaba numeros separados por comas y eso el terminal su lo podia leer porque era ascii, ahora es diferente  y por eso aparecen simbolos extraños y cosas sin sentido

### cambiamos a todo a HEX


<img width="1001" height="205" alt="image" src="https://github.com/user-attachments/assets/89a08451-acd3-41be-b2df-4ecb81445bc7" 

 si relacionamos estas letras y numeros que vienen de a dos, creo que cada uno de estos lo que hace es enviar si el boton A esta presionado o no esta presionado, por ejemplo. Si el boton A esta presionado se envia 1c, pero si no esta presionado el microbit manda 00 (estoy inventando).

 





 










