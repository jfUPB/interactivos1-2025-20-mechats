
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


<img width="1514" height="530" alt="image" src="https://github.com/user-attachments/assets/b4bef6ef-1be4-4d82-ba4e-9f0d6c7718ab" />


si relacionamos estas letras y numeros que vienen de a dos, creo que cada uno de estos lo que hace es enviar si el boton A esta presionado o no esta presionado, por ejemplo. Si el boton A esta presionado se envia 1c, pero si no esta presionado el microbit manda 00 (estoy inventando). Las ventajas de usar binario es que es mucho mas rapido,  no se desperdician bytes en comas o saltos de linea, es mas eficiente para sensores que generan muchos datos; desventajas: es dificil de leer directo, toca saber el orden y formato exacto para poder interpretarlo, si uno se equivoca en el parseo los datos no tienen sentido, y no es tan facil de debuggear a ojo


Ahora cambiamos el codigo

<img width="950" height="235" alt="image" src="https://github.com/user-attachments/assets/5ecd3a2f-0fd6-4d6d-bbaf-fab48021a918" />

ada vez que agitaba el microbit veia que en el monitor hex aparecian exactamente 6 bytes por mensaje, esto se relaciona directo con el formato '>2h2B' porque 2h son dos enteros cortos de 2 bytes cada uno (total 4 bytes) y 2B son dos enteros sin signo de 1 byte cada uno (total 2 bytes), en total 6 bytes; cada byte representa una parte del dato: los primeros dos bytes son el valor de x, los siguientes dos bytes son el valor de y, el quinto byte es el estado del boton a y el sexto el del boton b

sobre los numeros positivos y negativos en xValue e yValue, como se usan enteros cortos con signo (h) se representan en complemento a dos, entonces si mando por ejemplo un valor positivo como 1000 en hex se veria como 03 E8 (dependiendo del endianess), pero si mando un valor negativo como -1000 se veria como FC 18, o sea una representacion binaria donde el bit mas significativo indica que es negativo


### ¿que pasaria si en vez de mandar los datos solo cuando agito el microbit, los mando siempre pero le agrego un byte extra que indique si el microbit esta quieto o en movimiento?

#### Hipotesis 

el mensaje pasaria de 6 a 7 bytes, y el ultimo byte se podria usar como bandera (0 = quieto, 1 = en movimiento); en el monitor hex deberia aparecer un valor fijo en esa ultima posicion que cambia segun agite o no el microbit

#### Experimentacion

Con este nuevo codigo 
```python
# Imports go at the top
from microbit import *
import struct

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    
    # calculamos si el microbit esta en movimiento o quieto
    mov = 0
    if abs(xValue) + abs(yValue) > 500:   # umbral para decidir movimiento
        mov = 1
    
    # empaquetamos los datos: 2 enteros cortos y 3 enteros sin signo de 1 byte
    data = struct.pack('>2h3B', xValue, yValue, int(aState), int(bState), mov)
    
    uart.write(data)
    sleep(100)  # envia datos a 10 Hz
```

el nuevo codigo sigue leyendo los valores del acelerometro en los ejes x y y, y tambien revisa si los botones a y b estan presionados; la diferencia es que ahora calcula una variable extra llamada mov, esa variable sirve para indicar si el microbit esta quieto o en movimiento, y lo hace sumando los valores absolutos de x y y, si esa suma es mayor que 500 pone mov en 1 (movimiento), si no lo deja en 0 (quieto); despues empaqueta todo en binario con el formato >2h3B, eso significa que envia 2 enteros cortos con signo (x y y) y 3 bytes (estado del boton a, estado del boton b y el estado de movimiento), en total cada mensaje ocupa 7 bytes; al final manda esos bytes por el puerto serial 10 veces por segundo, asi que en el monitor se ve un paquete de 7 bytes donde el ultimo cambia a 00 o 01 dependiendo de si el microbit esta quieto o agitado

con este codigo confirmamos dos cosas clave:

cantidad de bytes enviados: ya no son 6 bytes como antes, ahora son 7, porque le agregamos un byte extra (mov) que indica si hay movimiento o no

funcion del ultimo byte: comprobamos que ese byte funciona como una bandera, cuando el microbit esta quieto aparece 00 y cuando lo agitas aparece 01

<img width="911" height="157" alt="image" src="https://github.com/user-attachments/assets/68b4e9c8-773a-4ac5-8d58-1cfe1da22216" />

