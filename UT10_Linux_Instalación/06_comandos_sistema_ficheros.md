4.- COMANDOS PARA EL SISTEMA DE FICHEROS
4.1.- MANIPULACIÓN DE FICHEROS
4.1.1- CREACIÓN DE FICHEROS
Bash proporciona un montón de comandos para manipular fichero en el sistema de ficheros Linux. El más sencillo es touch. Simplemente crea un fichero vacío con el nombre indicado.
  
Si el fichero ya existe el comando touch simplemente cambia la hora de modificación del fichero indicado, sin alterar para nada su contenido. Si queremos que no ponga la fecha actual sino otra hora y fecha diferentes lo podemos hacer con el parámetro –t.
  
Observa la sintaxis para indicar la nueva hora. Si tienes dudas sobre como interpretarla puedes recurrir al manual de ayuda.
 
Para copiar ficheros de una localización a otra utilizaremos el comando cp.
  
Si tanto el origen como el destino son ficheros el comando cp copiará el fichero origen a un nuevo fichero con el nombre de fichero indicado como destino.
  
Al igual que todos los comandos, el comando cp dispone de una serie de parámetros para modificar su comportamiento.
•	El parámetro –p copia el fichero, pero mantiene la fecha de acceso y modificación del fichero original en el fichero copiado.
•	El parámetro –R permite copiar recursivamente los contenidos de un directorio completo.
•	Por defecto, si el fichero destino existe, nos preguntará si queremos sobrescribirlo antes de realizar la operación. Con el parámetro –f indicaremos que no nos pregunte, sobrescribiéndolo sin más.
Como siempre podemos ver un listado completo de los parámetros de cp en su página del manual.
4.1.2.- ENLACES
Hay ocasiones en las que es necesario tener dos o más copias de un mismo fichero en el sistema de ficheros. Una opción que permite el sistema de ficheros de Unix es, en lugar de tener varias copias físicas diferentes, tener una única copia física y múltiples copias virtuales, denominadas enlaces.
Un enlace es una entrada en un directorio que apunta a la localización real del fichero. En Linux hay dos tipos diferentes de enlaces a ficheros:
•	Enlaces simbólicos o blandos
•	Enlaces duros
Un enlace duro crea un fichero diferente que contiene información acerca del fichero original y dónde localizarlo. Cuando referencias un enlace duro a un fichero, es igual que si referenciaras al fichero.
  
El parámetro –l crea un enlace duro para el fichero test1 y lo denomina test4. En esta captura podemos apreciar dos cosas:
•	Primero que el número de inodo (primera columna) de ambos ficheros es el mismo indicando que en realidad ambos ficheros son el mismo
•	Y segundo, que el número de enlaces indicados para cada fichero (tercera columna) es dos.
Hay que tener en cuenta que, dado que ambos ficheros comparten el inodo, solo se pueden crear enlaces duros entre ficheros del mismo dispositivo.
Por otro lado, para crear un enlace simbólico utilizaremos el parámetro –s.
  
Lo primero que podemos apreciar en la captura es que ahora los ficheros tienen números de inodo diferentes, por lo que el sistema los trata como ficheros independientes. Por otro lado, el tamaño del fichero también es diferente. Un fichero enlazado solo necesita guardar información acerca del fichero origen, no los datos que tenga.
Otra forma de crear enlaces es utilizando el comando ln. Por defecto este comando crea enlaces duros, pero se puede utilizar el parámetro –s para crear enlaces simbólicos.
4.1.3.- RENOMBRANDO FICHEROS
En Linux, renombrar un fichero es lo mismo que moverlo. El comando mv es el que utilizaremos para mover tanto ficheros como directorios.
Fíjate que al mover un fichero cambiamos su nombre, pero mantenemos el mismo número de inodo y fecha y hora de modificación.
4.1.4.- BORRAR FICHEROS
El comando que nos permitirá borrar ficheros es rm. En su forma más sencilla solo requerirá el nombre del fichero a eliminar. Ten en cuenta que en el shell Bash no hay papelera, por lo que el borrado de ficheros es irreversible. Una buena práctica es acostumbrarse a utilizar el parámetro –i, sobre todo cuando utilizamos comodines, que nos preguntará antes de borrar cada fichero.
  
4.2.- MANIPULACIÓN DE DIRECTORIOS
En Linux hay algunos comandos que funcionan tanto para ficheros como para directorios (como el comando cp) y otros que solo trabajan con directorios.
4.2.1.- CREACIÓN DE DIRECTORIOS
El comando que nos permitirá crear directorios en Linux es mkdir.
  
4.2.2.- ELIMINACIÓN DE DIRECTORIOS
El comando básico para eliminar directorios en Linux es rmdir. Este comando solo funcionará con directorios vacíos, mostrándonos un mensaje de error en caso de que el directorio no esté vacío.
  
Si queremos que nos borre un directorio que no está vacío podemos utilizar el parámetro --ignore-fail-on-non-empty.
El comando rm también nos puede servir para eliminar directorios. Sin embargo, si intentamos utilizarlo sin parámetros nos mostrará un mensaje de error y no nos permitirá eliminarlo. Para ello debemos utilizar el parámetro –r, que nos permitirá eliminar recursivamente todos los ficheros del directorio.
  
4.3.- MOSTRANDO EL CONTENIDO DE LOS DIRECTORIOS
4.3.1.- ESTADÍSTICAS DEL FICHERO
Como vimos con el comando ls –l podemos obtener mucha información acerca de un fichero. Sin embargo, el sistema almacena mucha más información acerca de los ficheros. El comando proporcionado por Bash para acceder a toda esta información es el comando stat, que nos mostrará toda la información disponible acerca del estado de un fichero.
 
4.3.2.- VIENDO EL TIPO DE FICHERO
Un tipo de información no mostrado por el comando stat es el tipo de fichero de que se trata. Podemos ver de qué tipo es un fichero mediante el comando file.
El comando file clasifica los ficheros en tres categorías:
•	Ficheros de texto: ficheros que contienen caracteres imprimibles.
•	Ficheros ejecutables: ficheros que pueden ser ejecutados en el sistema
•	Ficheros de datos: ficheros binarios que contienen caracteres no imprimibles, pero que de alguna forma pueden ser utilizados por el sistema.
 
4.3.3.- VIENDO UN FICHERO COMPLETO
Para ver el contenido de un fichero Linux dispone de tres comandos: cat, more y less.
El comando cat muestra todos los datos dentro de un fichero de texto.
 
 Sin ningún modificador es un comando muy sencillo. Sin embargo, nuevamente nos encontramos con un gran número de modificadores para alterar su comportamiento.
•	El modificador –n muestra el número de cada línea, útil por ejemplo para ver ficheros de código fuente.
 
•	Si solo queremos que nos numere las líneas que tengan texto entonces deberemos utilizar el parámetro –b.
 
•	Si el fichero tuviera muchas líneas en blanco y queremos que nos las comprima en una sola entonces el parámetro será –s.
•	Si queremos que no se muestren los caracteres de tabulación el parámetro será –T. Fíjate que cada carácter de tabulación que encuentre lo reemplazará por la combinación de caracteres ^I.
 
El comando cat tiene un problema cuando queremos visualizar ficheros con un gran número de líneas de texto ya que los mostrará sin realizar ninguna pausa. Si queremos que el contenido se nos muestre página a página el comando que utilizaremos será more.
El comando more visualizará una página y esperará antes de seguir mostrándonos un prompt. El prompt de more admite una serie de órdenes que se muestran en la siguiente tabla.
 
