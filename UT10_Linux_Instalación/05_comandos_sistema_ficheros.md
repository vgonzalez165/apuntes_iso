<link rel="stylesheet" href="../styles.css">

![Carátula UT07](imgs/caratula_ut10.png)

## Contenidos

1. [**Introducción a Linux**](01_introducción_linux.md)
2. [Instalación de Ubuntu](02_instalación_ubuntu.md)
3. [Bonding de red](03_bonding_red.md)
4. [El sistema de ficheros en Linux](04_sistema_ficheros_linux.md)
5. [Comandos para el sistema de ficheros](05_comandos_sistema_ficheros.md)
6. [Comandos avanzados del shell Bash](07_comandos_avanzados_bash.md)
7. [Expresiones regulares](08_expresiones_regulares.md)


# 5.- COMANDOS RELATIVOS AL SISTEMA DE FICHEROS

## 5.1.- Comandos para la manipulación de archivos

### 5.1.1.- Creación de ficheros. Comando `touch`

Bash proporciona un montón de comandos para manipular fichero en el sistema de ficheros Linux. El más sencillo es `touch`. Simplemente crea un fichero vacío con el nombre indicado.

```shell
    vgonzalez@PORTATIL:~$ touch test
    vgonzalez@PORTATIL:~$ ls -l
    total 0
    -rw-r--r-- 1 vgonzalez vgonzalez 0 Feb 10 12:35 test
```
  
Si el fichero ya existe el comando `touch` simplemente cambia la hora de modificación del fichero indicado, sin alterar para nada su contenido. Si queremos que no ponga la fecha actual sino otra hora y fecha diferentes lo podemos hacer con el parámetro `–t`.
  
```shell
    vgonzalez@PORTATIL:~$ touch -t 202201011200 test
    vgonzalez@PORTATIL:~$ ls -l
    total 0
    -rw-r--r-- 1 vgonzalez vgonzalez 0 Jan  1 12:00 test
```

Observa la sintaxis para indicar la nueva hora. Si tienes dudas sobre como interpretarla puedes recurrir al manual de ayuda.

```shell
    -t STAMP
        use [[CC]YY]MMDDhhmm[.ss] instead of current time
```


### 5.1.2.- Copia de ficheros. Comando `cp`

Para copiar ficheros de una localización a otra utilizaremos el comando `cp`.
  
```shell
    vgonzalez@PORTATIL:~$ cp test ./pruebas/
```

Si tanto el origen como el destino son ficheros, el comando `cp` copiará el fichero origen a un nuevo fichero con el nombre de fichero indicado como destino.

```shell
    vgonzalez@PORTATIL:~$ ls
    pruebas  test
    vgonzalez@PORTATIL:~$ cp test test2
    vgonzalez@PORTATIL:~$ ls
    pruebas  test  test2
```

Al igual que todos los comandos, el comando `cp` dispone de una serie de modificadores para alterar su comportamiento.
- El parámetro `–p` copia el fichero, pero mantiene la fecha de acceso y modificación del fichero original en el fichero copiado.
- El parámetro `–R` permite copiar recursivamente los contenidos de un directorio completo.
- Por defecto, si el fichero destino existe, nos preguntará si queremos sobrescribirlo antes de realizar la operación. Con el parámetro `–f` indicaremos que no nos pregunte, sobrescribiéndolo sin más.

Como siempre podemos ver un listado completo de los parámetros de `cp` en su página del manual.


### 5.1.2- Renombrando ficheros. Comando `mv`

En Linux, renombrar un fichero es lo mismo que moverlo. El comando `mv` es el que utilizaremos para mover tanto ficheros como directorios.

```shell
    vgonzalez@PORTATIL:~$ ls -l
    total 4
    drwxr-xr-x 2 vgonzalez vgonzalez 4096 Feb 10 12:43 pruebas
    -rw-r--r-- 1 vgonzalez vgonzalez    0 Jan  1 12:00 test
    vgonzalez@PORTATIL:~$ mv test test2
    vgonzalez@PORTATIL:~$ ls -l
    total 4
    drwxr-xr-x 2 vgonzalez vgonzalez 4096 Feb 10 12:43 pruebas
    -rw-r--r-- 1 vgonzalez vgonzalez    0 Jan  1 12:00 test2
```

Observa que al mover un fichero cambiamos su nombre, pero mantenemos el mismo número de inodo y fecha y hora de modificación.


### 5.1.3.- Borrado de ficheros. Comando `rm`

El comando que nos permitirá borrar ficheros es `rm`. En su forma más sencilla solo requerirá el nombre del fichero a eliminar. Ten en cuenta que en el shell Bash no hay papelera, por lo que **el borrado de ficheros es irreversible**. Una buena práctica es acostumbrarse a utilizar el parámetro `–i`, sobre todo cuando utilizamos comodines, que nos preguntará antes de borrar cada fichero.

