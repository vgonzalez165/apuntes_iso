---
layout: default
title: Primeros pasos en Linux
parent: UT09. Linux. Instalación y primeros pasos
nav_order: 2
---

# 2.- Primeros pasos en Linux

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


## 2.2.- Consideraciones preliminares

Antes de comenzar con Bash vamos a ver algunas tareas que debemos conocer para interactuar con nuestro sistema una vez que ha sido instalado.


### 2.2.1.- Configuración de red

La configuración de red en Ubuntu Server 22.04 se realiza mediante la herramienta **Netplan**, que reemplaza al antiguo NetworkManager. Para saber la configuración de red que tenemos en nuestro equipo debemos utilizar la orden `ip`.

```
victor@ubuntu:~$ ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:01:66:1d brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.1/8 brd 10.255.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fe01:661d/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:01:66:1e brd ff:ff:ff:ff:ff:ff
    inet 172.28.114.218/20 metric 100 brd 172.28.127.255 scope global dynamic eth1
       valid_lft 83503sec preferred_lft 83503sec
    inet6 fe80::215:5dff:fe01:661e/64 scope link
       valid_lft forever preferred_lft forever
```

En la salida de este comando podemos destacar:

- Se enumeran todos los adaptadores de red que tenemos en el equipo, representados cada uno por su identificador único. En la salida anterior se puede ver la **interfaz de bucle local** representada por **lo** y dos adaptadores de red cableados identificados por `eth0` y `eth1`.
- Para cada adaptador se muestra información entre la que destaca la dirección MAC y las direcciones IPv4 e IPv6.


### 2.2.2.- Instalación de software

El enfoque de la instalación de apliciones en Linux difiere enormemente de la forma en que se hace en Windows. Mientras que en Windows se obtienen las apliaciones de diversas fuentes y se instalan manualmente, en Linux se centraliza todo el software disponible en **repositorios**, de forma que la adquisición de software se realiza automáticamente.

El comando para gestionar el software instalado en Ubuntu es `apt`, siendo algunas de las opciones más comunes las siguientes:

- **`sudo apt update`**: actualiza en el equipo local el listado de software disponible en el repositorio, así como su versión. Siempre que realicemos cualquier operación con `apt` debemos ejecutar primero esta orden. Este comando debemos ejecutarlo como administrador, lo que se indica precediéndolo de la orden `sudo`.
- **`sudo apt upgrade`**: compara la versión de los paquetes instalados en nuestro sistema con la última versión disponible en el repositorio, y lo actualiza si esta última es más reciente.
- **`apt search nombre_paquete`**: devuelve un listado de paquetes disponibles en el repositorio cuyo nombre coincide con el indicado.
- **`sudo apt install nombre_paquete`**: instala el paquete cuyo nombre se ha indicado. Hay que tener en cuenta que el paquete que indiquemos puede tener **dependencias**, es decir, otros paquetes que necesite para funcionar, por lo que se nos avisará y nos permitirá instalarlos.


### 2.2.3.- Conexión remota con SSH

**SSH** (Secure Shell) es un protocolo que permite acceder de forma **remota** y **segura** a otra máquina Linux. Veremos todas las opciones de este protocolo en detalle en la próxima unidad, pero ahora adelantaremos la forma de conectarnos a una máquina utilizando este protocolo.

El primer requisito para realizar una conexión SSH es que el equipo al que nos queramos conectar tenga instalado el servidor. Durante la instalación de Ubuntu Server se nos da la posibilidad de instalarlo, pero, si no se ha hecho en ese momento, se podrá hacer instalando el paquete `openssh-server`.

Podemos comprobar si está instalado verificando que el servicio correspondiente esté en ejecución:

```
victor@ubuntu:~$ service sshd status
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2023-03-23 09:02:43 UTC; 28min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 1341 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
...
```

Si no estuviera instalado podemos hacerlo instalando el paquete `openssh-server`.

Una vez instalado solo nos queda conectarnos desde cualquier otro equipo utilizando el cliente `ssh`, disponible tanto en Powershell como en cualquier terminal de Linux. A este comando hay que pasarle como parámetro la dirección IP del equipo al que nos queremos conectar y el nombre de usuario con el que queremos indicar la sesión remota.

```
PS C:\Users\Victor> ssh victor@10.0.0.1
victor@10.0.0.1's password:
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-67-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of jue 23 mar 2023 10:17:39 UTC

  System load:  0.0                Processes:             101
  Usage of /:   5.5% of 123.41GB   Users logged in:       1
  Memory usage: 7%                 IPv4 address for eth0: 10.0.0.1
  Swap usage:   0%                 IPv4 address for eth1: 172.28.114.218
...
Last login: Thu Mar 23 09:04:19 2023 from 10.0.0.2
victor@ubuntu:~$
```

En cualquier momento podremos cerrar la sesión remota utilizando el comando `exit`.


## 2.3.- Primeros comandos de Linux

### 2.3.1.- El intérprete de comandos: bash, sh, fish, ...

El **intérprete de comandos** es el programa que se encarga de interpretar las órdenes que introducimos en la terminal. El intérprete de comandos que utiliza Ubuntu Linux, y por tanto el que utilizaremos este curso, es **Bash (GNU Bourne Again Shell)**, que es el intérprete más extendido y popular. Sin embargo, la gran flexibilidad de Linux permite la utilización de otros intérpretes simplemente instalándolos y ejecutándolos. Se escapa del ámbito de este curso, pero algunos de estos intérpretes son:

- **The Bourne Shell (sh)**: es el predecesor de Bash y tiene su origen en las grandes supercomputadores que utilizaban UNIX en los años 70s. 
- **Korn Shell (ksh)**: otro *shell* similar a Bash con algunas mejoras respecto a éste.
- **tcsh**: es el intérprete que viene por defecto en los sistemas basados en BSD, como FreeBSD. Es heredero del *shell* original de UNIX **C Shell (csh)** y uno de sus puntos fuertes es el lenguaje de scripting, muy similar al lenguaje C.
- **Friendly Interactive Shell (fish)**: un *shell* reciente, del año 2005, que se esfuerza en ser amigable para el usuario. Tienen múltiples funcionalidades en este sentido, como el coloreado en rojo de la sintaxis errónea o las sugerencias completadas codificadas por colores y basadas en el historia.

### 2.3.2.- Cerrando la sesión. Comando `exit`

Cada vez que el usuario inicia sesión en el ordenador y se le presenta el *prompt* está interactuando con el intérprete de comandos que tenga por defecto independientemente de que se conecte en local o en remoto a través de `ssh`. 

El comando para cerrar la sesión en ambos casos es `exit`, que no tienen ningún parámetro, y que cerrará la sesión que tengamos abierta en ese momento.

### 2.3.3.- Cerrando el sistema. Comando `shutdown`.

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

### 2.3.4.- Otras formas de cerrar el sistema. Comandos `halt`, `poweroff` y `reboot`

Cuando se quiere detener el sistema inmediatamente sin necesidad de programar el apagado hay otros tres comandos equivalentes a `shutdown`:

- `halt`: realiza una parada del sistema
- `poweroff`: realiza un apagado total del sistema
- `reboot`: realiza un reinicio del sistema.

Como curiosidad, estos comandos son intercambiables mediante modificadores, pudiendo, por ejemplo, realizar un apagado total del sistema con la orden `halt -p` o un reinicio con `halt --reboot`, al igual que se puede realizar una parada con `reboot --halt`.


### 2.3.5.- ¿Quén soy? Comando `whoami`

Siempre que estamos interactuando en Linux a través de una terminal lo hacemos a través de una credenciales definidas por nuestro nombre de usuario y su contraseña, de forma que los límites de seguridad que se nos impondrán serán los de nuestro usuario. Por norma general, el nombre de nuestro usuario se muestra en el **prompt**, pero habrá ocasiones en que no sea así por lo que puede que en algún momento dudemos de cuál es el usuario con el que estamos trabajando, sobre todo si estamos trabajando con diversas sesiones en remoto.

En estos casos, si necesitamos saber nuestro nombre de usuario únicamente debemos ejecutar la orden `whoami`.

```bash
victor@ubuntu:~$ whoami
victor
victor@ubuntu:~$ sh
$ whoami
victor
```

### 2.3.6.- ¿Cómo se llama mi máquina? Comando `hostname`

Análogo al ejemplo anterior es el caso del nombre del equipo. Este se muestra habitualmente en el *prompt*, por lo que siempre está a la vista, pero habrá ocasiones en que no sea así y dudemos de la máquina en la que estamos introduciendo órdenes. 

En este caso, el comando para imprimir el nombre de la máquina que estamos utilizando es `hostname`.

```bash
victor@ubuntu:~$ hostname
ubuntu
```

### 2.3.7.- ¿Quiénes están? Comando `who`

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


### 2.3.8.- ¿Qué kernel tengo? Comando `uname`

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


### 2.3.9.- La ayuda en Linux. Comando `man` y `whatis`

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


## 2.4.- Algunos comandos básicos de Linux

### 2.4.1.- ¿Dónde están mis comandos? Comando `which`

Cuando introducimos un comando en Linux estamos simplemente invocando la ejecución de un programa cuyo nombre es el del comando. Estos programas se encuentran en el sistema de ficheros, habitualmente en el directorio `/usr/bin`, aunque no tiene por qué ser así.

Si queremos saber en qué directorio se encuentra el programa que corresponde a un comando solamente hemos de utilizar el comando `which`.

```bash
victor@ubuntu:~$ ls -l /usr/bin/bash
-rwxr-xr-x 1 root root 1183448 Feb 25  2020 /usr/bin/bash
```

### 2.4.2.- Comando `echo`

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


### 2.4.3.- Variable `$PATH` y `./`

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

### 2.4.4.- Variables de entorno. Comandos `printenv` y `export`

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


### 2.4.5.- Quiero ser root. Comando `sudo`

Linux es un sistema que incide especialmente en la seguridad, y, entre otras cosas, se traduce en una distinción muy clara entre las tareas de un usuario normal y las tareas de administración. Cuando en Linux se quiere realizar una tarea que requiera privilegios de administración hay que indicarlo explícitamente anteponiendo el comando `sudo` al comando que querramos ejecutar como administrador.

Cuando hacemos esto nuestro usuario pasará a ser `root`, que es el único usuario del sistema que puede realizar tareas administrativas. 

```bash
       vgonzalez@ubuntu:~$ sudo whoami
       [sudo] password for vgonzalez:
       root
```

Esto tiene una serie de connotaciones que obligan a que haya que prestar mucha atención siempre que se ejecute un comando con `sudo`. Por ejemplo, si creo un fichero con `sudo` será `root` el propietario de dicho fichero por lo que solamente `root` podrá editarlo.


### 2.4.6.- Quiero ser cualquier otro. Comando `su`

El comanod `su` permite iniciar un **subshell** con otro usuario sin necesidad de cerrar la sesión actual. Su sintaxis es muy sencilla, si no se pasa ningún parámetro se inicia sesión con el usuario `root` mientras que si se le pasa el nombre de un usuario como parámetro se abrirá un intérprete con ese usuario,

```bash
       vgonzalez@ubuntu:~$ whoami
       vgonzalez
       vgonzalez@ubuntu:~$ su pepe
       Password:
       pepe@ubuntu:/home/vgonzalez$ whoami
       pepe
```


### 2.4.7.- Comando `wget`

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