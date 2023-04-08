# Tema 1 - Introducción a los sistemas ERP-CRM
## 1. Introducción a la gestión empresarial
Características de la administración:
- Universalidad
- Especificadad
- Unidad temporal
- Unidad jerárquica
- Valor instrumental
- Interdisciplinariedad
- Flexibilidad

#### Organización de una empresa y sus relaciones
El entorno de una empresa son aquellos elementos y factores relevantes externos a la misma y que pueden influir en su funcionamiento. En cada entorno podemos distinguir dos tipos de factores: los que afectan de forma global a todas las empresas y los que afectan de forma individual.

## 2. Conceptosx de ERP y CRM
### 2.1. Concepto de sistema de gestión empresarial
Un sistema de gestión empresarial es un sistema informática formado por aplicaciones que se encargan de las operaciones internas más importantes de una empresa.

Los principales tipos de aplicacioens para la gestión empresarial son los siguientes:
- ERP (Enterprise Resource Planning)
- CRM (Customer Relationship Management)
- SRM (Supplier Relationship Management)
- BI (Business Intelligence)

### 2.2. Concepto de ERP
El ERP permite a las empresas controlar todos los flujos de información que se generan en cada ámbito de la organización y, en consecuencia, integrar todos su departamentos.

La evolución de los ERP ha proporcionado a las empresas diversos beneficios:
- Unificación de los procesos.
- Organización den mesas estáticas y dinámicas.
- Relacionar la información de manera eficiente en el proceso de análisis y decisión.
- Optimización de los procesos empresariales.
- Acceso a información fiable, precisa y oportuna.
- Nos permite compartir información con todo el personal y los diversos departamentos de la organización.
- Eliminación de datos y operaciones innecesarios.
- Reducción del tiempo y de los costes de los procesos.

### 2.3. Concepto de CRM
Un CRM es una aplicación que permite centralizar en una única base de datos todas las interacciones entre la empresa y sus clientes.

Como resumen diremos que un CRM sirve principalmente para tres cosas:
1. A nivel interno de la empresa, **mejora la comunicación interna**. Las tareas relacionadas con los clientes se asignan a los empleados y se hace el seguimiento de su marcha y evolución.
2. Se **mejora la productividad en el trabajo**, ya que se evitan gran cantidad de correos electrónicos, reuniones y llamadas telefónicas solicitando o enviando información sobre los clientes a otros compañeros de trabajo.
3. **Los comerciales pueden vender más** gracias a que pueden hacer un seguimiento de todas sus ventas de manera personalizada y también gestionar más de una venta a la vez.

### 2.4. Características, ventajas y desventajas

#### Caracterísitcas de los ERP
- **Son sistemas integrales**: Se pueden controlar todos lso procesos de uan empresa, ya que cada departamento de la empresa se relaciona con el resto; es decir, muchos procesos empiezan partiendo de otro que ya ha terminado.
-  **Son sistemas modulates**: sabemos que la empresa está formada por departamentos y estos se relacionan entre sí, lo que implica que deben compartir información entre ellos. Para facilitar este aspecto, los ERP se dividen en módulos y solo instalaremos aquellos que se adecúan a nuestra empresa, permitiendo así la división de funciones.
- **Son sistemas adaptables**: los ERP permiten la adaptación a diferentes tipos de empresas. Para ello utilizaremos la parametrización de los procesos dependiendo de las necesidades de salida de la información.

#### Ventajas y desventajas de los ERP.

- **Ventajas**
    - Estandariación e integración de la información
    - Mayor control organizacional.
    - Minimiza el tiempo de análisis.
    - Disminuye los costes.
    - Información actualizada.
    - Optimización de tiempos.
    - Evita información duplicada.
    - Módulos configurables.
    - Mejora el servicio al cliente.

- **Desventajas**
    - Costes a primera vista.
    - Mucho tiempo de implementación.
    - Adquisición y adaptación del hardware.
    - Pocos expertos en este tipo de sistemas.
    - Dificultad en la utilización del sistema.
    - Costes de licencias.

## 3. Arquitectura de un sistema ERP
Según la integración que incorpora un ERP, encontramos:
- **Arquitectura modular**: Dentro de las ventajas propias de integración se encuentra su gran poder de ajuste y adaptación. Sus bloque funcionales están estrechamente interconectados y se pueden contratar de forma independiente.
- **Arquitectura indivisible**: Este tipo de diseño arquitectónico permite contar con todas las funcionalidades desde el inicio.

Desde una perspectiva técnica, los sistemas ERP se construyen basándose en dos elementos principales:
- **Arquitectura cliente-servidor**: Se necesita un servidor que ofrece servicios a los clientes. Los usuarios utilizan las aplicaciones para acceder a los servicios de dicho servidor.
- **Base de datos**: La base de datos es común a todos los módulos de la aplicación, esto permite la integración entre los diferentes departamentos o módulos del ERP. Normalmente las bases de datos que se utilizan son relacionales. A la base de datos solo se puede acceder desde el servidor ERP. nunca desde los clientes. El servidor actúa como una capa intermedia entre clientes y los datos.

### 3.1. Descripción de la arquitectura de un sistema ERP libre: Odoo
La arquitectura del sistema Odoo es cliente-servidor, esto nos permite trabajar sobre los mismos datos desde diferentes clientes. El servidor y los clientes pueden realizar el intercambio de datos mediante tres tipos de procedimientos: XML-RPC, Net-RPC o JSON.

El framework de Odoo facilita varios componentes que permiten construir la aplicación siguiendo la arquitectura MVC.
- La capa ORM hace el mapeo entre los objetos Python y la base de datos PostgreSQL. El diseñador-programador no efectúa el diseño de la base de datos, únicamente diseña clases, las cuales son utilizadas por la capa ORM de Odoo para efectuar el mapeado sobre el SGBD PostgreSQL.
- Un sistema de flujos de trabajo o workflows.
- Diseñadores de informes.
- Facilidades de traducción de la aplicación a varios idiomas.

### 3.2. Funcionamiento
El servidor Odoo proporciona un acceso a la base de datos en ORM y un cliente web. El cliente se comunica con el servidor en XML-RPC y los clientes web medianmte JSON-RPC. El cliente solo tiene que mostrar lo que le envía el servidor.

El acceso del cliente a los datos se hace mediante un servicio llamado WSGI, es una solución estándar para hacer servidores y clientes HTTP en Python.

Por otra parte, tenemos los Business Objects. Casi todo en Odoo es un Business Object. Son persistentes gracias a ORM y los podemos encontrar estructurados en el directorio /modules.

### 3.3. Resumiendo Odoo
- Tecnología que usa:
    - Python
    - SQL y PL/SQL
    - XML
- Arquitectura de diseño y funcionamiento:
    - MVC
    - PostgreSQL database server
    - Application server
    - Open Object client web
- Licencia:
    - GPL
- Tipos de cliente:
    - Web
    - Aplicación Desktop

## 4. Enlaces Web

- [Página oficial de Odoo Community.](https://www.odoo.com/es_ES/page/community)
- [SuiteCRM](https://www.pipedrive.com/es-es)
- [Wiki oficial de Openbravo](http://wiki.openbravo.com/wiki/Welcome_to_Openbravo)
- [Página oficial de SugarCRM](https://www.sugarcrm.com/es/)
- [Wikipedia Odoo](https://es.wikipedia.org/wiki/Odoo)
- [Wikipedia Licencia GPL](https://es.wikipedia.org/wiki/GNU_Lesser_General_Public_License)

## 5. Respuestas cuestionario

- [Cuestionario](./CUESTIONARIO.md)