<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introducción_linux.md)
2. [Instalación de Linux](02_instalación_linux.md)
3. [El sistema de ficheros en Linux](03_sistema_ficheros_linux.md)
4. [Comandos para el sistema de ficheros](04_comandos_sistema_ficheros.md)
5. [**Comandos avanzados del shell Bash**](05_avanzados.md)
6. [Expresiones regulares](06_expresiones_regulares.md)


# 5.- COMANDOS AVANZADOS DE BASH

## 5.1.- Gestión de las unidades de almancenamiento

Una parte muy importante de la administración de un sistema Linux es llevar el control de las diferentes unidades de almacenamiento así como de los sistemas de ficheros. Para ello dispone de un gran número de comandos que nos permitirán operar con estas unidades, así como extraer información relativa a ellas.

### 5.1.1.- Montaje de dispositivos. Comandos `mount` y `umount`

Como se vio anteriormente, Linux combina todos los dispositivos de almacenamiento en un único directorio virtual. Antes de utilizar un nuevo disco en el sistema necesitas localizarlo en el directorio virtual. Esta tarea se denomina **montaje**.

La mayoría de las actuales distribuciones Linux montan automáticamente algunas unidades como las unidades de CD o memorias USB, aunque habrá ocasiones en que sea necesario realizar esta operación manualmente.

El comando utilizado para montar dispositivos es el comando `mount`. Por defecto, si ejecutamos `mount` sin parámetros mostrará una lista de los dispositivos actualmente montados en el sistema.
 
La información que proporciona el comando `mount` es:

- La localización del dispositivo, es decir, en el caso de unidades de almacenamiento el fichero virtual que representa la dispositivo en el directorio `/dev`.
- El **punto de montaje** en el directorio virtual 
- El tipo de **sistema de ficheros**, por ejemplo, ext3, ext4, ....
- El **estado de acceso** del dispositivo montado, es decir, bajo qué condiciones se montó, por ejemplo, si es de lectura y escritura o solo lectura.

```shell
    vgonzalez@ubuntu:~$ mount
    /dev/sdc on / type ext4 (rw,relatime,discard,errors=remount-ro,data=ordered)
    tmpfs on /mnt/wsl type tmpfs (rw,relatime)
```

Por ejemplo, en la salida anterior la primera entrada hace referencia a un disco duro (`sdc`). El fichero que representa al dispositivo es **/dev/sdc** y está montado en el directorio raíz (`/`). El tipo del sistema de ficheros es ext4. Al final de la línea se muestra otra información del dispositivo como por ejemplo que es de lectura y escritura (rw, read and write).
 
Para montar un dispositivo manualmente primero es necesario tener privilegios de administrador. La sintaxis del comando `mount` es:

```shell
    $ mount –t tipo dispositivo directorio
```
Los parámetros son:

- El parámetro **tipo** indica el sistema de ficheros con el que está formateado el dispositivo. Linux reconoce un gran número de sistemas de ficheros, algunos ejemplos son:
  - *ext3* o *ext4,* nativos de Linux
  - *vfat*, que es el sistema utilizado por Windows en versiones antiguas o en memorias USB
  - *ntfs*, el sistema de ficheros de Windows 2000 y posteriores
  - **iso9660**, utilizado en los CDs y DVDs.
- Los siguientes dos parámetros definen la **localización** del dispositivo (los dispositivos en Linux se asocian con un archivo en el directorio /dev) y el **directorio donde queremos montarlo**. El usuario root debe tener acceso a este directorio, pero se puede limitar el acceso a otros usuarios mediante permisos.

Los modificadores del comando `mount` son las siguientes:
 
| Parámetro | Descripción |
| --------- | ----------- |
| `-a`      | Monta todos los sistemas de ficheros indicados en el fichero `/etc/fstab` |
| `-f`      | Simula el montaje pero sin montar realmente el dispositivo |
| `-F`      | Junto con el modificador `-a`, monta todos los dispositivos simultáneamente |
| `v`       | Modo *verboso*, mostrando más información al usuario |
| `-s`      | Ignorar opciones de montaje no soportadas por el sistema de ficheros | 
| `-r`      | Monta el dispositivo en modo solo lectura |
| `-w`      | Monta el dispositivo en modo lectura/escritura (opción por defecto) |
| `-o`      | Para indicar opciones específicas al sistema de ficheros |