```shell
    vgonzalez@PORTATIL:~$ ls
    pruebas  test  test2
    vgonzalez@PORTATIL:~$ rm test* -i
    rm: remove regular empty file 'test'? y
    rm: remove regular empty file 'test2'? y
    vgonzalez@PORTATIL:~$ ls
    pruebas
```


## 5.2.- Comandos para la manipulación de directorios

En Linux hay algunos comandos que funcionan tanto para ficheros como para directorios (como el comando `cp`), mientras que otros que solo trabajan con directorios.

### 5.2.1.- Creación de directorios. Comando `mkdir`

El comando que nos permitirá crear directorios en Linux es `mkdir`.

```shell
    vgonzalez@PORTATIL:~$ ls
    pruebas
    vgonzalez@PORTATIL:~$ mkdir pruebas2
    vgonzalez@PORTATIL:~$ ls
    pruebas  pruebas2
```
  
### 5.1.2.- Eliminación de directorios. Comando `rmdir`

El comando básico para eliminar directorios en Linux es `rmdir`. Algo importante es que este comando solo funcionará con directorios vacíos, mostrándonos un mensaje de error en caso de que el directorio no esté vacío.

```shell
    vgonzalez@PORTATIL:~$ ls
    pruebas  pruebas2
    vgonzalez@PORTATIL:~$ rmdir pruebas2
    vgonzalez@PORTATIL:~$ ls
    pruebas
    vgonzalez@PORTATIL:~$ rmdir pruebas
    rmdir: failed to remove 'pruebas': Directory not empty
```

Para borrar directorios que no estén vacíos es preferible utilizar el comando `rm` utilizando el modificador `-r` que realizará un **borrado recursivo** del directorio y de todo su contenido.

```shell
    vgonzalez@PORTATIL:~$ ls
    pruebas  pruebas2
    vgonzalez@PORTATIL:~$ rmdir pruebas
    rmdir: failed to remove 'pruebas': Directory not empty
    vgonzalez@PORTATIL:~$ rm -r pruebas
    vgonzalez@PORTATIL:~$ ls
    pruebas2
```

## 5.3.- Extracción de información de ficheros

### 5.3.1.- Obtener información de un fichero. Comando `stat`

Como ya se vió con el comando `ls –l` es posible obtener mucha información acerca de un fichero. Sin embargo, el sistema almacena mucha más información acerca de los ficheros, como puede ser la fecha de creación, modificación y último acceso, o información relativa a los inodos.

El comando proporcionado por Bash para acceder a toda esta información es el comando `stat`, que nos mostrará toda la información disponible acerca del fichero.

```shell
    vgonzalez@PORTATIL:~$ stat test
      File: test
      Size: 0               Blocks: 0          IO Block: 4096   regular empty file
    Device: 820h/2080d      Inode: 40072       Links: 1
    Access: (0644/-rw-r--r--)  Uid: ( 1000/vgonzalez)   Gid: ( 1000/vgonzalez)
    Access: 2022-02-10 13:04:53.076629500 +0100
    Modify: 2022-02-10 13:04:53.076629500 +0100
    Change: 2022-02-10 13:04:53.076629500 +0100
     Birth: -
```

### 5.3.2.- Ver el tipo de un fichero. Comando `file`

Una característica de Linux es que no utiliza la extensión de los ficheros para determinar el tipo de fichero de que se trata. 

Un tipo de información no mostrado por el comando `stat` es el tipo de fichero de que se trata. Podemos ver de qué tipo es un fichero mediante el comando `file`.

El comando `file` clasifica los ficheros en tres categorías:
- **Ficheros de texto**: ficheros que contienen caracteres imprimibles.
- **Ficheros ejecutables**: ficheros que pueden ser ejecutados en el sistema
- **Ficheros de datos**: ficheros binarios que contienen caracteres no imprimibles, pero que de alguna forma pueden ser utilizados por el sistema. En este caso indicará el tipo de ficheros que contiene.
 
```shell
vgonzalez@PORTATIL:/mnt/c/Users/victor$ file Desktop/temp.png
Desktop/temp.png: PNG image data, 960 x 480, 8-bit/color RGB, non-interlaced
vgonzalez@PORTATIL:/mnt/c/Users/victor$ file energy-report.html
energy-report.html: HTML document, UTF-8 Unicode (with BOM) text, with very long lines, with CRLF, CR line terminators
```

### 5.3.3.- Ver el contenido de un fichero. Comandos `cat`, `more`y `less`

Para ver el contenido de un fichero Linux dispone de tres comandos: `cat`, `more` y `less`.
El comando `cat` muestra todos los datos dentro de un fichero de texto.

```shell
vgonzalez@PORTATIL:~$ cat test
hola mundo!!!
```

Sin ningún modificador es un comando muy sencillo. Sin embargo, nuevamente nos encontramos con un gran número de modificadores para alterar su comportamiento.

