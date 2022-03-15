<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introducción_linux.md)
2. [Instalación de Linux](02_instalación_linux.md)
3. [El sistema de ficheros en Linux](03_sistema_ficheros_linux.md)
4. [**Comandos para el sistema de ficheros**](04_comandos_sistema_ficheros.md)
5. [Comandos avanzados del shell Bash](05_avanzados.md)
6. [Expresiones regulares](06_expresiones_regulares.md)


# 4.- COMANDOS RELATIVOS AL SISTEMA DE FICHEROS

## 4.1.- Comandos para la manipulación de archivos

### 4.1.1.- Creación de ficheros. Comando `touch`




### 4.1.2.- Copia de ficheros. Comando `cp`



### 4.1.3- Renombrando ficheros. Comando `mv`




### 4.1.4.- Borrado de ficheros. Comando `rm`




### 4.1.5.- Edición de ficheros. 




## 4.2.- Comandos para la manipulación de directorios

En Linux hay algunos comandos que funcionan tanto para ficheros como para directorios (como el comando `cp`), mientras que otros que solo trabajan con directorios.

### 4.2.1.- Creación de directorios. Comando `mkdir`



  
### 4.2.2.- Eliminación de directorios. Comando `rmdir`




## 4.3.- Extracción de información de ficheros

### 4.3.1.- Obtener información de un fichero. Comando `stat`




### 4.3.2.- Ver el tipo de un fichero. Comando `file`




### 4.3.3.- Ver el contenido de un fichero. Comandos `cat`, `more`y `less`

Para ver el contenido de un fichero Linux dispone de tres comandos: `cat`, `more` y `less`.



### 4.3.4.- Otros comandos para ver el contenido de un fichero: `more` y `less`

El comando `cat` es muy útil cuando queremos obtener el contenido de un fichero para procesarlo de alguna manera redireccionándolo a otro comando, pero, pero si lo que se quiere es visualizar el fichero y este excede del tamaño de la pantalla los mostrará todo sin realizar ninguna pausa, por lo que únicamente se podrán ver las últimas líneas. Para ver el contenido se nos muestre página a página el comando que utilizaremos será `more`.




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
