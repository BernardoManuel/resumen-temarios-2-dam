# Tema 1 - Acceso a ficheros
## 1. Sistema de ficheros
### 1.1. Acceso al sistema de ficheros
En Java para utilizar ficheros se utiliza la clase File, cuya utilidad es que representa un enlace a un elemento genérico del sistema de archivos.

Los constructorores principales que posee son:
```java
File(File parent, String child) // Crea el acceso a un elemento (child) a partir de un objeto File existente (parent), que representa a un directorio.

File(String pathname) // Crea el acceso a un elemento mediante una ruta absoluta, o bien un elemento dentro de la ruta de ejecución.
```

### 1.2. Rutas
Dependiendo del sistema en el que nos encontremos, deberemos tratar las rutas de forma diferente. Tenemos que tener en cuenta el **separator**, referido al carácter que separa un elemento contenido dentro de otro cuando especificamos las rutas de las carpetas.

```java
File f = new File("/home/dgomezm/main.java") // Linux

File f = new File("C:\\Usuarios\\dgomezm\\Escritorio\\main.java") // Windows
```

Una vez ya se ha obtenido la referencia al elemento del sistema de ficheros, se podrían hacer una serie de comprobaciones utilizando los métodos de la clase File:

```java
boolean exists() // Devuelve true si el objeto al que apuntamos existe.

boolean isFile()
boolean isDirectory() // Devuelven true en el caso que la referencia sea un fichero o un directorio, respectivamente.

boolean createNewFile() // Crea un fichero nuevo, solo en caso de que no exista.

boolean mkdir()
boolean mkdirs() // Crea un directorio nuevo, solo en caso de que no exista. Mediante mkdirs() crearemos también los directorios padre del actual, en caso de que no existan.

boolean renameTo(File) // Permite renombrar elemento actual a otro.

boolean delete() // Elimina el elemento.
```

### 1.3. Propiedades de los elementos

Para consultar ciertas propiedades de los focheros, se utilizan los siguientes métodos de la clase **File**:

```java
boolean canRead()
boolean canWrite()
boolean canExecute() // Informa de los permisos de usuario para cada fichero.

long length() // Devuelve el tamaño en bytes del fichero. Para los directorios, esta información no es representativa.

String[] list() // Devuelve un vector de String con los elementos que contiene el directorio actual.
```