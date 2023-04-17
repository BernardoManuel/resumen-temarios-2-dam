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
