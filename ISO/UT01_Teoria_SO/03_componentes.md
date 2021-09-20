# 3.- COMPONENTES DE LOS SISTEMAS OPERATIVOS

Por norma general, se suele considerar que un sistema operativo está formado por tres capas: el núcleo o kernel, los servicios del sistema operativo y el intérprete de mandatos o Shell.


## 3.1.- El núcleo o kernel

En el nivel más bajo se encuentra el **núcleo** o **kernel**, que es la parte del sistema operativo encargada de *interactuar con el hardware*. Por tanto, servirá de intermediario entre el resto de los componentes del sistema operativo y el hardware. Ningún otro componente necesita conocer las particularidades del hardware del ordenador, simplemente le dice al kernel qué es lo que quiere y es el este quien se encargará de comunicarlo al hardware.
Sus funciones se centran en la gestión de recursos como repartir el procesador entre los procesos, gestionar las interrupciones del sistema o realizar las funciones básicas de manipulación de memoria.

## 3.2.- Los servicios

El sistema operativo proporciona al usuario diversos **servicios**. Algunos de los más importantes son:

- **Gestión de procesos**: más adelante veremos en detalle los procesos, pero podemos avanzar que un proceso es un programa en ejecución, es decir, cada vez que ejecutamos un programa se crea el proceso correspondiente. La creación de un proceso no es trivial, ya que requiere asignarle tiempo de procesador, un espacio en memoria, acceso a las unidades de almacenamiento y dispositivos, … El sistema operativo proporciona el servicio de creación, suspensión, reanudación y eliminación de procesos al usuario.
- **Gestión de memoria**: un recurso clave en el sistema es la memoria ya que todos los procesos en ejecución del sistema deben estar en memoria. El problema es que hay diversas tecnologías, pero ninguna óptima: las memorias más rápidas tienen precios muy elevados, mientras que las memorias más baratas que permiten grandes capacidades son excesivamente lentas. La solución a esta situación son las **jerarquías de memoria**, que combinan diferentes tipos de memoria buscando un equilibrio entre capacidad, velocidad y coste.
- **Gestión de ficheros y directorios**: dado que la memoria principal es volátil y su tamaño limitado, los ordenadores disponen de dispositivos de almacenamiento secundario como discos duros, tarjetas de memoria, … Para almacenar la información en estos dispositivos el sistema operativo dispone de un sistema de ficheros que ayuda al usuario a organizar la información en archivos y carpetas.
- **Seguridad y protección**: este componente se encarga de garantizar la identidad de los usuarios y de definir lo que pueden hacer cada uno de ellos con los recursos del sistema.


## 3.3.- Intérprete de comandos o Shell

Es el componente que se encuentra al más alto nivel, en contacto directo con el usuario y por tanto, el que será responsable de interpretar las órdenes de este.

El intérprete de comandos puede ser textual, en el que el usuario introduce los comandos en modo texto siguiendo una sintaxis determinada, o gráfico, en el que el usuario interacciona a través de una interfaz gráfica mediante un ratón o dispositivo similar.



