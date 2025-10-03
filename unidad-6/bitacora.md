
# Evidencias de la unidad 6

## Actividad 01

### ¿qué ocurrió en la terminal cuando ejecutaste npm install? ¿cuál crees que es su propósito?

cuando ejecuté npm install en la terminal se descargaron e instalaron todos los paquetes que el proyecto necesita para funcionar, en total agregó 116 paquetes y también me mostró que había algunas vulnerabilidades de seguridad, creo que el propósito de este comando es preparar el entorno del proyecto, o sea traer todas las librerías y dependencias que usa el código para que no tenga errores al momento de correrlo

### ¿qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿qué indica este mensaje?

después de usar npm start salió el mensaje server is listening on http://localhost:3000, eso quiere decir que el servidor ya está activo y listo para recibir conexiones en ese puerto, luego apareció el mensaje a user connected dos veces, lo que indica que dos usuarios distintos se conectaron, en este caso son las dos pestañas o ventanas del navegador que abrí con page1 y page2

### describe lo que ves inicialmente en page1 y page2 en tu navegador

tanto en page 1 como en page 2 aparecen dos circulos exactamente iguales (rojos, contorno gris y un punto negro en el centro)

### ¿qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?

al abrir page1 en el navegador apareció el mensaje a user connected en la terminal, y cuando abrí page2 volvió a salir el mismo mensaje, entonces en total aparecieron dos, uno por cada página abierta, eso confirma que el servidor está detectando y registrando a cada usuario que entra

### describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas ¿cambia algo visualmente? ¿qué mensajes aparecen en la consola del navegador y en la terminal del servidor?

cuando muevo una de las ventanas el círculo que está en esa página cambia de posición y al mismo tiempo el otro círculo en la otra página también se ajusta, la línea negra que los une se va modificando para mantener la conexión entre ellos, visualmente parece como si ambos estuvieran sincronizados en tiempo real, en la consola del navegador aparecen mensajes normales de la conexión con sockets pero nada de errores, y en la terminal del servidor no aparecen mensajes nuevos mientras hago los movimientos, solo se queda con los de las conexiones iniciales


## Actividad 02

### Piensa en cómo te conectas a Internet en casa o en la Universidad. ¿Usas Wi-Fi? ¿Un cable de red? Eso es simplemente tu “rampa de acceso” a la gran red de carreteras. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.

básicamente me quedo incomunicado, ya no podría abrir páginas web, ni enviar correos, ni ver videos, ni usar aplicaciones que dependen de internet, sería como tener el carro listo para arrancar pero con la autopista cerrada, no importa que el navegador esté funcionando si no tiene por dónde viajar

### Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)? Por ejemplo, al pedir comida en un restaurante. ¿Quién es el cliente y quién el servidor? ¿Qué se pide y qué se entrega?

pues, el mas obvio, es cuando voy a un restaurante, yo soy el cliente porque pido un plato de comida, el mesero y la cocina son los que actúan como servidor, reciben mi pedido y luego me entregan lo que pedí, otro ejemplo puede ser cuando voy a la biblioteca, yo pido un libro en el mostrador y la persona encargada me lo entrega, en ambos casos se mantiene la lógica de pedir y responder

### Toma la URL de tu sitio web favorito. Intenta identificar el protocolo, el nombre de dominio y la ruta (si la hay). ¿Qué crees que pasa si solo escribes el nombre de dominio (ej. www.google.com) sin una ruta específica? ¿Qué “página por defecto” crees que te envía el servidor?

si tomo como ejemplo la url de youtube: https://www.youtube.com/watch?v=abcd, el protocolo es https://, el nombre de dominio es www.youtube.com y la ruta sería /watch?v=abcd porque me lleva a un video específico.

si yo solo escribo www.youtube.com
sin ninguna ruta, el servidor me envía por defecto la página principal de youtube, esa donde salen recomendaciones y el buscador, creo que funciona así porque el servidor debe tener configurada una página inicial que entrega automáticamente cuando no se pide nada en específico