La última opción (`-o`) permite indicar una serie de opciones adicionales separadas por comas, estas pueden ser:

- `ro`: montar como dispositivo de solo lectura
- `rw`: montar como dispositivo de lectura-escritura
- `user`: permite a un usuario montar el sistema de ficheros
- `check=none`: omite la comprobación de integridad del sistema de ficheros.
- `loop`: permite montar un fichero en lugar de un dispositivo.

La última opción (`-loop`) nos permite montar un fichero que sea una **imagen de un dispositivo**, por ejemplo, un fichero ISO (extensión `.iso`), tal y como se puede ver a continuación.

```shell
    vgonzalez@ubuntu:~$ sudo mount -t iso9660 -o loop slacko-6.3.0.iso /mnt
    mount: /mnt /home/vgonzalez/Descargas/slack0-6.3.0.iso ya está montado. 
``` 

### 5.1.2.- Espacio libre en disco. Comando `df`

Para conocer el espacio disponible en un dispositivo se puede utilizar el comando `df`. Si se indica sin ningún parámetro mostrará información general sobre todos los dispositivos montados en el sistema.

```shell
    vgonzalez@ubuntu:~$ df
    S. ficheros     bloques de 1K   Usados  Disponibles Uso%    Montado en
    udev                  2042636        0      2041636   0%    /dev
    tmpfs                  413720     1048       412672   1%    /run
    /dev/sda1            30830500 77780008     21461348  27%    /
    tmpfs                 2068584    13964      2054620   1%    /dev/shm
    tmpfs                    5120        4         5116   1%    /run/lock
    tmpfs                 2068584        0      2068584   0%    /sys/fs/cgroup
    /dev/loop0              84736    84736            0 100%    /snap/core/5330
    /dev/loop1                128      128            0 100%    /snap/hello/22
```

La información que nos muestra este comando es:

- La localización del dispositivo.
- Cuantos bloques de 1024 bytes de datos puede contener
- Cuantos bloques de 1024 bytes son utilizados
- Cuantos bloques de 1024 bytes están disponibles
- La cantidad de espacio utilizado como un porcentaje
- El punto de montaje del dispositivo

Un parámetro de este comando es `–h` que nos muestra el espacio en disco de una forma más legible mostrándolo en gigabytes o megabytes.


### 5.1.3.- Ocupación de espacio de un directorio. Comando `du`

El comando `du` muestra la utilización del disco de un directorio específico (por defecto el directorio de trabajo actual) desglosando lo que ocupa cada uno de sus ficheros y subdirectorios.

```shell
    vgonzalez@ubuntu:~$ du
    4       ./Vídeos
    8       ./Música/apt/apt.conf.d
    12      ./Música/apt
    16      ./Música
    4       ./Público
    4       ./Escritorio
```

El número de la izquierda muestra el **número de bloques** de disco que ocupa cada directorio o fichero.

Algunos parámetros de este comando que pueden hacer su salida más legible son:
- `-c`: muestra la ocupación total de todos los ficheros
- `-h`: muestra el tamaño de una forma legible para los humanos, utilizando k para kilobyte, M para megabyte y G para gigabyte
- `-s`: solo muestra el tamaño total del directorio.



## 5.2.- Trabajando con ficheros de datos

### 5.2.1.- Ordenando datos. Comando `sort`

El comando `sort` ordena las líneas de un fichero de texto utilizando las reglas de ordenación según el lenguaje del sistema.

```shell
    vgonzalez@ubuntu:~$ cat fichero
    uno
    dos
    tres
    cuatro
    cinco
    vgonzalez@ubuntu:~$ sort fichero
    cinco
    cuatro
    dos
    tres
    uno
```

Sin embargo, si los valores que contiene el fichero son numéricos el resultado puede no ser el esperado:

```shell
    vgonzalez@ubuntu:~$ cat fichero2
    47
    2
    29
    12
    109
    1
    vgonzalez@ubuntu:~$ sort fichero2
    1
    109
    12
    2
    29
    47
```
  
Si sabemos que el primer valor de cada fila es un número y queremos que lo trate como un número al ordenarlo en lugar de como un carácter debemos utilizar el parámetro `–n`.

