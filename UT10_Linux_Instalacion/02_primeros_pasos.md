<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introduccion.md)
2. [**Primeros pasos con Linux**](02_primeros_pasos.md)
3. [El sistema de ficheros](03_sistema_ficheros.md)
4. [Trabajando con datos textuales](04_comandos_texto.md)
5. [Expresiones regulares con el comando `sed`](05_expresiones_regulares.md)


# 2.- PRIMEROS PASOS EN LINUX

## 2.1.- Introducción a la línea de comandos

Dado que toda la interacción que realizaremos con el sistema operativo se realizará a través de la línea de comandos, es muy importante tener muy clara la **sintaxis** de los comandos en Linux, así como algunas normas básicas al introducir las órdenes en la terminal, ya que el cambio de un único carácter en los parámetros de un comando puede alterar por completo el comportamiento del mismo.

Los comandos en Linux tienen la siguiente sintaxis:

```bash
$ comando arg1 .. argN mod1 mod2 …
```

La primera palabra debe ser el comando a ejecutar, tras el que se indican los **argumentos** si los tiene, y los **modificadores**. Hay que tener en cuenta lo siguiente:

- Los **argumentos** suelen indicar los datos con los que trabaja el comando. Los **modificadores**, en cambio, alterar el funcionamiento habitual del comando.
- Linux es lo que se denomina *case sensitive*, es decir, que **distingue entre mayúsculas y minúsculas**.
- El separador de argumentos y de modificadores es el espacio, por lo que si un argumento contiene espacios debe rodearse de comillas, ya sean simples o dobles.
- El orden de los modificadores es indiferente, el de los argumentos debe ser el especificado en la sintaxis del comando.
- Los modificadores se pueden ver en la ayuda del comando, y tienen dos formas diferentes de indicarse:
  - Con una única letra, en cuyo caso se indican anteponiendo el símbolo guion (`-`)
  - Con una palabra, en cuyo caso se indican anteponiendo dos guiones (`--`)