- El modificador `–n` muestra el número de cada línea, útil por ejemplo para ver ficheros de código fuente.
- Si solo queremos que nos numere las líneas que tengan texto entonces deberemos utilizar el parámetro `–b`.
- Si el fichero tuviera muchas líneas en blanco y queremos que nos las comprima en una sola entonces el parámetro será `–s`.
- Si queremos que no se muestren los caracteres de tabulación el parámetro será `–T`. Fíjate que cada carácter de tabulación que encuentre lo reemplazará por la combinación de caracteres `^I`.
 
```shell
    vgonzalez@PORTATIL:~$ cat -b test
        1  hola mundo!!!
        2  ASIR
        3                  IES San Andrés
    vgonzalez@PORTATIL:~$ cat -T test
    hola mundo!!!
    ASIR
    ^I^IIES San Andrés
```

El comando `cat` tiene un problema cuando queremos visualizar ficheros con un gran número de líneas de texto ya que los mostrará sin realizar ninguna pausa. Si queremos que el contenido se nos muestre página a página el comando que utilizaremos será `more`.

El comando `more` visualizará una página y esperará antes de seguir mostrándonos un *prompt*. El prompt de `more` admite una serie de órdenes que se muestran en la siguiente tabla.
 
| Opción | Descripción |
| ------ | ----------- |
| H      | Muestra la ayuda |
| espacio| Avanza a la siguiente página del texto |
| z      | Avanza a la siguiente página del texto |
| ENTER  | Avanza una línea |
| d      | Avanza media pantalla de texto |
| q      | Salir del programa |
| /exp   | Busca el texto inicado en el fichero |
| n      | Busca la siguiente ocurrencia del texto buscado |
| =      | Muestra el número de la línea actual



## 5.2.- Enlaces

Hay ocasiones en las que es necesario tener dos o más copias de un mismo fichero en el sistema de ficheros. Una opción que permite el sistema de ficheros de Unix es, en lugar de tener varias copias físicas diferentes, tener **una única copia física y múltiples copias virtuales**, denominadas **enlaces**.

Un enlace es una entrada en un directorio que apunta a la localización real del fichero. En Linux hay dos tipos diferentes de enlaces a ficheros:

- Enlaces simbólicos o blandos
- Enlaces duros

Un **enlace duro** crea un fichero diferente que contiene información acerca del fichero original y dónde localizarlo. Cuando referencias un enlace duro a un fichero, es igual que si referenciaras al fichero.

Se puede crear un enlace duro a un fichero utilizando el modificador `-l` con el comando de copia `cp`.

```shell
    vgonzalez@PORTATIL:~$ ls -il test
    40072 -rw-r--r-- 1 vgonzalez vgonzalez 37 Feb 10 13:14 test
    vgonzalez@PORTATIL:~$ cp test test2 -l
    vgonzalez@PORTATIL:~$ ls -il
    total 8
    40072 -rw-r--r-- 2 vgonzalez vgonzalez 37 Feb 10 13:14 test
    40072 -rw-r--r-- 2 vgonzalez vgonzalez 37 Feb 10 13:14 test2
```

El parámetro `–l` crea un enlace duro para el fichero test1 y lo denomina test2. En esta captura podemos apreciar dos cosas:

- Primero que el número de inodo (primera columna) de ambos ficheros es el mismo indicando que en realidad ambos ficheros son el mismo
- Y segundo, que el número de enlaces indicados para cada fichero (tercera columna) es dos.

Hay que tener en cuenta que, dado que ambos ficheros comparten el inodo, solo se pueden crear enlaces duros entre ficheros **del mismo dispositivo**.

Por otro lado, para crear un **enlace simbólico** hay que utilizar el comando `cp`, pero con el parámetro –s.
  
```shell
    vgonzalez@PORTATIL:~$ cp test test3 -s
    vgonzalez@PORTATIL:~$ ls -il
    total 8
    40072 -rw-r--r-- 2 vgonzalez vgonzalez 37 Feb 10 13:14 test
    40072 -rw-r--r-- 2 vgonzalez vgonzalez 37 Feb 10 13:14 test2
    40058 lrwxrwxrwx 1 vgonzalez vgonzalez  4 Feb 10 13:25 test3 -> test
```

Lo primero que podemos apreciar en la captura es que ahora los ficheros tienen números de inodo diferentes, por lo que el sistema los trata como ficheros independientes. Por otro lado, el tamaño del fichero también es diferente. Un fichero enlazado solo necesita guardar información acerca del fichero origen, no los datos que tenga.

Otra forma de crear enlaces es utilizando el comando `ln`. Por defecto este comando crea enlaces duros, pero se puede utilizar el parámetro `–s` para crear enlaces simbólicos.



***
[Volver al índice principal](index_UT10.md)
