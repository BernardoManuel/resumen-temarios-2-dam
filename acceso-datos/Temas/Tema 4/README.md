# Tema 4 - Base de datos orientadas a objetos
## 1. Introducción. El modelo de datos ODMG
### 1.1. Evolución de los SGBD
Ls **BDR** surgen a partir del modelo relacional, como un sistema que representa fielmente la realidad, con una gran solidez por la lógica relacional subyacente. Dicho modelo representa la perspectiva estática del modelado de la aplicación, y en él se desglosa todos los datos hasta niveles atómicos, ya que, como recordamos, no se permiten valores multivaluados ni compuestos.

Dicho modelo ha sufiro el **desfase objeto-relacional**, en el cual los lenguajes de programación realizaron una evolución de las estructuras, adoptando la metodología de la orientación a objetos. Esto provoca que diseñemos por un lado la base de datos, siguiendo el modelo relacional, y que también necesitemos, por otra parte, el diseño de clases de la aplicación. Dichos diseños, que no suelen coincidir, podemos "encajarlos" con las herramientas **ORM** para mitigar el desfase objeto-relacional.

De este desfase surge la necesidad de añadir un diseño más orientado a objetos dentro de la propia base de datos. Si analizamos un esquema orientado a objetos y tratamos de aplicarle la teoría de la normalización, nos encontraremos que los objetos se nos "descomponen" en varias entidades, lo que provoca que aparezcan muchas tablas.

Esta cantidad elevada de tablas conduca a un aumento de las referencias entre estas, lo que incrementa considerablemente las relaciones entre ellas y, por tanto, un mayor número de restricciones que controlar (**FOREIGN KEY**), lo cual supone un acrecentamiento en el número de comprobaciones que debe realizar el SGBD.

Así pues, las BDOO permiten una definición de tipos complejos de datos, frente a los simples que incorpora el SGBDR, lo que posibilita la definición de tipos estructurados e incluso multivaluados. Con todo esto, las BDOO deberían simplemente permitir el diseño mediante objetos, indicando los objetos que participan, sus atributos y métodos, y las relaciones que afectan a estos, ya sean participaciones o herencias.

Estas BDOO no terminan de despegar, y algunas alternativas que implementan las soluciones comerciales consisten den dotar al sistema de bases de datos relacionales de las capacidades semánticas de la orientación a objetos, con lo que aparecen las **BDOR (bases de datos objeto-relacionales)**.

### 1.2. BDOO
Página 54