### Compara HTTP con los protocolos seriales que usaste.

### ¿Qué similitudes encuentras?

### ¿Qué diferencias clave ves?

### ¿Por qué crees que HTTP necesita ser más complejo que un simple envío de bytes como hacías con el micro:bit?

encuentro varias similitudes, por ejemplo que en ambos casos hay reglas claras de cómo se manda y recibe la información, en serial yo tenía que definir un formato para que p5.js entendiera los datos, y en http también se necesita un lenguaje común para que el navegador y el servidor puedan comunicarse, o sea que siempre hay un “idioma” que deben compartir

las diferencias son que el protocolo serial era más sencillo, básicamente solo mandaba bytes de un lado al otro y yo me encargaba de interpretarlos, mientras que http es mucho más complejo porque además de enviar datos también manda información extra como el tipo de archivo, el estado de la respuesta, los encabezados, etc, todo eso hace que la comunicación sea más organizada y escalable

pienso que http necesita ser más complejo porque en la web no basta con solo mandar datos, hay que saber qué tipo de archivo es, cómo mostrarlo, qué hacer si hay error, si el contenido se puede almacenar en caché, entre otras cosas, entonces el protocolo no es solo un canal de datos sino una forma de administrar todo lo que pasa entre millones de clientes y servidores

### Piensa en una página web simple, como un formulario de login.

### ¿Qué parte crees que es HTML (ej. los campos de texto, el botón)?
### ¿Qué parte es CSS (ej. el color del botón, el tipo de letra)?
### ¿Qué parte es JavaScript (ej. la comprobación de si escribiste algo antes de enviar, el mensaje de “contraseña incorrecta” que aparece sin recargar la página)?

si pienso en una página de login simple, como cuando uno entra a gmail, el html sería la estructura básica: los cuadros donde escribo usuario y contraseña, y el botón de ingresar, el css sería lo que le da estilo a esa página, por ejemplo que el botón sea azul, que las letras estén en cierto tamaño y que todo se vea alineado bonito, y el javascript sería lo que hace que la página reaccione, como comprobar si dejé un campo vacío, mostrar un mensaje de “contraseña incorrecta” sin recargar la página o incluso enviar los datos al servidor

### Compara el bucle draw() de p5.js con este modelo de “esperar a que algo pase y reaccionar”.

### ¿Qué ventajas crees que tiene el modelo basado en eventos para una interfaz de usuario web?
### ¿Sería eficiente tener un bucle draw() redibujando toda la página 60 veces por segundo si nada ha cambiado?

veo que el draw funciona como un motor que siempre está corriendo, redibujando todo a 60 cuadros por segundo, incluso si nada cambia, mientras que el modelo basado en eventos espera a que ocurra algo y solo reacciona en ese momento, creo que la ventaja de este modelo es que ahorra recursos, porque no tiene que estar repitiendo procesos innecesarios, además permite que la interfaz responda justo cuando el usuario hace clic, escribe o cambia algo

si tuviéramos un bucle como draw() redibujando una página completa 60 veces por segundo, sería un desperdicio enorme y probablemente el navegador se volvería lento, en cambio con eventos solo se actualiza lo necesario en el momento justo

### ¿Por qué crees que podría ser útil usar JavaScript tanto en el cliente (navegador) como en el servidor? ¿Se te ocurre alguna ventaja para los desarrolladores?

porque los desarrolladores solo tienen que aprender un lenguaje para trabajar en los dos lados, eso facilita mucho las cosas y hace que los equipos de trabajo puedan compartir código y lógica, además puede reducir el tiempo de desarrollo porque no hay que estar cambiando entre lenguajes distintos, por ejemplo no tienes que usar php en el servidor y javascript en el cliente, sino que todo se puede manejar en un mismo lenguaje

### Resume con tus propias palabras la diferencia fundamental entre una comunicación HTTP tradicional y una comunicación usando WebSockets/Socket.IO. ¿En qué tipo de aplicaciones has visto o podrías imaginar que se usa esta comunicación en tiempo real?

