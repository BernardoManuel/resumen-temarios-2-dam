# Tema 3 - Comunicación de programas
## 1. Arquitectura en la comunicación de programas
### 1.1. Comunicación de procesos
Para el escenario en que los procesos a comunicar no están ubicados en la misma máquina, tenemos que establecer otros mecanismos; comunicarse a través de las redes de comunicaciones.

Hoy en día, las redes forman una parte esencial de nuestras vidas, ya que no entendemos un mundo en el que nuestros dispositivos no permanezcan conectados e interactuando entre ellos. Nosotros, como programadores, hemos de ser capaces de desarrollar programas que funcionen, sin tener en cuenta ni la ubicación ni la tecnología subyacente (si la red es Wi-Fi, cableada, etc.).

### 1.2. El modelo de referencia TCP/IP
Como seguramente conocerás, las comunicaciones a través de Internet vienen establecidas por una serie de protocolos y convenciones. Estos protocolos se conocen como la pila de protocolos TCP/IP: protocolos abiertos implementados por diferentes plataformas HW y SW. Destacan entre todos ellos los que les dan nombre: TCP, Protocolo de Control de la Transmisión, e IP, Protocolo de Internet.

Esta pila de protocolos se realizó a partir del modelo de referencia OSI (teórico, nunca implementado).

El modelo TCP/IP tiene cuatro capas:
- **La capa interfaz red**. Esta capa permite comunicar el ordenador con el medio que conecta el equipo a la red. Utiliza un direccionamiento físico mediante las direcciones MAC y permite acceder a los dispositivos dentro de su misma red.
- **La capa de red**. Esta capa es la fundamental de la arquitectura TCP/IP, ya que permite que los equipos envíen paquetes entre redes distintas y viajen a su destino. Los paquetes pueden llegar en un orden distinto del que se enviaron, y las capas superiores se encargarán de reordenarlos. En esta capa es en la que funciona el direccionamiento y el encaminamiento (a dónde ir y por qué camino).
- **La capa de transporte**. La capa de transporte permite que los equipos lleven a cabo una conversación. Aquí se definieron dos protocolos de transporte: TCP (Transmission Control Protocol) y UDP (User Datagram Protocol). El protocolo TCP es un protocolo orientado a la conexión y fiable, y el protocolo UDP es un protocolo no orientado a la conexión y no fiable. En esta capa se añade el concepto de puerto, para identificar de manera unívoca a cada elemento de comunicación dentro de una misma máquina.
- **La capa de aplicación**. Esta capa engloba las funcionalidades de las capas de sesión, presentación y aplicación del modelo OSI. Incluye todos los protocolos de alto nivel relacionados con las aplicaciones que se utilizan en Internet (por ejemplo, HTTP, FTP, TELNET).

### 1.3. Dirección de una máquina
#### InetAddress
Esta clase nos servirá para obtener una representación de una dirección de internet. Para representar dicha dirección podemos optar por la representación numérica, o bien por su representación canónica de nombre de host. Ten en cuenta que una dirección se asigna a una interfaz de red, por lo que una máquina puede tener múltiples direcciones.

### 1.4. Puerto de una aplicación
El puerto es un número que nos permite identificar de manera unívoca un proceso dentro de una máquina. Cada programa tiene su puerto, en el que escuchará/invocará peticiones desde/hacia otros programas de otras máquinas.

Esta lista de puertos se claseifica en tres categorías:
- **Puertos bien conocidos, del 0 al 1023**: son los puertos que utilizan las aplicaciones relacionadas con la pila de protocolos TCP/IP. Están reservados.
- **Puertos registrados 1024 a 49151**: son los puertos registrados por empresas y organizaciones para sus programas, como por ejemplo MySQL, Postgres, PlayStation Network, etc.
- **Puertos libres**: todos los siguientes hasta 65535.

