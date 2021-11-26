![Carátula UT04](imgs/caratula_ut04.png)

### Contenidos

1. [Organización del almacenamiento](01_organización.md)
2. [**Configuración de discos en Windows**](02_configuración_discos.md)
3. [Sistemas de archivos](03_sistemas_archivos.md)
4. [El sistema de ficheros NTFS](04_ntfs.md)
5. [Carpetas compartidas](05_compartidas.md)


# 3.- CONFIGURACIÓN DE DISCOS EN WINDOWS

Al instalar Windows 10 en un equipo se realizan de forma automática todas las tareas de preparación del disco donde va a instalarse el sistema operativo. Sin embargo, cuando se añaden discos adicionales en el equipo hay una serie de tareas que deberán realizarse manualmente:

- Seleccionar el estilo de particionamiento: Windows 10 soporta dos estilos de particionamiento de disco: el particionamiento con master boot record (MBR) y las tablas de particiones GUID (globally unique identifier) o GPT.
- Seleccionar el tipo de disco: Windows 10 soporta dos tipos de disco: los discos básicos y dinámicos. No se pueden usar ambos tipos en el mismo disco duro, pero si en diferentes discos del mismo sistema.
- Dividir el disco en particiones o volúmenes: aunque en ocasiones se utilizan ambos términos indistintamente, lo correcto es utilizar el término particiones en discos básicos y volúmenes en los discos dinámicos.
- Formatear las particiones o volúmenes con un sistema de ficheros que puede ser el sistema NTFS o el sistema FAT (con sus variantes FAT16, FAT32 y exFAT).


## 3.1.- Selección del estilo de particionamiento

El término estilo de particionamiento se refiere al método que el sistema operativo usa para organizar las particiones en el disco. Las dos posibilidades que hay están directamente relacionadas con los dos tipos de tablas de particiones que hay: basadas en **MBR** y **GPT**.

Esta es una decisión que hay que tomar al inicializar el disco e influye en el número y tipo de particiones que podemos crear.

## 3.2.- Tipos de discos

La mayoría de los ordenadores personales utilizan **discos básicos** porque son los más sencillos de gestionar. Un disco básico usando el estilo de particionamiento MBR organiza los datos utilizando particiones primarias, extendidas y lógicas. 

Al trabajar con discos básicos MBR en el Administrador de discos, las tres primeras particiones creadas toman la forma de particiones primarias. Al crear la cuarta partición, el sistema crea una partición extendida que ocupa todo el espacio de disco que hubiera sin particionar, y una partición lógica dentro. El resto de las particiones que se creen serán particiones lógicas.
 
Si en cambio el disco básico tiene particionamiento GPT, se pueden crear hasta 128 particiones que verán como particiones primarias. En los discos GPT no hay particiones extendidas ni lógicas.
 
La alternativa al uso de discos básicos es convertir el disco en **disco dinámico**. El proceso de convertir un disco básico en dinámico crea una única partición que ocupa todo el disco. Se pueden crear un número ilimitado de volúmenes dentro de la partición que ha creado. Los discos dinámicos soportan diferentes tipos de volúmenes.


## 3.3.- Tipos de volúmenes

Un disco dinámico puede contener un número ilimitado de volúmenes que en cierto modo se puede decir que son equivalentes a las particiones. Al crear un volumen se puede escoger uno de los siguientes tipos:

- **Volumen simple**: consiste en espacio de un único disco. Después de crear un volumen simple, se puede extender a múltiples discos para crear un volumen distribuido (spanned) o seccionado (striped), siempre y cuando no sea un volumen del sistema o de arranque. Además, se puede extender un volumen simple sobre espacio adyacente sin asignar en el mismo disco, o, con algunas limitaciones, comprimir el volumen liberando espacio no utilizado en él.
- **Volumen distribuido (spanned)**: consiste en espacio repartido en varios discos dinámicos (entre 2 y 32 discos). Un volumen extendido es esencialmente un método para combinar el espacio desde múltiples discos dinámicos en un único volumen mayor. Windows escribe en los discos de forma secuencial, primero ocupa el primer disco, luego pasa al segundo, y así sucesivamente. En cualquier momento se puede añadir más almacenamiento a un disco extendido añadiendo discos adicionales. Los discos distribuidos no incrementan el rendimiento de lectura y/o escritura ni proporcionan tolerancia a fallos. De hecho, si falla un único disco de un volumen extendido, todos los datos en el volumen se perderán.
- **Volumen seccionado (striped)**: consiste en espacio repartido entre varios discos dinámicos (entre 2 y 32 discos). La diferencia entre un volumen seccionado y uno distribuido es que el sistema va _alternando_ entres los sucesivos discos cada vez que escribe un bloque. Los discos seccionados mejoran el rendimiento porque las operaciones tanto de lectura como de escritura, se pueden realizar simultáneamente en diferentes discos. Los volúmenes seccionados no proporcionan tolerancia a fallos y tampoco se pueden extender una vez creados. Si falla un único disco se perderán todos los datos del volumen.
- **Volumen en espejo (mirrored)**: consisten en una cantidad de espacio idéntica en dos discos físicos, los cuales deben ser dinámicos. El sistema realiza todas las operaciones de lectura y escritura simultáneamente em ambos discos, por lo que contendrán copias duplicadas de todos los datos almacenados. Si un disco falla, el otro sigue proporcionando acceso al volumen hasta que el disco que falló sea reemplazado y reparado.
- **Volúmenes RAID-5**: consiste en tres o más discos físicos, todos ellos dinámicos. El sistema distribuye datos e información de paridad a lo largo de todos los discos, de forma que, si falla un disco, sus datos pueden ser recreados utilizando la información de paridad en el resto de discos. Los volúmenes RAID5 proporcionan un mejor rendimiento de lectura porque, al estar distribuidos los datos en varios discos, pueden leerse simultáneamente. En cambio, el rendimiento de escritura se ve ligeramente afectado por tener que realizar los cálculos de paridad.


