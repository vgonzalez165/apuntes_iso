<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introduccion.md)
2. [Primeros pasos con Linux](02_primeros_pasos.md)
3. [El sistema de ficheros](03_sistema_ficheros.md)
4. [**Trabajando con datos textuales**](04_comandos_texto.md)
5. [Expresiones regulares con el comando `sed`](05_expresiones_regulares.md)


## Índice del apartado 4

- [4.1.- Redireccionamiento](#41--redireccionamiento)
  - [4.1.1.- Redirección. `STDIN`, `STDOUT` Y `STDERR`](#411--redirección-stdin-stdout-y-stderr)
  - [4.1.2.- Tuberías](#412--tuberías)
- [4.2.- Comandos para trabajar con datos](#42--comandos-para-trabajar-con-datos)
  - [4.2.1.- Ordenando. Comando `sort`](#421--ordenando-comando-sort)
  - [4.2.2.- Filtrando. Comando `grep`](#422--filtrando-comando-grep)
  - [4.2.1.- Contando. Comando `wc`](#421--contando-comando-wc)
  - [4.2.3.- Cortando. Comando `cut`](#423--cortando-comando-cut)
  - [4.2.4.- Y reemplazando. Comando `tr`](#424--y-reemplazando-comando-tr)


# 4.- TRABAJANDO CON DATOS TEXTUALES

## 4.1.- Redireccionamiento

En UNIX, y por tanto en Linux, todo es un **flujo** (**stream**) de bytes. Estos flujos están normalmente representados como ficheros, pero hay tres flujos especiales que raramente son accedidos a través de un nombre de fichero. Estos son los **flujos de entrada y salida** asociados a cada comando: la **entrada estándar**, la **salida estándar** y la **salida de error**. Por defecto, estos flujos o streams están conectados a la terminal.

Cuando un comando lee un carácter o una línea, lo hace desde la entrada estándar, que es el teclado. Cuando tiene una salida, la imprime en la salida estándar, que es el monitor. La salida de error también está conectada al monitor.

Estos flujos son referenciados mediante números, llamados descriptores de fichero (FD), y son `0`, `1` y `2` respectivamente. También se conocen como `stdin`, `stdout` y `stderr`.


### 4.1.1.- Redirección. `STDIN`, `STDOUT` Y `STDERR`

El carácter `>` sirve para redireccionar la salida estándar a un fichero siguiendo la siguiente sintaxis:

```
comando > fichero
```

Si el nombre de fichero indicado no existe lo crea, siendo su contenido la salida del comando que se ha ejecutado. Si el fichero ya existe borra su contenido reemplazándolo por la salida del comando.

Si no queremos que borre el contenido de un fichero existente entonces debemos utilizar el operador `>>`.
Redirigir la salida estándar no redirige la salida de error. Si queremos redirigir la salida de error entonces el operador a utilizar es `2>`.
 
```bash
vgonzalez165@PORTATIL:~$ printf '%s\n%v\n' OK? Ooops!
OK?
-bash: printf: `v': invalid format character
vgonzalez165@PORTATIL:~$ printf '%s\n%v\n' OK? Ooops! > FILE 2> ERRORFILE
vgonzalez165@PORTATIL:~$ cat ERRORFILE
-bash: printf: `v': invalid format character
vgonzalez165@PORTATIL:~$ cat FILE
OK?
```

En este caso, los mensajes de error son enviados a un fichero especial, `/dev/null`, cuyo cometido únicamente es descartar todo lo que se copie en él.
 
```
vgonzalez165@PORTATIL:~$ printf '%s\n%v\n' OK? Ooops! 2> /dev/null
OK?
```

En lugar de enviar una salida a un fichero, puede ser redireccionada a otro flujo de entrada/salida usando `>&N` donde `N` es el número del descriptor de fichero. Este comando envía tanto la salida estándar como la de error al fichero FILE.

```
vgonzalez165@PORTATIL:~$ printf '%s\n%v\n' OK? Ooops! >FILE 2>&1
```

Aquí el orden es importante. La salida estándar es enviada a FILE, y la salida de error es redireccionada a donde vaya la salida estándar. Si se invierte el orden, el efecto es diferente. La redirección envía la salida de error a donde esté yendo actualmente la salida estándar y a continuación cambia la salida estándar.

```
vgonzalez165@PORTATIL:~$ printf '%s\n%v\n' OK? Ooops! 2>&1 >FILE
-bash: printf: `v': invalid format character
```

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
 
```
vgonzalez165@PORTATIL:~$ cat << END > datos
> hola mundo
> END
vgonzalez165@PORTATIL:~$ cat datos
hola mundo
```


### 4.1.2.- Tuberías

Las **tuberías** (**pipelines**) conectan la salida estándar de un comando con la entrada estándar de otro. El símbolo barra (`|`) debe ser utilizado entre los dos comandos.
 
 ```
 vgonzalez165@PORTATIL:~$ printf "%s\n" "$RANDOM" "$RANDOM" "$RANDOM" "$RANDOM" | sort
11215
13599
22262
27839
```


## 4.2.- Comandos para trabajar con datos

### 4.2.1.- Ordenando. Comando `sort`

El comando `sort` ordena las líneas de un fichero de texto utilizando las reglas de ordenación según el lenguaje del sistema.

```
vgonzalez165@PORTATIL:~$ cat fichero1
uno
dos
tres
cuatro
cinco
vgonzalez165@PORTATIL:~$ sort fichero1
cinco
cuatro
dos
tres
uno
```

Sin embargo, si los valores que contiene el fichero son numéricos el resultado puede no ser el esperado:

```
vgonzalez165@PORTATIL:~$ cat fichero2
1
2
100
45
3
10
45
75
vgonzalez165@PORTATIL:~$ sort fichero2
1
10
100
2
3
45
45
75
```

Si sabemos que el primer valor de cada fila es un número y queremos que lo trate como un número al ordenarlo en lugar de como un carácter debemos utilizar el parámetro `–n`

```
vgonzalez165@PORTATIL:~$ sort fichero2 -n
1
2
3
10
45
45
75
100
```

Muchos ficheros de *log* en Linux comienzan cada línea por el mes en que se ha producido la entrada. Si queremos que el comando `sort` nos reconozca estos caracteres como un mes deberemos utilizar el parámetro `–M`

Hay otros muchos parámetros que se pueden ver en la página del manual.

Los parámetros `–k` y `–t` son muy útiles para ordenar ficheros de datos que usan campos, tales como el fichero `/etc/passwd`. El parámetro –t servirá para especificar el carácter que separará los campos y el parámetro `–k` especifica el campo que se utilizará para ordenarlo.

Por ejemplo, el siguiente comando ordenará el fichero según el identificador del usuario, que por cierto es un valor numérico.

```
    vgonzalez@PORTATIL:~$ sort -t ':' -k 3 -n /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
```
 
### 4.2.2.- Filtrando. Comando `grep`

Si queremos buscar una línea que contenga un determinado valor en un fichero deberemos utilizar la orden `grep`. El formato para el comando grep es el siguiente:

```
    grep [opciones] patrón [fichero]
```

El comando `grep` busca en el fichero indicado como entrada aquellas líneas que contengan los caracteres que coincidan con el patrón indicado

``` 
    vgonzalez@PORTATIL:~$ cat fichero1
    uno
    dos
    tres
    cuatro
    cinco
    vgonzalez@PORTATIL:~$ grep cuatro fichero1
    cuatro
    vgonzalez@PORTATIL:~$ grep c fichero1
    cuatro
    cinco
```


Este comando admite multitud de parámetros.

- El parámetro `–v` nos permite invertir la búsqueda, es decir, mostrar aquellas líneas que no incluyan el patrón indicado.
- Con el parámetro `–n` mostraremos el número antes de cada línea. 
- El parámetro `–c` contará el número de líneas que contienen el patrón.
- Se puede especificar más de un patrón mediante el parámetro `–e`. Por ejemplo, la orden `grep –e t –e f fichero1` busca todas las líneas que contengan el carácter `t` o el carácter `f`.
- 
También admite **expresiones regulares** al indicar el patrón a buscar. Las expresiones regulares ya se verán más adelante, pero a modo de avance un ejemplo puede ser el siguiente:

```
    vgonzalez@PORTATIL:~$ grep ^[cu] fichero1
    uno
    cuatro
    cinco
```

### 4.2.1.- Contando. Comando `wc`

El comando `wc` (*word count*) se limita a contar el número de líneas, palabras y caracteres del texto que reciba a la entrada, bien sea desde un fichero o redireccionado desde otro comando mediante una tubería.

```bash
    victor@ubuntu:~$ cat /usr/share/dict/spanish | wc
    86016   86016  852190
```

También se le puede pasar uno o varios ficheros como parámetro.

```bash
    victor@DESKTOP-483UVTV:~$ wc /usr/share/dict/words /usr/share/dict/spanish
    86016   86016  852190 /usr/share/dict/words
    86016   86016  852190 /usr/share/dict/spanish
    172032  172032 1704380 total
 ```

 #### Modificadores del comando `wc`

| Modificador         | Descripción |
| ------------------- | ----------- |
| `-w` | Cuenta solo el número de palabras  |
| `-l` | Cuenta solo el número de líneas |
| `-c` | Cuenta solo el número de caracteres |
| `-L` | Devuelve el número de caracteres de la línea más larga del fichero |


### 4.2.3.- Cortando. Comando `cut`

El comando sirve para **cortar** secciones de cada línea de la entrada y enviarlas a la salida. Puede servir para cortar partes de una línea por posición de cada caracter o por campos. Siempre se debe incluir el modificador que indique el criterio.


 #### Modificadores del comando `wc`

| Modificador         | Descripción |
| ------------------- | ----------- |
| `-b` | Indica qué bytes se cortan. Se pueden indicar valores absolutos separados por comas o rangos separándolos por un guión. Ojo con la codificación |
| `-c` |   |
| `-f field` | Útil cuando el fichero está tiene una serie de campos separados por un carácter (p.e. CSV). Indica qué campo hay que devolver |
| `-d delimiter` | Se usa junto con el anterior e indica el delimitador de campo  |
| `--output-delimiter` | Usado junto con los anteriores, sirve para modificar el separador en la salida   |


#### Ejemplos de uso del comando `wc`

El fichero `/etc/passwd` tiene una serie campos separados por el carácter `:`, correspondiendo el primero de ellos con el nombre de cada usuario del sistema. Muestra el listado de usuarios del sistema.

```bash
    victor@ubuntu:~$ cat /etc/passwd | cut -f 1 -d ":"
    root
    daemon
    bin
    sys
    sync
    games
```


### 4.2.4.- Y reemplazando. Comando `tr`

El comando `tr` (*translate*)en Linux es una utilidad que permite trasladar o borrar caracteres de la entrada. Incluye un gran número de transformaciones como mayúsculas a minúsculas, comprimir caracteres repetidos, borrar caracteres específicos y tareas básicas de búsqueda y sustitución.

 #### Modificadores del comando `tr`

| Modificador         | Descripción |
| ------------------- | ----------- |
| `-s` | Elimina las ocurrencias repetidas del carácter indicado |
| `-d` | Elimina las ocurrencias del carácter indicado |
| `-c` | Complementa el conjunto indicado en el modificador anterior |



#### Ejemplos de uso del comando `tr`

Convertir todos los caracteres de mayúsculas a minúsculas. Se puede hacer de cualquiera de estas dos formas.

```bash
    victor@ubuntu:~$ echo "Victor J. González" | tr "[A-Z]" "[a-z]"
    victor j. gonzález
    victor@ubuntu:~$ echo "Victor J. González" | tr "[:upper:]" "[:lower:]"
    victor j. gonzález
```

Reemplazar unos caracteres por otros.

```bash
    victor@ubuntu:~$ echo "password" | tr 'aeios' '43105'
    p455w0rd
```

Eliminar caracteres repetidos (*squeeze*).

```bash
    victor@ubuntu:~$ echo "Victor      J.   González" | tr -s ' '
    Victor J. González
```

Eliminar todas las mayúsculas de la entrada.

```bash
    victor@ubuntu:~$ echo "Victor J. González" | tr -d "[:upper:]"
    ictor . onzález
```

Elimina todos los caracteres de la entrada salvo los dígitos.

```bash
    victor@ubuntu:~$ echo "Mi clave es 1234" | tr -cd "[:digit:]"
    1234
```

### 4.2.4.- Eliminando líneas repetidas. Comando `uniq`



***
>>>>>>> 1ddf28e06ecd51af5460ba4ca752a65b335d7af6
[Volver al índice principal](index_UT10.md)