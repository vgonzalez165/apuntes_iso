---
layout: default
title: Gestión de procesos
parent: UT10. Linux. Administración
nav_order: 6
---

# 6.- Gestión de procesos

Todo lo que hacemos en un servidor Linux toma la forma de procesos, por lo que es muy importante aprender a trabajar con ellos. Linux dispone de un gran número de comandos para monitorizar los procesos en nuestro sistema. Veamos los más importantes.

## 6.1.- Procesos automáticos e interactivos

Una clasificación de los procesos que hay en Linux puede ser la que los divide en procesos automáticos e interactivos.

- **Procesos automáticos**: estos son los **servicios** o **daemons** que se inician automáticamente al arrancar el servidor. El responsable de lanzar todos estos procesos es `systemd`, tal y como veremos más adelante. Los demonios son procesos que se ejecutan en **segundo plano**, es decir, no tienen ninguna salida por la salida estándar.
- **Procesos interactivos**: son iniciados por los usuarios en *Shell*. Para iniciar un proceso interactivo, el usuario debe introducir el comando correspondiente.
  
Los procesos tienen una relación de parentesco donde cada proceso tiene un **proceso padre** que es el que le ha creado. Por ejemplo, un proceso interactivo iniciado en el terminal tiene como padre el proceso Bash correspondiente al terminal.

Cuando un proceso finaliza, debe enviar el estado de salida al proceso padre. Esto es requisito para que el proceso pueda finalizar su ejecución. Si por algún motivo el proceso no pudiera comunicarse con el proceso padre (por ejemplo, porque este hubiera finalizado su ejecución) el proceso hijo no podrá finalizar y quedará en un estado en el que no se podrá realizar ninguna tarea de administración con él. Un proceso que se encuentre en este estado se denomina **proceso zombi**.

Los conceptos de proceso padre e hijo se aplican a todos los procesos del sistema, siendo `systemd` el primer proceso de todos. Es posible ver la estructura jerárquica de los procesos utilizando el comando `pstree`.
 
```
vgonzalez@ubuntu:~$ pstree
systemd─┬─ModemManager───2*[{ModemManager}]
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─atd
        ├─cron
        ├─dbus-daemon
        ├─irqbalance───{irqbalance}
        ├─login───bash
        ├─multipathd───6*[{multipathd}]
```


## 6.2.- Procesos en primer y segundo plano

Los procesos interactivos se pueden clasificar como procesos en primer y en segundo plano. Los procesos en **primer plano** interactúan directamente con el usuario tomando el control de la terminal. Por el contrario, los procesos en **segundo plano**, aunque también son procesos hijos del Shell, están desvinculados de este en el sentido de que no envían ni la salida estándar ni la de errores al Shell.

Hay dos formas de crear un proceso en segundo plano:

- Poner el carácter *ampersand* (`&`) después del comando al iniciarlo. Esto hace que el proceso pase inmediatamente a segundo plano. Por ejemplo, `nmap 192.168.1.10 > ~/nmap.out &` ejecutará el comando `nmap` en segundo plano. Esto le permitirá realizar su función sin necesidad de que el usuario tenga que esperar su finalización para poder hacer otra cosa.
- Interrumpir el proceso con la combinación de teclas `Ctrl+Z` y luego utilizar el comando `bg` para reiniciarlo en segundo plano.
- 
Aunque el comando se está ejecutando en segundo plano, sigue siendo posible controlarlo. Con el comando `jobs` se puede ver la lista de procesos en segundo plano


## 6.3.- Comandos relacionados con los procesos

### 6.3.1.- Listado de procesos: `ps`

El comando `ps` lista todos los procesos del sistema, su estado, tamaño, nombre, propietario, tiempo de CPU, tiempo de reloj y otros muchos.

Este comando dispone de muchos modificadores, los más utilizados son los siguientes:

| Modificador | Descripción |
| ----------- | ----------- |
| `-a` | Muestra todos los procesos, incluso los que no están controlados por ningún terminal |
| `-r` | Muestra solo los procesos en ejecución |
| `-x` | Muestra los procesos que no tienen un terminal controlado |
| `-u` | Muestra los propietarios de los procesos |
| `-f` | Visualiza las relaciones padre/hijo entre los procesos |
| `-l` | Produce un listado en formato largo |
| `-w` | Salida ancha, no se truncan las líneas para que entren en la misma línea |

Si invocamos la orden `ps –auxw` tendremos la siguiente salida:

```
vgonzalez@ubuntu:~$ ps -auxw
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.5 101936 11208 ?        Ss   10:05   0:01 /sbin/init
root           2  0.0  0.0      0     0 ?        S    10:05   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   10:05   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   10:05   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   10:05   0:00 [kworker/0:0H-kblockd]
root           9  0.0  0.0      0     0 ?        I<   10:05   0:00 [mm_percpu_wq]
root          10  0.0  0.0      0     0 ?        S    10:05   0:00 [ksoftirqd/0]
```

El significado de cada columna es el siguiente:

- `USER`: propietario del proceso
- `PID`: identificador del proceso
- `%CPU`: porcentaje de CPU utilizado por el proceso, en caso de tener varios procesadores en el sistema este valor puede ser superior a 100
- `%MEM`: porcentaje de memoria ocupada por el proceso.
- `VSZ`: Cantidad de memoria virtual utilizada por el proceso.
- `RSS`: La cantidad de memoria residente ocupada por el proceso
- `TTY`: Terminal controlado por el proceso. Una interrogación indica que no está asociado a ningún terminal.
- `STAT`: estado del proceso. Puede tener los siguientes valores:
	- `S`: El proceso está dormido. Quiere decir que el proceso está preparado para ejecutarse, pero está esperando a que otro proceso finalice su ejecución.
	- `R`: El proceso está actualmente en ejecución.
	- `D`: Dormido sin interrupción posible (normalmente quiere decir que está esperando a finalizar una operación de E/S)
	- `T`: El proceso está haciendo trazas por un depurador o ha sido parado.
	- `Z`: El proceso está en estado `zombi`. 
- `START`: Hora en que comenzó el proceso
- `TIME`: Cantidad de tiempo que el proceso ha gastado de CPU
- `COMMAND`: Nombre del comando que ha invocado el proceso y sus parámetros.


### 6.3.2.- Visualización interactiva: `top`

El comando `top` es la versión interactiva de `ps`. Muestra los procesos, pero actualiza la información cada determinado tiempo. Además, permite al usuario realizar operaciones sobre los procesos mientras se ejecuta.

```
top - 11:21:03 up  1:15,  2 users,  load average: 0,00, 0,00, 0,00
Tasks: 106 total,   1 running, 105 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0,0 us,  0,0 sy,  0,0 ni,100,0 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st
MiB Mem :   1918,1 total,   1103,5 free,    191,3 used,    623,3 buff/cache
MiB Swap:   2048,0 total,   2048,0 free,      0,0 used.   1574,9 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    396 root      20   0       0      0      0 I   0,3   0,0   0:03.45 kworker/0:3-events
   1958 vgonzal+  20   0    9232   3816   3168 R   0,3   0,2   0:00.01 top
      1 root      20   0  101936  11208   8200 S   0,0   0,6   0:01.13 systemd
      2 root      20   0       0      0      0 S   0,0   0,0   0:00.00 kthreadd
      3 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_gp
      4 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_par_gp
      6 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker/0:0H-kblockd
      9 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 mm_percpu_wq
     10 root      20   0       0      0      0 S   0,0   0,0   0:00.09 ksoftirqd/0
     11 root      20   0       0      0      0 I   0,0   0,0   0:00.26 rcu_sched
     12 root      rt   0       0      0      0 S   0,0   0,0   0:00.03 migration/0
     13 root     -51   0       0      0      0 S   0,0   0,0   0:00.00 idle_inject/0
     14 root      20   0       0      0      0 S   0,0   0,0   0:00.00 cpuhp/0
     15 root      20   0       0      0      0 S   0,0   0,0   0:00.00 cpuhp/1
     16 root     -51   0       0      0      0 S   0,0   0,0   0:00.00 idle_inject/1
     17 root      rt   0       0      0      0 S   0,0   0,0   0:00.07 migration/1
     18 root      20   0       0      0      0 S   0,0   0,0   0:00.08 ksoftirqd/1
```

Este comando muestra también información general sobre el sistema en la parte superior. De esta información se puede destacar el **uptime**, o tiempo que lleva el servidor funcionando desde que fue arrancado, y la carga media del sistema en el último minuto, en los últimos 5 minutos y en los últimos 15 minutos.

La **carga media** es mostrada por un número que indica la actividad actual de la cola de procesos. El valor aquí se interpreta como el número de procesos que están esperando de media para hacer uso del procesador. En un sistema con un procesador y un núcleo, un valor de 1.00 indica que el sistema está continuamente ocupado procesando los procesos que hay en la cola, pero no hay procesos esperando en la cola.

