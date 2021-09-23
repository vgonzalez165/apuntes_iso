![Carátula UT04](imgs/caratula_ut04.png)

# UT04. WINDOWS 10. GESTIÓN DEL ALMACENAMIENTO

### Contenidos

1. Organización del almacenamiento
2. Configuración de discos en Windows
3. El sistema de ficheros NTFS
4. Carpetas compartidas


## 1.- CONFIGURACIÓN DE DISCOS EN WINDOWS

2.- CONFIGURACIÓN DE LOS DISCOS EN WINDOWS
Al instalar Windows 10 en un equipo se realizan de forma automática todas las tareas de preparación del disco donde va a instalarse el sistema operativo. Sin embargo, cuando se añaden discos adicionales en el equipo hay una serie de tareas que deberán realizarse manualmente:
•	Seleccionar el estilo de particionamiento: Windows 10 soporta dos estilos de particionamiento de disco: el particionamiento con master boot record (MBR) y las tablas de particiones GUID (globally unique identifier) o GPT.
•	Seleccionar el tipo de disco: Windows 10 soporta dos tipos de disco: los discos básicos y dinámicos. No se pueden usar ambos tipos en el mismo disco duro, pero si en diferentes discos del mismo sistema.
•	Dividir el disco en particiones o volúmenes: aunque en ocasiones se utilizan ambos términos indistintamente, lo correcto es utilizar el término particiones en discos básicos y volúmenes en los discos dinámicos.
•	Formatear las particiones o volúmenes con un sistema de ficheros que puede ser el sistema NTFS o el sistema FAT (con sus variantes FAT16, FAT32 y exFAT).
2.1.- SELECCIÓN DEL ESTILO DE PARTICIONAMIENTO
El término estilo de particionamiento se refiere al método que el sistema operativo usa para organizar las particiones en el disco. Las dos posibilidades que hay están directamente relacionadas con los dos tipos de tablas de particiones que hay: basadas en MBR y GPT.
Esta es una decisión que hay que tomar al inicializar el disco e influye en el número y tipo de particiones que podemos crear.
2.2.- TIPOS DE DISCOS
La mayoría de los ordenadores personales utilizan discos básicos porque son los más sencillos de gestionar. Un disco básico usando el estilo de particionamiento MBR organiza los datos utilizando particiones primarias, extendidas y lógicas. 
Al trabajar con discos básicos MBR en el Administrador de discos de Windows Server 2012, las tres primeras particiones creadas toman la forma de particiones primarias. Al crear la cuarta partición, el sistema crea una partición extendida que ocupa todo el espacio de disco que hubiera sin particionar, y una partición lógica dentro. El resto de las particiones que se creen serán particiones lógicas.
 
Si en cambio el disco básico tiene particionamiento GPT, se pueden crear hasta 128 particiones que verán como particiones primarias. En los discos GPT no hay particiones extendidas ni lógicas.
 
