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

| --- | --- |
| --- | --- |
| URL(String url) <br/> URL(String protocolo, String host, int port, String recurso) | xd |

```java
URL(String url)
URL(String protocolo, String host, int port, String recurso)
```