en la captura del monitor en hex se observan los paquetes binarios que envia el microbit con el nuevo codigo, cada paquete tiene 7 bytes en total; los dos primeros bytes corresponden al valor de x del acelerometro, los dos siguientes al valor de y, el quinto byte indica el estado del boton a, el sexto el estado del boton b y el septimo byte es la bandera de movimiento que agregamos; en varios de los paquetes se ve que el ultimo byte aparece como 00, lo que significa que el microbit estaba quieto, y en otros paquetes el ultimo byte aparece como 01, lo que indica que se detecto movimiento; esto confirma que el formato >2h3B se esta aplicando correctamente y que el byte adicional funciona como indicador simple de movimiento

#### Hallazgos

en este experimento se confirmo que el microbit envia ahora 7 bytes por mensaje, de los cuales los primeros seis siguen representando los valores de los ejes x y y del acelerometro junto con los estados de los botones a y b, mientras que el septimo byte corresponde a la bandera de movimiento; se observo que este ultimo byte toma el valor 00 cuando el microbit permanece quieto y cambia a 01 cuando se agita, lo que demuestra que la logica implementada funciona correctamente; ademas se pudo comprobar que es posible ampliar el formato binario sin perder eficiencia y que los datos se mantienen compactos y organizados para su posterior interpretacion

### Actividad 03

n la unidad anterior era necesario enviar los datos delimitados y marcados con un salto de línea porque el tamaño de cada mensaje podía variar. Como los datos se transmitían en formato texto (ASCII), cada número podía tener diferente cantidad de dígitos dependiendo de su valor (por ejemplo, el número 9 ocupa un solo carácter mientras que 500 ocupa tres caracteres). Eso significaba que no había una forma fija de saber dónde terminaba un valor y empezaba el siguiente. Para resolverlo, se usaban delimitadores (como comas o espacios) y un salto de línea para indicar el final del paquete, de modo que el receptor pudiera separar los valores de forma correcta.

En cambio, en esta nueva unidad los datos se envían en formato binario con un tamaño fijo por paquete. Aquí sabemos exactamente que el mensaje siempre ocupa 6 bytes (2 para xValue, 2 para yValue, 1 para aState y 1 para bState). Como cada dato ocupa siempre la misma cantidad de espacio, el receptor puede leer directamente los bloques de bytes sin necesidad de un delimitador ni un salto de línea

### ¿que pasaria si cambiamos el orden de los bytes en el paquete (little endian en vez de big endian), el receptor seguiria interpretando bien los datos o se dañaria toda la lectura?

#### Hipotesis

si cambiamos a little endian puede que los valores numericos de x e y se vean raros o diferentes en el receptor, no se si van a aparecer como numeros totalmente distintos o solo con un ligero cambio; los bytes de un solo octeto (los botones) probablemente no cambien, pero puede que aparezcan patrones que compliquen la lectura hasta que ajustemos el parseo.

#### Experimentacion  

 para probar la hipotesis vamos a modificar el codigo del microbit, en la parte donde usamos struct.pack, cambiamos el formato de >2h2B a <2h2B. esto hara que los datos de xValue y yValue se empaquen en little endian. luego cargamos el programa en el microbit y abrimos el monitor serial o el codigo en p5.js para ver como llegan los bytes. vamos a comparar los 6 bytes que se envian en big endian (ejemplo 01 f4 02 0c 01 00) contra lo que aparece en little endian (esperamos ver los mismos numeros pero ordenados de otra manera en sus bytes). tambien anotamos si los botones cambian o no.

 ```python
# Imports go at the top
from microbit import *
import struct
uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = 500
    yValue = 524
    aState = True
    bState = False
    # aqui cambiamos el formato de > (big endian) a < (little endian)
    data = struct.pack('<2h2B', xValue, yValue, int(aState), int(bState))
    uart.write(data)
    sleep(100) # envia datos a 10 hz
```

<img width="963" height="232" alt="image" src="https://github.com/user-attachments/assets/2ca08e4b-4190-475d-8db8-7781c334eb9e" />