## 2. Clases HTTP, URL y Client
### 2.1. El paquete java.net
El conjunto de clases del paquete **java.net** agrupa todo lo necesario para realizar las comunicaciones. Este paquete está dividido en dos partes:
- **API de bajo nivel**: para la gestión directa de direcciones, puertos y sockets.
- **API de alto nivel**: para proveer el acceso rápido a recursos en internet.
    - **URI**: indica el identificador de recurso, sin indicar cómo obtenerlo.
    - **URL**: indica como acceder a un recurso.
    - **URLConnection**: es el enlace de comunicación con la URL, con algunos métodos básicos.
    - **HTTPUrlConnection**: a partir de la anterior, contiene métodos específicos para cada protocolo.

### 2.2. La clase URL
Esta clase nos sirve para referenciar un Localizador Universal de Recurso. Este recurso puede ser desde una página web, un fichero, un documento o una petición a alguna API REST.

Veamos los métodos más relevantes de la clase URL:
| **Método** | **Descripción** |
| --- | --- |
| **URL(String url)**<br/>**URL(String protocolo, String host, int port, String recurso)** | Crea una URL, bien indicando todos los detalles en el primer constructor, bien separando los elementos en el segundo. |
| **public URLConnection openConnection()** | Devuelve un objeto URLConnection que representa una conexión abierta a la URL anterior. |
| **public InputStream openStream()** | Devuelve un InputStream para leer desde una conexión de esa URL. |

### 2.3. LA clase URLConnection
Mediante el objeto URL podemos establecer el recurso que queremos y algunos argumentos en la petición REQUEST. Dichas propiedades son estáticas independientemente de que esté conectado o no. Para especificar más detalles y controlar la recepción de los datos necesitaremos **URLConnection**, y más aún, otra clase heredera de esta última que es **HTTPUrlConnection**.
- El objeto **URLConnection** se creará a partir del objeto URL invocando al método **openConnection()**
- Podemos obtener los datos de la cabecera, bien uno a uno mediante métodos **getHeaderField(String nombreCampo)**.
- Obtener todos los campos de un **Map** con **getHeaderFiels()**
- Acceder a la respuesta obteniendo un stream de entrada mediante **getInputStream()**. Debemos tener mucho cuidado aquí con la manera de leer del stream, ya que el origen del tipo de datos dependerá del valor del atributo **content-type**.

### 2.4. JAX-RS Client
JAX-RS es una API Java de alto nivel que proporciona soporte para servicios REST. En este apartado nos vamos a centrar en la implementación de la parte cliente.

| **Client** |
| :--- |
| Es la clase base, que nos permite crear de manera rápida un cliente de una API para consumir la respuesta. Crearemos una instancia de esta clase mediante el método estático **ClientBuilder.newClient()** |

| **WebTarget** |
| :--- |
| Es la web objetivo de la consulta que vamos a realizar. Se indica mediante el método **target(String URI)** de la clase **Client**. |

| **Target.request().get()** |
| :--- |
| Enviamos la petición al servidor, obteniendo una respuesta de tipo **Response**. |

| **Response** |
| :--- |
| Es la respuesta obtenida desde el servidor.<br/><ul><li>Estado de la petición: **getStatus()** con el código http habitual.</li><li>Tipo de datos devueltos: **getMediaType**. Habitualmente serán JSON.</li></ul>**Response.readEntity(Class)**: nos permite recuperar los datos de la respuesta parseados a la clase indicada. |

| **Target.request().get(class)** |
| :--- |
| Nos permite acceder directamente a los datos devueltos del servidor, pero en este caso no tenemos el objeto Response. Se utiliza para comprobar en qué estado ha quedado la petición. |

## 3. Sockets: ¿UDP o TCP?
Como se indicó en el primer apartado, nuestras aplicaciones están situadas en la capa de aplicación, ofreciendo sus utilidades a los usuarios. Dichas aplicaciones utilizan por debajo toda la pila TCP/IP y las aplicaciones intercambiarán **mensajes**. Por debajo, en la capa de transporte, habrá que decidir qué protocolo hemos de utilizar, y aquí nos surge el gran debate, que intentaremos clarificar.

