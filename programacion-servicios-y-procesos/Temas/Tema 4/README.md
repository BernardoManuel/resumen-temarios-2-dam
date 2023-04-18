# Tema 4 - Programación de servicios
## 1. Servicios en red. Protocolos de comunicación
### 1.1. Servicios
La comunicación entre dos aplicaciones que están en distintas máquinas se establece mediante elementos de la arquitectura TCP/IP y su estructura de capas. En dichas capas existen una serie de protocolos que ofrecen sus servicios a los protocolos de la capa superior, y que utilizan los servicios de las capas inferiores para conseguir sus fines.

¿Qué es un servicio? Un **servicio** prodemos definirlo como un conjunto de funciones o tareas que pueden cumplir una misión para alguien o algo.

### 1.2. Protocolos de comunicaciones
Un protocolo de comunicaciones es el conjunto de normas y reglas que tienen que seguir emisor y receptor para establecer una comunicación e interactuar de manera adecuada.

En los servicios tenemos que definir un protocolo de comunicaciones entre nuestras aplicaciones. El servicio establecerá unas reglas para su uso, y el usuario debe cumplir normas para poder hacer uso de dicho servicio.

### 1.3. Tipos de protocolos
#### Protocolos orientados a la conexión
En este tipo de protocolos, cliente y servidor establecen una conexión que se mantiene abierta todo el tiempo que dura el intercambio de información o el uso del programa que gestiona dicho protocolo. Un ejemplo de este tipo de protocolo es una conexión SSH o una conexión por FTP. En estos protocolos la operativa es:
1. **Establecimiento de la conexión**: el servidor le pide al cliente que se valide o identifique. En este paso el cliente habitualmente le enviará su usuario y contraseña.
2. **Acción del servicio**: cliente y servidor intercambiarán los mensajes que consideren. Estos pueden ser mensajes como tal, intercambio de ficheros, etc.
3. **Cierre de la conexión**: se termina el intercambio de información y, finalmente, se liberan los recursos de la misma.

Habitualmente, este tipo de protocolos suelen trabajar con TCP, debido a la seguridad y a la existencia de la conexión.

#### Protocolos no orientados a la conexión
Son protocolos en los que, como su nombre indica, no existe un canal continuo entre los dos elementos. En ellos, básicamente el servidor escucha una petición, la procesa y da respuesta mediante un intercambio simple de mensajes: petición – respuesta. HTTP es el protocolo por excelencia no orientado a la conexión.

Estos protocolos se caracterizan por la ausencia de memoria. Cuando se sirve una petición, se olvida lo anterior y no hay datos que se mantengan entre peticiones (stateless). Para garantizar la seguridad y la autenticación en estos protocolos sin conexión se recurre a las sesiones http y a las cookies:
- Las cookies son archivos que contienen información (muy poca) sobre las preferencias de un usuario respecto a las páginas que visita. Se guardan en el cliente, y cuando se solicita una página, el navegador comprueba si tiene una cookie de esa página, para ya tener algunas preferencias cargadas (idioma, aspecto, etc.).
- Las sesiones crean también cookies, pero las almacenan en la parte del servidor, creando un identificador único para cada cliente. Este ID se envía al cliente en la respuesta de la primera petición. En cada petición sucesiva, el cliente enviará esa ID, con lo que el servidor ya puede conoce o recuperar información anterior.

Otro mecanismo de autenticación son los tokens que, siguiendo un procedimiento similar al anterior, se generan al validarse el usuario por primera vez Estos token, además, se encriptan, de manera que en las futuras peticiones que realice un cliente enviará su token, y el servidor puede conocer sin ningún lugar a dudas que ese token ha sido expedido por él mismo y la identificación del usuario.

#### Intercambios de mensajes - notificaciones
Habitualmente, en la mayoría de los protocolos cliente servidor es el cliente quien inicia la petición y el servidor la atiende y le da respuesta. Así pues, al abrir nuestro cliente de correo electrónico, este le mandará una petición al servidor, preguntando si tenemos nuevo correo, y en caso afirmativo, lo descargamos.

