---
layout: default
title: Uso del sistema de ficheros desde Powershell
parent: UT06. Powershell
nav_order: 9
---

# 9.- Uso del sistema de ficheros desde Powershell

Tabla resumen de comandos

| Comando           | Alias         | Descripción                                   |
|-------------------|---------------|-----------------------------------------------|
| `Add-Content`     | `ac`            | Añade contenido a un fichero                  |
| `Clear-Host`      | `cls`, `clear`    | Borra la pantalla                             |
| `Clear-Item`      | `cli`           | Borra el contenido de un fichero, pero no el propio fichero |
| `Copy-Item`       | `copy`, `cp`, `cpi` | Copia un fichero o directorio                 |
| `Get-ChildItem`   | `dir`, `ls`, `gci`  | Muestra el contenido de un directorio         |
| `Get-Content`     | `type`, `cat`, `gc` | Muestra el contenido de un fichero            |
| `Get-Item`        | `gi`            | ????????????????????????                      |
| `Get-ItemProperty`| `gp`            | Obtiene una propiedad de un fichero o directorio |
| `Join-Path`       | -             | Une dos partes de una ruta, p.e. una unidad de disco y un nombre de fichero |
| `Move-Item`       | `mi`, `mv`, `move`  | Mueve ficheros y directorios                  |
| `New-Item`        | `ni`            | Crea un fichero o directorio                  |
| `Remove-Item`     | `ri`, `rm`, `rmdir`, `del`, `erase`, `rd` | Borra un fichero o directorio |
| `Rename-Item`     | `ren`, `rni`      | Renombra un fichero o directorio              |
| `Set-Location`    | `cd`, `chdir` | Cambia el directorio de trabajo al directorio especificado |
| `Test-Path`       | -             | Devuelve True si la ruta indicada existe      |


## 9.1.- Navegación por el sistema de ficheros

### 9.1.1.- Directorio de trabajo

El sistema de ficheros en Windows es una estructura arborescente o jerárquica donde la raíz de dicho árbol es cada una de las unidades de almacenamiento que tengamos en el sistema, y cada nodo será, o bien un **fichero** o bien una **carpeta**. Los ficheros contienen datos, mientras que las carpetas o directorios son meros contenedores que sirven para almacenar en su interior tanto directorios como otras carpetas. 

Cuando abrimos una terminal de Powershell, siempre nos encontraremos en algún punto de ese árbol, que puede ser en el directorio raíz o bien en alguna de las carpetas del árbol. El directorio en el que nos entramos en cada momento se llama **directorio de trabajo** y es importante tener claro en todo momento cuál es, ya que las órdenes de Powershell que requieren un fichero como parámetro lo buscarán por defecto en el directorio de trabajo (salvo que se indique explícitamente la ruta).

Hay dos formas de saber cuál es el directorio de trabajo:

- Cuando estamos en la línea de comandos del Powershell siempre se muestra el directorio de trabajo actual en el **prompt**, el texto que aparece antes del cursor que espera que introduzcamos un comando.
- Con el comando `Get-Location` o su alias `pwd`.


### 9.1.2.- Rutas absolutas y relativas