Si el valor supera a 1.00, los procesos sufrirán esperas para ejecutarse, peores cuanto mayor sea el valor, por lo que es aconsejable que en el uso diario no exceda el valor de 1.00. Si el sistema tiene múltiples núcleos o *hyperthreading*, el valor aconsejable se incrementará proporcionalmente. Por ejemplo, un servidor con 8 núcleos e *hyperthreading* podría trabajar cómodamente con un valor próximo a 16.00

También muestra el número de procesos que hay actualmente en memoria, clasificados por tipos y el uso general tanto de CPU como de memoria.

Este comando dispone de varios modificadores:

| Modificador | Descripción |
| ------------| ----------- | 
| `-b` | Se ejecuta en modo por lotes (batch). Esta opción es útil cuando se quiere redireccionar la salida a un fichero o a otro comando. Por defecto realiza una iteración pero se pueden realizar más utilizando el parámetro `–n num` |
| `-i` | Ignora los procesos en estado *idle*, es decir, los inactivos. Solo muestra los procesos “interesantes” |

Además, el comando `top` dispone de algunas opciones interactivas:

| Opción | Descripción |
| ------ | ----------- |
| `k`    | Mata un proceso. Nos pedirá su PID y la señal a enviar al proceso (por defecto 15 – SIGTERM) |
| `n`    | Cambia el número de procesos que se muestran |
| `r`    | Cambia la prioridad de un proceso (*renice*). Pedirá el PID del proceso y el valor de la prioridad:  un valor positivo hará que el proceso pierda prioridad, un valor negativo aumentará la prioridad del proceso (esto solo puede hacerlo el usuario root) |


### 6.3.3.- Envío de una señal a un proceso: `kill`

A pesar de su nombre este comando no mata procesos, sino que sirve para enviarles **señales** a los procesos en ejecución. El sistema operativo, por defecto, proporciona a cada proceso un conjunto estándar de **manejadores de señales** para gestionar las señales entrantes.

Algunas de las señales más importantes son las siguientes:

| Señal | Descripción |
| ----- | ----------- |
| `SIGHUP (1)`   | Esta señal es enviada por el Shell a todos los procesos cuando el usuario cierra sesión, lo que hace que se cierren dichos  |procesos.
| `SIGINT (2)`   | Interrumpe la ejecución de un proceso. Es la señal enviada cuando pulsamos `Ctrl-C` |
| `SIGQUIT (3)`  | Muy similar a `SIGINT` |
| `SIGKILL (9)`  | Mata el proceso, finaliza su ejecución incondicional e inmediatamente. No puede ser ignorada por el proceso. |
| `SIGTERM (15)` | Solicita al proceso que finalice de una forma adecuada. |
| `SIGTSTP (20)` | Detiene temporalmente el proceso y lo deja preparado para continuar la ejecución. Es la señal enviada cuando pulsamos `Ctrl-Z` |
| `SIGCONT (18)` | Continúa la ejecución de un proceso parado. |

Cuando se invoca a `kill` se requiere al menos un parámetro: el **número de identificación del proceso (PID)**. Cuando solo se le pasa el PID la señal que se envía es la señal TERM (15).

El parámetro opcional para `kill` es `–n` donde n representa el número de señal.

La señal 9 es la forma brutal de finalizar un proceso. En lugar de pedirlo al proceso que se pare, el sistema operativo mata el proceso. La única vez que esto falla es cuando el proceso está en mitad de una llamada al sistema (como una petición de apertura de un archivo), en cuyo caso el proceso muere una vez que la llamada ha finalizado.

### 6.3.4.- Visualizando la memoria: `free`

Muestra la cantidad de total de memoria física y de intercambio presente en el sistema, así como la memoria compartida y los buffers utilizados por el núcleo.

Sus modificadores son:

| Modificador | Descripción |
| ----------- | ----------- |
- `-b|k|m` | Muestra la cantidad de memoria en bytes, kilobytes o megabytes |
- `-t`     | Muestra en una línea los totales |
- `-s seg` | Se actualiza cada seg segundos |


### 6.3.5.- Tiempo de marcha: `uptime`

Muestra una línea con la siguiente información: hora actual, cuánto tiempo lleva en marcha el sistema, el número de usuarios actualmente conectados, y la carga media del sistema en los últimos 1, 5 y 15 minutos.

El valor de **carga media** tiene la misma interpretación que tenía en el caso del comando `top`.


### 6.3.6.- Mostrar el nombre del sistema: `uname`

