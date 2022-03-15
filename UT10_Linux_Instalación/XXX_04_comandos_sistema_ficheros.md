<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introducción_linux.md)
2. [Instalación de Linux](02_instalación_linux.md)
3. [El sistema de ficheros en Linux](03_sistema_ficheros_linux.md)
4. [**Comandos para el sistema de ficheros**](04_comandos_sistema_ficheros.md)
5. [Comandos avanzados del shell Bash](05_avanzados.md)
6. [Expresiones regulares](06_expresiones_regulares.md)


```

1.- Manipulación de archivos
touch
cp
mv
rm
vim/emacs/nano

2.- Manipulación de directorios
mkdir
rmdir

3.- Información de ficheros
stat
file
cat
more/less
* head/tail

4.- Otros comandos
* alias/unalias
* exit
* shutdown
* history
* which
* wc
* uname
* find
* locate
* wget
* clear
* uptime
* whoami / who



4.- Enlaces
```





# 4.- COMANDOS RELATIVOS AL SISTEMA DE FICHEROS

## 4.1.- Comandos para la manipulación de archivos

### 4.1.1.- Creación de ficheros. Comando `touch`

Bash proporciona un montón de comandos para manipular fichero en el sistema de ficheros Linux. El más sencillo es `touch`. Simplemente crea un fichero vacío con el nombre indicado.

```shell
    vgonzalez@ubuntu:~$ touch test
    vgonzalez@ubuntu:~$ ls -l
    total 0
    -rw-r--r-- 1 vgonzalez vgonzalez 0 Feb 10 12:35 test
```
  
Si el fichero ya existe el comando `touch` simplemente cambia la hora de modificación del fichero indicado, sin alterar para nada su contenido. Si queremos que no ponga la fecha actual sino otra hora y fecha diferentes lo podemos hacer con el parámetro `–t`.
  
```shell
    vgonzalez@ubuntu:~$ touch -t 202201011200 test
    vgonzalez@ubuntu:~$ ls -l
    total 0
    -rw-r--r-- 1 vgonzalez vgonzalez 0 Jan  1 12:00 test
```

Observa la sintaxis para indicar la nueva hora. Si tienes dudas sobre como interpretarla puedes recurrir al manual de ayuda.

```shell
    -t STAMP
        use [[CC]YY]MMDDhhmm[.ss] instead of current time
```


### 4.1.2.- Copia de ficheros. Comando `cp`

Para copiar ficheros de una localización a otra utilizaremos el comando `cp`.
  
```shell
    vgonzalez@ubuntu:~$ cp test ./pruebas/
```

Si tanto el origen como el destino son ficheros, el comando `cp` copiará el fichero origen a un nuevo fichero con el nombre de fichero indicado como destino.

```shell
    vgonzalez@ubuntu:~$ ls
    pruebas  test
    vgonzalez@ubuntu:~$ cp test test2
    vgonzalez@ubuntu:~$ ls
    pruebas  test  test2
```

Al igual que todos los comandos, el comando `cp` dispone de una serie de modificadores para alterar su comportamiento.

- El parámetro `–p` copia el fichero, pero mantiene la fecha de acceso y modificación del fichero original en el fichero copiado.
- El parámetro `–R` permite copiar recursivamente los contenidos de un directorio completo.
- Por defecto, si el fichero destino existe, nos preguntará si queremos sobrescribirlo antes de realizar la operación. Con el parámetro `–f` indicaremos que no nos pregunte, sobrescribiéndolo sin más.

Como siempre podemos ver un listado completo de los parámetros de `cp` en su página del manual.


### 4.1.3- Renombrando ficheros. Comando `mv`

En Linux, renombrar un fichero es lo mismo que moverlo. El comando `mv` es el que utilizaremos para mover tanto ficheros como directorios.

```shell
    vgonzalez@ubuntu:~$ ls -l
    total 4
    drwxr-xr-x 2 vgonzalez vgonzalez 4096 Feb 10 12:43 pruebas
    -rw-r--r-- 1 vgonzalez vgonzalez    0 Jan  1 12:00 test
    vgonzalez@ubuntu:~$  mv test test2
    vgonzalez@ubuntu:~$  ls -l
    total 4
    drwxr-xr-x 2 vgonzalez vgonzalez 4096 Feb 10 12:43 pruebas
    -rw-r--r-- 1 vgonzalez vgonzalez    0 Jan  1 12:00 test2
```

Observa que al mover un fichero cambiamos su nombre, pero mantenemos el mismo número de inodo y fecha y hora de modificación.


### 4.1.4.- Borrado de ficheros. Comando `rm`

El comando que nos permitirá borrar ficheros es `rm`. En su forma más sencilla solo requerirá el nombre del fichero a eliminar. Ten en cuenta que en el shell Bash no hay papelera, por lo que **el borrado de ficheros es irreversible**. Una buena práctica es acostumbrarse a utilizar el parámetro `–i`, sobre todo cuando utilizamos comodines, que nos preguntará antes de borrar cada fichero.