| **UDP** | **TCP** |
| --- | --- |
| La unidad de envío es el **datagrama**. | La unidad de envío es el **paquete**. |
| No orientado a la conexión.<br/>No confiable.<br/>Se envía y puede llegar o no. | Orientado a la conexión y fiable. Si un paquete no llega, se reenvía. Si llegan desordenados, se reordenan. |
| La comunicación es símplex: un solo sentido. | Comunicación full dúplex. |
| Permite difusiones multicast y broadcast. | Solo son unicast (punto a punto). |
| Muy rápido (ya que no controla casi nada). | Lento (control de errores y de congestión). |
| Usos:<ul><li>Protocolos de difusión: DHCP.</li><li>Streaming de vídeo/audio.</li><li>Servicios de respuesta rápida</li></ul> | Usos:<ul><li>Web, correo.</li><li>Cualquier servicio que implique seguridad.</li></ul> |

### 3.1. Transmisión mediante UDP
| **Clase** | **Descipción** |
| :--- | :--- |
| **DatagramSocket** | Es el equivalente al buzón o punto de entrega. Se encarga del envío y de la recepción de los datos. |
| **DatagramPacket** | Es el contenido que queremos enviar/recibir; es decir, el propio sobre con la carta. |

Dependiendo de si queremos enviar o recibir, deberemos proceder de un modo u otro.

| **Emisor** | **DescipcReceptorión** |
| :--- | :--- |
| Creamos un **DatagramSocket.** | Creamos un **DatagramSocket**, enlazado a un puerto de escucha (**bind**). Puede hacerse en el propio constructor. |
| Creamos un **DatagramPacket** con la siguiente información:<ul><li>Tamaño del datagrama.</li><li>IP – Puerto del receptor.</li><li>Datos.</li></ul> | Creamos un DatagramPacket con:<ul><li>Tamaño del datagrama.<li></ul> |
| **datagramSocket.send(datagramPacket)** | **datagramSocket.receive(datagramPacket)** |
| Cerramos el **datagramSocket**. | Cerramos el **datagramSocket**. |

Uno de los detalles más importantes en el que debes fijarte es que el contenido del **DatagramPacket** son **bytes**. Debes convertir todo lo que quieras mandar a un array de bytes. En el caso de los String es fácil, debido a que ya incorpora dichos métodos, pero apra mandar objetos deberás tenerlo en cuenta.

### 3.2. Transmisión mediante TCP
Su mayor uso se debe a la necesidad de conexión en la mayoría de las aplicaciones y, sobretodo, a las confirmacioens de que lo que enviamos ha llegado a su destino de manera adecuada.

El mecanismo que ofrece Java para abstraer todo el proceso de comunicaciones mediante TCP son las clases **Socket** y **ServerSocket**. Como puede intuirse uno pertenecerá al lado del cliente y otro al lado del servidor.

#### ServerSocket
- **ServerSocket(int puerto)**: crea un servidor escuchando en el puerto indicado. 
- **ServerSocket(int puerto, int tamCola)**: añade la opción de controlar el número de clientes en espera de ser aceptados.
- **ServerSocket(int puerto, int tamCola, IpAddress)**: añade la opción de seleccionar algunas de las IP del servidor.

Mediante el **ServerSocket** no podemos comunicarnos con nadie. Es un servidor escuchando en un puerto a la espera de que alguien se conecte. El método más importante es:
- **Socket ServerSocket.accept()**: este es un método que bloque al servidor, hasta que alguien intenta conectarse. Se activa cuando un cliente se conecta al servidor, devolviendo un socket. Este socket estará enlazado a un puerto asignado aleatoriamente en el servidor. Si queremos establecer un temporizador para que el servidor se desbloquee si no ha recibido una petición dentro de ese periodo de tiempo utilizaremos **setSoTimeout(int temporizador)**. Si vence dicho tiempo, saltará la excepción **java.net.SocketTimeoutException**.

