5.- COMANDOS AVANZADOS DEL SHELL BASH
5.1.- MONITORIZANDO EL ESPACIO EN DISCO
Una parte muy importante de la administración de un sistema Linux es llevar el control de la ocupación del sistema de ficheros.
5.1.1.- MONTAJE DE DISPOSITIVOS
Como se vio anteriormente Linux combina todos los dispositivos de almacenamiento en un único directorio virtual. Antes de utilizar un nuevo disco en el sistema necesitas localizarlo en el directorio virtual. Esta tarea se denomina montaje.
La mayoría de las actuales distribuciones Linux montan automáticamente algunas unidades como las unidades de CD o memorias USB.
El comando utilizado para montar dispositivos es el comando mount. Por defecto mount muestra una lista de los dispositivos actualmente montados en el sistema.
 
La información que proporciona el comando mount es:
•	La localización del dispositivo
•	El punto de montaje en el directorio virtual 
•	El tipo de sistema de ficheros
•	El estado de acceso del dispositivo montado
Por ejemplo, la sexta entrada hace referencia a una partición del disco duro (sda1). El fichero que representa al dispositivo es /dev/sda1 y está montado en el directorio raíz (/). El tipo del sistema de ficheros es ext4. Al final de la línea se muestra otra información del dispositivo como por ejemplo que es de lectura y escritura (rw, read and write).
 
Para montar un dispositivo manualmente primero debes tener privilegios de administrador. La sintaxis del comando mount es:
mount –t tipo dispositivo directorio
Los parámetros son:
•	El parámetro tipo indica el sistema de ficheros con el que está formateado el dispositivo. Linux reconoce un gran número de sistemas de ficheros, algunos ejemplos son ext3 o ext4 nativos de Linux, vfat que es el sistema utilizado por Windows en versiones antiguas o en memorias USB, ntfs, el sistema de ficheros de Windows 2000 y posteriores o iso9660, utilizado en los CDs.
•	Los siguientes dos parámetros definen la localización del dispositivo (los dispositivos en Linux se asocian con un archivo en el directorio /dev) y el directorio donde queremos montarlo. El usuario root debe tener acceso a este directorio, pero se puede limitar el acceso a otros usuarios mediante permisos.
Los modificadores del comando mount son las siguientes:
 
 La última opción (-o) permite indicar una serie de opciones adicionales separadas por comas, estas pueden ser:
•	ro: montar como dispositivo de solo lectura
•	rw: montar como dispositivo de lectura-escritura
•	user: permite a un usuario montar el sistema de ficheros
•	check=none: omite la comprobación de integridad del sistema de ficheros.
•	loop: permite montar un fichero en lugar de un dispositivo.
La última opción nos permite montar un fichero que sea una imagen de un dispositivo, por ejemplo, un fichero ISO, tal y como se puede ver en la siguiente imagen.
 
Una vez que hemos acabado de utilizar el dispositivo podemos desmontarlo con la orden umount. Este comando admite como parámetro o bien la ruta del dispositivo o bien el directorio donde está montado.
5.1.2.- EL COMANDO DF
Si queremos saber cuánto espacio disponible tenemos en un dispositivo deberemos utilizar el comando df. Si no ponemos parámetros nos mostrará información general sobre todos los dispositivos montados en el sistema.
  
La información que nos muestra este comando es:
•	La localización del dispositivo.
•	Cuantos bloques de 1024 bytes de datos puede contener
•	Cuantos bloques de 1024 bytes son utilizados
•	Cuantos bloques de 1024 bytes están disponibles
•	La cantidad de espacio utilizado como un porcentaje
•	El punto de montaje del dispositivo
Un parámetro de este comando es –h que nos muestra el espacio en disco de una forma más legible mostrándolo en gigabytes o megabytes.
5.1.3.- EL COMANDO DU
El comando du muestra la utilización del disco de un directorio específico (por defecto el directorio actual) desglosando lo que ocupa cada uno de sus ficheros y directorios.
 
 El número de la izquierda muestra el número de bloques de disco que ocupa cada directorio o fichero.