es que en http siempre es el cliente el que pide algo y el servidor responde, como mandar un correo y esperar la respuesta, en cambio con websockets se abre una conexión directa y permanente entre los dos, es como tener una llamada telefónica abierta donde ambos pueden hablar en cualquier momento sin esperar una nueva petición, esto hace posible cosas en tiempo real como chats, juegos en línea o compartir la posición de un cursor en varias pantallas

## Actividad 03

### experimento 1

codigo actualizado

```js
const express = require('express');
const http = require('http'); // Node.js built-in module
const socketIO = require('socket.io');
const path = require('path');

const app = express();
const server = http.createServer(app); // Create an HTTP server
const io = socketIO(server); // Attach Socket.IO to the server

const port = 3000;


let winstate = {
    "pagina_uno": { x: 0, y: 0, width: 100, height: 100 },
    "page2": { x: 0, y: 0, width: 100, height: 100 }
}


// Serve static files from the "public" directory
app.use(express.static(path.join(__dirname, 'public')));

// Route for pagina_uno
app.get('/pagina_uno', (req, res) => {
    res.sendFile(path.join(__dirname, 'views', 'page1.html'));
});

// Route for page2
app.get('/page2', (req, res) => {
    res.sendFile(path.join(__dirname, 'views', 'page2.html'));
});



// Socket.IO connection event
io.on('connection', (socket) => {
    console.log('A user connected');

    // Handle events from the client if needed

    // Socket.IO disconnection event
    socket.on('disconnect', () => {
        console.log('User disconnected');
    });


    socket.on('win1update', (window1, sendid) => {
        // console.log(window1)

        winstate.pagina_uno = window1

        io.emit('getdata', winstate)

    })


    socket.on('win2update', (window2, sendid) => {
        //     console.log(window2)

        winstate.page2 = window2

        io.emit('getdata', winstate)

    })



});




server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
});
```

iniciamos el servidor y entramos en page 1 


<img width="1915" height="996" alt="image" src="https://github.com/user-attachments/assets/853b6ac7-895d-4e30-b53e-c8c3723f2fc2" />