```shell
    vgonzalez@ubuntu:~$ sort fichero2 -n
    1
    2
    12
    29
    47
    109
```
  
Muchos ficheros de log o registro en Linux comienzan cada línea por el mes en que se ha producido la entrada. Si queremos que el comando `sort` nos reconozca estos caracteres como un mes deberemos utilizar el parámetro `–M`
 
Los parámetros `–k` y `–t` son muy útiles para ordenar ficheros de datos que usan campos, tales como el fichero `/etc/passwd`. El parámetro `–t` servirá para especificar el carácter que separará los campos y el parámetro `–k` especifica el campo que se utilizará para ordenarlo.

Por ejemplo, el siguiente comando ordenará el fichero `/etc/passwd` según el identificador del usuario, que por cierto es un valor numérico.

```shell
    vgonzalez@ubuntu:~$ sort -t ':' -k 3 -n /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
```
  
### 5.2.2.- Búsqueda de datos. Comando `grep`

Si queremos buscar una línea que contenga un determinado valor en un fichero deberemos utilizar la orden `grep`. El formato para el comando `grep` es el siguiente:

```shell
    grep [opciones] patrón [fichero]
```
El comando `grep` busca en el fichero indicado como entrada aquellas líneas que contengan los caracteres que coincidan con el patrón indicado.
  
```shell
    vgonzalez@ubuntu:~$ cat fichero
    uno
    dos
    tres
    cuatro
    cinco
    vgonzalez@ubuntu:~$ grep cuatro fichero
    cuatro
    vgonzalez@ubuntu:~$ grep c fichero
    cuatro
    cinco
```

Este comando admite multitud de parámetros:

- El parámetro `–v` nos permite invertir la búsqueda, es decir, mostrar aquellas líneas que _no incluyan_ el patrón indicado.
- Con el parámetro `–n` se mostrará el número de línea antes de cada una. 
- El parámetro `–c` contará el número de líneas que contienen el patrón.
- Se puede especificar más de un patrón mediante el parámetro `–e`. Por ejemplo, la orden `grep –e t –e f fichero1` busca todas las líneas que contengan el carácter `t` o el carácter `f`.

También admite **expresiones regulares** al indicar el patrón a buscar. Las expresiones regulares ya se verán más adelante, pero a modo de avance un ejemplo puede ser el siguiente, en el que se muestran todas las líneas del fichero cuyo primer carácter sea `c` o `u`.

```shell
    vgonzalez@ubuntu:~$ grep ^[cu] fichero
    uno
    cuatro
    cinco
```
 
### 5.2.3.- Compresión de datos. Comandos `bzip2`, `gzip` y `zip`

De forma análoga a WinZip en Windows, Linux dispone de un gran número de herramientas de **compresión de datos**. Algunas de las más utilizadas son las siguientes:

- `bzip2`: este comando comprime un fichero generando ficheros comprimidos con extensión `bz2`. Por defecto, el fichero comprimido reemplazará el fichero original. Está compuesto por cuatro herramientas diferentes:
    - `bzip2` para comprimir ficheros
    - `bzcat` que muestra el contenido de ficheros de texto comprimidos.
    - `bunzip2` para descomprimir ficheros
    - `bzip2recover` cuya utilidad es intentar recuperar datos de ficheros comprimidos dañados.
- `gzip`: probablemente la herramienta de compresión más popular. Como en el caso anterior incluye varias aplicaciones:
    - `gzip` para comprimir ficheros
    - `gzcat` para mostrar el contenido de los ficheros comprimidos
    - `gunzip` para descomprimir ficheros.
- `zip`: es compatible con la versión de MS-DOS y Windows e incluye cinco utilidades:
    - `zip` crea un fichero comprimido
    - `zipcloak` crea un fichero comprimido y cifrado
    - `zipnote` extrae los comentarios de un fichero zip
    - `zipsplit` divide un fichero zip en fichero más pequeños del tamaño indicado
    - `unzip` extrae los ficheros y directorios de un fichero comprimido.


### 5.2.4.- Archivado de datos. Comando `tar`

Aunque el comando `zip` sirve para comprimir varios ficheros y directorios en un solo fichero, es más común utilizar el comando `tar` para archivar varios ficheros en uno solo.

El formato del comando `tar` es el siguiente:

```shell
    tar función [opciones] objeto1 objeto2 …
```

