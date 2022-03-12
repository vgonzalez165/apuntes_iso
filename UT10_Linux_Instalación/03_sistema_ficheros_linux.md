<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introducción_linux.md)
2. [Instalación de Linux](02_instalación_linux.md)
3. [**El sistema de ficheros en Linux**](03_sistema_ficheros_linux.md)
4. [Comandos para el sistema de ficheros](04_comandos_sistema_ficheros.md)
5. [Comandos avanzados del shell Bash](05_avanzados.md)
6. [Expresiones regulares](06_expresiones_regulares.md)


# 3.- EL SISTEMA DE FICHEROS EN LINUX

## 3.1.- Introducción al sistema de ficheros

La gestión del sistema de fichero en Linux difiere en algunos puntos a como lo hace Windows. La primera diferencia es que Linux no utiliza letras de unidades (por ejemplo, `C:`) en las rutas, sino que Linux almacena los ficheros en **una única estructura de directorios**, llamado **directorio virtual**. El directorio virtual contiene todos los ficheros de todas las unidades de almacenamiento instaladas en el PC unidos en una única estructura de directorios.

Todos los directorios de Linux cuelgan del **directorio raíz**, referenciado por el símbolo barra (`/`). Por ejemplo, una ruta en Linux será de la forma `/home/vgonzalez/docs/test.txt`.

Para incorporar otras unidades Linux crea directorios especiales denominados **puntos de montaje**, que son directorios en el directorio virtual a través de los que se puede acceder a otras unidades. Por ejemplo, Ubuntu monta algunas unidades en el directorio `/media`

 Sin embargo, las unidades se pueden montar en cualquier directorio. Por ejemplo, en un sistema con dos discos duros se puede instalar el sistema en el primer disco y montar el segundo disco en el directorio `/home`, de forma que todos los directorios de los usuarios estén en este segundo disco.

 Aunque puede variar según la distribución en la siguiente tabla se pueden ver los principales directorios de Linux y que se almacena en ellos.

 | Directorio | Uso                                                                                 |
 | ---------- | ----------------------------------------------------------------------------------- |
 | `/`        | El directorio raíz. No suele contener ficheros                                      |
 | `/bin`     | Contiene ejecutables o binarios, normalmente utilidades de usuario                  |
 | `/boot`    | Contiene los ficheros de arranque del sistema                                       |
 | `/dev`     | Almacena ficheros especiales que representan los dispositivos del sistema           |
 | `/etc`     | Ficheros de configuración del sistema                                               |
 | `/home`    | Aquí se almacenan las carpetas personales de los usuarios                           |
 | `/lib`     | Contiene las librerías del sistema y de las apliaciones                             |
 | `/media`   | Habitualmente utilizado como punto de montaje para los dispositivos extraíbles      |
 | `/mnt`     | Otro directorio también utilizado en ocasiones para el montaje de dispositivos      |
 | `/opt`     | Se suele utilizar para almacenar paquetes de software opcionales                    |
 | `/root`    | Directorio personal del administrador o *root*                                      |
 | `/sbin`    | Binarios del sistema, normalmente utilidades a nivel de administrador               |
 | `/tmp`     | Directorio para almacenar ficheros temporales                                       |
 | `/usr`     | Contiene software instalado por el usuario                                          |
 | `/var`     | Almacena ficheros que cambian frecuentemente, tales como ficheros de registro o log |


## 3.2.- Comandos básicos para la navegación por el sistema de ficheros

### 3.2.1.- Cambiando de directorio. Comando `cd`

El comando `cd` nos sirve para desplazarnos por los diferentes directorios del sistema de ficheros. Este comando toma un único parámetro que será el directorio destino al que nos queramos mover. Si no se indica ningún parámetro nos llevará al directorio home del usuario.

El directorio de destino se puede especificar de dos maneras diferentes:

- **Ruta absoluta**: define exactamente dónde está el directorio destino partiendo del directorio raíz. Por ejemplo `/home/vgonzalez/Escritorio/apuntesClase`
- **Ruta relativa**: aquí definimos como se accede al directorio partiendo del directorio actual. Una ruta relativa **no comienza** con el símbolo barra (`/`). Por ejemplo, si estamos en el directorio home y queremos referenciar el directorio anterior la ruta sería `Escritorio/apuntesClase`. Para ascender en la jerarquía de directorios podemos utilizar dos caracteres especiales:
    - El punto (`.`) que representa el directorio actual
    - El doble punto (`..`) que representa el directorio padre del actual.

Cada usuario en Linux tiene un directorio llamado **directorio personal** que es en el que tiene permisos de lectura y escritura sin necesidad de ser `root` y por tanto donde están todos sus archivos personales. Este directorio se encuentra dentro del directorio `/home` y se llama igual que el usuario.

Dada la importancia de este directorio, se puede hacer referencia a él mediante el carácter **virgulilla** (`~`) (combinación de teclas `AltGr + 4`), de forma que poner este carácter es equivalente a poner la ruta del directorio personal del usuario.

```bash
  vgonzalez@ubuntu:/etc$ cd ~
  vgonzalez@ubuntu:~$ 
```

También se puede incluir este carácter dentro de una ruta.

```bash
  vgonzalez@ubuntu:/etc$ cat ~/documentos/datos   # Equivale a /home/vgonzalez/documentos/datos
  hola mundo
```