en la captura se ven los datos que el microbit esta enviando pero con un orden diferente al que esperabamos; en lugar de salir como 01 f4 02 0c 01 00 aparece como f4 01 0c 02 01 00, esto significa que los bytes de los numeros se estan enviando al reves, o sea en little endian; entonces el numero 500 que deberia ser 01 f4 en big endian ahora aparece como f4 01, y lo mismo pasa con el 524 que deberia ser 02 0c y aparece como 0c 02; los ultimos dos bytes (01 y 00) si se mantienen igual porque son de un solo byte y no les afecta el cambio de orden; en resumen la imagen muestra que el orden de los bytes cambia la forma como se representan los numeros y por eso es importante que el receptor sepa en que formato esta llegando la informacion

#### Hallazgos

al hacer el cambio de big endian a little endian vimos que los numeros no desaparecen ni se dañan, pero si cambian de orden los bytes y eso hace que si el programa en p5 no esta preparado para leerlos asi, va a interpretar valores equivocados; tambien notamos que los datos de un solo byte como los botones no se ven afectados por el cambio de endianess, se mantienen igual; este experimento confirma que el formato binario funciona bien pero hay que ser muy cuidadosos con el orden de los bytes porque de eso depende que los numeros se entiendan correctamente

en la unidad anterior el codigo era mucho mas basico porque solo se encargaba de abrir el puerto y esperar datos en texto, entonces lo que llegaba se podia separar con comas y ya, pero ahora el codigo cambio porque los datos vienen en binario y eso obliga a leerlos de otra forma; en vez de usar lineas de texto ahora se leen bloques fijos de 6 bytes y con DataView se interpreta cada parte como int16 o como uint8 segun corresponda, eso hace que no tengamos que convertir cadenas sino que el microbit y p5 ya se entienden directamente con numeros; la diferencia principal es que pasamos de manejar strings a manejar datos binarios estructurados


### ejecutoamos en p5.js
<img width="1917" height="777" alt="image" src="https://github.com/user-attachments/assets/27f02543-cf5e-49a0-ad68-3564a75e6ebb" />


En la consola se ve que al principio los datos del micro:bit llegan bien, por ejemplo con microBitX: 500 microBitY: 524 microBitAState: true microBitBState: false. Pero después aparecen números sueltos como 11 o 157, y valores extraños como microBitY: 256. Esto ocurre porque los bytes que manda el micro:bit no se leen al mismo tiempo en p5.js, y por eso la información se descuadra y se mezclan datos que no corresponden. Algo que no se porque pasara es porque el microbit dibuja solo en el p5.js, pero es algo bueno que podemos investigar

### ¿Por que el microbit dibuja solo cuando se ejecuta el codigo?

#### Hipotesis

puede que el microbit este enviando datos constantemente (por el loop) y por eso p5.js recibe paquetes que lo hacen dibujar sin presionar, o puede que haya un problema de sincronizacion donde bytes mal alineados hacen que el byte del boton a parezca 1 aunque no se haya presionado; otra posibilidad es que el codigo de prueba en el microbit este enviando aState=true por defecto (como en el ejemplo que usamos antes) y entonces p5 interpreta eso como pulsacion real

#### Experimentacion

vamos a cambiar un poco el codigo de p5.js para  que nos muestre los bytes crudos en HEX y podamos confirmar si el microbit esta mandando true cuando no deberia o si es un problema de sincronizacion
``` python
const lineModule = [];

function preload() {
  lineModule[1] = loadImage("02.svg");
  lineModule[2] = loadImage("03.svg");
  lineModule[3] = loadImage("04.svg");
  lineModule[4] = loadImage("05.svg");
}

let port;
let connectBtn;
let connectionInitialized = false;
let microBitConnected = false;

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};
let appState = STATES.WAIT_MICROBIT_CONNECTION;

let prevmicroBitAState = false;
let prevmicroBitBState = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

function draw() {
  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      checkForMicrobitConnection();
      break;
    case STATES.RUNNING:
      readMicrobitData();
      break;
  }
}

// ****************************************************
// chequeo conexion
function checkForMicrobitConnection() {
  if (port.availableBytes() > 0) {
    console.log("Microbit ready to draw");
    microBitConnected = true;
    appState = STATES.RUNNING;

    // limpiar buffer al inicio para evitar bytes basura
    port.readBytes(port.availableBytes());
  }
}

// ****************************************************
// lectura de datos
function readMicrobitData() {
  if (port.availableBytes() >= 6) {
    let rawData = port.readBytes(6);

    // log de bytes crudos en HEX para ver si hay errores
    console.log("Raw bytes:", rawData.map(b => b.toString(16).padStart(2, "0")).join(" "));

    let microBitX = rawData[0] << 8 | rawData[1];
    let microBitY = rawData[2] << 8 | rawData[3];
    let microBitAState = (rawData[4] === 1);
    let microBitBState = (rawData[5] === 1);

    console.log("microBitX:", microBitX,
                "microBitY:", microBitY,
                "microBitAState:", microBitAState,
                "microBitBState:", microBitBState);

    updateButtonStates(microBitAState, microBitBState);
  }
}

// ****************************************************
// debounce simple
function updateButtonStates(newAState, newBState) {
  if (newAState === true && prevmicroBitAState === false) {
    console.log("A pressed (valido)");
  }
  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}
```
este codigo nuevo lo que hace es que ademas de leer los 6 bytes que envia el microbit por el puerto serial, tambien imprime esos mismos bytes en crudo pero en formato hexadecimal; esto sirve para ver exactamente lo que esta mandando el microbit sin interpretar nada; despues de mostrar los bytes en hex, el codigo los acomoda segun el formato >2h2B y los convierte en valores que entendemos: la posicion x y del acelerometro y el estado de los botones a y b; en resumen, primero vemos los datos tal cual llegan y luego la traduccion de esos datos a numeros y estados recognoscibles