El parámetro `función` define el modo de funcionamiento del comando tar tal y como se muestra en la siguiente tabla:

| Función   | Nombre largo      | Descripción   |
| --------- | ----------------- | ------------- |
| `-A`      | `--concatenate`   | Anexa un fichero *tar* a otro fichero *tar* existente.  |
| `-c`      | `--create`        | Crea un nuevo archivo *tar* |
| `-d`      | `--diff`          | Comprueba las diferencias entre un archivo *tar* y el sistema de ficheros. |
|           | `--delete`        | Borra un fichero de un archivo *tar* |
| `-r`      | `--append`        | Anexa uno o varios ficheros al final de un archivo *tar* |
| `-t`      | `--list`          | Muestra el contenido de un archivo *tar* |
| `-u`      | `--update`        | Reemplaza 

Cada una de las funciones se puede combinar con una serie de parámetros de los que se indican en la otra tabla. Por ejemplo:

```bash
tar –cvf test.tar test/ test2/
```

Crea un fichero llamado `test.tar` que contiene los contenidos de los directorios indicados:

```bash
tar –tf test.tar
```

Muestra, pero no extrae el contenido del fichero `test.tar`

```bash
tar –xvf test.tar
```

Extrae el contenido del fichero `test.tar`.
 

 ## 5.3.- Redireccionamiento

En UNIX, y por tanto en Linux, todo es un **flujo** (stream) de bytes. Estos flujos están normalmente representados como ficheros, pero hay tres flujos especiales que raramente son accedidos a través de un nombre de fichero. Estos son los flujos de entrada y salida asociados a cada comando: **la entrada estándar, la salida estándar y la salida de error**. Por defecto, estos flujos o streams están conectados a la terminal.

Cuando un comando lee un carácter o una línea, lo hace desde la entrada estándar, que es el teclado. Cuando tiene una salida, la imprime en la salida estándar, que es el monitor. La salida de error también está conectada al monitor.

Estos flujos son referenciados mediante números, llamados descriptores de fichero (FD), y son `0`, `1` y `2` respectivamente. También se conocen como `stdin`, `stdout` y `stderr`.


### 5.3.1.- Redirección >, >>,  < Y <<

El carácter `>` sirve para redireccionar la salida estándar a un fichero siguiendo la siguiente sintaxis:

```
comando > fichero
```

Si el nombre de fichero indicado no existe lo crea, siendo su contenido la salida del comando que se ha ejecutado. Si el fichero ya existe borra su contenido reemplazándolo por la salida del comando.

Si no queremos que borre el contenido de un fichero existente entonces debemos utilizar el operador `>>`.

Redirigir la salida estándar no redirige la salida de error. Si queremos redirigir la salida de error entonces el operador a utilizar es `2>`.
 
En este caso, los mensajes de error son enviados a un fichero especial, `/dev/null`, cuyo cometido únicamente es descartar todo lo que se copie en él.
 
En lugar de enviar una salida a un fichero, puede ser redireccionada a otro flujo de entrada/salida usando `>&N` donde N es el número del descriptor de fichero. Este comando envía tanto la salida estándar como la de error al fichero `FILE`.
 
Aquí el orden es importante. La salida estándar es enviada a `FILE`, y la salida de error es redireccionada a donde vaya la salida estándar. Si se invierte el orden, el efecto es diferente. La redirección envía la salida de error a donde esté yendo actualmente la salida estándar y a continuación cambia la salida estándar.
 
El Shell Bash también tiene una sintaxis no estándar para redirigir tanto la salida estándar como la de error al mismo sitio usando los operadores `&>` y `&>>`.

El operador `<` sirve para reemplazar la entrada estándar de un comando por el contenido del fichero que se indique, de la forma:

```
comando < fichero
```

El último operador de redireccionamiento es `<<`, cuya sintaxis es:

```
comando << etiqueta
```

Lo que hace es tomar la entrada para el comando de las siguientes líneas que se escriban hasta llegar a una línea que solo contenga la etiqueta.
 

### 5.3.2.- TUBERÍAS

Las **tuberías** o **pipelines** conectan la salida estándar de un comando con la entrada estándar de otro. El símbolo barra (`|`) debe ser utilizado entre los dos comandos.
 



***
[Volver al índice principal](index_UT10.md)


 