```shell
    vgonzalez@ubuntu:~$ ls
    pruebas  test  test2
    vgonzalez@ubuntu:~$ rm test* -i
    rm: remove regular empty file 'test'? y
    rm: remove regular empty file 'test2'? y
    vgonzalez@ubuntu:~$ ls
    pruebas
```

### 4.1.5.- Edición de ficheros. 

Hay multitud de programas en Linux para editar ficheros de texto, tanto en entorno gráfico como en la terminal. 

Los más conocidos para editar textos desde una terminal son:

- `vim`: un editor de textos con multitud de opciones pero con una curva de aprendizaje muy pronunciada que hace que sea poco recomendable para principiantes.
- `GNU Emacs`: creado por Richard Stallman es un editor con infinitas opciones y funciones, llegando a disponer de una calculadora, un administrador de archivos o incluso un tetris.
- `nano`: un editor muy sencillo pero que cumple sobradamente con su función para la mayoría de las ocasiones. Además es fácil encontrarlo en la mayoríade las distribuciones ya instalado. 


## 4.2.- Comandos para la manipulación de directorios

En Linux hay algunos comandos que funcionan tanto para ficheros como para directorios (como el comando `cp`), mientras que otros que solo trabajan con directorios.

### 4.2.1.- Creación de directorios. Comando `mkdir`

El comando que nos permitirá crear directorios en Linux es `mkdir`.

```shell
    vgonzalez@ubuntu:~$ ls
    pruebas
    vgonzalez@ubuntu:~$ mkdir pruebas2
    vgonzalez@ubuntu:~$ ls
    pruebas  pruebas2
```
  
### 4.2.2.- Eliminación de directorios. Comando `rmdir`

El comando básico para eliminar directorios en Linux es `rmdir`. Algo importante es que este comando solo funcionará con directorios vacíos, mostrándonos un mensaje de error en caso de que el directorio no esté vacío.

```shell
    vgonzalez@ubuntu:~$ ls
    pruebas  pruebas2
    vgonzalez@ubuntu:~$ rmdir pruebas2
    vgonzalez@ubuntu:~$ ls
    pruebas
    vgonzalez@ubuntu:~$ rmdir pruebas
    rmdir: failed to remove 'pruebas': Directory not empty
```

Para borrar directorios que no estén vacíos es preferible utilizar el comando `rm` utilizando el modificador `-r` que realizará un **borrado recursivo** del directorio y de todo su contenido.

```shell
    vgonzalez@ubuntu:~$ ls
    pruebas  pruebas2
    vgonzalez@ubuntu:~$ rmdir pruebas
    rmdir: failed to remove 'pruebas': Directory not empty
    vgonzalez@ubuntu:~$ rm -r pruebas
    vgonzalez@ubuntu:~$ ls
    pruebas2
```


## 4.3.- Extracción de información de ficheros

### 4.3.1.- Obtener información de un fichero. Comando `stat`

Como ya se vió con el comando `ls –l` es posible obtener mucha información acerca de un fichero. Sin embargo, el sistema almacena mucha más información acerca de los ficheros, como puede ser la fecha de creación, modificación y último acceso, o información relativa a los inodos.

El comando proporcionado por Bash para acceder a toda esta información es el comando `stat`, que nos mostrará toda la información disponible acerca del fichero.

```shell
    vgonzalez@ubuntu:~$ stat test
      File: test
      Size: 0               Blocks: 0          IO Block: 4096   regular empty file
    Device: 820h/2080d      Inode: 40072       Links: 1
    Access: (0644/-rw-r--r--)  Uid: ( 1000/vgonzalez)   Gid: ( 1000/vgonzalez)
    Access: 2022-02-10 13:04:53.076629500 +0100
    Modify: 2022-02-10 13:04:53.076629500 +0100
    Change: 2022-02-10 13:04:53.076629500 +0100
     Birth: -
```

### 4.3.2.- Ver el tipo de un fichero. Comando `file`

Una característica de Linux es que no utiliza la extensión de los ficheros para determinar el tipo de fichero de que se trata. 

Un tipo de información no mostrado por el comando `stat` es el tipo de fichero de que se trata. Podemos ver de qué tipo es un fichero mediante el comando `file`.

El comando `file` clasifica los ficheros en tres categorías:
- **Ficheros de texto**: ficheros que contienen caracteres imprimibles.
- **Ficheros ejecutables**: ficheros que pueden ser ejecutados en el sistema
- **Ficheros de datos**: ficheros binarios que contienen caracteres no imprimibles, pero que de alguna forma pueden ser utilizados por el sistema. En este caso indicará el tipo de ficheros que contiene.
 