## 3.4.- El comando `DISKPART` de Windows

Aunque Windows dispone del *Administrador de discos* para poder gestionar todo lo relativo a la manipulación y gestión de discos y particiones, hay ocasiones en que no es posible utilizar esta herramienta, por ejemplo, en el caso de que Windows no pueda arrancar. En estas ocasiones disponemos de la herramienta en línea de comandos `diskpart`. Esta herramienta permite operaciones como obtener información detallada de discos y particiones o crear y eliminar particiones.

Hay diferentes formas de ejecutar `diskpart`:

- Si Windows arranca correctamente simplemente hay que ejecutar el comando `diskpart` en la línea de comandos.
- Si Windows no arranca los pasos a seguir varían en función de la versión del sistema operativo:
   - En Windows 7 es necesario utilizar el disco de instalación y, en la primera ventana, seleccionar la opción *Repair your computer*. Aquí saldrán una serie de opciones donde seleccionaremos la última: *Command prompt*. Esto hará que se muestra una interfaz de línea de comandos y ahí ya podremos ejecutar el comando `diskpart`.
   - En Windows 8 y 10, se puede acceder reiniciando el PC y manteniendo pulsadas las teclas `Shift + F8` para abrir el menú avanzado. Ahí seleccionamos *Resolución de problemas -> Command Prompt*.
   - También podemos pulsar la combinación de teclas `Shift-F10` una vez haya cargado Windows PE para abrir una ventana de consola.
- 
Independientemente de cómo ejecutemos diskpart nos aparecerá un prompt en pantalla esperando que introduzcamos las órdenes que queramos realizar. Las más importantes son:

- `HELP`: muestra la ayuda. Se le puede pasar también una orden para obtener ayuda detallada sobre la misma
- `LIST DISK`: Muestra un listado de todos los discos que hay conectados al PC con información como el estado del mismo, su tamaño y el espacio libre.
- `SELECT DISK {NUM}`: Selecciona un disco determinado identificado por un número. A partir del momento en que se selecciona todas las operaciones indicadas serán sobre ese disco.
- `DETAIL DISK`: Muestra detalles completos sobre el disco seleccionado
- `LIST PARTITION`: Muestra todas las particiones del disco seleccionado
- `SELECT PARTITION {NUM}`: selecciona una partición identificada por su número dentro del disco seleccionado.
- `DETAIL PARTITION`: muestra información detallada sobre la partición actualmente seleccionada.
- `DELETE PARTITION`: esta orden elimina la partición actualmente seleccionada.
- `LIST VOLUME`: muestra todos los volúmenes que hay en el sistema.
- `SELECT VOLUME {NUM}`: selecciona el volumen identificado por su valor numérico.
- `DETAIL VOLUME`: muestra información detallada sobre el volumen actualmente seleccionado.
- `DELETE VOLUME`: elimina un volumen.
- `CREATE VOLUME`: permite crear un volumen. La sintaxis es de la forma:
   - `create volume simple [size] [disk #]`
   - `create volume stripe [size] [disks # (2 o más)]`
   - `create volume raid [size] [disks (3 o más)]`
- `FORMAT`: permite formatear cualquier volumen que haya sido previamente seleccionado. Algunos parámetros que se le pueden pasar son:
   - `FS`: sistema de ficheros
   - `LABEL`: etiqueta del disco
   - `QUICK`: para indicar que realice un formato rápido
   - `COMPRESS`: para indicar que se deben comprimir los datos
- Ejemplo: `FORMAT FS=NTFS LABEL=”Mi disco” QUICK COMPRESS`
- `CREATE PARTITION {TIPO}`: crea una partición del tipo indicado. Este puede ser PRIMARY, LOGICAL, EXTENDED o EFI. Además, opcionalmente se puede indicar un tamaño en MB (`SIZE`) y un desplazamiento en KB (`OFFSET`). Si no se indica el tamaño ocupa todo el tamaño disponible y si no se indica el desplazamiento se sitúa en la primera extensión de disco capaz de contener la partición. Ejemplo: `CREATE PARTITION EFI SIZE=500` 
- `CONVERT MBR`: permite convertir un disco con tabla de particiones GPT en MBR. Hay que tener en cuenta que el disco debe estar vacío, ya que en caso contrario eliminará todos los datos en él.
- `CONVERT GPT`: convierte un disco MBR en GPT. Al igual que en el caso anterior, el disco debe estar vacío ya que en caso contrario eliminará todos los datos.
- `RESCAN`: permite escanear los buses de entrada y salida para localizar nuevos discos que se hayan añadido al sistema.





***
[Volver al índice principal](index_UT04.md)