Algunos parámetros de este comando que pueden hacer su salida más legible son:
•	-c: muestra la ocupación total de todos los ficheros
•	-h: muestra el tamaño de una forma legible para los humanos, utilizando k para kilobyte, M para megabyte y G para gigabyte
•	-s: solo muestra el tamaño total del directorio.


5.2.- TRABAJANDO CON FICHEROS DE DATOS
5.2.1.- ORDENANDO DATOS
El comando sort ordena las líneas de un fichero de texto utilizando las reglas de ordenación según el lenguaje del sistema.
 
 Sin embargo, si los valores que contiene el fichero son numéricos el resultado puede no ser el esperado:
  
Si sabemos que el primer valor de cada fila es un número y queremos que lo trate como un número al ordenarlo en lugar de como un carácter debemos utilizar el parámetro –n
  
Muchos ficheros de log en Linux comienzan cada línea por el mes en que se ha producido la entrada. Si queremos que el comando sort nos reconozca estos caracteres como un mes deberemos utilizar el parámetro –M
 
 

Hay otros muchos parámetros que se pueden ver en la página del manual.
Los parámetros –k y –t son muy útiles para ordenar ficheros de datos que usan campos, tales como el fichero /etc/passwd. El parámetro –t servirá para especificar el carácter que separará los campos y el parámetro –k especifica el campo que se utilizará para ordenarlo.
Por ejemplo, el siguiente comando ordenará el fichero según el identificador del usuario, que por cierto es un valor numérico.
  
5.2.2.- BÚSQUEDA DE DATOS
Si queremos buscar una línea que contenga un determinado valor en un fichero deberemos utilizar la orden grep. El formato para el comando grep es el siguiente:
grep [opciones] patrón [fichero]
El comando grep busca en el fichero indicado como entrada aquellas líneas que contengan los caracteres que coincidan con el patrón indicado.
  
Este comando admite multitud de parámetros.
•	El parámetro –v nos permite invertir la búsqueda, es decir, mostrar aquellas líneas que no incluyan el patrón indicado.
•	Con el parámetro –n mostraremos el número antes de cada línea. 
•	El parámetro –c contará el número de líneas que contienen el patrón.
•	Se puede especificar más de un patrón mediante el parámetro –e. Por ejemplo, la orden grep –e t –e f fichero1 busca todas las líneas que contengan el carácter t o el carácter f.
También admite expresiones regulares al indicar el patrón a buscar. Las expresiones regulares ya se verán más adelante, pero a modo de avance un ejemplo puede ser el siguiente:
 
5.2.3.- COMPRESIÓN DE DATOS
De forma análoga a WinZip en Windows, Linux dispone de un gran número de herramientas de compresión de datos. Algunas de las más utilizadas son las siguientes:
•	bzip2: es un paquete compuesto por las cuatro herramientas indicadas a continuación:
o	bzip2 para comprimir ficheros
o	bzcat que muestra el contenido de ficheros de texto comprimidos.
o	bunzip2 para descomprimir ficheros
o	bzip2recover cuya utilidad es intentar recuperar datos de ficheros comprimidos dañados.
Por defecto el comando bzip2 reemplaza el fichero indicado por el fichero comprimido, el cual se identifica por la extensión .bz2.
•	gzip: probablemente la herramienta de compresión más popular. Como en el caso anterior incluye varias aplicaciones:
o	gzip para comprimir ficheros
o	gzcat para mostrar el contenido de los ficheros comprimidos
o	gunzip para descomprimir ficheros.
•	zip: es compatible con la versión de MS-DOS y Windows e incluye cinco utilidades:
o	zip crea un fichero comprimido
o	zipcloak crea un fichero comprimido y cifrado
o	zipnote extrae los comentarios de un fichero zip
o	zipsplit divide un fichero zip en fichero más pequeños del tamaño indicado
o	unzip extrae los ficheros y directorios de un fichero comprimido.
5.2.4.- ARCHIVADO DE DATOS
Aunque el comando zip sirve para comprimir varios ficheros y directorios en un solo fichero, es más común utilizar el comando tar para archivar varios ficheros en uno solo.
El formato del comando tar es el siguiente:
tar función [opciones] objeto1 objeto2 …
El parámetro función define el modo de funcionamiento del comando tar tal y como se muestra en la siguiente tabla:
 