La alternativa al uso de discos básicos es convertir el disco en disco dinámico. El proceso de convertir un disco básico en dinámico crea una única partición que ocupa todo el disco. Se pueden crear un número ilimitado de volúmenes. Los discos dinámicos soportan diferentes tipos de volúmenes.
2.3.- TIPOS DE VOLÚMENES
Un disco dinámico puede contener un número ilimitado de volúmenes que en cierto modo se puede decir que son equivalentes a las particiones.
Al crear un volumen se puede escoger uno de los siguientes tipos:
•	Volumen simple: consiste en espacio de un único disco. Después de crear un volumen simple, se puede extender a múltiples discos para crear un volumen distribuido (spanned) o seccionado (striped), siempre y cuando no sea un volumen del sistema o de arranque. Además, se puede extender un volumen simple sobre espacio adyacente sin asignar en el mismo disco, o, con algunas limitaciones, comprimir el volumen liberando espacio no utilizado en él.
•	Volumen distribuido (spanned): consiste en espacio repartido en varios discos dinámicos (entre 2 y 32 discos). Un volumen extendido es esencialmente un método para combinar el espacio desde múltiples discos dinámicos en un único volumen mayor. Windows Server 2012 R2 escribe en los discos de forma secuencial, primero ocupa el primer disco, luego pasa al segundo, y así sucesivamente. En cualquier momento se puede añadir más almacenamiento a un disco extendido añadiendo discos adicionales.
Los discos extendidos no incrementan el rendimiento de lectura y/o escritura ni proporcionan tolerancia a fallos. De hecho, si falla un único disco de un volumen extendido, todos los datos en el volumen se perderán.
•	Volumen seccionado (striped): consiste en espacio repartido entre varios discos dinámicos (entre 2 y 32 discos). La diferencia entre un volumen seccionado y uno distribuido es que el sistema va alternando entres los sucesivos discos cada vez que escribe un bloque. Los discos seccionados mejoran el rendimiento porque las operaciones tanto de lectura como de escritura, se pueden realizar simultáneamente en diferentes discos. Los volúmenes seccionados no proporcionan tolerancia a fallos y tampoco se pueden extender una vez creados. Si falla un único disco se perderán todos los datos del volumen.
•	Volumen en espejo (mirrored): consisten en una cantidad de espacio idéntica en dos discos físicos, los cuales deben ser dinámicos. El sistema realiza todas las operaciones de lectura y escritura simultáneamente em ambos discos, por lo que contendrán copias duplicadas de todos los datos almacenados. Si un disco falla, el otro sigue proporcionando acceso al volumen hasta que el disco que falló sea reemplazado y reparado.
•	Volúmenes RAID-5: consiste en tres o más discos físicos, todos ellos dinámicos. El sistema distribuye datos e información de paridad a lo largo de todos los discos, de forma que, si falla un disco, sus datos pueden ser recreados utilizando la información de paridad en el resto de discos. Los volúmenes RAID5 proporcionan un mejor rendimiento de lectura porque, al estar distribuidos los datos en varios discos, pueden leerse simultáneamente. En cambio, el rendimiento de escritura se ve ligeramente afectado por tener que realizar los cálculos de paridad.
2.4.- EL COMANDO DISKPART DE WINDOWS
Aunque Windows dispone del Administrador de discos para poder gestionar todo lo relativo a la manipulación y gestión de discos y particiones, hay ocasiones en que no es posible utilizar esta herramienta, por ejemplo, en el caso de que Windows no pueda arrancar. En estas ocasiones disponemos de la herramienta en línea de comandos diskpart. Esta herramienta permite operaciones como obtener información detallada de discos y particiones o crear y eliminar particiones.
Hay diferentes formas de ejecutar diskpart:
•	Si Windows arranca correctamente simplemente hay que ejecutar el comando diskpart en la línea de comandos.
•	Si Windows no arranca los pasos a seguir varían en función de la versión del sistema operativo:
o	En Windows 7 es necesario utilizar el disco de instalación y, en la primera ventana, seleccionar la opción Repair your computer. Aquí saldrán una serie de opciones donde seleccionaremos la última: Command prompt. Esto hará que se muestra una interfaz de línea de comandos y ahí ya podremos ejecutar el comando diskpart.
o	En Windows 8 y 10, se puede acceder reiniciando el PC y manteniendo pulsadas las teclas Shift + F8 para abrir el menú avanzado. Ahí seleccionamos Resolución de problemas -> Command Prompt.
o	También podemos pulsar la combinación de teclas Shift-F10 una vez haya cargado Windows PE para abrir una ventana de consola.
Independientemente de cómo ejecutemos diskpart nos aparecerá un prompt en pantalla esperando que introduzcamos las órdenes que queramos realizar. Las más importantes son:
•	HELP: muestra la ayuda. Se le puede pasar también una orden para obtener ayuda detallada sobre la misma
•	LIST DISK: Muestra un listado de todos los discos que hay conectados al PC con información como el estado del mismo, su tamaño y el espacio libre.
•	SELECT DISK {NUM}: Selecciona un disco determinado identificado por un número. A partir del momento en que se selecciona todas las operaciones indicadas serán sobre ese disco.
•	DETAIL DISK: Muestra detalles completos sobre el disco seleccionado
•	LIST PARTITION: Muestra todas las particiones del disco seleccionado
•	SELECT PARTITION {NUM}: selecciona una partición identificada por su número dentro del disco seleccionado.
•	DETAIL PARTITION: muestra información detallada sobre la partición actualmente seleccionada.
•	DELETE PARTITION: esta orden elimina la partición actualmente seleccionada.
•	LIST VOLUME: muestra todos los volúmenes que hay en el sistema.
•	SELECT VOLUME {NUM}: selecciona el volumen identificado por su valor numérico.
•	DETAIL VOLUME: muestra información detallada sobre el volumen actualmente seleccionado.
•	DELETE VOLUME: elimina un volumen.
•	CRATE VOLUME: permite crear un volumen. La sintaxis es de la forma:
o	create volume simple [size] [disk #]
o	create volume stripe [size] [disks # (2 o más)]
o	create volume raid [size] [disks (3 o más)]
•	FORMAT: permite formatear cualquier volumen que haya sido previamente seleccionado. Algunos parámetros que se le pueden pasar son:
o	FS: sistema de ficheros
o	LABEL: etiqueta del disco
o	QUICK: para indicar que realice un formato rápido
o	COMPRESS: para indicar que se deben comprimir los datos
Ejemplo: FORMAT FS=NTFS LABEL=”Mi disco” QUICK COMPRESS
•	CREATE PARTITION {TIPO}: crea una partición del tipo indicado. Este puede ser PRIMARY, LOGICAL, EXTENDED o EFI. Además, opcionalmente se puede indicar un tamaño en MB (SIZE) y un desplazamiento en KB (OFFSET). Si no se indica el tamaño ocupa todo el tamaño disponible y si no se indica el desplazamiento se sitúa en la primera extensión de disco capaz de contener la partición.
Ejemplo: CREATE PARTITION EFI SIZE=500 
•	CONVERT MBR: permite convertir un disco con tabla de particiones GPT en MBR. Hay que tener en cuenta que el disco debe estar vacío, ya que en caso contrario eliminará todos los datos en él.
•	CONVERT GPT: convierte un disco MBR en GPT. Al igual que en el caso anterior, el disco debe estar vacío ya que en caso contrario eliminará todos los datos.
•	RESCAN: permite escanear los buses de entrada y salida para localizar nuevos discos que se hayan añadido al sistema.

2.5.- GESTIÓN DE DISCOS CON POWERSHELL
Como no puede ser de otra manera, también es posible gestionar los discos del sistema utilizando Powershell. Sin embargo, en este caso hay una serie de limitaciones ya que no está implementada en Powershell la administración de discos básicos y dinámicos, limitándose la funcionalidad disponible a la gestión básica de discos y de particiones.
Por otro lado, hay una posibilidad interesante si tenemos instalado Hyper-V en nuestro equipo y es el hecho de poder tratar discos virtuales como si fueran discos físicos, añadiéndolos a nuestra máquina y utilizándolos como si fuera un disco más.
2.5.1.- MOSTRAR INFORMACIÓN DE LOS DISCOS CONECTADOS
Para mostrar todos los discos instalados en el sistema utilizaremos el comando Get-Disk, que los muestro junto con propiedades como el número de serie, el estado de salud (SMART), el tamaño o el tipo de tabla de particiones. Además, identifica cada disco con un número que nos servirá para hacerle referencia desde otros comandos. También podemos utilizar este número con el parámetro -Number para que nos muestre un único disco.
Si queremos mostrar las particiones de un disco usaremos el comando Get-Partition que nos muestra todas las particiones que hay en todos los discos, o bien las de un único disco si utilizamos el parámetro -DiskId indicando el identificador del disco o -DiskNumber con el número que le identifica.
2.5.2.- CREAR Y MONTAR UN DISCO VIRTUAL
Como mencionamos antes, desde Powershell podemos trabajar con discos virtuales, ya sea para utilizarlos con máquinas virtuales en Hyper-V o para montarlos directamente en el sistema y utilizarlos como un disco normal. Hay diversos comandos para trabajar con discos virtuales, que podemos mostrar con el comando Get-Command -Noun VHD.
Para crear un disco virtual el comando utilizado es New-VHD, algunos de cuyos parámetros son:
•	-Path: ruta y nombre de archivo donde se guardará el disco virtual.
•	-SizeBytes: tamaño del disco virtual, se indica el número seguido por la unidad de medida (por ejemplo, 100GB)
•	-Fixed | -Dynamic: estas dos opciones excluyentes indicarán si el disco será de crecimiento fijo o dinámico (cuidado no confundas los discos virtuales de crecimiento dinámico con los discos dinámicos que permiten crear volúmenes en espejo, distribuidos, …)
Una vez creado el disco se puede utilizar en cualquier máquina virtual, pero también se puede montar en el propio sistema mediante el comando Mount-VHD. A este comando únicamente hay que pasarle la ruta del disco virtual mediante el parámetro -Path.
Si por algún motivo quisiéramos desmontar el disco lo haremos con el comando Dismount-VHD con el parámetro -DiskNumber.
2.5.3.- INICIALIZAR UN DISCO 
Cuando añadimos un nuevo disco a nuestro sistema, ya sea físico o virtual, es simplemente un espacio de almacenamiento capaz de contener una gran cantidad de información, pero sin ninguna estructura. El primer paso para darle una estructura lógica es la inicialización del disco, que básicamente consiste en crear la tabla de particiones para que posteriormente pueda contener particiones. 
El comando de Powershell para inicializar un disco es Initialize-Disk, al cual se le debe pasar el número de disco que queremos inicializar mediante el parámetro -Number. Al igual que cuando inicializamos un disco mediante el Administrador de discos, tendremos que escoger el estilo de la tabla de particionamiento que queremos utilizar. Esto lo haremos con el parámetro -PartitionStyle indicando a continuación si es MBR o GPT.
Una vez
Si quisiéramos cambiar el estilo de la tabla de particiones de un disco ya inicializado se puede utilizar el comando Set-Disk con los mismos parámetros mencionados anteriormente. Este comando también nos es útil si queremos marcar un disco como de solo lectura (parámetro -IsReadOnly $True) o si queremos deshabilitarlo (-IsOffline $True/$False)
2.5.4.- CREAR Y FORMATEAR PARTICIONES
Una vez inicializado el disco solo queda crear las particiones y aplicarles formato. Recuerda que desde Powershell no podemos trabajar con discos dinámicos, por lo que todas las particiones que creemos serán particiones normales.
El comando para crear particiones es New-Partition con el que se pueden utilizar estos parámetros:
•	-DiskNumber: número del disco en el que se creará la partición
•	-DriveLetter: le asigna la letra que se le indique a la partición
•	-AssignDriveLetter: le asigna la primera letra disponible
•	-UseMaximumSize: para indicar que la partición ocupará todo el espacio disponible en el disco.
•	-Size: si queremos indicar un tamaño específico.
Si queremos formatear una partición usaremos el comando Format-Volume, que admite estos parámetros:
•	-DriveLetter: para indicar la letra de la partición que queremos formatear
•	-FileSystem: para especificar el sistema de ficheros (NTFS, ReFS, exFAT, FAT32, and FAT)
•	-FileSystemLabel: etiqueta del disco
•	-Full: realiza un formato completo, escribiendo en cada sector del disco. Está desaconsejado en los discos de crecimiento dinámico.
Por último, si queremos limpiar un disco y eliminar toda la información, así como el contenido de la tabla de particiones, deberemos usar el comando Clear-Disk de la forma Clear-Disk -Number 1 -RemoveData.

2.6.- ADMINISTRACIÓN AUTOMÁTICA DE DISCOS
Vamos a ver ahora un ejemplo de un script en el que automatizamos la administración de discos virtuales. En este caso el script pedirá al usuario por teclado el tamaño del disco, tras lo cual creará un disco duro virtual de dicho tamaño, lo montará, los formateará y le asignará una letra.
2.6.1.- EL PARÁMETRO PASSTHRU
Antes de ver el script es necesario conocer un parámetro que es utilizado por muchos cmdlets, que es el parámetro -Passthru. La razón de ser de este parámetro está en los comandos que realizan una tarea, pero no devuelven ningún objeto, es decir, que no muestran ninguna salida por pantalla o que si están en un pipeline no le pasan nada al siguiente comando.
Un ejemplo de este tipo de comandos puede ser Mount-VHD, que simplemente monta un disco virtual sin mostrar ningún otro tipo de salida. Si vamos a la ayuda del parámetro Passthru de este comando veremos que nos dice lo siguiente:
 
Es decir, al incluir ese parámetro no solamente montará el disco, sino que también devolverá el objeto correspondiente al disco, lo que nos permitiría pasarlo mediante el pipeline a otro comando. Así, utilizando este parámetro podríamos juntar todos los pasos de preparación del disco en una única orden:


Mount-VHD -Path c:\disco.vhd -Passthru |
	Initialize-Disk -Passthru |
	New-Partition -AssignDriveLetter -UseMaximumSize |
	Format-Volume -FileSystem NTFS -Confirm $False

Observa que el comando New-Partition no necesita el parámetro -Passthru porque, por defecto, aunque crea una partición también devuelve el objeto representando la partición que ha creado.
2.6.2.- SCRIPT DE CREACIÓN AUTOMÁTICA DE DISCOS
Teniendo en cuenta lo anterior, el script quedaría de la siguiente forma:
$disco = Read-Host “Introduce el nombre del disco (path):”
[double]$tamano = Read-Host “Tamaño del disco en bytes:”
New-VHD -Path $disco -SizeBytes $tamano -Dynamic
Mount-VHD -Path $disco -Passthru |
   Initialize-Disk -Passthru |
   New-Partition -AssignDriveLetter -UseMaximumSize |
   Format-Volume -Filesystem NTFS -Confirm $false

	
 
