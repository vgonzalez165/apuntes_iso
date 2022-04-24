<link rel="stylesheet" href="../styles.css">

![Carátula UT10](imgs/caratula_ut10.png)

## Contenidos

1. [Introducción a Linux](01_introduccion.md)
2. [Primeros pasos con Linux](02_primeros_pasos.md)
3. [El sistema de ficheros](03_sistema_ficheros.md)
4. [Trabajando con datos textuales](04_comandos_texto.md)
5. [**Expresiones regulares con el comando `sed`**](05_expresiones_regulares.md)


## Índice del apartado 5


- [5.1.- El editor `sed`](#51--el-editor-sed)
  - [5.1.1.- Introducción](#511--introducción)
  - [5.1.2.- Utilizando el editor en la línea de comandos](#512--utilizando-el-editor-en-la-línea-de-comandos)
  - [5.1.3.- Múltiples comandos en la misma línea](#513--múltiples-comandos-en-la-misma-línea)
  - [5.1.4.- Obtención de los comandos de un fichero](#514--obtención-de-los-comandos-de-un-fichero)
  - [5.1.5.- Más opciones de sustitución](#515--más-opciones-de-sustitución)
  - [5.1.6.- Direccionamiento de líneas](#516--direccionamiento-de-líneas)
  - [5.1.7.- Eliminación de líneas](#517--eliminación-de-líneas)
  - [5.1.8.- Insertando y añadiendo texto](#518--insertando-y-añadiendo-texto)
  - [5.1.9.- Cambio de líneas](#519--cambio-de-líneas)
  - [5.1.10.- El comando transformar](#5110--el-comando-transformar)
  - [5.1.11.-Referencia con el carácter &](#5111-referencia-con-el-carácter-)
  - [5.1.12.- Agrupación y referencias en `sed`](#5112--agrupación-y-referencias-en-sed)
- [5.2.- Expresiones regulares](#52--expresiones-regulares)
  - [5.2.1.- Tipos de expresiones regulares](#521--tipos-de-expresiones-regulares)
  - [5.2.2.- Motor BRE](#522--motor-bre)
    - [5.2.2.1- Texto plano](#5221--texto-plano)
    - [5.2.2.2- Caracteres especiales](#5222--caracteres-especiales)
    - [5.2.2.3.- Caracteres ancla](#5223--caracteres-ancla)
    - [5.2.2.4.- El carácter punto](#5224--el-carácter-punto)
    - [5.2.2.5.- Clases de caracteres](#5225--clases-de-caracteres)
    - [5.2.2.6.- Clases de caracteres especiales](#5226--clases-de-caracteres-especiales)
    - [5.2.2.7.- El asterisco](#5227--el-asterisco)
  - [5.2.3.- Motor ERE](#523--motor-ere)
    - [5.2.3.1.- La interrogación](#5231--la-interrogación)
    - [5.2.3.2.- El carácter barra (`|`). Alternancia](#5232--el-carácter-barra--alternancia)
    - [5.2.3.3.- El carácter más (`+ `)](#5233--el-carácter-más--)
    - [5.2.3.4.- Las llaves](#5234--las-llaves)
    - [5.2.3.5.- Los paréntesis](#5235--los-paréntesis)
    - [5.2.3.6.- Los símbolos `<` y `>`](#5236--los-símbolos--y-)
    - [5.2.3.7.- Clases](#5237--clases)
    - [5.2.3.8.- Resumen de expresiones regulares](#5238--resumen-de-expresiones-regulareas)


# 5.- EXPRESIONES REGULARES CON EL COMANDO `sed`

## 5.1.- El editor `sed`

### 5.1.1.- Introducción

El editor `sed` es lo que se conoce como un **editor en stream** (flujo), en contraposición a un editor normal interactivo. En un editor interactivo el usuario introduce de forma interactiva por el teclado las operaciones que quiere realizar con el texto. Por el contrario, un editor en stream procesa un flujo de datos en base a un conjunto de reglas establecidas de antemano, antes de que el editor comience a procesarlos.

El editor `sed` puede manipular datos en un flujo de datos introducido por el teclado o bien de un fichero de texto. Lee una línea de datos en cada momento de la entrada, encaja los datos con los comandos proporcionados al editor, cambia los datos en el flujo tal como se ha especificado y finalmente envía los datos procesados a la salida estándar. A continuación, pasa a procesar la siguiente línea de datos.

El formato para utilizar el comando `sed` es:

```bash
	sed opciones script fichero
```

El parámetro *opciones* permite personalizar el comportamiento del comando `sed` e incluye las opciones mostradas en la siguiente tabla:

| Opción      | Descripción                                                                 |
| ----------- | --------------------------------------------------------------------------- |
| `-e` 		  | Permite ejecutar más de un comando de `sed`                                 |
| `-f file`   | Añade los comandos especificados en `file` mientras se procesa la entrada   |
| `-n`		  | No produce salida para cada comando                                         |

El parámetro `script` especifica un único comando para aplicar a la cadena de datos. Los comandos de `sed` están identificados por un único carácter más una serie de apartados separados por el carácter `/`. Ya veremos cuál es la estructura completa de un comando, pero un ejemplo puede ser `'s/1/uno'` que indica que hay que reemplazar todas las ocurrencias del carácter `1` por la cadena `uno`.

Si es necesario más de un comando hay que utilizar la opción `–e` para indicarlo en el propio comando o la opción `–f` para indicarlo en un fichero aparte.


### 5.1.2.- Utilizando el editor en la línea de comandos

Por defecto, el editor `sed` aplica los comandos especificados al flujo de entrada en `stdin`. Por ejemplo:
 
```bash
	vgonzalez@ubuntu:~$ echo "Esto es una prueba" | sed 's/una prueba/un ejemplo/'
	Esto es un ejemplo
```

En este ejemplo hemos utilizado el comando `s` del editor `sed`. El comando `s` sustituye la cadena indicada entre las dos primeras barras por la cadena indicada entre las dos siguientes.

También se puede indicar como entrada un fichero y procesará todas las líneas del fichero.

```bash
	vgonzalez@ubuntu:~$ echo datos
	datos
	vgonzalez@ubuntu:~$ sed 's/Linux/GNU\/Linux/' datos
	Esto es una prueba de GNU/Linux
	Comandos básicos de GNU/Linux
	Introducción a GNU/Linux
```

Aprovechando el ejemplo anterior, observa como es necesario **escapar** el carácter barra (`/`) en la cadena de reemplazo anteponiéndole el símbolo barra invertida (`\`) ya que, en caso contrario, se interpretaría que ha finalizado la cadena de reemplazo.

 
### 5.1.3.- Múltiples comandos en la misma línea

Para utilizar varios comandos hay que utilizar la opción `–e`.

```bash
	vgonzalez@ubuntu:~$ cat datos
	uno
	dos
	tres
	cuatro
	cinco
	vgonzalez@ubuntu:~$ sed -e 's/uno/1/' -e 's/dos/2/' datos
	1
	2
	tres
	cuatro
	cinco
```

También es posible, y más cómodo, separar varios comandos utilizando el carácter punto y coma (`;`) como separador.
 
```bash
	vgonzalez@ubuntu:~$ sed -e 's/uno/1/;s/dos/2/' datos
	1
	2
	tres
	cuatro
	cinco
```

Otra alternativa que puede dar más claridad a la hora de mostrar la expresión es utilizar el **prompt secundario** del Shell insertando un salto de línea dentro de las comillas tal como se ve en el siguiente código.
  
```bash
	vgonzalez@ubuntu:~$ sed '
	> s/uno/1/
	> s/dos/2/
	> ' datos
	1
	2
	tres
	cuatro
	cinco
```


### 5.1.4.- Obtención de los comandos de un fichero

Si tienes muchos comandos de sed es más sencillo almacenarlos todos en un fichero y utilizar la opción –f para referenciarlos.

```bash
    vgonzalez@ubuntu:~$ cat script1
    s/prueba/muestra/
    s/de Linux/del editor sed/
    vgonzalez@ubuntu:~$ sed -f script1 datos
    Esto es una muestra del editor sed
    Esto es una muestra del editor sed
```
  

### 5.1.5.- Más opciones de sustitución

Cuando se utiliza el comando de sustitución para reemplazar una cadena por defecto **solo reemplaza la primera ocurrencia de dicha cadena en cada línea**.
 
```bash
    vgonzalez@ubuntu:~$ cat datos2
    el editor sed es más rápido que cualquier otro editor
    el editor sed es más rápido que cualquier otro editor
    vgonzalez@ubuntu:~$ sed 's/editor/programa/' datos2
    el programa sed es más rápido que cualquier otro editor
    el programa sed es más rápido que cualquier otro editor

```

Si queremos que reemplace todas las ocurrencias de la cadena en cada línea debes utilizar un flag de sustitución. Los flags de sustitución se indican a continuación de las cadenas.

```bash
    s/cadena/reemplazo/flags
```

Hay cuatro flags de sustitución diferentes:

- Un número, que indica cuál de las ocurrencias se sustituirá
- `g`: indica que serán reemplazadas todas las ocurrencias
- `p`: indica que el contenido de la línea original debe mostrarse también
- `w fichero`: escribe el resultado de la sustitución en un fichero
  
En este ejemplo se reemplaza la segunda ocurrencia de la cadena `editor`.

```bash
    vgonzalez@ubuntu:~$ sed 's/editor/programa/2' datos2
    el editor sed es más rápido que cualquier otro programa
    el editor sed es más rápido que cualquier otro programa
```
 
El **flag `p`** muestra por pantalla cada línea que modifique, es decir, cada línea que contenga la cadena a reemplazar. 
 
Como se puede ver en el ejemplo imprime las líneas en las que ha realizado la comprobación. Sin embargo, podemos ver que como por defecto va a imprimir dos veces todas las líneas en las que realice sustituciones.  Por ello se suele utilizar en combinación con la opción `–n` del comando `sed`. Esta opción elimina la salida por defecto de `sed`.
 
El **flag `w`** produce la misma salida, pero la almacena en un fichero.
 
También hay ocasiones en que la cadena a reemplazar puede contener caracteres cuyo uso puede resultar confuso. Un ejemplo son las cadenas que contengan la barra, como en el siguiente ejemplo, donde los caracteres especiales hay que *escaparlos*.
  
Para evitar estos problemas el editor `sed` permite utilizar otro carácter como separador de las cadenas. Algunos ejemplos de caracteres permitidos son `@ ! % | ; :`
  

### 5.1.6.- Direccionamiento de líneas

Por defecto, las órdenes indicadas en el editor sed se aplican a todas las líneas de los datos de entrada. Para ejecutar el comando únicamente sobre unas pocas líneas se debe usar el direccionamiento de líneas. Hay dos posibilidades:

- Indicando un rango numérico de líneas
- Indicando un patrón de texto que filtre las líneas

El formato para ambos casos es precediendo la orden con la expresión que determina las filas sobre las que se va a aplicar.

```
	[dirección] orden
```

Cuando utilizamos direccionamiento numérico de líneas es indicando el número de línea dentro del flujo de datos, teniendo en cuenta que **la primera línea es la línea 1**.

Se puede especificar **una única línea** o bien **un rango de líneas** indicando la primera línea, una coma y la última línea. En el primer ejemplo se aplica el comando sobre la segunda línea mientras que en el segundo se aplica a las segunda y tercera líneas.

```bash
	vgonzalez@ubuntu:~$ sed '2s/Administración/Implantación/' datos
	Administración de Sistemas Operativos
	Implantación de Sistemas Operativos
	Administración de Sistemas Operativos
	Administración de Sistemas Operativos
	vgonzalez165@PORTATIL:~$ sed '2,3s/Administración/Implantación/' datos
	Administración de Sistemas Operativos
	Implantación de Sistemas Operativos
	Implantación de Sistemas Operativos
	Administración de Sistemas Operativos
```
 
Si queremos aplicar una orden a varias líneas no consecutivas tendremos que hacerlo con varias órdenes utilizando el **modificador `-e`**.
 
```bash
	vgonzalez@ubuntu:~$ sed -e '1s/o/0/' -e '3s/e/3/' test
	un0
	dos
	tr3s
```

Si se quiere indicar que la orden se aplica desde una determinada línea hasta el final lo indicaremos mediante el carácter dólar (`$`).
 
```bash
	vgonzalez@ubuntu:~$ sed '2,$s/Administración/Implantación/' datos
	Administración de Sistemas Operativos
	Implantación de Sistemas Operativos
	Implantación de Sistemas Operativos
	Implantación de Sistemas Operativos
```

También podemos seleccionar líneas según un paso determinado utilizando el operador `~`. Este se utiliza precediéndole de un número que indica la primera línea en la que se aplicará la orden e indicando después de dicho carácter un número que indica el paso o salto que se aplicará.

Por ejemplo, si ponemos `1~3` la orden afectará a las líneas 1, 4, 7, 10, …. Otro ejemplo lo vemos en el código siguiente, donde se aplica la orden únicamente a las líneas impares.
 
```bash
	vgonzalez@ubuntu:~$ sed '1~2s/o/0/' test
	un0
	dos
	tres
	cuatro
	cinc0
```

La otra forma de restringir las líneas es indicando un patrón de texto que han de cumplir. El formato para hacer esto es:

```
	/patrón/orden
```

El patrón se encierra entre dos barras justo delante de la orden. En el siguiente ejemplo se indica que la sustitución solo se aplique a las líneas que contienen la palabra `vgonzalez`.

```bash
	vgonzalez@ubuntu:~$ sed '/vgonzalez/s/bash/csh/' /etc/passwd
```
  
Aunque esta opción pueda verse un poco limitada se verá que es una herramienta muy potente cuando se combina con el uso de **expresiones regulares** para indicar el patrón, tal y como veremos en el siguiente apartado.


### 5.1.7.- Eliminación de líneas

Pero el comando de sustitución no es el único comando del editor sed. Si lo que deseas es eliminar líneas del texto deberás utilizar el comando de borrado, indicado por el **carácter `d`**.
Este comando borra todas las líneas indicadas por el direccionamiento de líneas especificado. Hay que tener cuidado porque si no se especifica ningún direccionamiento de líneas borrará todas las líneas.

Se puede utilizar con una única línea:

```bash
	vgonzalez165@PORTATIL:~$ sed '3d' datos
	Esta es la línea uno
	Esta es la línea dos
	Esta es la línea cuatro
```

Con un rango de líneas:

```bash
	vgonzalez165@PORTATIL:~$ sed '2,3d' datos
	Esta es la línea uno
	Esta es la línea cuatro
```

O con un patrón de texto:
 
```bash
	vgonzalez165@PORTATIL:~$ sed '/dos/d' datos
	Esta es la línea uno
	Esta es la línea tres
	Esta es la línea cuatro
```
También podemos eliminar la última línea de la entrada utilizando el **carácter dólar** (`$`) de la siguiente forma.

```bash
	vgonzalez165@PORTATIL:~$ sed '$d' datos
	Esta es la línea uno
	Esta es la línea dos
	Esta es la línea tres
```

También se pueden eliminar todas las líneas menos las indicadas mediante el **carácter exclamación**, que se indica después del rango de líneas.

```bash
	vgonzalez165@PORTATIL:~$ sed '2!d' datos
	Esta es la línea dos
	vgonzalez165@PORTATIL:~$ sed '2,3!d' datos
	Esta es la línea dos
	Esta es la línea tres
```


### 5.1.8.- Insertando y añadiendo texto

Al igual que cualquier otro editor, el editor sed permite insertar y añadir texto al flujo de datos. La diferencia entre estas dos acciones puede ser confusa:

- El comando **insertar** (`i`) añade una nueva línea antes de la línea especificada
- El comando **añadir** (`a`) añade una nueva línea después de la línea especificada.

Hay que tener mucho cuidado con la sintaxis de este comando ya que la línea a insertar o añadir se tiene que indicar en una línea diferente, es decir, hay que pulsar `<enter>` antes de introducir la línea. También hay que observar que después del comando `i` hay que utilizar el carácter barra invertida (`\`)

```bash
	vgonzalez@ubuntu:~$ sed 'i\
	> Esto es una prueba' datos
	Esto es una prueba
	Esta es la línea 1
	Esto es una prueba
	Esta es la línea 2
```
 
Si solo ponemos el carácter `i` añadiremos la línea indicada antes de cada una de las líneas del flujo de datos. Si queremos ponerlo antes de una línea determinada debemos utilizar el direccionamiento de líneas.

```bash
	vgonzalez@ubuntu:~$ sed '3a\
	Esta es una línea insertada' datos
	Esta es la línea 1
	Esta es la línea 2
	Esta es la línea 3
	Esta es una línea insertada
	Esta es la línea 4
```
 
Si queremos añadir la línea al final del flujo de datos podemos utilizar **el carácter dólar (`$`)** para referenciar la última línea.
 
```bash
	vgonzalez@ubuntu:~$ sed '$a\
	> Esta línea es nueva' datos
	Esta es la línea 1
	Esta es la línea 2
	Esta es la línea 3
	Esta es la línea 4
	Esta línea es nueva
```


Si quieres añadir más de una línea de texto deberás poner al final de cada línea a añadir el carácter barra invertida (\) salvo en la última de todas.

```bash
	vgonzalez@ubuntu:~$ sed '1i\
	> Esta es una línea insertada\
	> Esta es otra.' datos
	Esta es una línea insertada
	Esta es otra.
	Esta es la línea 1
	Esta es la línea 2
	Esta es la línea 3
	Esta es la línea 4
```


### 5.1.9.- Cambio de líneas

El comando de **cambio de líneas**, indicado por el carácter `c`, permite cambiar el contenido de una línea de texto completa en el flujo de entrada. Funciona de la misma manera que los comandos insertar y añadir, en los que debes indicar la línea a insertar en una línea diferente.
 
```bash
	vgonzalez@ubuntu:~$ sed '3c\
	> Esta línea la cambiamos.' datos
	Esta es la línea 1
	Esta es la línea 2
	Esta línea la cambiamos.
	Esta es la línea 4
```

También se puede utilizar un patrón para indicar la línea a reemplazar.

```bash
	vgonzalez@ubuntu:~$ sed '/línea 2/c\
	> Hemos cambiado la segunda línea.' datos
	Esta es la línea 1
	Hemos cambiado la segunda línea.
	Esta es la línea 3
	Esta es la línea 4
```


Otra posibilidad es indicar un rango de líneas para reemplazar, pero hay que tener en cuenta que el comando no reemplazará cada una de las líneas por la línea indicada, sino que sustituirá todas las líneas dentro del rango por la línea indicada.
 
```bash
	vgonzalez@ubuntu:~$ sed '2,3c\
	> Esta es la línea nueva.' datos
	Esta es la línea 1
	Esta es la línea nueva.
	Esta es la línea 4
```



### 5.1.10.- El comando transformar

El **comando transformar**, indicado por el carácter `y`, es la única orden de `sed` que opera con caracteres individuales. La sintaxis de este comando es:

```
	[dirección]y/caracteres entrada/caracteres salida
```

El comando transformar realiza un **mapeado** uno a uno entre los caracteres de entrada y los de salida. El primer carácter de entrada es reemplazado por el primero de salida, el segundo por el segundo, … Las longitudes de las secuencias de caracteres tienen que ser iguales ya que en caso contrario el editor producirá un mensaje de error.

```bash
	vgonzalez@ubuntu:~$ sed 'y/123/ABC/' datos
	Esta es la línea A
	Esta es la línea B
	Esta es la línea C
	Esta es la línea 4
```

Este comando es global, lo que significa que la sustitución se aplicará sobre **todas las ocurrencias del carácter** y no puede limitarse a algunas en concreto.


### 5.1.11.-Referencia con el carácter &

En algunas ocasiones querremos localizar una cadena y reemplazarla por esa misma cadena, pero añadiéndole algo. Por ejemplo, supón que tenemos un documento de texto y queremos buscar todas las ocurrencias de mi nombre y rodearlas de comillas. En ese caso simplemente habría que poner una orden de la forma:

```bash
	$ sed 's/Victor/"Victor"/' fichero
```

En este caso la solución es muy sencilla, pero el problema surge cuando queremos utilizar una expresión regular (que veremos en el siguiente apartado) y queremos rodear con comillas todas las ocurrencias que se ajusten a dicha expresión regular.

En ese caso necesitaremos utilizar el **carácter ampersand (`&`)**, que, situado en la segunda parte de la orden, significa o es reemplazado por la cadena que se ajustó a la expresión regular.

Utilizando este carácter, el ejemplo anterior quedaría de la siguiente forma:

```bash
	$ sed 's/Victor/"&"/' fichero
```


### 5.1.12.- Agrupación y referencias en `sed`

Si queremos ir más allá de la utilización del carácter *ampersand*, podemos hacer uso de la agrupación y las referencias (*back-reference*) en nuestras órdenes de `sed`. Ambas características deben ser utilizadas conjuntamente y básicamente permiten reutilizar partes de la expresión regular y no la expresión regular completa como pasa cuando utilizamos el carácter *ampersand*.

La **agrupación** se consigue rodeando la parte de la expresión regular mediante paréntesis, pero que deberán estar escapados con el carácter contrabarra (`\`) dado que los paréntesis tienen significado en las expresiones regulares.

La **referencia** se consigue con el símbolo contrabarra seguido de un número en la parte final de la orden.

Veámoslo mejor con un ejemplo:

```bash
	$ sed 's/\(Victor\) González/"\1"/' fichero
```

En este caso buscamos todas las ocurrencias de la cadena `Victor González`. Observa que rodeamos con los paréntesis la cadena `Victor`, con lo que indicamos que querremos referenciar esa parte posteriormente. Eso lo hacemos en la segunda parte con la cadena `\1`. 

Con la orden anterior conseguiremos que, en todos los sitios que ponga `Victor González`, lo reemplace por `"Victor"`.

Si tenemos varias partes de la expresión regular entre paréntesis podremos referenciarlas con sucesivos números: `\1`, `\2`, `\3`, …


## 5.2.- Expresiones regulares

Una **expresión regular** es un patrón que defines para que una utilidad de Linux pueda filtrar texto.  Una utilidad Linux, como por ejemplo los editores `sed` y `awk` comprueba si el flujo de datos de entrada coincide con el patrón regular especificado, en caso afirmativo lo procesa y en caso contrario rechaza los datos.

Las expresiones regulares utilizan una serie de **caracteres comodín** para representar el patrón que se quiere establecer de forma similar a la utilización del asterisco en el comando `ls`.


### 5.2.1.- Tipos de expresiones regulares

El mayor problema con la utilización de expresiones regulares es que no hay un único conjunto de ellas, sino que diferentes aplicaciones pueden tener diferentes sintaxis para representar las expresiones. Estas pueden ser aplicaciones como lenguajes de programación (Java, Perl o Python), utilidades Linux (los editores `sed` y `awk` o la utilidad `grep`) y otras aplicaciones más complejas como los servidores de bases de datos MySQL o PostgreSQL.

Las expresiones regulares son implementadas utilizando un **motor de expresiones regulares** que es el software subyacente que se encarga de procesarlas. En el mundo Linux los motores de expresiones regulares más populares son:

- El motor POSIX Basic Regular Expression (BRE)
- El motor POSIX Extender Regular Expresión (ERE)
- 
La mayoría de las aplicaciones Linux por lo menos reconocen las especificaciones del motor BRE mientras que el motor ERE solo es utilizado por unas pocas aplicaciones que centrar su potencia en un gran soporte para expresiones regulares, tal como el editor avanzado `awk`.

Tanto `grep` como `sed` reconocen las expresiones que se ajustan al motor BRE, sin embargo, necesitan que se les indique explícitamente si se utilizan expresiones del juego extendido. Esto se consigue con el uso del modificador `-E`.


### 5.2.2.- Motor BRE

#### 5.2.2.1- Texto plano

La forma más sencilla de un patrón regular es simplemente indicar un carácter o conjunto de caracteres que han de aparecer en el texto a procesar.

```bash
	vgonzalez@ubuntu:~$ echo 'Esto es una prueba' | sed -n '/prueba/p'
	Esto es una prueba
	vgonzalez@ubuntu:~$ echo 'Esto es una prueba' | sed -n '/test/p'
	vgonzalez@ubuntu:~$
```


El primer patrón define la palabra `prueba`. Por lo tanto, lo que hace el comando indicado es buscar en la entrada (la cadena `Esto es una prueba`) aquellas líneas que contengan dicha palabra y mostrarlas por pantalla ya que esa es la orden indicada en el comando `sed`.

En la segunda línea el patrón viene dado por la palabra `test`. Como en la entrada no hay ninguna línea que contenga dicha palabra el comando `sed` no procesará ninguna línea.

Hay que tener en cuenta que los patrones regulares, al igual que en prácticamente todos los ámbitos de Linux, se consideran diferentes caracteres las mayúsculas y minúsculas.

También hay que tener en cuenta que no es necesario ceñirse a caracteres alfabéticos a la hora de especificar las expresiones regulares. También se pueden indicar dígitos u otros símbolos como puede ser el espacio.


#### 5.2.2.2- Caracteres especiales

Hay una serie de **caracteres especiales** que tienen un significado específico en las expresiones regulares, por lo que si los incluimos en un patrón buscando dichos caracteres el resultado no será el esperado. Estos caracteres especiales son:

```
. * [ ] ^ $ { } \ + ? | ( )
```

Si quieres buscar uno de estos caracteres como carácter de texto será necesario **escaparlo**. Para escapar un carácter tienes que anteponerle un carácter especial que le indicará al motor de expresiones regulares que el carácter que le sigue ha de interpretarse como un carácter normal. El carácter que se utiliza para escapar otros caracteres es la **barra invertida (`\`)**.

Por ejemplo, si quieres buscar el símbolo dólar en un texto tendrás que precederlo con la barra invertida de la siguiente manera:

```bash
	vgonzalez@ubuntu:~$ cat datos
	Esto cuesta 5$
	vgonzalez@ubuntu:~$ sed -n '/\$/p' datos
	Esto cuesta 5$
```
  
Dado que el carácter barra invertida también es un carácter especial si queremos buscarlo en el patrón también debemos precederlo de otra barra invertida.


#### 5.2.2.3.- Caracteres ancla

Por lo que hemos visto hasta ahora las cadenas a buscar pueden estar en cualquier posición dentro de la cadena. Sin embargo, también podemos especificar que las cadenas se encuentren al principio o al final de la cadena. Esto se hace mediante los caracteres ancla, que son `^` y `$`.

El carácter `^` define que el patrón **comienza al principio de la línea** de texto en el flujo de datos de entrada. Si el patrón se encuentra en una posición diferente la expresión regular fallará.

Para utilizarlo hay que ponerlo **antes del patrón** especificado en la expresión regular:

```bash
	vgonzalez@ubuntu:~$ echo '1º de ASIR' | sed -n '/^ASIR/p'
	vgonzalez@ubuntu:~$ echo 'ASIR 1' | sed -n '/^ASIR/p'
	ASIR 1
```

Si poner el carácter `^` en algún lugar que no sea el primer carácter del patrón, el motor de expresiones regulares lo interpretará como un carácter normal en lugar de un carácter especial, por lo que no será necesario escaparlo.
 
```bash
	vgonzalez@ubuntu:~$ echo "Esto ^ es una prueba" | sed -n '/o ^/p'
	Esto ^ es una prueba
```

Por otro lado, para especificar que un patrón se ha de encontrar al **final de la cadena** debemos utilizar el símbolo dólar (`$`). Añadir este carácter al final del patrón del texto indica que la línea de datos debe terminar con el patrón de texto indicado.

Hay un par de situaciones en las que se pueden combinar ambos caracteres ancla. La primera sería si queremos buscar aquellas líneas que coincidan exactamente con la cadena especificada.

```bash
	vgonzalez@ubuntu:~$ cat datos
	ASIR
	1º ASIR
	2º ASIR
	vgonzalez@ubuntu:~$ sed -n ' /^ASIR$/p' datos
	ASIR
```
  
La otra situación es muy útil cuando queremos eliminar las líneas en blanco de un texto de entrada y sería de la siguiente manera.

```bash
	vgonzalez@ubuntu:~$ cat datos
	Esta línea está antes de una línea en blanco

	y esta después
	vgonzalez@ubuntu:~$ sed '/^$/d' datos
	Esta línea está antes de una línea en blanco
	y esta después
```



#### 5.2.2.4.- El carácter punto

El **carácter especial punto (`.`)** sirve para coincidir con un único carácter excepto el carácter de nueva línea.

```bash
	vgonzalez@ubuntu:~$ cat datos
	esta es la línea 1
	es aquí la línea 2
	la línea tres antes que la cuatro
	por último la 4 tras la tres
	vgonzalez@ubuntu:~$ sed -n '/.es/p' datos
	esta es la línea 1
	la línea tres antes que la cuatro
	por último la 4 tras la tres
```

Fíjate en el ejemplo que el carácter espacio (primera línea) también es válido, mientras que en la segunda línea falla el patrón porque no hay ningún carácter delante de la cadena `es`.


#### 5.2.2.5.- Clases de caracteres

El carácter punto es muy útil para determinar un carácter cualquiera en una posición, pero ¿qué pasa cuando queremos limitar los caracteres que pueden estar en dicha posición?  Para esto disponemos de las clases de caracteres.

Para definir una **clase de caracteres** debemos utilizar los corchetes (`[]`). Dentro de ellos deberá estar cualquier carácter que pueda estar en esa posición como se puede ver en el siguiente ejemplo.

```bash
	vgonzalez@ubuntu:~$ cat datos
	esta es la línea 1
	y aquí la línea 2
	la 3 aquí la ves
	por último la 4 tras la tres
	vgonzalez@ubuntu:~$ sed -n '/[ r]es/p' datos
	esta es la línea 1
	por último la 4 tras la tres
```
  
También puedes utilizar más de una clase de caracteres dentro de una expresión.

```bash
	vgonzalez@ubuntu:~$ echo "Si" | sed -n '/[Ss][Ii]/p'
	Si
	vgonzalez@ubuntu:~$ echo "SI" | sed -n '/[Ss][Ii]/p'
	SI
```
  
En el ejemplo anterior nos aseguramos de que se detecte la cadena ‘si’ con cualquier combinación de mayúsculas y minúsculas.

También es posible utilizar las clases de caracteres para **negar la presencia de determinados caracteres**. Es decir, en lugar de buscar el patrón que coincida con un determinado carácter buscar un patrón que no tenga un determinado carácter. Esto se hace utilizando el símbolo `^`.

```bash
	vgonzalez@ubuntu:~$ cat datos
	esta es la línea 1
	y aquí la línea 2
	la 3 aquí la ves
	por último la 4 tras la tres
	vgonzalez@ubuntu:~$ sed -n '/[^vr]es/p' datos
	esta es la línea 1
```
 
Sin embargo, este mecanismo puede bastante engorroso cuando se quiere que la cadena encaje con muchos caracteres, por ejemplo, para hacer que sea un dígito habría que ponerlo de la forma `[0123456789]`. Para solucionar este problema podemos utilizar los **rangos**. Para especificar un rango debemos utilizar el **carácter guion (`-`)**. De esta forma la expresión `[0-9]` será equivalente a `[0123456789]`. En una misma expresión también se pueden especificar varios rangos no contiguos, por ejemplo, `[a-fm-v]`.


#### 5.2.2.6.- Clases de caracteres especiales

Cuando un rango contiene sólo un tipo de caracteres, BRE contiene clases de caracteres especiales que nos facilitarán la tarea. Las clases especiales de caracteres se pueden ver en la siguiente tabla.

| Clase | Descripción |
| ----- | ----------- |
| [[:alpha:]] | Cualquier carácter alfabético, tanto mayúsculas como minúsculas |
| [[:alnum:]] | Cualquier carácter alfanumérico, números y letras mayúsculas o minúsculas |
| [[:blank:]] | Un carácter espacio o de tabulación |
| [[:digit:]] | Un dígito del 0 al 9 |
| [[:lower:]] | Un carácter alfabético en minúscula |
| [[:print:]] | Un carácter imprimible |
| [[:punct:]] | Un símbolo de puntuación |

Su utilización se puede ver en este ejemplo:

```bash
	vgonzalez@ubuntu:~$ echo 'abc' | sed -n '/[[:digit:]]/p'
	vgonzalez@ubuntu:~$ echo 'abc' | sed -n '/[[:alpha:]]/p'
	abc
```
 

#### 5.2.2.7.- El asterisco

Si en una expresión regular ponemos un **asterisco (`*`) detrás de un carácter** significa que **este carácter debe aparecer cero o más veces en la cadena**.

Ten cuidado porque no es igual que el uso del carácter asterisco en Bash donde significa 0 o más caracteres cualesquiera. Aquí significa **0 o más repeticiones del carácter que le precede**.

```bash
	vgonzalez@ubuntu:~$ echo "hla" | sed -n '/ho*l/p'
	hla
	vgonzalez@ubuntu:~$ echo "hola" | sed -n '/ho*l/p'
	hola
	vgonzalez@ubuntu:~$ echo "hoooooola" | sed -n '/ho*l/p'
	hoooooola
```
  
Una posibilidad también es combinar el carácter punto con el carácter asterisco para hacer coincidir cero o más coincidencias de cualquier carácter.

```bash
	vgonzalez@ubuntu:~$ echo "esto es una expresión regular" | sed -n '/una.*regular/p'
	esto es una expresión regular
```

El carácter asterisco también se puede combinar con clases de caracteres, lo que nos permitirá especificar un grupo o rango de caracteres que deberán tener varias ocurrencias.

```bash
	vgonzalez@ubuntu:~$ echo "bt" | sed -n '/b[ae]*t/p'
	bt
	vgonzalez@ubuntu:~$ echo "bat" | sed -n '/b[ae]*t/p'
	bat
	vgonzalez@ubuntu:~$ echo "beaeaeeat" | sed -n '/b[ae]*t/p'
	beaeaeeat
```


### 5.2.3.- Motor ERE

Este motor dispone de mayor número de opciones, pero debemos indicar expresamente que vamos a usar este juego utilizando el modificador `-E` en los comandos `grep` y `sed`.


#### 5.2.3.1.- La interrogación

El **carácter interrogación (`?`)** se aplica al carácter que le precede de forma similar a como lo hace el carácter asterisco. En este caso significa que el carácter anterior **puede aparecer 0 ó 1 veces**.


#### 5.2.3.2.- El carácter barra (`|`). Alternancia

El **carácter barra (`|`)** sirve para separar varias expresiones regulares y empareja con cualquiera de ellas.

```bash
	vgonzalez@ubuntu:~$ cat fichero
	uno
	dos
	tres
	cuatro
	cinco
	seis
	vgonzalez@ubuntu:~$ sed -nE '/uno|dos/p' fichero
	uno
	dos
```
 

#### 5.2.3.3.- El carácter más (`+ `)

El **carácter más (`+`)** se aplica al carácter que le precede, al igual que nos pasaba con los caracteres asterisco e interrogación, y significa que **ese carácter se repite una o más veces**.


#### 5.2.3.4.- Las llaves

Nuevamente tenemos una expresión que afecta al carácter inmediatamente anterior. Entre las llaves debe ir un valor numérico que **indica exactamente el número de ocurrencias del carácter anterior**.

Hay que tener cuidado porque las llaves también se pueden utilizar en el motor básico, pero en ese caso **sí es necesario escaparlas**. Recuerda que para indicarle al comando `sed` que deseamos utilizar el motor ERE debemos utilizar el modificador `-E`.

```bash
	vgonzalez@ubuntu:~$ cat fichero
	Matrículas
	2346KSD
	8332LNK
	9584HFS
	vgonzalez@ubuntu:~$ sed -nE '/[0-9]{4}[A-Z]{3}/p' fichero
	2346KSD
	8332LNK
	9584HFS
	vgonzalez@ubuntu:~$ sed -n '/[0-9]\{4\}[A-Z]\{3\}/p' fichero
	2346KSD
	8332LNK
	9584HFS
```
 
También se pueden utilizar rangos dentro de las llaves separando dos valores numéricos por el símbolo coma.

```bash
	vgonzalez@ubuntu:~$ cat fichero
	NIFs
	71892777J
	10199189K
	2456723A
	vgonzalez@ubuntu:~$ sed -n '/[0-9]{7,8}[A-Z]{1}/p' fichero
	vgonzalez@ubuntu:~$ sed -nE '/[0-9]{7,8}[A-Z]{1}/p' fichero
	71892777J
	10199189K
	2456723A
```

#### 5.2.3.5.- Los paréntesis

Estos símbolos sirven para **agrupar partes de una expresión regular**, normalmente con idea de que otro símbolo afecte globalmente a todo lo incluido dentro de los paréntesis.

```bash
	vgonzalez@ubuntu:~$ cat fichero
	abba
	ababa
	aa
	abbbbbbba
	vgonzalez@ubuntu:~$ sed -nE '/(ab){2}a/p' fichero
	ababa
```
 
En la orden anterior, se buscan todas las ocurrencias en las que la cadena de texto ab se repita exactamente 2 veces.


#### 5.2.3.6.- Los símbolos `<` y `>`

Estos símbolos representan **el inicio y el fin de palabra respectivamente**. Cuando hablamos de principio o fin de palabra nos referimos a que antes o después de la expresión regular haya cualquier carácter no alfabético (símbolos de puntuación, espacios o tabuladores, dígitos, …) o bien el comienzo o fin de línea.

Ahora bien, para utilizar estos símbolos en una expresión regular es necesario **escaparlos** ya que si no son tomados literalmente como los símbolos menor y mayor.

```bash
	vgonzalez@ubuntu:~$ cat fichero
	<uno> uno tuno
	vgonzalez@ubuntu:~$ sed 's/<uno>/XXX/g' fichero
	XXX uno tuno
	vgonzalez@ubuntu:~$ sed 's/\<uno\>/XXX/g' fichero
	<XXX> XXX tuno
```
 
Por ejemplo, en el primer comando de la imagen anterior se busca literalmente la cadena `<uno>`, mientras que en el segundo comando se buscan todas las ocurrencias de la cadena `uno` en las que se muestre como una palabra completa.


#### 5.2.3.7.- Clases

Además de las clases que vimos en el motor básico, el motor ERE añade más clases especiales representadas por un carácter escapado. Estas clases se pueden ver en la tabla del siguiente apartado.


#### 5.2.3.8.- Resumen de expresiones regulareas

En las siguientes tablas, extraídas de https://sio2sio2.github.io/doc-linux/02.conbas/10.texto/01.regex.html tienes un resumen que te puede ser muy útil para utilizar expresiones regulares.

**Patrones básicos**

| Operación      | Operador BRE | Operador ERE  | Descripción | Ejemplo (ERE) |
| -------------- | ------------ | ------------- | ----------- | ------------- |
| Cuantificación | `\?` 		| `?` 			| Una vez o ninguna | `a?` |
| Cuantificación | `*`			| `*`			| Las veces que sea (incluso ninguna) | `a*`|
| Cuantificación | `\+`			| `+`			| Al menos una vez | `a+` |
| Cuantificación | `\{n,m\}`	| `{n,m}`		| Entre `n` y `m` veces. Puede omitirse uno de los límites | a{5,9} |
| Agrupación 	 | 				| `(?regex)`	| Para modificar el alcance de un operador | `(?123)+`  |
| Alternativa 	 | `\|`			| `|`			| O lo uno o lo otro | `(?Blas|Luis)` | 
| Principio		 | `^`			| `^`			| El patrón comienza | `^a` |
| Fin			 | `$`			| `$`			| El patrón acaba	 | `a$` |
| Repr. universal| `.`			| `.`			| Cualquier carácter | `.{2,3}`       
| Escape		 | `\`			| `\`			| No interpretar un carácter especial | `\.`  |
	

**Patrones avanzados**

| Operación      | Operador BRE | Operador ERE  | Descripción | Ejemplo (ERE) |
| -------------- | ------------ | ------------- | ----------- | ------------- |
| Alternativa	 | `[...]`		| `[...]`		| Uno de los caracteres incluidos. Puede ser un rango | `[A-Za.z]` |
| Alternativa	 | `[^...]`		| `[^...]`		| Un carácter que no sea de los incluidos | `^A-Z` |
| Clases		 |				| `\w`			| Un carácter de palabra (letra, dígito o subrayado) | `\w+` |
| Clases		 |				| `\W`			| Un carácter que no sea de palabra | `^\W` | 
| Clases		 |				| `\d`			| Un dígito, es decir, [0-9] | `\d{4}` |
| Clases		 |				| `\D`			| Un carácter que no sea un dígito | `\D+` |
| Clases		 |				| `\s`			| Un carácter de espaciado, es decir `[\t\r\b\f]` | `\w+\s\w+` |
| Clases		 |				| `\S`			| Un carácter que no sea de espaciado | `\S+\s` |
| Clases		 |				| `[:nombre:]`	| Un carácter de la clase nombre | `[[:alpha:],;.]+` |
| Clases		 |				| `[=x=]`		| Cualquier variante del carácter `x` | `[[=a]]` |
| Límite de palabra |			| `\b`			| Principio o fin de palabra | `\ndado\b` |
| Grupos		 | `\{regex\}`  | `(regex)`		| Captura un grupo | |
| Grupos		 | `\1`, `\2`,..| `\1`, `\2`,.. | Refiere un grupo previamente capturado | `(\w+)\s+\1` |