```shell
    vgonzalez@ubuntu:/mnt/c/Users/victor$ file Desktop/temp.png
    Desktop/temp.png: PNG image data, 960 x 480, 8-bit/color RGB, non-interlaced
    vgonzalez@ubuntu:/mnt/c/Users/victor$ file energy-report.html
    energy-report.html: HTML document, UTF-8 Unicode (with BOM) text, with very long lines, with CRLF, CR line terminators
```

### 4.3.3.- Ver el contenido de un fichero. Comandos `cat`, `more`y `less`

Para ver el contenido de un fichero Linux dispone de tres comandos: `cat`, `more` y `less`.
El comando `cat` muestra todos los datos dentro de un fichero de texto.

```shell
    vgonzalez@ubuntu:~$ cat test
    hola mundo!!!
```

Sin ningún modificador es un comando muy sencillo. Sin embargo, nuevamente nos encontramos con un gran número de modificadores para alterar su comportamiento.

- El modificador `–n` muestra el número de cada línea, útil por ejemplo para ver ficheros de código fuente.
- Si solo queremos que nos numere las líneas que tengan texto entonces deberemos utilizar el parámetro `–b`.
- Si el fichero tuviera muchas líneas en blanco y queremos que nos las comprima en una sola entonces el parámetro será `–s`.
- Si queremos que no se muestren los caracteres de tabulación el parámetro será `–T`. Fíjate que cada carácter de tabulación que encuentre lo reemplazará por la combinación de caracteres `^I`.
 
```shell
    vgonzalez@ubuntu:~$ cat -b test
        1  hola mundo!!!
        2  ASIR
        3                  IES San Andrés
    vgonzalez@ubuntu:~$ cat -T test
    hola mundo!!!
    ASIR
    ^I^IIES San Andrés
```

El comando `cat` tiene un problema cuando queremos visualizar ficheros con un gran número de líneas de texto ya que los mostrará sin realizar ninguna pausa. Si queremos que el contenido se nos muestre página a página el comando que utilizaremos será `more`.

El comando `more` visualizará una página y esperará antes de seguir mostrándonos un *prompt*. El prompt de `more` admite una serie de órdenes que se muestran en la siguiente tabla.
 
| Opción | Descripción                                      |
| ------ | ------------------------------------------------ |
| H      | Muestra la ayuda                                 |
| espacio| Avanza a la siguiente página del texto           |
| z      | Avanza a la siguiente página del texto           |
| ENTER  | Avanza una línea                                 |
| d      | Avanza media pantalla de texto                   |
| q      | Salir del programa                               |
| /exp   | Busca el texto inicado en el fichero             |
| n      | Busca la siguiente ocurrencia del texto buscado  |
| =      | Muestra el número de la línea actual             |


## 4.4.- Enlaces

Hay ocasiones en las que es necesario tener dos o más copias de un mismo fichero en el sistema de ficheros. Una opción que permite el sistema de ficheros de Unix es, en lugar de tener varias copias físicas diferentes, tener **una única copia física y múltiples copias virtuales**, denominadas **enlaces**.

Un enlace es una entrada en un directorio que apunta a la localización real del fichero. En Linux hay dos tipos diferentes de enlaces a ficheros:

- Enlaces simbólicos o blandos
- Enlaces duros

Un **enlace duro** crea un fichero diferente que contiene información acerca del fichero original y dónde localizarlo. Cuando referencias un enlace duro a un fichero, es igual que si referenciaras al fichero.

Se puede crear un enlace duro a un fichero utilizando el modificador `-l` con el comando de copia `cp`.

```shell
    vgonzalez@ubuntu:~$ ls -il test
    40072 -rw-r--r-- 1 vgonzalez vgonzalez 37 Feb 10 13:14 test
    vgonzalez@ubuntu:~$ cp test test2 -l
    vgonzalez@ubuntu:~$ ls -il
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
    vgonzalez@ubuntu:~$ cp test test3 -s
    vgonzalez@ubuntu:~$ ls -il
    total 8
    40072 -rw-r--r-- 2 vgonzalez vgonzalez 37 Feb 10 13:14 test
    40072 -rw-r--r-- 2 vgonzalez vgonzalez 37 Feb 10 13:14 test2
    40058 lrwxrwxrwx 1 vgonzalez vgonzalez  4 Feb 10 13:25 test3 -> test
```

Lo primero que podemos apreciar en la captura es que ahora los ficheros tienen números de inodo diferentes, por lo que el sistema los trata como ficheros independientes. Por otro lado, el tamaño del fichero también es diferente. Un fichero enlazado solo necesita guardar información acerca del fichero origen, no los datos que tenga.

Otra forma de crear enlaces es utilizando el comando `ln`. Por defecto este comando crea enlaces duros, pero se puede utilizar el parámetro `–s` para crear enlaces simbólicos.



***
[Volver al índice principal](index_UT10.md)