no funciona ;(((



tratamos con pagina uno

<img width="1917" height="869" alt="image" src="https://github.com/user-attachments/assets/f66e4208-82ad-488a-a81c-3cd488a9e63f" />


SI FUNCIONA :0 



esto me muestra que el servidor no “adivina” ni busca por sí solo: solo responde a las rutas exactas que nosotros configuramos en el código. Si escribo algo distinto, aunque exista el archivo, el servidor no lo entrega a menos que la ruta esté definida

### Experimento 2

ID page 1 y 2

<img width="457" height="71" alt="image" src="https://github.com/user-attachments/assets/71625ef6-8ec6-49d5-b8e4-13c888630c3a" />


ID de desconexion page 1 y 2

<img width="446" height="93" alt="image" src="https://github.com/user-attachments/assets/5cdb5dab-324a-404d-95fe-56a9c493e918" />


Ambos ID de sus respectivas paginas, tanto de conexion como desconexion son iguales, pienso que esto significa que cada URL cuenta con su propia ID para identificarse en los servidores, ene ste caso, el mio

### Experimento 3

Cambiamos el puerto

<img width="823" height="539" alt="image" src="https://github.com/user-attachments/assets/a3d32469-5873-4fad-99a7-d0b55b66a0e2" />



Lo que vemos en consola


<img width="566" height="108" alt="image" src="https://github.com/user-attachments/assets/4b913cb5-442e-4955-b575-fb099a018dab" />


no deja abrir ni page 1 ni page 2 :(((

<img width="1825" height="1022" alt="image" src="https://github.com/user-attachments/assets/6e137ee7-b9bb-42a4-8adb-e038d743c4cb" />


La variable port define en qué puerto va a escuchar el servidor, la función server.listen(port, ...) se encarga de ponerlo a la espera de conexiones justo en ese puerto.
Si intent0 conectarme e a un puerto distinto al que está escuchando, el navegador no encuentra nada

## Actividad 04

### Experimento 1

Errores que muestra la pagina cuando cierro el servidor

<img width="688" height="613" alt="image" src="https://github.com/user-attachments/assets/47e3e568-0b44-44e1-aed0-d0299aaa2280" />


la pagina despues de reiniciar el servidor

<img width="644" height="547" alt="image" src="https://github.com/user-attachments/assets/3e758824-2530-4822-bc67-a559ec598561" />


Los erroes desaparecieron. El cliente (mi navegador) necesita al servidor activo para funcionar correctamente. Si el servidor se apaga, la conexión se pierde y el navegador no puede recibir ni enviar información. Cuando lo vuelvo a encender, la comunicación se restablece

### Experimento 2

Lo que pasa cuando comento la linea socket

<img width="1908" height="941" alt="image" src="https://github.com/user-attachments/assets/2b88c5ea-a3e0-4a2a-87a7-f3f871c8af86" />

no se mueve conforme muevo la pagina 2, supongo que es por que La línea socket.emit('win2update', ...) es la que permite que los cambios de page2 viajen al servidor y luego se compartan con otros navegadores conectados, si se comenta, page2 puede moverse, pero su estado no se comunica con nadie más


### Experimento 3 


datos que muestra la consola de page2 cuando muevo page1

<img width="1919" height="929" alt="image" src="https://github.com/user-attachments/assets/748b5dd1-5c7e-460c-9986-73aed7f92a72" />


segun yo esas son las coordenadas que manda page1 de donde se encuentra para que page 2 interactue con ella


datos que muestra la consola de page 1 cuando muevo page 2

<img width="1917" height="890" alt="image" src="https://github.com/user-attachments/assets/1bbd13eb-7c80-4cb7-86ac-605668062aca" />


No muestra nada la consola, me imagino que porque comentamos el codigo de page2 y ahora no puede enviar datos al servidor

### Experimento 4

<img width="1918" height="959" alt="image" src="https://github.com/user-attachments/assets/1c30e713-2976-4035-8e09-bf9e35ddf0c6" />

<img width="1912" height="948" alt="image" src="https://github.com/user-attachments/assets/4e78dc6e-1f8b-4d81-ab79-c8ec8da6c5ae" />



Ademas de añadirle el cambio del fondo le puse que el tamaño de los circulos cambiara según que tan lejos se encontraban uno del otro, aunque no lo pude hacer del todo bien porque no se porque no me aparece el otro circulo cuando paso uno por encima del otro


## Autoevaluacion

nota propuesta: 3.0

por diferentes motivos no logre realizar mi propia aplicación y no pude completar la fase mas importante de la unidad, sin embargo considero que el desarrollo de las otras actividades junto con sus experimentos es bastante bueno, pues como se puede ver comprendi el funcionamiento del puerto serial y por que al cambiar del puerto 3000 al 3001 no se logra ejecutar la app, pues se esta esperando el envio de datos por un puerto diferente

<img width="1233" height="753" alt="image" src="https://github.com/user-attachments/assets/0d12fcf6-a813-4399-9b6d-b9e27c5bc9f2" />


Ademas de esto, formule hipotesis muy validas del por que cada pagina nueva genera una ID unica desde el momento de conexion hasta el momento de desconexion 


<img width="1319" height="431" alt="image" src="https://github.com/user-attachments/assets/666892db-62e2-4f2c-b07c-f9321cf71028" />


Entendi correctamente los conceptos y para que sirven los protocolos, las rutas y los dominios 


<img width="1262" height="352" alt="image" src="https://github.com/user-attachments/assets/c6ee8d9d-7551-4886-8a8f-f8da1e2f4354" />


Por ultimo, considero que mi comparación de envio de datos entre https y bytes es bastante bueno y demuestra un entendimiento de ambos temas

<img width="1311" height="366" alt="image" src="https://github.com/user-attachments/assets/bcc8e98c-bde5-445b-b532-65fb41339b06" />






