- Se pueden combinar varios modificadores de una letra poniendo todos seguidos precedidos de un único guion. Por ejemplo, las órdenes `ls -l -a` y `ls -la` son equivalentes.
- Si un comando es muy largo se puede indicar en varias líneas. También se puede forzar el salto de línea poniendo el carácter barra invertida (`\`).
- Se pueden indicar varios comandos en la misma línea separándolos con el carácter punto y coma (`;`)
- La tecla  `Tab`  sirve para autocompletar la palabra que estamos escribiendo, ya sea un comando o un argumento. Al pulsar esta tecla completará la palabra actual a partir de los caracteres que ya hayamos introducido. Si hubiera varios que coinciden con los caracteres ya introducidos no mostrará nada y debemos pulsar el tabulador una segunda vez para que se muestren todas las posibilidades.
- Con los cursores arriba y abajo se puede navegar entre los comandos que hemos introducido previamente.
- Asimismo, se puede ver el listado de estos comandos con el comando `history`.
- También se puede buscar en el historial con la combinación de teclas  `Ctrl+R`.
- Por defecto, cualquier orden introducida se ejecuta sin privilegios de administrador. Para ejecutar una orden con privilegios de administrador hay que ejecutarla anteponiéndole el comando `sudo`. Algo importante a tener encuenta es que, en este caso, el usuario que ejecutará la orden no será el nuestro, sino que será el usuario `root`, con todas las consecuencias que ello tiene (por ejemplo, `root` será el propietario de los ficheros que se creen).


## 2.2.- Primeros comandos de Linux

### 2.2.1.- El intérprete de comandos: bash, sh, fish, ...

El **intérprete de comandos** es el programa que se encarga de interpretar las órdenes que introducimos en la terminal. El intérprete de comandos que utiliza Ubuntu Linux, y por tanto el que utilizaremos este curso, es **Bash (GNU Bourne Again Shell)**, que es el intérprete más extendido y popular. Sin embargo, la gran flexibilidad de Linux permite la utilización de otros intérpretes simplemente instalándolos y ejecutándolos. Se escapa del ámbito de este curso, pero algunos de estos intérpretes son:

- **The Bourne Shell (sh)**: es el predecesor de Bash y tiene su origen en las grandes supercomputadores que utilizaban UNIX en los años 70s. 
- **Korn Shell (ksh)**: otro *shell* similar a Bash con algunas mejoras respecto a éste.
- **tcsh**: es el intérprete que viene por defecto en los sistemas basados en BSD, como FreeBSD. Es heredero del *shell* original de UNIX **C Shell (csh)** y uno de sus puntos fuertes es el lenguaje de scripting, muy similar al lenguaje C.
- **Friendly Interactive Shell (fish)**: un *shell* reciente, del año 2005, que se esfuerza en ser amigable para el usuario. Tienen múltiples funcionalidades en este sentido, como el coloreado en rojo de la sintaxis errónea o las sugerencias completadas codificadas por colores y basadas en el historia.

### 2.2.2.- Cerrando la sesión. Comando `exit`

Cada vez que el usuario inicia sesión en el ordenador y se le presenta el *prompt* está interactuando con el intérprete de comandos que tenga por defecto independientemente de que se conecte en local o en remoto a través de `ssh`. 

El comando para cerrar la sesión en ambos casos es `exit`, que no tienen ningún parámetro, y que cerrará la sesión que tengamos abierta en ese momento.

### 2.2.3.- Cerrando el sistema. Comando `shutdown`.

El comando `shutdown` sirve para planificar el apagado de la máquina, que puede ser de tres tipos:

- **Parada (*halt*)**: el sistema operativo se cierra deteniendo todas las funcioens en la CPU, pero la fuente de alimentación sigue suministrando energía.
- **Detención (*poweroff*)**: el sistema se apaga completamente
- **Reinicio (*reboot*)**: se produce un reinicio del sistema.

Aparte del tipo de apagado, se le pasa como parámetro la hora en que se realizará el apagado, o bien `now` para indicar que el sistema se apagará inmediatamente. Por defecto, se mostrará un mensaje a todos los usuarios que estén logados en el sistema para advertirles del apagado del sistema.

#### Modificadores del comando `shutdown`

| Modificador       | Descripción                                                             |
| ----------------- | ----------------------------------------------------------------------- |
| `-H`, `--halt`    | Realiza una parada del sistema    |
| `-P`, `-poweroff` | Detiene el sistema (opción por defecto) |
| `r`, `-reboot`    | Reinicia el sistema |
| `--no-wall`       | No envía ningún mensaje al resto de usuarios |
| `-c`              | Cancela un apagado pendiente |

#### Ejemplos de uso

En el siguiente código puedes ver algunos ejemplos de uso del comando.

```bash
    $  shutdown now         # Detiene la máquina inmediatamente
    $  shutdown 13:20       # Programa la máquina para detenerse a las 13:20 h.
    $  shutdown -p now	    # Realiza una parada del sistema
    $  shutdown -r09:35	    # Programa el reinicio del sistemas para las 09:35 h.
```

### 2.2.4.- Otras formas de cerrar el sistema. Comandos `halt`, `poweroff` y `reboot`

Cuando se quiere detener el sistema inmediatamente sin necesidad de programar el apagado hay otros tres comandos equivalentes a `shutdown`:

- `halt`: realiza una parada del sistema
- `poweroff`: realiza un apagado total del sistema
- `reboot`: realiza un reinicio del sistema.

Como curiosidad, estos comandos son intercambiables mediante modificadores, pudiendo, por ejemplo, realizar un apagado total del sistema con la orden `halt -p` o un reinicio con `halt --reboot`, al igual que se puede realizar una parada con `reboot --halt`.


### 2.2.5.- ¿Quén soy? Comando `whoami`

Siempre que estamos interactuando en Linux a través de una terminal lo hacemos a través de una credenciales definidas por nuestro nombre de usuario y su contraseña, de forma que los límites de seguridad que se nos impondrán serán los de nuestro usuario. Por norma general, el nombre de nuestro usuario se muestra en el **prompt**, pero habrá ocasiones en que no sea así por lo que puede que en algún momento dudemos de cuál es el usuario con el que estamos trabajando, sobre todo si estamos trabajando con diversas sesiones en remoto.

En estos casos, si necesitamos saber nuestro nombre de usuario únicamente debemos ejecutar la orden `whoami`.

```bash
victor@ubuntu:~$ whoami
victor
victor@ubuntu:~$ sh
$ whoami
victor
```

### 2.2.6.- ¿Cómo se llama mi máquina? Comando `hostname`

Análogo al ejemplo anterior es el caso del nombre del equipo. Este se muestra habitualmente en el *prompt*, por lo que siempre está a la vista, pero habrá ocasiones en que no sea así y dudemos de la máquina en la que estamos introduciendo órdenes. 

En este caso, el comando para imprimir el nombre de la máquina que estamos utilizando es `hostname`.

```bash
victor@ubuntu:~$ hostname
ubuntu
```

### 2.2.7.- ¿Quiénes están? Comando `who`

Linux es un **sistema multiusuario** lo que implica que puede haber múltiples usuarios conectados simultáneamente. Un ejemplo es cuando varios usuarios se conectan mediante `ssh` a la misma máquina desde diferentes ubicaciones.

Otra posibilidad es tener abiertas varias sesiones en local es a través de las sesiones de TTY. Para alternar entre ellas simplemente hay que utilizar la combinación de teclas `Ctrl+Alt+Fn`, siendo `Fn` una de las teclas de función.

Independientemente de la forma en que se conecten, es posible saber qué usuarios están conectados en un momento determinado en el sistema utilizando el comando `who`.

```bash
       vgonzalez@ubuntu:~$ who
       paco     tty1         2022-03-15 08:57
       vgonzalez pts/0        2022-03-15 08:50 (10.0.0.101)
       pepe     pts/1        2022-03-15 08:51 (10.0.0.101)
```

Por ejemplo, en el código anterior el usuario `paco` ha iniciado sesión desde la primera terminal (`tty1`), mientras que los usuarios `vgonzalez` y `pepe` lo están haciendo mediante una conexión remota desde el equipo con IP `10.0.0.101`.


### 2.2.8.- ¿Qué kernel tengo? Comando `uname`

El comando `uname` es muy interesante para extraer información sobre el kernel de la máquina con la que estoy trabajando. Tiene un gran número de modificadores para determinar qué tipo de información quiero conocer.

#### Modificadores del comando `uname`

| Modificador       | Descripción                                                             |
| ----------------- | ----------------------------------------------------------------------- |
| `-r`              | Versión del kernel  | 
| `-n`              | Nombre del equipo (equivalente a `hostname`)  |
| `-m`              | Arquitectura del procesador  |
| `-o`              | Sistema operativo  |
| `-a`              | Toda la información  |

#### Ejemplos

```bash
victor@ubuntu:~$ uname -a
Linux ubuntu 5.10.60.1-microsoft-standard-WSL2 #1 SMP Wed Aug 25 23:20:18 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```


### 2.2.9.- La ayuda en Linux. Comando `man` y `whatis`

Linux dispone de un gran número de comandos y muchos con una sintaxis bastante compleja y con muchas opciones, por lo que habitualmente será necesario acceder a la ayuda. Afortunadamente, la ayuda en Linux es muy extensa y accesible.

El principal comando para consultar la ayuda en `man`, que invoca la página del manual de Linux del comando que se le pase como argumento. Por ejemplo, en la siguiente imagen se muestra la página del manual del comando `cp`.
 
 ```
CP(1)                                               User Commands                                               CP(1)

NAME
       cp - copy files and directories

SYNOPSIS
       cp [OPTION]... [-T] SOURCE DEST
       cp [OPTION]... SOURCE... DIRECTORY
       cp [OPTION]... -t DIRECTORY SOURCE...

DESCRIPTION
       Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --archive
              same as -dR --preserve=all

       --attributes-only
              don't copy the file data, just the attributes

       --backup[=CONTROL]
              make a backup of each existing destination file

       -b     like --backup but does not accept an argument

       --copy-contents
              copy contents of special files when recursive

 Manual page cp(1) line 1 (press h for help or q to quit)
 ```

Otro comando muy similar para obtener la ayuda es el comando `info`, que muestra la ayuda de una forma muy similar al comando `man`.

Si solo queremos saber qué hace un determinado comando disponemos de la orden `whatis`, que mostrará únicamente la función realizada por el comando pasado como parámetro.
 
 ```bash
┌──(victor㉿PORTATIL)-[/mnt/c/Users/victor]
└─$ whatis cp
cp (1)               - copy files and directories
 ```


## 2.3.- Algunos comandos básicos de Linux

### 2.3.1.- ¿Dónde están mis comandos? Comando `which`

Cuando introducimos un comando en Linux estamos simplemente invocando la ejecución de un programa cuyo nombre es el del comando. Estos programas se encuentran en el sistema de ficheros, habitualmente en el directorio `/usr/bin`, aunque no tiene por qué ser así.

Si queremos saber en qué directorio se encuentra el programa que corresponde a un comando solamente hemos de utilizar el comando `which`.

```bash
victor@ubuntu:~$ ls -l /usr/bin/bash
-rwxr-xr-x 1 root root 1183448 Feb 25  2020 /usr/bin/bash
```

### 2.3.2.- Comando `echo`

El comando `echo` es un comando muy sencillo pero que es sorpredentemente útil en muchas situaciones. Lo único que hace en mostrar por pantalla el texto que se le pase como parámetro.

```bash
       vgonzalez@ubuntu:~$ echo "hola mundo"
       hola mundo
```

#### Modificadores del comando `echo`

| Modificador       | Descripción                                                             |
| ----------------- | ----------------------------------------------------------------------- |
| `-n`              | No imprime el salto de línea del final   |
| `-e`              | Habilita la interpretación de los caracteres de escape (`\`) |


#### Ejemplos de uso

Por defecto, `echo` finalizará todas las salidas con un salto de línea. Para evitar este comportamiento simplemente hay que utilizar el modificador `-n`.

```bash
       vgonzalez@ubuntu:~$ echo -n "hola mundo"
       hola mundovgonzalez@ubuntu:~$
```

En algunas ocasiones, querremos mostrar o enviar a la salida de `echo` caracerteres no imprimibles como pueden ser saltos de línea o tabulados. Todos estos caracteres se representan con el carácter de escape, que es la barra invertida ( `\`) seguido de otro carácter, por ejemplo, `\n` para representar un salto de línea.

Si queremos utilizar caracteres escapados con el comando `echo` hay que indicarlo con el modificador `-e`.

```bash
       vgonzalez@ubuntu:~$ echo -e "hola \n mundo"
       hola
       mundo
       vgonzalez@ubuntu:~$ echo "hola \n mundo"
       hola \n mundo
```

Algunos de los caracteres escapados más comunes se muestran en la siguiente tabla.

| Carácter    | Valor                     |
| ----------- | ------------------------- |
| `\\`        | Barra invertida           |
| `\n`        | Salto de línea            |
| `\t`        | Tabulado                  |
| `\a`        | Alerta (emite un sonido)  |


### 2.3.3.- Variable `$PATH` y `./`

Cuando hablamos del comando `which`  vimos que cada comando de Linux se corresponde con un programa que se encuentra alojado en algún directorio de nuestro disco duro, por ejemplo, `bash` se encuentra en `/usr/bin`
En el punto anterior hemos visto que cada comando de Linux se corresponde con un fichero que se encuentra almacenado en algún lugar del disco duro, por ejemplo, el programa correspondiente a `bash` se encuentra en `/usr/bin`.

De este hecho surge una pregunta, ¿cómo sabe el sistema en qué directorio se encuentra cada programa? La respuesta es sencilla, no lo sabe. Cada vez que invocamos un comando busca en una serie de directorios donde se espera que estén los ejecutables hasta que encuentra un programa cuyo nombre coincide con el comando que hemos introducido.

Este listado de directorios no está prefijado, sino que hay una variable de entorno, llamada `$PATH` que contiene el listado de directorios en los que tiene que buscar un ejecutable.

```bash
       vgonzalez@ubuntu:/opt$ echo $PATH
       /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Observa en la salida que se indican varios directorios separados por el carácter dos puntos (`:`), de forma que irá buscando consecutivamente en cada uno de ellos hasta que encuentre el comando buscado.

Todo lo anterior implica que únicamente se pueden ejecutar programas que se encuentran dentro del *path*. Pero, ¿qué pasa si se quiere ejecutar un comando que se encuentra en un directorio que no está en el *path*? En ese caso hay que indicar expresamente al sistema dónde se encuentra dicho programa, lo cual se hace anteponiendo la ruta al nombre del ejecutable.

```bash
       vgonzalez@ubuntu:~$ /scripts/saluda
       Hola mundo
```

Si el ejecutable se encuentra en el directorio de trabajo en el que se encuentra el usuario se puede indicar la ruta mediante una ruta relativa de la forma `./`.

```bash
       vgonzalez@ubuntu:~$ ./saluda
       Hola mundo
```

### 2.3.4.- Variables de entorno. Comandos `printenv` y `export`

Lo que vimos en el punto anterior (`$PATH`), es lo que se llama una **variable de entorno** y no es la única que hay en el sistema. Las variables de entorno almacenan información relativa al sistema que puede ser accedida por los programas. Podemos ver todas las variables de entorno mediante el comando `printenv`.

```bash
       vgonzalez@ubuntu:/opt$ printenv
       SHELL=/bin/bash
       PWD=/opt
       LOGNAME=vgonzalez
       XDG_SESSION_TYPE=tty
       MOTD_SHOWN=pam
       HOME=/home/vgonzalez
```

Cada variable se identifica por un nombre que por norma general se escribe en mayúsculas (aunque no es obligatorio). Para ver el valor de una variable de entorno simplemente hay que indicar el nombre de la misma precediéndolo del carácter dólar (`$`).

```bash
       vgonzalez@ubuntu:/opt$ echo $SHELL
       /bin/bash
```

En cualquier sitio donde pongamos una variable de entorno la reemplazará automáticamente por el valor que contiene.

```bash
       vgonzalez@ubuntu:/$ echo "Tu nombre de usuario es" $USER.
       Tu nombre de usuario es vgonzalez.
```

Si se desea crear una nueva variable de entorno o modificar una existente hay que utilizar el comando `export` con el nombre de la variable y el valor que se le quiere adjudicar.

```bash
       vgonzalez@ubuntu:/opt$ export MSG="Hola mundo"
       vgonzalez@ubuntu:/opt$ echo $MSG
       Hola mundo
       vgonzalez@ubuntu:/opt$ echo $PATH
       /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
       vgonzalez@ubuntu:/opt$ export PATH=$PATH":/scripts"
       vgonzalez@ubuntu:/opt$ echo $PATH
       /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/scripts
```


### 2.3.5.- Quiero ser root. Comando `sudo`

Linux es un sistema que incide especialmente en la seguridad, y, entre otras cosas, se traduce en una distinción muy clara entre las tareas de un usuario normal y las tareas de administración. Cuando en Linux se quiere realizar una tarea que requiera privilegios de administración hay que indicarlo explícitamente anteponiendo el comando `sudo` al comando que querramos ejecutar como administrador.

Cuando hacemos esto nuestro usuario pasará a ser `root`, que es el único usuario del sistema que puede realizar tareas administrativas. 

```bash
       vgonzalez@ubuntu:~$ sudo whoami
       [sudo] password for vgonzalez:
       root
```

Esto tiene una serie de connotaciones que obligan a que haya que prestar mucha atención siempre que se ejecute un comando con `sudo`. Por ejemplo, si creo un fichero con `sudo` será `root` el propietario de dicho fichero por lo que solamente `root` podrá editarlo.


### 2.3.6.- Quiero ser cualquier otro. Comando `su`

El comanod `su` permite iniciar un **subshell** con otro usuario sin necesidad de cerrar la sesión actual. Su sintaxis es muy sencilla, si no se pasa ningún parámetro se inicia sesión con el usuario `root` mientras que si se le pasa el nombre de un usuario como parámetro se abrirá un intérprete con ese usuario,

```bash
       vgonzalez@ubuntu:~$ whoami
       vgonzalez
       vgonzalez@ubuntu:~$ su pepe
       Password:
       pepe@ubuntu:/home/vgonzalez$ whoami
       pepe
```


### 2.3.8.- Comando `wget`

La orden `wget` es un comando que no tiene relación con los que hemos visto hasta ahora, pero que puede ser útil en múltiples ocasiones. Su función es descargar archivos de Internet, lo cual viene muy bien cuando estamos utilizando una terminal de texto donde no hay acceso a un navegador.

Las sintaxis básica es muy sencilla, debiendo indicar como parámetro la URL del fichero que se quiere descargar.

```bash
       vgonzalez@ubuntu:~$ wget https://releases.ubuntu.com/20.04.4/ubuntu-20.04.4-live-server-amd64.iso
       --2022-03-15 10:25:29--  https://releases.ubuntu.com/20.04.4/ubuntu-20.04.4-live-server-amd64.iso
       Resolving releases.ubuntu.com (releases.ubuntu.com)... 91.189.88.248, 91.189.88.247, 91.189.91.123, ...
       Connecting to releases.ubuntu.com (releases.ubuntu.com)|91.189.88.248|:443... connected.
       HTTP request sent, awaiting response... 200 OK
       Length: 1331691520 (1,2G) [application/x-iso9660-image]
       Saving to: ‘ubuntu-20.04.4-live-server-amd64.iso’

       ubuntu-20.04.4-live-server-a   0%[                                                  ]   6,30M  2,12MB/s 
```


#### Modificadores del comando `wget`

| Modificador        | Descripción                                                             |
| ------------------ | ----------------------------------------------------------------------- |
| `-O <name>`        | Guarda el fichero con el nombre indicado |
| `-P <path>`        | Guarda el fichero descargado en otra ruta |
| `-b`               | Realiza la descarga en segundo planp |
| `--ftp-user=<username>` | Nombre de usuario para descargar desde un servidor FTP |
| `--ftp-password=<pass>` | Contraseña para descargar desde un servidor FTP |





***
[Volver al índice principal](index_UT10.md)