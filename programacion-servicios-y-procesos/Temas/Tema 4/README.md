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