<img width="1919" height="787" alt="image" src="https://github.com/user-attachments/assets/76a4136f-6c16-45b5-be53-7a91e54dd3c6" />

En el experimento con el micro:bit me di cuenta de que siempre aparecen los mismos números en la consola. Los “raw bytes” salen iguales cada vez y eso significa que el micro:bit está mandando la misma información todo el tiempo. Cuando el programa traduce esos datos, se ve que el valor en X y en Y no cambia y que el botón A aparece como si estuviera presionado aunque yo no lo toque. Por eso parece que el micro:bit dibuja solo, porque en realidad está enviando siempre el mismo estado fijo.

#### Hallazgos

Con este experimento concluimos que el micro:bit estaba enviando datos repetidos sin que yo lo tocara. Eso significa que no siempre depende de que yo presione un botón, sino que el programa lee lo que recibe en crudo y lo interpreta como si el botón estuviera activado todo el tiempo. También aprendimos que los “raw bytes” muestran el estado de los sensores y que, si esos valores no cambian, el micro:bit se comporta de manera automática. En otras palabras, descubrimos que el dibujo se genera solo porque los datos enviados están fijos y no porque yo los controle directamente.



En la consola ahora se ven los mensajes de conexión y desconexión normales del puerto serial y los eventos de los botones “A pressed” y “B released” en orden. Ya no aparecen números extraños ni valores dañados como antes.

Esto pasa porque con el nuevo código se agregó un mecanismo de framing: cada paquete empieza con un byte de inicio, luego van los datos, y al final un checksum. De esa forma, p5.js solo procesa los paquetes completos y válidos, ignorando bytes sueltos o mezclados.

El hallazgo principal es que el framing soluciona el problema de sincronización. Con esto los datos llegan bien alineados, los botones responden cuando deben y el dibujo en pantalla ya no se activa solo ni muestra valores incoherentes.

<img width="1919" height="867" alt="image" src="https://github.com/user-attachments/assets/2db178d1-61bd-4dbd-bd21-1bbeea0edd28" />


### Axtividad 04

1. Lo primero que vamos a hacer es ejecutar nuestro codigo en p5.js tal cual, lo unico que tiene diferente  es el codigo del micro:bit, vaoms a ver que sucede


 <img width="1917" height="854" alt="image" src="https://github.com/user-attachments/assets/96b3083e-78ae-4137-bd63-bd6a6293bcff" />

 No muestra nada, nisiquiera la consola muestra los datos que le llegan, lo unico raro es que aparecen unas lineas punteadas. Pero probablemente solo significa el intento que hace el codigo por leer los datos que el micro:bit envia


 2. en vez de usar readUntil("\n"), vamos a leer bytes crudos con readBytes(). Imprimimos en consola los bytes en hexadecimal para inspeccionar.

    <img width="1916" height="854" alt="image" src="https://github.com/user-attachments/assets/daf33a8f-4402-444f-8671-e931f5f2cd2d" />

    Ahora podemos ver que datos binarios llegan del microbit

3. ahora implementaremos el buffer y framing







4.   

<img width="1917" height="968" alt="image" src="https://github.com/user-attachments/assets/4f5e5865-5c14-4e45-8203-493817605f52" />


   


 









 





 