En caso de que sea el servidor quien inicie la contienda sin pedirlo el cliente, estamos ante lo que se conoce como una **notificación push**. Estas botificaciones han tenido un gran auge con la aparición de los SMS, y más tarde con las aplicaciones de mensaje ía instantánea, como **WhatsApp** o **Telegram**. Cuando hay algún mensaje para nosotros, es el servidor el que o bien directamente nos indica que tenemos un nuevo mensaje, o bien nos lo manda a nuestra aplicación.

Para emular las notificaciones **push** con las peticiones clásicas, por ejemplo, en el correo electrónico, basta con programar algún hilo temporizador que, cada 5 o 10 minutos, pregunte al servidor si hay correo nuevo; es lo que se conoce como **polling**.

## 2. Modelo cliente-servidor
### 2.1. Paradigma cliente-servidor
En el mundo de las comunicaciones entre equipos, el modelo de comunicación más utilizado es el modelo cliente-servidor, ya que ofrece una gran flexibilidad, interoperabilidad y estabilidad para acceder a los recursos de forma centralizada. El término modelo cliente-servidor se acuñó por primera vez en los años ochenta para explicar un sencillo paradigma: un equipo cliente requiere un servicio de un equipo servidor.

Desde el punto de vista funcional, se puede definir el modelo cliente-servidor como una arquitectura distribuida que permite a los usuarios finales obtener acceso a los recursos de forma transparente en entornos multiplataforma. Normalmente, los recursos que suele ofrecer el servidor son datos, pero también puede permitir acceso a dispositivos hardware (discos de red, impresoras), tiempo de procesamiento, ejecución de tareas, etc.

#### Cliente
Es el proceso que permite interactuar con el usuario, realizar las peticiones, enviarlas al servidor y mostrar los datos al cliente. En definitiva, se comporta como la interfaz (**front-end**) que utiliza el usuario para interactuar con el servidor. Sus funciones serán:
- Interactuar con el usuario.
- Procesar las peticiones para ver si son válidas y evitar peticiones maliciosas al servidor.
- Recibir los resultados del servidor.
- Formatear y mostrar los resultados.

#### Servidor
Es el proceso encargado de recibir y procesar las peticiones de los clientes para permitir el acceso a algún recurso (**back-end**). Las funciones del servidor son:
- Aceptar las peticiones de los clientes.
- Procesa las peticiones.
- Formatear y enviar el resultado a los clientes.
- Procesar la lógica de la aplicación y realizar validaciones de datos.
- Asegurar la consistencia de la información.
- Evitar que las peticiones de los clientes interfieran entre sí.
- Mantener la seguridad del sistema.

La idea es tratar al servidor como una entidad que realiza un determinado conjunto de tareas y que las ofrece como servicio a los clientes. La forma más habitual de utilizar el modelo cliente-servidor es mediante a utilización de equipos a través de interfaces gráficas; mientras que la administración de datos y su seguridad e integridad se dejan a cargo del servidor.

### 2.2. Modelos cliente-servidor según la función
Según las funciones que realizan los equipos dentro de la estructura, existen diferentes clasificaciones. En el modelo cliente-servidor, como hemos visto es el cliente el que inicia la comunicación y formula una petición y el servidor es quien atiende y sirve la petición.

Puede ocurrir que para resolver su tarea el servidor deba recurrir a otro servidor, como es el caso de los servidores DNS. Si un servidor no posee la información de un dominio, recurre al servidor DNS de dicho dominio para la consulta. Entonces el servidor, como es quien realiza una petición, invierte su rol y pasa a ser un cliente.

A veces, simplemente los clientes se conectan entre ellos, ofreciéndose mutuamente los servicios los unos a los otros. Este tipo de organización se denomina **redes entre iguales** o **peer to peer (p2p)**. Este tipo de redes se generalizó con la aparición de ciertos protocolos de compartición de información como eMule o BitTorrent. Estas redes están habitualmente relacionadas con la piratería por la compartición de archivos sin permiso, pero muchas distribuciones Linux las utilizan para la descarga de sus imágenes.

Las redes/servicios en las que ninguno de los equipos que las forman realiza tareas de gestión se denominan **redes descentralizadas**. Si no están correctamente implementadas, pueden caer en la anarquía; por ello suelen aparecer servidores que se encargan de “poner orden” dentro de la maraña de equipos, dando pie a los **modelos híbridos** o **p2p centralizados**. Estos servidores no almacenan ningún tipo de información, sino que almacenan metainformación para su funcionamiento.

