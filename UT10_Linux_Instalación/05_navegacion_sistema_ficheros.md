3.- NAVEGACIÓN POR EL SISTEMA DE FICHEROS
3.1.- EL SISTEMA DE FICHEROS DE LINUX
La gestión del sistema de fichero en Linux difiere en algunos puntos a como lo hace Windows. La primera diferencia es que Linux no utiliza letras de unidades (por ejemplo, C:) en las rutas, sino que Linux almacena los ficheros en una única estructura de directorios, llamado directorio virtual. El directorio virtual contiene todos los ficheros de todas las unidades de almacenamiento instaladas en el PC unidos en una única estructura de directorios.
Todos los directorios de Linux cuelgan del directorio raíz, referenciado por el símbolo barra (/). Por ejemplo, una ruta en Linux será de la forma /home/vgonzalez/docs/test.txt.
Para incorporar otras unidades Linux crea directorios especiales denominados puntos de montaje, que son directorios en el directorio virtual a través de los que se puede acceder a otras unidades. Por ejemplo, Ubuntu monta algunas unidades en el directorio /media
 Sin embargo, las unidades se pueden montar en cualquier directorio. Por ejemplo, en un sistema con dos discos duros se puede instalar el sistema en el primer disco y montar el segundo disco en el directorio /home, de forma que todos los directorios de los usuarios estén en este segundo disco.

 Aunque puede variar según la distribución en la siguiente tabla se pueden ver los principales directorios de Linux y que se almacena en ellos.
 
3.2.- MOVIÉNDOSE POR LOS DIRECTORIOS
El comando cd nos sirve para desplazarnos por los diferentes directorios del sistema de ficheros.
El comando cd toma un único parámetro que será el directorio destino al que nos queramos mover. Si no se indica ningún parámetro nos llevará al directorio home del usuario.
El directorio de destino se puede especificar de dos maneras diferentes:
•	Ruta absoluta: define exactamente dónde está el directorio destino partiendo del directorio raíz. Por ejemplo /home/vgonzalez/Escritorio/apuntesClase
•	Ruta relativa: aquí definimos como se accede al directorio partiendo del directorio actual. Una ruta relativa no comienza con el símbolo barra (/). Por ejemplo, si estamos en el directorio home y queremos referenciar el directorio anterior la ruta sería Escritorio/apuntesClase. Para ascender en la jerarquía de directorios podemos utilizar dos caracteres especiales:
o	El punto (.) que representa el directorio actual
o	El doble punto (..) que representa el directorio padre del actual.
Si queremos saber cuál es el directorio de trabajo actual el comando a utilizar es el pwd.
3.3.- MOSTRANDO EL CONTENIDO DE LOS DIRECTORIOS
El comando ls es la herramienta que nos ayudará a ver qué ficheros y directorios tenemos en nuestro sistema de archivos.
En su forma más básica nos mostrará los ficheros y directorios situados en el directorio actual.
 
 Si utilizamos un terminal en color se nos mostrará diferentes colores según sean el tipo de objetos de que se trate (directorios, ficheros de datos, ejecutables, …)
Si no tenemos un terminal en color podemos utilizar el parámetro –F para que nos muestre información textual del tipo de ficheros de que se trata.
 
 Pero ls no muestra necesariamente todos los ficheros. Linux a menudo utiliza ficheros ocultos para almacenar información de configuración. Linux no identifica los ficheros ocultos con un atributo tal como hace Windows, sino que lo hace anteponiendo un punto antes del nombre del fichero.
Para mostrar también los ficheros ocultos deberemos utilizar el parámetro –a.
 
 Otro parámetro interesante de ls es el parámetro –R, que nos muestra los ficheros del directorio actual y de todos sus subdirectorios.
 
 Si queremos ver información más completa acerca de cada fichero el parámetro a utilizar será –l
 
La información que nos muestra este parámetro es la siguiente:
•	El tipo de fichero (directorio (d), fichero (-), dispositivo de caracteres (c) o dispositivo de bloques (b))
•	Los permisos del fichero
•	El número de enlaces duros al fichero
•	El nombre del usuario propietario del fichero
•	El nombre del grupo propietario del fichero
•	El tamaño del fichero en bytes
•	La fecha y hora de la última modificación del fichero
•	El nombre del fichero o directorio
Aparte de estos parámetros ls tiene otros muchos parámetros que determinarán la forma en que se nos muestre la información. Estos parámetros, al igual que con todos los comandos Linux, pueden ser de dos tipos:
•	Parámetros de una única letra
•	Parámetros de palabra completa
Los parámetros de una única letra (por ejemplo –R) van precedidos de un guion y se pueden combinar varios, por ejemplo, ls –aRl.
Los parámetros de palabra son mucho más descriptivos, van precedidos de dos guiones y no pueden combinarse, sino que tienen que poner por separado, por ejemplo, ls --all --file-type. 
Muchos modificadores disponen de ambas formas. Se pueden consultar los parámetros de ls en su página del manual.
Si queremos filtrar la salida para que ls no nos muestre todos los ficheros se puede indicar el fichero que queremos que nos muestre.
 
 También se pueden utilizar caracteres comodín para mostrar los ficheros que se adapten al patrón indicado. Los caracteres comodín son:
•	Un símbolo de interrogación (?) representa un carácter.
•	Un símbolo de asterisco (*) representa cero o más caracteres.
•	Los corchetes ([]) nos sirven para representar un conjunto de caracteres, por ejemplo, ls [a-c]* representa todos los ficheros que comiencen por a, b ó c
•	El símbolo exclamación (!) se usa junto con los corchetes para indicar que el conjunto se excluye, por ejemplo ls [!a-c]* indica todos los ficheros que no comiencen por a, b ó c.

