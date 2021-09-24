![Carátula UT06](imgs/caratula_ut06.png)

# 7.- GESTIÓN DE DISCOS CON POWERSHELL

Como no puede ser de otra manera, también es posible gestionar los discos del sistema utilizando Powershell. Sin embargo, en este caso hay una serie de limitaciones ya que no está implementada en Powershell la administración de discos básicos y dinámicos, limitándose la funcionalidad disponible a la gestión básica de discos y de particiones.

Por otro lado, hay una posibilidad interesante si tenemos instalado Hyper-V en nuestro equipo y es el hecho de poder tratar discos virtuales como si fueran discos físicos, añadiéndolos a nuestra máquina y utilizándolos como si fuera un disco más.

### 7.1.- MOSTRAR INFORMACIÓN DE LOS DISCOS CONECTADOS

Para mostrar todos los discos instalados en el sistema utilizaremos el comando `Get-Disk`, que los muestro junto con propiedades como el número de serie, el estado de salud (SMART), el tamaño o el tipo de tabla de particiones. Además, identifica cada disco con un número que nos servirá para hacerle referencia desde otros comandos. También podemos utilizar este número con el parámetro -Number para que nos muestre un único disco.

Si queremos mostrar las particiones de un disco usaremos el comando Get-Partition que nos muestra todas las particiones que hay en todos los discos, o bien las de un único disco si utilizamos el parámetro `-DiskId` indicando el identificador del disco o `-DiskNumber` con el número que le identifica.


### 7.2.- CREAR Y MONTAR UN DISCO VIRTUAL

Como mencionamos antes, desde Powershell podemos trabajar con discos virtuales, ya sea para utilizarlos con máquinas virtuales en Hyper-V o para montarlos directamente en el sistema y utilizarlos como un disco normal. Hay diversos comandos para trabajar con discos virtuales, que podemos mostrar con el comando `Get-Command -Noun VHD`.

Para crear un disco virtual el comando utilizado es New-VHD, algunos de cuyos parámetros son:

- `-Path`: ruta y nombre de archivo donde se guardará el disco virtual.
- `-SizeBytes`: tamaño del disco virtual, se indica el número seguido por la unidad de medida (por ejemplo, 100GB)
- `-Fixed` | `-Dynamic`: estas dos opciones excluyentes indicarán si el disco será de crecimiento fijo o dinámico (cuidado no confundas los discos virtuales de crecimiento dinámico con los discos dinámicos que permiten crear volúmenes en espejo, distribuidos, …)

Una vez creado el disco se puede utilizar en cualquier máquina virtual, pero también se puede montar en el propio sistema mediante el comando `Mount-VHD`. A este comando únicamente hay que pasarle la ruta del disco virtual mediante el parámetro `-Path`.

Si por algún motivo quisiéramos desmontar el disco lo haremos con el comando Dismount-VHD con el parámetro -DiskNumber.

### 7.3.- INICIALIZAR UN DISCO 

Cuando añadimos un nuevo disco a nuestro sistema, ya sea físico o virtual, es simplemente un espacio de almacenamiento capaz de contener una gran cantidad de información, pero sin ninguna estructura. El primer paso para darle una estructura lógica es la inicialización del disco, que básicamente consiste en crear la tabla de particiones para que posteriormente pueda contener particiones. 

El comando de Powershell para inicializar un disco es `Initialize-Disk`, al cual se le debe pasar el número de disco que queremos inicializar mediante el parámetro `-Number`. Al igual que cuando inicializamos un disco mediante el *Administrador de discos*, tendremos que escoger el estilo de la tabla de particionamiento que queremos utilizar. Esto lo haremos con el parámetro `-PartitionStyle` indicando a continuación si es MBR o GPT.

Si quisiéramos cambiar el estilo de la tabla de particiones de un disco ya inicializado se puede utilizar el comando `Set-Disk` con los mismos parámetros mencionados anteriormente. Este comando también nos es útil si queremos marcar un disco como de solo lectura (parámetro `-IsReadOnly $True`) o si queremos deshabilitarlo (`-IsOffline $True/$False`)


### 7.4.- CREAR Y FORMATEAR PARTICIONES

Una vez inicializado el disco solo queda crear las particiones y aplicarles formato. Recuerda que desde Powershell no podemos trabajar con discos dinámicos, por lo que todas las particiones que creemos serán particiones normales.

El comando para crear particiones es New-Partition con el que se pueden utilizar estos parámetros:

- `-DiskNumber`: número del disco en el que se creará la partición
- `-DriveLetter`: le asigna la letra que se le indique a la partición
- `-AssignDriveLetter`: le asigna la primera letra disponible
- `-UseMaximumSize`: para indicar que la partición ocupará todo el espacio disponible en el disco.
- `-Size`: si queremos indicar un tamaño específico.

Si queremos formatear una partición usaremos el comando Format-Volume, que admite estos parámetros:

- `-DriveLetter`: para indicar la letra de la partición que queremos formatear
- `-FileSystem`: para especificar el sistema de ficheros (NTFS, ReFS, exFAT, FAT32, and FAT)
- `-FileSystemLabel`: etiqueta del disco
- `-Full`: realiza un formato completo, escribiendo en cada sector del disco. Está desaconsejado en los discos de crecimiento dinámico.
- 
Por último, si queremos limpiar un disco y eliminar toda la información, así como el contenido de la tabla de particiones, deberemos usar el comando `Clear-Disk` de la forma `Clear-Disk -Number 1 -RemoveData`.


### 7.5.- ADMINISTRACIÓN AUTOMÁTICA DE DISCOS

Vamos a ver ahora un ejemplo de un script en el que automatizamos la administración de discos virtuales. En este caso el script pedirá al usuario por teclado el tamaño del disco, tras lo cual creará un disco duro virtual de dicho tamaño, lo montará, los formateará y le asignará una letra.

#### 7.5.1.- EL PARÁMETRO PASSTHRU

Antes de ver el script es necesario conocer un parámetro que es utilizado por muchos cmdlets, que es el parámetro `-Passthru`. La razón de ser de este parámetro está en los comandos que realizan una tarea, pero no devuelven ningún objeto, es decir, que no muestran ninguna salida por pantalla o que si están en un pipeline no le pasan nada al siguiente comando.

Un ejemplo de este tipo de comandos puede ser `Mount-VHD`, que simplemente monta un disco virtual sin mostrar ningún otro tipo de salida. Si vamos a la ayuda del parámetro `-Passthru` de este comando veremos que nos dice lo siguiente:
 



Es decir, al incluir ese parámetro no solamente montará el disco, sino que también devolverá el objeto correspondiente al disco, lo que nos permitiría pasarlo mediante el pipeline a otro comando. Así, utilizando este parámetro podríamos juntar todos los pasos de preparación del disco en una única orden:

```powershell
Mount-VHD -Path c:\disco.vhd -Passthru |
	Initialize-Disk -Passthru |
	New-Partition -AssignDriveLetter -UseMaximumSize |
	Format-Volume -FileSystem NTFS -Confirm $False
```

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

	
 