Hay que indicar que los servidores pueden estar centralizados o distribuidos entre distintas máquinas a los largo de la red. La distribución de los servicios se utiliza por varios motivos: replicación de la información, proximidad, tolerancia a fallos, escalabilidad y mucho otros factores.

Hoy en día, con el uso de los contenedores y los servidores virtualizados, prácticamente la totalidad de los servidores están replicados.

Este tipo de servicios van asociados a un **balanceo de carga**, que es un servicio que se encarga de asignar la tarea a aquel servidor que menos carga de trabajao tenga, o menos tiempo de respuesta necesite, etc.

### 2.3. Programación cliente-servidor
En este apartado vamos a ver el algoritmo que tenemos que aplicar a nuestros clientes y servidores, una vez establecido el protocolo a utilizar. Considerad que dicho algoritmo no tiene en cuenta el propio servicio del servidor, sino solo como deben actuar tanto el cliente como el servidor.

#### Cliente
La lógica del cliente es muy simple. Una vez establecida la conexión, la parte del cliente que se comunica con el usuario le solicita que petición quiere hacer, con su interfaz de usuario. La petición se envía al servidor y se recibe la respuesta. Todo esto mientras el usuario no decida salir.

#### Servidor
Básicamente, el servidor acaba cuando el cliente en su petición decide acabar ya que la variable que controla el bucle (semi) infinito se activará según lo de ida el cliente.

Otro concepto importante es la finalización del servidor: **nunca**. Los servicios están pensados para ejecutarse de manera infinita, con las lógicas paradas de mantenimiento, escalado (añadir más servicios) y actualizaciones. Por eso es muy importante que lo protejamos frente a peticiones incorrectas o errores en la atención a los clientes, y que un error en la petición de un cliente no afecte al funcionamiento general del servidor.

#### Desarrollo de la aplicación
Es importante en el desarrollo tanto de cliente como del servidor, que nos organicemos como programadores. Hay que tener en cuenta que hemos de desarrollar dos rogramas, y que dichos programas serán ejecutados muy probablemente en máquinas distintas. 

Así pues, la recomendación es que, dado que Java nos ofrece la posibilidad de separar bloques del programa en paquetes podemos gestionarlo como sigue:
1. Un paquete con las clases de utilidad y configuraciones comunes de cliente y servidor (textos, números de puertos temporizadores).
2. Un paquete con el programa del cliente.
3. Un paquete con el programa del servidor.

¿Qué ocurre después de probarlo y testearlo? Que si empaquetamos el programa, tenemos todo el código junto. Así pues, es preferible refactorizar nuestro proyecto en dos programas distintos, para empaquetar solo lo necesario.

Finalmente, se recomienda probar la ejecución no solo como dos programas distintos, sino en dos máquinas distintas, e incluso con distinto sistema operativo, para asegurarnos la portabilidad.

## 3. Programación de servidores
### 3.1. Difusión
Cuando se implementan aplicaciones distribuidas, y más aún en un chat, aparece el concepto de difusión o broadcast. Es decir, debemos entregar nuestros mensajes a todos los participantes. Si delegamos la función de dicha difusión en los clientes, pueden ocurrir cosas como la que podemos ver en la imagen de la izquierda.

Aparece el concepto de difusión atómica o en orden total, que garantiza que los mensajes lleguen a todos los par icipantes en el mismo orden. Para ello necesitaremos añadir la figura de un servidor centra izado, al cual los usuarios enviarán sus mensajes y que se encargará de difundirlos, incluso al propio cliente que generó el mensaje.

Aquí también aparecen dos conceptos interesantes, relacionados con la manera en la que el servidor enviará los mensajes. Habitualmente en el modelo cliente-servidor los mensajes del servidor siempre son en respuesta a peticiones de los clientes. Observamos aquí:
- **Notificaciones push**: el servidor mandará mensajes sin que los haya solicitado el cliente. Este tipo
de funcionamiento apareció junto con las aplicaciones de mensajería instantánea.
- **Polling**: técnica mediante la cual un cliente pregunta, cada poco tiempo, si tiene algún mensaje para
él. Es el funcionamiento de ciertos clientes de correo. Con esta técnica es difícil conseguir la difusión
atómica.

