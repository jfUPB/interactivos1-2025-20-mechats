
# Evidencias de la unidad 7

## ACTIVIDAD 1

### ¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000
 o la IP local de tu computador para que el celular se conecte?

Este fue el enlace obtenido:
https://51c0043k-3000.use2.devtunnels.ms/

El motivo por el que se usa esta URL es porque localhost solo funciona en el mismo computador donde se ejecuta el servidor. Es decir, si levantamos el servidor en nuestro PC, solo ese equipo puede acceder a la página, ya que la dirección es completamente local. En cambio, Dev Tunnels actúa como un túnel que “expone” ese servidor local a Internet mediante un enlace público, permitiendo acceder desde cualquier otro dispositivo.
De esa forma, el mismo servidor puede probarse desde el celular o cualquier otro equipo: solo cambia el acceso añadiendo /Desktop/ o /mobile/ según el dispositivo desde el que se ingrese.

### Describe brevemente qué hace npm install y npm start.

El comando npm install se encarga de revisar el proyecto y descargar todas las librerías y dependencias que este necesita para funcionar correctamente.
Por otro lado, npm start ejecuta el servidor (generalmente el archivo server.js), pero requiere que previamente se hayan instalado las dependencias con npm install.
En resumen, el primero prepara el entorno instalando todo lo necesario, y el segundo pone en marcha la aplicación.

### ¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?

El mensaje que muestra la terminal es prácticamente el mismo para ambos clientes, ya que el servidor reconoce las conexiones del móvil y del computador. Sin embargo, en la consola se nota más actividad cuando se realiza algún movimiento táctil desde el celular, porque es ahí donde se genera la interacción que se refleja en el escritorio.

### Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?

Sí, la comunicación funcionó correctamente. Al mover el dedo en la pantalla del celular, el círculo del escritorio se desplazaba de forma correspondiente. Aunque la sincronización fue buena, se percibía una ligera demora entre la acción en el móvil y la reacción en el computador, lo cual indica una pequeña latencia en la conexión.

## ACTIVIDAD 2

### Explica con tus propias palabras: ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?

Dev Tunnels actúa como un canal intermedio y seguro que permite que un servidor local (que normalmente solo sería accesible en el mismo equipo) pueda ser alcanzado desde cualquier otro dispositivo a través de una dirección pública.
Esto es importante porque localhost únicamente habilita el acceso dentro del propio computador. En cambio, Dev Tunnels crea un enlace temporal con conexión segura (HTTPS), que hace posible probar la aplicación o interactuar con ella desde otro dispositivo, como un celular.

### Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.

La función touchMoved() se ejecuta constantemente mientras el usuario mantiene el dedo presionado y lo mueve por la pantalla del celular. Dentro de esta función, mouseX y mouseY guardan las coordenadas actuales del toque, permitiendo que el movimiento del dedo controle la posición del círculo o elemento en pantalla.

La variable threshold cumple el papel de filtro o límite: evita que se envíen mensajes al servidor por movimientos mínimos. Solo cuando la diferencia entre las posiciones actuales y las anteriores (lastTouchX y lastTouchY) supera ese umbral, se envía un nuevo mensaje. Así se previene una sobrecarga de eventos innecesarios y se mejora el rendimiento de la aplicación.

### Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?

Usar la IP local permite que los dispositivos dentro de una misma red (LAN) accedan a la aplicación sin depender de servicios externos. Su principal ventaja es la velocidad y estabilidad, pues la conexión es directa. Sin embargo, su alcance es limitado: solo funciona dentro de la misma red y puede verse afectado por configuraciones del firewall o la falta de HTTPS, lo que complica el uso con servicios que exigen seguridad.

En cambio, Dev Tunnels crea una dirección pública y segura que permite exponer el servidor local en Internet, sin necesidad de abrir puertos ni configurar la red. Es muy útil para compartir la app o probar integraciones que requieren un enlace accesible desde fuera (por ejemplo, autenticaciones o webhooks).
Su desventaja está en la latencia y en que depende de la estabilidad del túnel. También puede requerir reactivación si expira. En conclusión, la IP local es más directa y rápida, mientras que Dev Tunnels ofrece mayor flexibilidad y alcance.