#### Socket
El **socket** nos servirá para conectar un cliente a un servidor. Tiene muchos constructores, pero aquí indicaremos el más habitual:
- **Socket(String host, int puerto)**
- **Socket(InetAddress host, int puerto)**: Se conectan al servidor indicado por el host, bien mediante un objeto InetAddress creado previamente, bien mediante una cadena de texto indicando el nombre de host, o bien mediante una cadena de texto con la IP del servidor.

## 4. Sockets seguros. Criptografía
### 4.1. Aspectos de seguridad informática
Hoy en día, la seguridad es un aspecto vital en nuestras aplicaciones informáticas, siendo la criptografía uno de los aspectos en los que más se ha incidido en los últimos años. Criptografía significa ocultar la grafía, es decir, ocultar aquello que se quiere transmitir.

Centrándonos en la informática, veamos los elementos más destacados:

| **Elementos** | **Descripción** |
| :--- | :--- |
| **Integridad** | Saber que los datos no se han corrompido y/o que nadie los ha manipulado durante su trayecto por el canal de comunicación. Esto se consigue mediante funciones de resumen. |
| **Confidencialidad** | Asegurarnos de que los datos no pueden ser interpretados por ningún otro elemento que no sean el emisor o el receptor. Esto se consigue cifrando/encriptando el mensaje. Veremos que existen técnicas de cifrado con claves simétricas o asimétricas. |
| **Autenticación y no repudio** | Consiste en verificar que emisor/receptor son realmente quienes dicen que son, y que alguien pueda verificar que el mensaje recibido es de un emisor de manera única. Esto se consigue mediante certificados y firmas digitales. |

### 4.2. Funciones de resumen
Las técnicas de resumen (Message Digest o Función Hash) generan un código de la misma longitud a partir de una entrada, de manera que es imposible generar la entrada a partir de resumen. Sus usos más extendidos son:
| **Usos** | **Descripción** |
| :--- | :--- |
| **Comprobación de la integridad de los archivos** | Al descargar archivos de la red, estos pueden corromperse (a veces a propósito). Algunas páginas incorporan junto al archivo el resumen, de manera que al concluir la descarga podemos verificar si se ha corrompido |
| **Almacenaje de las contraseñas** | En ningún SGBD se guardan as contraseñas almacenadas en claro. Normalmente se calcula el resumen y se almacena Cuando hay que comprobar una contraseña, se calcula el resumen y se compara con el valor almacenado |

La clase Java que permite calcular esúmenes es **MessageDigest**, de la manera que vemos a continuación:
1. Primero necesitaremos crear una instancia de la clase, indicando el algoritmo, que habitualmente será **MD5**, **SHA-1**, **SHA-256** o **SHA-512**:
```java
MessageDigest md = MessageDigest.getInstance(“MD5”);
```
2. Después procederemos a calcular el resumen, de un conjunto de bytes. Aquí cambia el proceso, dependiendo de si es un bloque pequeño o más grande, en cuyo caso trabajaremos por bloques:
```java
// Opción con bloque pequeño
byte[] resumen = md.digest(textoClaro.getBytes());

// Opción con bloque grande
int bytesLeidos;
while(llenar_buffer_con_bytes_leidos) {
    md.update(buffer,0,bytesLeidos);
}
byte[] resumen = md.digest();
```
3. Finalmente, terminado el resumen con la función digest(), lo convertimos a Hexadecimal en un String. Podemos utilizar cualquier función propia o la función DatatypeConverter.printHexBinary(resumen), de la librería javax.xml.bind.

### 4.3. Cifrado simétrico vs asimétrico
El cifrado es la técnica que permite convertir un contenido en claro o legible en un contenido no entendible. En todas las técnicas de cifrado aparecen dos conceptos: algoritmo y clave.
- **La técnica**: desplazar letras.
- **La clave**: el número de posiciones que se aplica.

Existen dos tipos de algoritmos: los de **clave simétrica** y **asimétrica**. En la encriptación simétrica se  utiliza la misma clave para encriptar y desencriptar, mientras que en la asimétrica son claves diferentes, generadas a la vez.