El comando uname imprime información acerca de la máquina y el sistema operativo en los que está corriendo. 
Los diferentes modificadores que tiene son:

| Modificador | Descripción |
| `-m`   | Escribe el tipo de hardware de la máquina |
| `-n`   | Escribe el nombre de la máquina |
| `-r`   | Escribe el número de versión del sistema operativo |
| `-s`   | Escribe el nombre del sistema operativo |
| `-v`   | Escribe la versión del sistema operativo |
| `-a`   | Escribe todo lo anterior |



## 6.4.- Envío de señales entre procesos (ESTO NO)

### 6.4.1.- Captura de señales con el comando `trap`

Ya hemos visto que la comunicación entre procesos en Linux se realiza mediante señales que pueden ser lanzadas con el comando kill. 
Desde los scripts que creamos, también es posible capturar las señales, bien para modificar su valor por defecto o bien para bloquearla e impedir que realice su función. El comando para capturar señales en un script es el comando trap.
 
Cada vez que se presiona Ctrl+C, la señal es atrapada y ejecuta la orden que se le ha pasado al comando trap. 
En lugar de atrapar una señal se puede capturar la salida del script, o la finalización de este, indicándolo con la palabra EXIT.
 
Otra posibilidad es eliminar una trampa para que el script no responda ante una determinada señal. Esto lo conseguimos poniendo dos guiones donde se indica la orden de la trampa.
 
Ten cuidado porque si ejecutas el script anterior entrará en un bucle infinito y no podrás finalizarlo con la combinación de teclas Ctrl+C (ya que estamos atrapando estas señales). La única forma de finalizarlo será enviando una señal diferente con el comando kill, por ejemplo, SIGKILL.
También se pueden capturar varias señales en la misma orden de la siguiente forma:
 
6.4.2.- EJECUCIÓN DE SCRIPT CON LA SESIÓN CERRADA
Cuando cerramos la sesión, el Shell envía la señal SIGHUP a todos los procesos que cuelgan bajo él. Esta señal hace que los procesos finalicen su ejecución, pero esto en ocasiones no es lo deseable si queremos que los scripts continúen ejecutándose más allá de la sesión del usuario. Para evitar esto se dispone del comando nohup.
La sintaxis es muy sencilla, simplemente se le debe pasar el comando exactamente igual que si lo hubiéramos ejecutado.
 
Una cuestión a tener en cuenta es la gestión que se realiza de la salida estándar y de error del comando ejecutado, ya que al no haber terminal no puede mostrarla por pantalla.
Para ello, si la salida no está redirigida, la redirige al fichero nohup.out del directorio actual, y si no tuviera permiso de escritura sobre el directorio actual, al directorio personal del usuario. La salida de error se redirige al mismo lugar que la salida estándar.
Información adicional en:
•	http://www.vicente-navarro.com/blog/2007/04/19/sobre-la-senal-sighup-nohup-disown-trap/
•	https://likegeeks.com/es/scripting-de-bash-en-linux-senales-y-trabajos/



## 6.5.- Programación de procesos con `cron` y `crontab`

El demonio `crond` permite a cualquier usuario de un sistema programar aplicaciones para ejecutar en cualquier fecha, hora o día de la semana. El uso del `cron` es una forma extremadamente eficiente de automatizar el sistema, generando informes en una base regular, y de realizar otras tareas periódicas.

Al igual que otros servicios `cron` se inicia mediante los scripts de arranque. Para localizarlos podemos ejecutar la orden:

```
$ ps auxw | grep cron | grep –v grep
```

El servicio `crond` trabaja despertándose cada minuto y comprobando el archivo crontab de cada usuario (directorio `/var/spool/cron`) así como el archivo `crontab` del sistema (`/etc/ crontab`). Este archivo contiene la lista de los eventos de los usuarios que quieren que se ejecuten en una fecha y hora en particular. Cualquier evento que coincida con la fecha y hora actual se ejecutará.


### 6.5.1.- El archivo `crontab`

La herramienta que permite editar entradas que ejecuta `cron` es `crontab`. Esencialmente todo lo que hace es verificar su permiso para modificar las configuraciones de `cron` y después invoca un editor de texto para que pueda realizar sus cambios.

Las tareas `cron` se definen en una serie de scripts dentro de los directorios definidos en el archivo `/etc/crontab`. Las listas de directorios dentro de este archivo tienen el siguiente aspecto:
 