### 3.2.2.- Conociendo el directorio de trabajo. Comando `pwd`

Si queremos saber cuál es el directorio de trabajo actual el comando a utilizar es el `pwd`.

```bash
  vgonzalez@ubuntu:~$ pwd
  /home/vgonzalez
```


### 3.2.2.- Mostrando el contenido de los directorios. Comando `ls`

El comando `ls` es la herramienta que nos ayudará a ver qué ficheros y directorios tenemos en nuestro sistema de archivos.

En su forma más básica nos mostrará los ficheros y directorios situados en el directorio actual.

```bash
  vgonzalez@ubuntu:/$ ls
  bin   dev  home  lib    lib64   lost+found  mnt  proc  run   srv  tmp  var
  boot  etc  init  lib32  libx32  media       opt  root  sbin  sys  usr
```
 
 En la siguiente tabla se muestran los parámetros más comunes del comando `ls`.

 | Parámetro        | Descripción                                                             |
 | ---------------- | ----------------------------------------------------------------------- |
 | `-F`             | Muestra información textual sobre el tipo de ficheros de que se trata   |
 | `-a`, `--all`    | Muestra ficheros ocultos                                                |
 | `-R`             | Muestra recursivamente el contenido del directorio y sus subdirectorios |
 | `-l`             | Muestra los ficheros en formato largo                                   |

Cuando utilizamos un terminal en color se identifican los diferentes tipos de ficheros con un código de colores, por ejemplo, los ejecutables se muestran de color verde o los directorios de color azul. Cuando se utiliza un terminal sin colores la forma de saber de qué tipo de elementos se trata es con el **parámetro `-F`** que muestra información textual sobre cada tipo de archivo.

```bash
  vgonzalez@ubuntu:/$ ls -F
  bin@   dev/  home/  lib@    lib64@   lost+found/  mnt/  proc/  run/   srv/  tmp/  var/
  boot/  etc/  init*  lib32@  libx32@  media/       opt/  root/  sbin@  sys/  usr/
```
 
 Al contrario que en Windows, en Linux no hay un atributo de los ficheros que indique que están ocultos. Si se quiere ocultar un fichero o directorio simplemente hay que anteponer un punto (`.`) al nombre del fichero y eso hará que no se muestre con el comando `ls`. Para que este comando muestre todos los ficheros, incluidos los ocultos, hay que utilizar el **parámetro `-a`**
 
```bash
  vgonzalez@ubuntu:~$ ls -a
  .  ..  .bash_history  .bash_logout  .bashrc  .bashrc.original  .cache  .config  .dbus  .java  .profile  .zshrc
```

Otra opción interesante es el **parámetro `-R`**, que muestra el contenido del directorio actual y también el de todos sus subdirectorios de forma recursiva.
 
```bash
  vgonzalez@ubuntu:/$ ls -R
  .:
  bin   dev  home  lib    lib64   lost+found  mnt  proc  run   srv  tmp  var
  boot  etc  init  lib32  libx32  media       opt  root  sbin  sys  usr

  ./boot:
  grub

  ./boot/grub:
  themes
```

Por último, el **parámetro `-l`** muestra información extendida sobre los elementos que se encuentran en el directorio.
 
```bash
  vgonzalez@ubuntu:/etc$ ls -l
  total 1356
  -rw-r--r--  1 root     root      2981 Dec  8 23:29 adduser.conf
  drwxr-xr-x  3 root     root      4096 Jan 26 19:21 alsa
  drwxr-xr-x  2 root     root     20480 Jan 26 20:00 alternatives
  drwxr-xr-x  8 root     root      4096 Jan 26 19:59 apache2
  drwxr-xr-x  3 root     root      4096 Jan 26 19:54 apparmor
```

La información que nos muestra este parámetro es la siguiente:

- El **tipo** de fichero, que puede ser:
  - `d`: directorio
  - `-`: fichero
  - `c`: dispositivo de caracteres
  - `b`: dispositivo de bloques
- Los **permisos** del fichero, que se representan mediante tres tripletas de caracteres, y que veremos en la próxima unidad.
- El **número de enlaces** duros al fichero, los enlaces en linux son algo parecido a los accesos directos de Windows.
- El **nombre del usuario** propietario del fichero
- El **nombre del grupo** propietario del fichero
- El **tamaño** del fichero en bytes
- La **fecha y hora** de la última modificación del fichero
- El **nombre** del fichero o directorio


Otra utilidad del comando `ls`, al margen de los parámetros que admita, es la posibilidad de usar **caracteres comodín** para mostrar los ficheros que se adapten al **patrón** indicado.

Los caracteres comodín son:

- Un símbolo de **interrogación** (`?`) representa un carácter.
- Un símbolo de **asterisco** (`*`) representa cero o más caracteres.
- Los **corchetes** (`[]`) nos sirven para representar un conjunto de caracteres, por ejemplo, `ls [a-c]*` representa todos los ficheros que comiencen por a, b ó c
- El símbolo **exclamación** (`!`) se usa junto con los corchetes para indicar que el conjunto se excluye, por ejemplo `ls [!a-c]*` indica todos los ficheros que no comiencen por a, b ó c.


***
[Volver al índice principal](index_UT10.md)