Estas claves asimétricas se denominan también claves públicas y privadas, y el funcionamiento es el siguiente (de manera simplificada):
1. Un servidor S crea un par de claves, una pública y otra privada. La privada solo la tiene él, y la pública la pone a disposición de todos los clientes que quieran comunicarse con él.
2. Cuando un cliente quiere comunicarse con S, utiliza la clave pública de S para encriptar el contenido. ¿Qué nos garantiza esto? Que como el mensaje ha sido encriptado con la clave pública de S, solo quien esté en disposición de la clave privada de S puede descifrarlo, y ese no es otro que S. Se cumple el principio de integridad.

La firma digital es el proceso inverso en cuanto al uso de claves (también de manera simplificada):
1. El servidor S emite un texto firmado con su clave privada.
2. Todos pueden leerlo, ya que todos tienen la clave pública de S.
3. ¿Quién ha escrito ese mensaje? Es obvio que S, ya que es el único que dispone de su clave privada. Se
cumple el principio de No repudio.

#### Cipher
Para la realización del cifrado simétrico en Java disponemos de la clase **Cipher**, dentro del paquete **JCA (Java Cryptography Architecture)**. Algunos de los algoritmos son AES, DES o 3DES.

Para realizar un cifrado, hay unos pasos previos a seguir, y el mecanismo variará también si encriptamos unos datos pequeños o datos más grandes, en cuyo caso deberemos trabajar en bloques, del mismo modo que hicimos en el cálculo de resúmenes. Las claves de cifrado/descifrado las generaremos en nuestro programa, pero podemos cargarlas desde ficheros externos.

Primero crearemos una instancia de la clase Cipher:
```java
Cipher cipher = Cipher.getInstance("AES");
```
En principio indicamos solo el algoritmo, pero el constructor presenta sobrecarga, mostrando como posibilidades el modo de operación y el relleno.

Necesitaremos también una clave para realizar la encriptación. Hay muchas maneras de obtenerlas, mediante generadores (concretos o aleatorios) certificados. Aquí la generamos a partir de una contraseña:
```java
SecretKey clave = new SecretKeySpec(pass.getBytes(), "AES");
```
A continuaciión deberemos iniciar el cifrado, pudiendo ser de varios modos, los más comunes en modo encriptar/desencriptar.
```java
if (encript) {
    cipher.init(Cipher.ENCRYPT_MODE, clave);
} else {
    cipher.init(Cipher.DECRYPT_MODE, clave);
}
```
y por último realizamos la encriptación/desencriptación:
```java
out = cipher.doFinal(input);
```
#### Cifrado asimétrico
Para la realización del cifrado asimétrico necesitaremos un par de claves, la pública y la privada. El cifrado mediante estas claves es inverso, lo que se cifra con una sola puede descifrase con la otra. Para las claves podemos optar por dos posibilidades: generarlas nosotros en nuesto programa o generarlas mediante herramientas externas y cargarlas en un fichero.

Para crearlas usamos **OpenSSL**, software libre y de amplio uso:
```bash
openssl genrsa -out private_key.pem

openssl pkcs8 -topk8 -inform PEM -outform DER -in private_key.pem -out private_key.der -nocrypt

openssl resa -in private_key.pem -pbout -outform DER -out public_key.der
```
Estos dos ficheros **private_key.der** y **public_key.der** contiene las claves privada y pública que Java puede cargar. Veamos como podemos cargar dichas claves en un programa Java. Básicamente en cargar los ficheros generados anteriormente y crear la clave con al codificación **PKCS8** o **X509**.

- **Clave privada**
```java
File f = new File(filename);
fis = new FileInputStream(f);
DataInputStream dis = new DataInputStream(fis);
byte[] keyBytes = new byte[(int)f.length()];
dis.readFully(keyBytes);
dis.close();
PKCS8EncodedKeySpec spec = new PKCS8EncodedKeySpec(keyBytes);
KeyFactory kf = KeyFactory.getInstance(asimALG);
PrivateKey privateKey = kf.generatePrivate(spec);
```