### 3.2. Diseño del servidor
Debemos tener en cuenta que nuestro servidor debe atender a múltiples clientes, y lo que es más importante, debe atenderles a todos a la vez. Por lo tanto, la tarea de atender a cada cliente debe ser realizada por un hilo de ejecución distinto. Siguiendo el algoritmo visto en el apartado anterior, lo modificaremos para delegar toda la lógica de negocio en el hilo que se crea para cada cliente. Es lo que hemos denominado **HiloServidor**.

Además de delegar la lógica en este hilo, necesitaremos tener una clase que coordine a todos los elementos, permitiendo realizar las difusiones, controlando cuántos usuarios hay e identificando a cada usuario. Esta clase es la que denominaremos en nuestro proyecto el **Gestor**. Dicho gestor contendrá como elemento principal una colección de todos los participantes que hay en la sala en un momento dado.

Como puedes ver, la colección enlazará a todos los hilos que crea el servidor cuando se conecta un participante. Como métodos destacan:
- **Difundir**: que difunde un mensaje de los recibidos de los clientes o de la lista de participantes. La lista de participantes la obtenemos de los **nicks** de cada hilo. La difusión no es más que recorrer la colección y enviar el mensaje al cliente asociado.
- **añadir()/eliminar()**: son los métodos que se encargan de permitir la entrada y la salida de los clientes en el chat.

Como puede observarse, cada hilo tendrá una referencia al Gestor, lógicamente para poder invocar a los métodos de comunicación con el resto de los clientes.

### 3.3. Diseño del intermediario-hilo servidor
#### Establecimiento de la conexión
El intermediario será un hilo de ejecución, que será creado en cuanto el servidor reciba una conexión desde algún cliente. Es decir, en primera instancia se aceptan todos los clientes. Como el servidor, al establecer la conexión, genera un socket que se comunica con el cliente, al crear el hilo le pasará dicho socket, así como una referencia al Gestor.

El primer mensaje que recibiremos será el **nick** del cliente. Como tenemos la referencia al Gestor, podemos comprobar si dicho **nick** existe o no, para permitir la entrada. También pueden revisarse más criterios, como acotar el número de participantes, para no saturar al servidor y evitar ataques **DoS**. Si se cumplen los criterios para poder entrar, bastará con invocar al método **añadir()** del Gestor.

#### Intercambio de mensajes
En la fase en la que ya estamos conectados, simplemente el hilo debe leer los mensajes desde su socket y llamar al método **difundir** del Gestor.

#### Finalización de la conexión
Si en alguno de estos mensajes viene el mensaje de finalización, por parte del cliente, deberá avisar al resto de participantes de que este clietne quiere desconectarse y llamar al método **eliminar()**, para sacarlo del listado del Gestor. Una vez eliminado el cliente, deberá publicarse la nueva lista de usuarios llamando a **difundirLista()**.

### 3.4. Diseño del cliente
El cliente presentará una interfaz gráfica, que es con la que se comunicará el usuario final. Dicho cliente dispondrá de un socket para comunicarse con su intermediario. Tenemos que descomponer la lógica de leer/escribir desde/al socket en dos procesos distintos. Así pues, tendremos:
- Un hilo que gestiona la vista, que se encargará de recoger las peticiones del usuario y mandarlas hacia el servidor. Este puede ser el hilo principal de la aplicación.
- Un hilo que se encargará de leer del socket y plasmar lo que ha leído en la vista. Dicho hilo se creará en el mismo momento que estemos conectados al servidor.

#### Protocolo
Recopilamos aquí los mensajes que intercambiarán cliente y servidor.

Recuerda que el protocolo es una parte fundamental en el diseño de este tipo de aplicaciones, ya que cliente y servidor deben cumplirlo a rajatabla. En este diseño se enviarán empaquetados en objetos **JSON**, ya que podemos indicar con un campo el tipo de mensaje y con el otro, el contenido. Esta versatilidad nos ayudará a hacer crecer nuestro servidor añadiendo servicios, mejorando así la escalabilidad del mismo.