Cada una de las funciones se puede combinar con una serie de parámetros de los que se indican en la otra tabla. Por ejemplo:
tar –cvf test.tar test/ test2/
Crea un fichero llamado test.tar que contiene los contenidos de los directorios indicados:
tar –tf test.tar
Muestra, pero no extrae el contenido del fichero test.tar
tar –xvf test.tar
Extrae el contenido del fichero test.tar.
 

5.3.- REDIRECCIONAMIENTO
En UNIX, y por tanto en Linux, todo es un flujo (stream) de bytes. Estos flujos están normalmente representados como ficheros, pero hay tres flujos especiales que raramente son accedidos a través de un nombre de fichero. Estos son los flujos de entrada y salida asociados a cada comando: la entrada estándar, la salida estándar y la salida de error. Por defecto, estos flujos o streams están conectados a la terminal.
Cuando un comando lee un carácter o una línea, lo hace desde la entrada estándar, que es el teclado. Cuando tiene una salida, la imprime en la salida estándar, que es el monitor. La salida de error también está conectada al monitor.
Estos flujos son referenciados mediante números, llamados descriptores de fichero (FD), y son 0, 1 y 2 respectivamente. También se conocen como stdin, stdout y stderr.
5.3.1.- REDIRECCIÓN >, >>,  < Y <<
El carácter > sirve para redireccionar la salida estándar a un fichero siguiendo la siguiente sintaxis:
comando > fichero
Si el nombre de fichero indicado no existe lo crea, siendo su contenido la salida del comando que se ha ejecutado. Si el fichero ya existe borra su contenido reemplazándolo por la salida del comando.
Si no queremos que borre el contenido de un fichero existente entonces debemos utilizar el operador >>.
Redirigir la salida estándar no redirige la salida de error. Si queremos redirigir la salida de error entonces el operador a utilizar es 2>.
 
En este caso, los mensajes de error son enviados a un fichero especial, /dev/null, cuyo cometido únicamente es descartar todo lo que se copie en él.
 
En lugar de enviar una salida a un fichero, puede ser redireccionada a otro flujo de entrada/salida usando >&N donde N es el número del descriptor de fichero. Este comando envía tanto la salida estándar como la de error al fichero FILE.
 
Aquí el orden es importante. La salida estándar es enviada a FILE, y la salida de error es redireccionada a donde vaya la salida estándar. Si se invierte el orden, el efecto es diferente. La redirección envía la salida de error a donde esté yendo actualmente la salida estándar y a continuación cambia la salida estándar.
 
El Shell Bash también tiene una sintaxis no estándar para redirigir tanto la salida estándar como la de error al mismo sitio usando los operadores &> y &>>.
El operador < sirve para reemplazar la entrada estándar de un comando por el contenido del fichero que se indique, de la forma:
comando < fichero
El último operador de redireccionamiento es <<, cuya sintaxis es:
comando << etiqueta
Lo que hace es tomar la entrada para el comando de las siguientes líneas que se escriban hasta llegar a una línea que solo contenga la etiqueta.
 
5.3.2.- TUBERÍAS
Las tuberías (pipelines) conectan la salida estándar de un comando con la entrada estándar de otro. El símbolo barra (|) debe ser utilizado entre los dos comandos.
 




 
