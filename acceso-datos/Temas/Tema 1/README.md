# Tema 1 - Acceso a ficheros
## 1. Sistema de ficheros
En Java para utilizar ficheros se utiliza la clase File, cuya utilidad es que representa un enlace a un elemento genérico del sistema de archivos.

Los constructorores principales que posee son:
```java
File(File parent, String child) // Crea el acceso a un elemento (child) a partir de un objeto File existente (parent), que representa a un directorio.

File(String pathname) // Crea el acceso a un elemento mediante una ruta absoluta, o bien un elemento dentro de la ruta de ejecución.
```

## 2. Rutas
Dependiendo del sistema en el que nos encontremos, deberemos tratar las rutas de forma diferente. Tenemos que tener en cuenta el **separator**, referido al carácter que separa un elemento contenido dentro de otro cuando especificamos las rutas de las carpetas.

```java
File f = new File("/home/dgomezm/main.java") // Linux

File f = new File("C:\\Usuarios\\dgomezm\\Escritorio\\main.java") // Windows
```