```
vgonzalez@ubuntu:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

No se debería modificar este archivo, pero vamos a ver las partes que tiene. Las primeras líneas indican los valores de las variables del *Shell*.

A continuación, hay cuatro líneas con una serie de columnas cada una, cada línea corresponde con una tarea programada. Los significados de las columnas son:

•	Las cinco primeras columnas representan los minutos, horas, día del mes, mes y día de la semana.
 
Hay diferentes formas de indicar estos valores:
o	Si contiene un valor numérico indicará el momento exacto. Por ejemplo, la última fila tiene una tarea que se ejecutará a las 5:52 horas.
Una alternativa en los días de la semana y mes es utilizar abreviaturas de la forma sun o sug.
o	Si contiene el símbolo asterisco indica que ese campo tomará todos los valores posibles. Por ejemplo, en la misma fila que antes (la última), al tener un asterisco en el campo mes indica que se ejecutará todos los meses. En cambio, como en el campo día del mes tiene un valor numérico, solo se ejecutará ese día concreto. Por tanto, esta tarea se ejecutará el primer día de cada mes todos los meses.
o	Se pueden poner una serie de valores separados por comas, en cuyo caso la tarea se ejecutará para cada uno de los valores indicados.
o	Otra opción es indicar días alternos poniéndolo de la forma */num, esto indicará que la tarea se ejecutará cuando el valor de ese campo sea múltiplo exacto de num.
o	Otra opción es indicar rangos separando los dos valores con un guión (por ejemplo, 5-10).
o	Por último, se pueden utilizar una serie de palabras reservadas. Estas son:
	@reboot: se ejecuta una única vez al inicio.
	@yearly/@annually: ejecutar cada año.
	@monthly: ejecutar una vez al mes.
	@weekly: una vez a la semana.
	@daily/@midnight: una vez al día.
	@hourly: cada hora.
•	La siguiente columna indica el usuario en cuyo nombre se ejecutará el script.
•	La última columna indica la tarea que se va a ejecutar. En este ejemplo concreto utiliza el comando run-parts. Este es un comando especial que ejecutará cualquier script ejecutable dentro del directorio especificado, en el último ejemplo sería el directorio /etc/cron.monthly.
6.5.2.-CREACIÓN DE TAREAS POR PARTE DEL USUARIO
Si analizas el fichero crontab anterior, verás que al final lo que hace es programar una serie de tareas con periodicidades fijas: cada hora, diaria, semanal, … de forma que cualquier script que se guarde en uno de los directorios indicados se ejecutará con dicha periodicidad. Por ejemplo, todo lo que se guarde en /etc/cron/weekly se ejecutará una vez cada semana. 
Eso es una tarea propia del administrador, pero los usuarios normales también pueden crear tareas de cron. Para crear y modificar una crontab utilizaremos el comando crontab –e. Si aún no existe una crontab para el usuario, este comando creará un archivo crontab en /var/spool/cron/username.
La sintaxis utilizada en los trabajos cron de los usuarios es idéntica a la del archivo crontab del sistema que vimos anteriormente, con una diferencia: no podemos indicar el nombre del usuario, ya que la tarea pertenecerá siempre al usuario que la cree.
Para crear una tarea cron debemos invocar la orden crontab con el modificador –e. Eso nos abrirá un editor donde podremos añadir nuestra línea. La primera vez que lo invoquemos nos pedirá un editor por defecto 
 
Y tras ello ya podremos introducir datos en nuestro fichero crontab.
 
Algunas otras tareas que podemos realizar con crontab son:
•	crontab –l: muestra nuestro fichero crontab para ver todas las tareas programadas.
•	crontab –u user –l: si somos administradores podremos ver las tareas programadas de cualquier usuario del sistema.
•	crontab –r: elimina todas las tareas de crontab. Esto hace que se elimine el fichero /var/spool/cron/user.

Por parte del administrador, es posible restringir los usuarios que pueden programar tareas en el sistema. Esto se hace mediante dos ficheros: cron.allow y cron.deny. El contenido de estos ficheros es muy sencillo, únicamente tienen una lista de nombres de usuario, uno por línea.
Funcionan de la siguiente manera:
•	Si cron.allow existe, solo los usuarios que están listados en dicho fichero pueden crear, editar, mostrar o eliminar ficheros crontab.
•	Si cron.allow no existe, todos los usuario pueden usar ficheros crontab con excepción de los que están incluidos en el fichero cron.deny.
•	Si no existen ni cron.allow ni cron.deny, entonces cualquier usuario puede programar tareas.



 