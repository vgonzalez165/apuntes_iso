![Carátula UT06](imgs/caratula_ut06.png)

# 4.- EL SISTEMA DE FICHEROS

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


## 4.1.- Navegación por el sistema de ficheros

### 4.1.1.- Directorio de trabajo

El sistema de ficheros en Windows es una estructura arborescente o jerárquica donde la raíz de dicho árbol es cada una de las unidades de almacenamiento que tengamos en el sistema, y cada nodo será, o bien un **fichero** o bien una **carpeta**. Los ficheros contienen datos, mientras que las carpetas o directorios son meros contenedores que sirven para almacenar en su interior tanto directorios como otras carpetas. 

Cuando abrimos una terminal de Powershell, siempre nos encontraremos en algún punto de ese árbol, que puede ser en el directorio raíz o bien en alguna de las carpetas del árbol. El directorio en el que nos entramos en cada momento se llama **directorio de trabajo** y es importante tener claro en todo momento cuál es, ya que las órdenes de Powershell que requieren un fichero como parámetro lo buscarán por defecto en el directorio de trabajo (salvo que se indique explícitamente la ruta).

Hay dos formas de saber cuál es el directorio de trabajo:

- Cuando estamos en la línea de comandos del Powershell siempre se muestra el directorio de trabajo actual en el **promp**, el texto que aparece antes del cursor que espera que introduzcamos un comando.
- Con el comando `Get-Location` o su alias `pwd`.

La forma de representar los directorios es mediante la enumeración de todos los directorios que hay que atravesar desde el directorio raíz hasta el directorio indicado partiendo siempre desde la letra de la unidad en que se encuentra y separando los directorios con el símbolo barra invertida (`\`) o bien el símbolo barra (`/`). Windows no distingue entre mayúsculas y minúsculas, por lo que es indiferente utilizar unas u otras en las rutas.

Así, por ejemplo, la ruta `C:\Usuarios\victor\Documents` hace referencia al directorio `Documents`, que se encuentra dentro del directorio victor, que a su vez está dentro de `Usuarios` que está ubicado en la unidad de almacenamiento `C:`

Cuando indicamos una ruta de esta forma se dice que utilizamos **rutas absolutas**, ya que identifica unívocamente el directorio independientemente del directorio de trabajo en el que nos encontremos.

La alternativa es el uso de **rutas relativas**, en ellas se indica el camino que hay que seguir para llegar al directorio que se desea partiendo del directorio de trabajo actual, en lugar de hacerlo desde el directorio raíz. Estas rutas se identifican fácilmente porque **no comienzan por una letra de unidad**, y, para saber a qué directorio apuntan, debemos saber cuál es el directorio de trabajo actual.

Si por ejemplo, nuestro directorio de trabajo actual es `C:\Usuarios` y tenemos la ruta `vgonzalez\Documents`, sabemos que hace referencia al directorio `Documents`, que se encuentra dentro de `vgonzalez`, que a su vez está en el directorio `Usuarios` del disco `C:`. Un buen hábito es utilizar un directorio especial llamado `.` que se encuentra en todos los directorios y que apunta a él mismo para preceder las rutas locales, de forma que se enfatiza el hecho de que sea una ruta local y mejora la legibilidad. De esta forma, la ruta anterior la podíamos poner de la forma `.\vgonzalez\Documents`.

Cuando descendemos en el árbol de directorios, como en el ejemplo anterior, simplemente hay que poner los nombres de todos los directorios por los que se pasa. Sin embargo, hay ocasiones en que necesitaremos **subir por el árbol de directorios**. Para esos casos necesitamos poder hacer referencia al **directorio padre** de un directorio, y eso se consigue con otro directorio especial, presente en todos los directorios, denominado `..`.

Así, si por ejemplo estamos en el directorio `D:\apuntes\ISO\UT01` y queremos hacer referencia al directorio `D:\apuntes\ISO\UT02` mediante una ruta relativa, la indicaríamos de la forma `..\UT02`, que indica, *vete al directorio padre del actual y ahí entra al subdirectorio `UT02`*.

Por norma general es equivalente el uso de rutas absolutas o relativas, por lo que siempre haremos uso de aquellas que nos resulten más cortas o con las que nos encontremos más cómodos. Hay una excepción a esta norma, y es cuando utilicemos rutas dentro de ficheros, por ejemplo, scripts o ficheros de configuración. En esos casos **siempre debemos utilizar rutas absolutas**, ya que no podemos garantizar que en el momento en que se vaya a ejecutar el script o leer el fichero de configuración el directorio de trabajo sea el que esperamos.


### 4.1.2.- Navegación por el sistema de ficheros

Para movernos por el árbol de directorios debemos utilizar el comando `Set-Location`, o su alias `cd`. Como parámetro hay que indicarle la ruta del directorio al que nos queremos mover, el cual pasará a ser el directorio de trabajo.

Para ver el contenido de un directorio utilizaremos el comando `Get-ChildItem` (o sus alias `dir` o `ls`), que mostrará todos los ficheros y directorios que se encuentran dentro del directorio de trabajo actual si no hemos indicado ningún directorio como parámetros, o del directorio que hayamos pasado como parámetro.

```powershell
PS C:\apuntes_iso> Get-ChildItem

    Directory: C:\apuntes_iso

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          12/09/2021     9:48                iso
-a---          12/09/2021     9:33            220 readme.md

```