La forma de representar los directorios es mediante la enumeración de todos los directorios que hay que atravesar desde el directorio raíz hasta el directorio indicado partiendo siempre desde la letra de la unidad en que se encuentra y separando los directorios con el símbolo barra invertida (`\`) o bien el símbolo barra (`/`). Windows no distingue entre mayúsculas y minúsculas, por lo que es indiferente utilizar unas u otras en las rutas.

Así, por ejemplo, la ruta `C:\Usuarios\victor\Documents` hace referencia al directorio `Documents`, que se encuentra dentro del directorio victor, que a su vez está dentro de `Usuarios` que está ubicado en la unidad de almacenamiento `C:`

Cuando indicamos una ruta de esta forma se dice que utilizamos **rutas absolutas**, ya que identifica unívocamente el directorio independientemente del directorio de trabajo en el que nos encontremos.

La alternativa es el uso de **rutas relativas**, en ellas se indica el camino que hay que seguir para llegar al directorio que se desea partiendo del directorio de trabajo actual, en lugar de hacerlo desde el directorio raíz. Estas rutas se identifican fácilmente porque **no comienzan por una letra de unidad**, y, para saber a qué directorio apuntan, debemos saber cuál es el directorio de trabajo actual.

Si por ejemplo, nuestro directorio de trabajo actual es `C:\Usuarios` y tenemos la ruta `vgonzalez\Documents`, sabemos que hace referencia al directorio `Documents`, que se encuentra dentro de `vgonzalez`, que a su vez está en el directorio `Usuarios` del disco `C:`. Un buen hábito es utilizar un directorio especial llamado `.` que se encuentra en todos los directorios y que apunta a él mismo para preceder las rutas locales, de forma que se enfatiza el hecho de que sea una ruta local y mejora la legibilidad. De esta forma, la ruta anterior la podíamos poner de la forma `.\vgonzalez\Documents`.

Cuando descendemos en el árbol de directorios, como en el ejemplo anterior, simplemente hay que poner los nombres de todos los directorios por los que se pasa. Sin embargo, hay ocasiones en que necesitaremos **subir por el árbol de directorios**. Para esos casos necesitamos poder hacer referencia al **directorio padre** de un directorio, y eso se consigue con otro directorio especial, presente en todos los directorios, denominado `..`.

Así, si por ejemplo estamos en el directorio `D:\apuntes\ISO\UT01` y queremos hacer referencia al directorio `D:\apuntes\ISO\UT02` mediante una ruta relativa, la indicaríamos de la forma `..\UT02`, que indica, *vete al directorio padre del actual y ahí entra al subdirectorio `UT02`*.

Por norma general es equivalente el uso de rutas absolutas o relativas, por lo que siempre haremos uso de aquellas que nos resulten más cortas o con las que nos encontremos más cómodos. Hay una excepción a esta norma, y es cuando utilicemos rutas dentro de ficheros, por ejemplo, scripts o ficheros de configuración. En esos casos **siempre debemos utilizar rutas absolutas**, ya que no podemos garantizar que en el momento en que se vaya a ejecutar el script o leer el fichero de configuración el directorio de trabajo sea el que esperamos.


### 9.1.3.- Cambiar el directorio de trabajo

Para movernos por el árbol de directorios debemos utilizar el comando `Set-Location`, o su alias `cd`. Como parámetro hay que indicarle la ruta del directorio al que nos queremos mover, el cual pasará a ser el directorio de trabajo.


### 9.1.4.- Pila del historial de navegación

Cuando cambiamos de directorio puede ser útil llevar un registro de las localizaciones que hemos recorrido para volver a ellas. El comando `Push-Location` crea un **historial de rutas de directorio** por las que hemos pasado, pudiendo volver a ellas con el comando complementario `Pop-Location`.

Se puede ver como una **pila**, cada vez que ejecutamos `Push-Location` se guarda en la parte superior de la pila el directorio que abandonamos. Cada vez que ejecutamos `Pop-Location` volvemos al directorio que se encuentra en la parte superior de la pila, eliminándolo de la misma.


### 9.1.5.- Obtener el contenido de un directorio

Para ver el contenido de un directorio utilizaremos el comando `Get-ChildItem` (o sus alias `dir` o `ls`), que mostrará todos los ficheros y directorios que se encuentran dentro del directorio de trabajo actual si no hemos indicado ningún directorio como parámetros, o del directorio que hayamos pasado como parámetro.

Por defecto, muestra los atributos del archivo, la fecha de la última modificación, el tamaño y el nombre del fichero. Los atributos pueden ser:

- Directorio (`d`)
- Archivo (`a`)
- Solo lectura (`r`)
- Oculto (`h`)
- Sistema (`s`)

```powershell
PS C:\apuntes_iso> Get-ChildItem

    Directory: C:\apuntes_iso

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          12/09/2021     9:48                iso
-a---          12/09/2021     9:33            220 readme.md

```

Se le puede pasar como **parámetro** el nombre de un directorio o de un fichero, mostrando de esta forma o el contenido del directorio o la información del fichero indicado.

Otra posibilidad es la utilización de los **caracteres comodín** que permiten hacer referencia a grupos de ficheros con una parte común en el nombre. Los caracteres comodín pueden ser:

- **Asterisco**: representa 0 o más caracteres. Por ejemplo, el comando `Get-ChildItem A*` muestra todos los ficheros del directorio actual cuya primera letra sea una `A`, y a continuación una cadena que puede ser incluso la cadena vacía.
- **Interrogación**: representa un único carácter. Por ejemplo, el comando `Get-ChildItem A?` muestra todos los ficheros que comiencen por la letra `A`, y a continuación tengan un único carácter.
- **Corchetes**: dentro de los corchetes deben ir dos o más caracteres y representa un único carácter que debe ser uno de los incluidos dentro de los corchetes. Por ejemplo, el comando `Get-ChildItem [abc]*` muestra todos los ficheros cuya primera letra sea `A`, `B` o `C` y a continuación tenga una cadena de caracteres de cualquier longitud.
- **Rangos**: en los corchetes también se pueden incluir los denominados rangos, indicados por dos letras separadas por el carácter guión (`-`) que representan todos los caracteres que se encuentran alfabéticamente entre los dos caracteres indicados. Por ejemplo, el comando `Get-ChildItem [a-g]*` muestra todos los ficheros que comiencen por una letra comprendida entre la `a` y la `g`. También es posible combinar rangos y caracteres sueltos concatenándolos dentro de los corchetes, de la forma `[a-gxyz]`.

Los **parámetros** que se pueden pasar al comando `Get-ChildItem` son:

- `-Path <String>`: tras este parámetro se indica la ruta de la que se debe mostrar el contenido. Es opcional, pudiendo poner directamente la ruta.
- `-Recurse`: muestra el contenido del directorio de forma recursiva, de forma que se vea el contenido del directorio indicado y el de todos sus directorios descendientes.
- `-Depth <UInt32>`: si queremos limitar la profundidad de la búsqueda recursiva hemos de utilizar este parámetro al que se le pasa un número entero.
- `-Include <String>`: se indica una cadena que servirá de filtro para señalar que ficheros se quieren mostrar.
- `-Exclude <String>`: de forma análoga al anterior, se indican los ficheros que no se mostrarán.
- `-Hidden`: muestra únicamente los ficheros ocultos
- `-ReadOnly`: muestra únicamente los ficheros de solo lectura.
- `-System`: muestra únicamente los ficheros con el atributo de sistema. Ten en cuenta que estos ficheros habitualmente están también ocultos, por lo que solo se muestran si también se indica el parámetro `-Hidden`.


### 9.1.6.- Creación de un nuevo elemento (archivo o directorio)

El comando `New-Item` (alias `ni') crea un nuevo elemento que puede ser un archivo o una carpeta. Este comando también puede establecer el valor de los elementos que crea, por ejemplo, se puede crear un fichero e indicar el contenido de este.

Algunos parámetros que admite son:

- `-Path <String>`: ruta del directorio donde creará el elemento.
- `-Name <String>`: nombre del elemento creado.
- `-ItemType <String>`: tipo de elemento que se va a crear. Los valores pueden ser `File` o `Directory`, aunque también admite otros valores que no veremos, como los enlaces.
- `-Value <Object>`: Indica el valor que tendrá el elemento creado. En el caso de ficheros será el contenido del mismo.


### 9.1.7.- Eliminación de archivos y directorios

El comando `Remove-Item`, que tiene múltiples alias (`ri`, `rm`, `rmdir`, `del`, `erase`, `rd`) permite **borrar** uno o más elementos. Aquí veremos su uso en el sistema de ficheros para borrar archivos y carpetas, pero permite borrar otros elementos como claves del registro, variables, alias o funciones.

En su uso básico se le pasa la ruta de un fichero, pudiendo contener comodines para afectar a varios ficheros.

Algunos de sus parámetros son:

- `-Include <String>`: indica los elementos que se borrarán.
- `-Exclude <String>`: indica los elementos que no se borrarán.
- `-Force`: este parámetro debe indicarse si se quieren borrar elementos que estén marcados como ocultos o de solo lectura.


### 9.1.8.- Copiar y mover archivos

Los comandos `Copy-Item` (`cpi` `cp` `copy`) y `Move-Item` (`mi` `mv` `move`) copian o mueven un elemento de una localización del sistema de ficheros a otra respectivamente. Estos comandos serían los equivalentes a copiar y pegar o cortar y pegar del entorno gráfico. La diferencia entre ambos es que el primero mantiene el elemento original mientras que el otro lo elimina.

Algunos parámetros son:

- `-Path <String>`: indica la ruta de origen del elemento.
- `-Destination <String>`: indica el directorio de destino. Al igual que el parámetro `-Path`, es opcional poner el nombre del parámetro, de forma que el comando entiende que el primer elemento pasado es el origen y el segundo el destino.
- `-Recurse`: copia también los subdirectorios que se encuentren en el directorio origen, así como su contenido.


### 9.1.9.- Mostrar el contenido de un archivo

***
[Volver al índice principal](index_UT06.md)