### Coloca en tu bitácora capturas de pantalla del sistema completo funcionando. Esto lo puedes hacer abriendo tanto el mobile como el desktop en tu computador y tomando una captura de pantalla de todos los involucrados (celular, computador y terminal).

## ACTIVIDAD 3

### ¿Cuál es la función principal de express.static('public') en este servidor? ¿Cómo se compara con el uso de app.get('/ruta', …) del servidor de la Unidad 6?

La función express.static('public') le indica al servidor que debe compartir automáticamente los archivos que se encuentren dentro de la carpeta public (como HTML, CSS o JavaScript), sin que sea necesario definir manualmente cada ruta.
A diferencia de app.get('/ruta', …), donde hay que programar una respuesta específica para cada solicitud, express.static simplifica todo ese proceso, haciendo que los recursos estáticos estén disponibles de forma automática y eficiente.

### Explica detalladamente el flujo de un mensaje táctil: ¿Qué evento lo envía desde el móvil? ¿Qué evento lo recibe el servidor? ¿Qué hace el servidor con él? ¿Qué evento lo envía el servidor al escritorio? ¿Por qué se usa socket.broadcast.emit en lugar de io.emit o socket.emit en este caso?

El proceso inicia cuando el dispositivo móvil detecta un evento táctil (por ejemplo, touchmove o touchstart) y lo envía al servidor mediante socket.emit('message', datos).
El servidor recibe esa información a través de socket.on('message', ...), la muestra en la consola para registro y luego la reenvía al resto de los clientes con socket.broadcast.emit.
Los demás dispositivos (como los escritorios) escuchan ese mensaje con su propio socket.on, lo que les permite reaccionar de inmediato al movimiento del usuario en el móvil.
Se utiliza socket.broadcast.emit porque este método envía el mensaje a todos los clientes excepto al que lo originó, evitando que el celular reciba una copia innecesaria de su propio mensaje.

Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿Quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?

En ese caso, los dos computadores de escritorio serían los que recibirían el mensaje emitido por el servidor.
Esto se debe a que socket.broadcast.emit reenvía la información a todos los clientes conectados menos al que envió el mensaje. Por lo tanto, el celular no recibiría su propio evento, pero los otros dispositivos sí, permitiendo que reaccionen en tiempo real.

### ¿Qué información útil te proporcionan los mensajes console.log en el servidor durante la ejecución?

Los mensajes de console.log sirven para monitorear la actividad del servidor y verificar su correcto funcionamiento. Muestran cuándo un cliente se conecta o se desconecta, qué datos se están recibiendo y cuándo se emiten nuevos mensajes.
Esta información es muy útil para depurar errores, entender el flujo de comunicación entre los dispositivos y asegurarse de que la aplicación esté respondiendo correctamente en cada interacción.

## ACTIVIDAD 4
### DIAGRAMA DE FLUJO
<img width="912" height="711" alt="image" src="https://github.com/user-attachments/assets/77f37035-e051-4127-877f-eb2d32030126" />


## APPLY
no lo hice :(

## AUTOEVALUACIÓN

durante el desarrollo de la unidad logre comprender y aplicar los conceptos principales relacionados con la comunicacion cliente servidor y el uso de herramientas como dev tunnels para la conexion remota entre dispositivos entendi que localhost limita el acceso al propio equipo mientras que dev tunnels actua como un puente seguro que permite exponer el servidor local a internet y acceder desde diferentes dispositivos a traves de una url publica

tambien comprendi el funcionamiento de npm install y npm start identificando que el primero se encarga de instalar las dependencias necesarias del proyecto y el segundo inicia el servidor con esas librerias ya configuradas ademas asimile la logica de las funciones en el cliente movil como touchmoved que detecta el movimiento continuo del dedo y envia las coordenadas al servidor solo cuando se supera un umbral determinado con threshold evitando asi la saturacion de mensajes y optimizando el rendimiento

en cuanto a la comunicacion en tiempo real entendi el papel de socket io y como los eventos emitidos desde un dispositivo pueden ser recibidos y retransmitidos por el servidor usando socketbroadcastemit para que lleguen a los demas clientes sin duplicar el mensaje en el emisor pude observar y analizar los mensajes en consola para verificar la conexion y la interaccion entre el celular y el computador identificando que habia una pequeña latencia pero que la logica funcionaba correctamente

nota propuesta: 3.5