## 4. Servicios REST
Muchos servicios están diseñados para no depender de un estado, y por tanto la respuesta que elaborará el servidor no depende de una serie de pasos realizados previamente, sino que solo dependerá de la información recibida desde el cliente. En el apartado 2 se comentó que **http** era el protocolo de referencia sin conexión. Este protocolo consigue **"recordar"** información entre peticiones mediante mecanismos de sesiones o cookies, que consumen recursos en el servidor.

Los servicios **REST (REpresentational State Transfer)** son otros de los protocolos **stateless** por excelencia. El protocolo está disñeado sin memoria, por lo que no recuerda nada entre llamadas del mismo cliente. Estos protocolos tienen la ventaja del gran ahorro de recursos en cuanto a la cantidad de memoria que consumen los servicios con estado. Por otra parte, tienen la desventaja de que, debido a esto, si queremos dotar de memoria al propio servicio la tarea recae también en el cliente, que tendrá que reenviar en cada petición los datos necesarios, como pueden ser datos de validación, tokens, etc.

Además, **REST** utiliza http como mecanismo de intercambio de mensajes mediante las peticiones **POST**, **GET**, **PUT** y **DELETE**. Se identifican las peticiones a través de la URI, y de ningún otro elemento, los datos que quieran facilitarse se envían a través del cuerpo de la petición.

### 4.1. Autenticación en servicios sin estado
Habitualmente, en los servicios con conexión el primer paso consiste en establecer la conexión, y para ello el usuario manda su usuario y contraseña creándose la sesión para dicho usuario.

En los servicios sin conexión basados en token, durante el proceso de identificación del usuario el servidor generará un testigo o token, y le enviará al usuario, junto con la respuesta, dicho token, en caso de ser una autenticación satisfactoria. Este token contendrá la información para validar al usuario, más otra que se considere interesante.

El usuario que recibe el token se da por validado y a partir de ese momento enviará en sus peticiones siguientes dicho token. El servidor solo aceptará peticiones que tengan un token válido.

El token puede ser "manipulado" de manera que el servidor podrá recibir peticiones falsas. Para evitar esto, firmaremos el token antes de enviarlo al cliente.

Así pues, eñ servidor, al recibir una petición de un cliente, lo primero que hace es verificar la autenticidad del token. Al estar firmado, cualquier manipulación que se haya realizado sería detectada por el servidor, que, por tanto, rechazaría dicha petición.

#### JSON Web Tokens
El modelo escogido para trabajar es el JSON Web Token (JWT). En dicho modelo, el token es una cadena codificada en Base64.

Como podemos comprobar, el token está dividido en tres partes. Cada una de estas partes es un objeto individual JSON:
- **Header o cabecera**
- **Payload o carga útil**
- **Firma**

#### JWT en Java
Existen multitud de librerías para manipular JWT en varios lenguajes de programación. Nosotros, para Java, utilizaremos **io.jsonwebtoken**. Para construir el JWT se puede hacer de manera muy cómoda mediante el encadenamiento de llamadas a distintos métodos (**objeto .aplicarMetodo1() .aplicarMetodo2() aplicarMetodo3() ...**).

Crearemos el token de la siguiente manera:
```java
String jws = Jwts.builder()
    .setHeaderParam("unatributo", unvalor)
    .claim("atributo", valor)
    .signWith(key)
    .compact();
```
1. Creamos la instancia del **Jwt** y vamos encadenando las llamadas necesarias.
2. Podemos añadir atributos en la cabecera. Si no aparecen, se generan valores por defecto, como por ejemplo el algoritmo seleccionado.
3. La carga útil (dependiendo de los lenguajes se denomina **payload** o **claim**) se añade con el método **claim(atributo,valor)**, con parejas **atributo-valor**. Recuerda que el contenido se genera como un JSON.
4. Finalmente se establece la clave para la generación, o también indicando el algoritmo y la clave para la generación de los resúmenes y la firma.
5. El método **compact()** es el último de la cadena y genera la cadena en Base64 con la cabecera, la carga y el resumen, separado por puntos.