- **Clave pública**
```java
File f = new File(filename);
FileInputStream fis = new FileInputStream(f);
DataInputStream dis = new DataInputStream(fis);
byte[] keyBytes = new byte[(int)f.length()];
dis.readFully(keyBytes);
dis.close();
X509EncodedKeySpec spec = new X509EncodedKeySpec(keyBytes);
KeyFactory kf = KeyFactory.getInstance(asimALG);
PublicKey res = kf.generatePublic(spec);
```

Si quiséramos generarlas mediante código, el mecanismo es muy sencillo:
```java
// Se crea un generador de claves, con el alg ritmo DSA
KeyPairGenerator keyPairGen = KeyPairGenerator.getIn tance(“DSA”);
// Iniciamos con 2048 bits
keyPairGen.initialize(2048);
// Las generamos
KeyPair pair = keyPairGen.generateKeyPair();
// Recogemos la clave privada
PrivateKey privKey = pair.getPrivate();
// Recogemos la clav pública
PublicKey publicKey = pair.getPublic();
```

### 4.4. Comunicación segura con SSL
El cifrado simétrico tiene la ventaja de que es muy rápido, pero la desventaja es que, si alguno de los dos participantes pierde su clave, quedan los dos expuestos. El asimétrico es más seguro, pero también más lento.

Al final, la solución que se toma está balanceada en cuanto a seguridad/rendimiento.

Java ofrece las clases **SSLSocket** y **SSLServerSocket** (y dos clases factoría para cada uno) para la creación de sockets seguros. Estas clases heredan de Socket y ServerSocket, respectivamente, por lo que todo lo que podíamos hacer con los originales (no seguros) lo podemos hacer con estos seguros. En nuestros programas no seguros solo tendremos que cambiar la creación de la conexión, y a partir de ahí, el código queda igual.

Se utilizará el cifrado asimétrico para establecer un inicio de sesión, también conocida como el **handshake** (choque de manos o presentación). Ahí se genera una clave de sesión que se utilizará de manera simétrica para el resto de la comunicación.

```java
// En el servidor
SSLServerSocketFactory sssf = (SSLServerSocketFactory) SSLServerSocketFactory.getDefault();
SSLServerSocket sss = (SSLServerSocket) = sssf.createServerSocket(puerto);

//En el cliente
SSLSocketFactory ssf = (SSLSocketFactory) SSLSocketFactory.getDefault();
SSLSocket ss = (SSLSocket) ssf.createSocket(Host, Puerto);
```

## 5. Enlaces Web

- [Documentación de las clases y métodos que forman parte del ecosistema de las comunicaciones de Java.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/package-summary.html)
- [Posibles tipos devueltos por un servidor.](https://javaee.github.io/javaee-spec/javadocs/javax/ws/rs/core/MediaType.html)
- [Documentación completa de la clase DatagramPacket.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/DatagramPacket.html)
- [Documentación completa de la clase DatagramSocket.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/DatagramSocket.html)
- [Tutorial de Oracle sobre el uso de Sockets.](https://docs.oracle.com/javase/tutorial/networking/sockets/index.html)
- [Documentación sobre la clase ServerSocket.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/ServerSocket.html)
- [Documentación completa sobre la clase Socket.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/Socket.html)
- [Documentación oficial de la clase Cipher, que nos sirve para el cifrado simétrico.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/javax/crypto/Cipher.html)
- [Tutorial sobre cómo utilizar claves generadas mediante el software libre openssl en tus programas Java.](https://www.javacodegeeks.com/2020/04/encrypt-with-openssl-decrypt-with-java-using-openssl-rsa-public-private-keys.html)
- [Kit de herramientas de funciones y generación de claves relacionadas con criptografía y las comunicaciones seguras.](https://www.openssl.org/)

## 6. Respuestas cuestionario

- [Cuestionario](./CUESTIONARIO.md)