Veamos ahora cómo es el proceso inverso de, a partir de un token, leer la carga útil, validando que dicho token es correcto.
```java
String token=.....;
Jwts.parser()
    .setSigningKey(password)
    .parseClaimsJws(token)
    .getBody()
    .get(“role”) 
```
1. Obtenemos el token, que vendrá con la petición del cliente. Habitualmente vendrá en la cabecera de la petición, pudiéndolo obtener mediante **request.headers(“Authorization”)**
2. En esta línea obtenemos el **parser** o conversor, que nos permitirá obtener lo métodos para la conversión del String al objeto que contiene los JSON.
3. Los pasos siguientes son los de la validación y autenticación del token. En este primero indicamos la clave o password de cifrado, que debe coincidir con la orig nal, en caso de cifrado simétrico, o ser la pareja de una pública/privada en el cifrado asimétrico
4. En este último paso es cuando se valida el token y se obtiene la carga útil (los claims). Este objeto que se obtiene es un **Map** con los elementos que lo componen.
5. De dichos claims podemos obtener elementos fijos como **getHeader()** o **getBody()**.
6. De este úl imo obtendremos los elementos que contiene el cuerpo mediante métodos **get(“etiqueta”)**, para acceder al valor asociado a un atributo.

Si se produce alguna manipulación en el token (caducidad, generación con un password distinto) pueden aparecer las siguientes excepciones, que deberán ser capturadas en el momento de la validación del mismo:

```java
catch (SignatureException e) {
    System.out println( La firma del JWT es incorrecta.”);
} atch (MalformedJwtException e) {
    System.out.println(“Token JWT incorrecto.”);
} catch (ExpiredJwtException e) {
    System.out.println(“Token JWT caducado.”);
} catch (UnsupportedJwtException e) {
    System.out.println(“Token JWT no soportado.”);
} catch (IllegalArgumentException e) {
    System.out.println(“Compactación incorrecta.”);
}
```

#### Spark
Para finalizar, presentaremos en la unidad un entorno de trabajo para generar servicios sin estado, como es **Spark**.

Este entorno integra de manera embebida un servidor Jetty, y su programación es muy simple, ya que simplemente indicaremos las características que deseemos mediante llamadas a funciones, como pueden ser el puerto, el número de hilos, etc. Si conoces el lenguaje de programación **Node.js**, Spark vendría a ser el equivalente al servidor web **Express**, que permite con pocas líneas de código levantar un servidor y atender peticiones.

El “hermano mayor” como gran entorno para la creación de servicios y aplicaciones web es **Spring**.

#### Microservicios
Inicialmente se crean las aplicaciones ofreciendo estas a varios servicios, todos ellos integrados. Esto es lo que se conoce como aplicaciones con **arquitecturas monolíticas**. Hay que tratar de identificar qué servicios no comparten contextos, y tratar de aislarlos, creando así lo que se denomina **microservicios**. Si existe algún punto en común, pueden enlazarse unos microservicios con otros con lamadas entre ellos, en vez de la lógica interna de la aplicación. Normalmente este canal de comunicación será la API mediante HTTP.

Todo esto permite una serie de ventajas:
- Al ser más pequeños, son más "fáciles" de diseñar y programar.
- Al ser independientes entre ellos, tienen despliegues y mantenimientos dis intos. Podemos mejorar un servicio sin tener que para los otros microservicios.
- Pueden estar implementados con tecnologías distintas solo deben respetar las interfaces entre ellos.

Algunos inconvenientes pueden ser:
- Falta de visión global de la aplicación.
- Comunicación entre servicios más lenta que dentro del mismo proceso.
- Dificultad en la detección de errores y en la correcc ón Cuando para recibir un resultado se encadenan varias peticiones entre microservicios y algo falla puede ser difícil detectar en qué eslabón de la cadena se ha producido el error. Y más aún corregirlo si, por ejemplo, se han utilizado tecnologías distintas.

## 5. Enlaces Web

- [Protocolo completo de http, original.](https://datatracker.ietf.org/doc/html/rfc2616)
- [Página oficial con los productos que ofrece dicho protocolo.](https://bittorrent.com/es)
- [Página con completa información acerca de los JSON Web Tokens](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/DatagramPacket.html)
- [Documentación de la librería io.jsonwebtoken para la creación y validación de JWT.](https://github.com/jwtk/jjwt)
- [Framework ligero para la creación de aplicaciones web para Java y Kotlin.](https://sparkjava.com/)

## 6. Respuestas cuestionario

- [Cuestionario](./CUESTIONARIO.md)