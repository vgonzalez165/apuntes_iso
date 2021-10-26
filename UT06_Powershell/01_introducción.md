![Carátula UT06](imgs/caratula_ut06.png)

### Contenidos

1. [Introducción a Powershell](01_introducción.md)
2. [Objetos y el pipeline](02_pipelines.md)
3. [El sistema de ficheros en Powershell](03_sistema_ficheros.md)
4. [Gestión de Hyper-V desde Powershell](04_hyperv.md)
5. [Gestión de usuarios y grupos](05_usuarios.md)
6. [Gestión avanzada de usuarios y grupos](06_usuarios_avanzado.md)
7. [Powershell y el almacenamiento](07_almacenamiento.md)


# 1.- INTRODUCCIÓN

## 1.1.- Conceptos importantes de Windows Powershell

El diseño de Windows PowerShell integra conceptos de muchos entornos distintos. Algunos de estos conceptos son:

- **Los comandos no están basados en texto**: los cmdlets están diseñados para usar objetos: información estructurada que es más que una simple cadena de caracteres que se muestra en pantalla.
- **El conjunto de comandos es ampliable**: las interfaces como `cmd.exe` no proporcionan al usuario una manera de ampliar directamente el conjunto de comandos integrados. Los comandos nativos de PowerShell, denominados **cmdlets**, se pueden ampliar con cmdlets que crees o agregues mediante complementos o módulos.
- **PowerShell controla la entrada y la presentación de la consola**: cuando se escribe un comando la información es procesada y aplica un formato a los resultados que se muestren en pantalla. Esto es importante porque el usuario siempre puede hacer las cosas de la misma manera independientemente del cmdlet utilizado.
- **PowerShell utiliza sintaxis del lenguaje C#**: incluye palabras clave y funciones de sintaxis muy parecidas a las que se usan en el lenguaje C# por lo que su aprendizaje facilitaría el aprendizaje de C#.


## 1.2.- Powershell vs Powershell Core

En el año 2018 Microsoft anunció una nueva edición de Powershell que denominó **Powershell Core** de forma que en la actualidad coexisten ambas ediciones. La diferencia más reseñable es que, mientras que Powershell es exclusivo de Windows, Powershell Core puede ser instalado en otros sistemas, como Mac OS X o Linux, lo que permite crear scripts compatibles para múltiples plataformas.

|               | Powershell        | Powershell Core   |
|---------------|-------------------|-------------------|
| Versiones     | 1.0 a 5.1         | 6.0 a 7.1         |
| Plataforma    | Solo Windows      | Windows, MAC OS y Linux |
| Dependencia   | .NET Framework    | .NET Core         |
| Comando       | powershell.exe    | pwsh.exe          |
| Políticas de actualización    | Solo correcciones de errores críticos | Todas las actualizaciones |


## 1.3.- Preparación del entorno

Si vamos a trabajar con Powershell es conveniente preparar el entorno de trabajo. Por defecto, Windows 10 incluye Powershell, pero si queremos utilizar Powershell Core deberemos instalarlo manualmente. La última versión de esta herramienta siempre la podremos encontrar en el repositorio oficial disponible en GitHub, accesible en [web del proyecto de Github](https://github.com/PowerShell/PowerShell).

Tras instalarlo podremos acceder a uno u otro entorno ejecutando los comandos `powershell.exe` o `pwsh.exe`. Como trabajaremos con diversos terminales o intérpretes de comandos, una opción recomendable es instalar **Windows Terminal** (accesible desde la tienda), que permite integrar diversos terminales en diferentes pestañas.

Si quieres personalizar más Windows Terminal tienes múltiples guías por internet, por ejemplo [aquí](https://terminaldelinux.com/terminal/wsl/configurar-windows-terminal/) o [aquí](https://www.developerro.com/2020/11/18/custom-windows-terminal/).

Un complemento de Powershell es el editor **Powershell ISE**, un editor que ya viene instalado en Windows 10 y que permite el trabajo con scripts de Powershell. Sin embargo, este editor es bastante limitado y además, solo compatible con Powershell (no con Core). Por ello, en caso de querer realizar scripts de Powershell, lo ideal es utilizar el editor [**Visual Studio Code**](https://code.visualstudio.com/) con el [plugin de Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell). En la [documentación de Microsoft](https://docs.microsoft.com/es-es/powershell/scripting/dev-cross-plat/vscode/using-vscode?view=powershell-7.1) se explican los pasos a realizar para configurar este editor.


## 1.4.- Ejecución de scripts

Cuando intentamos ejecutar un script de PowerShell realizado por nosotros la primera vez nos dará un mensaje de error. Esto se debe a la política de ejecución que está definida y que determina cómo se ejecutan los scripts.  Por defecto la política de ejecución está definida como *Restricted* lo que significa que los scripts creados por ti no tienen permisos de ejecución. Se puede comprobar la política que tenemos con la orden `Get-ExecutionPolicy`.

Podemos cambiar la política que se va a aplicar en el sistema con el cmdlet `Set-ExecutionPolicy` que admite uno de los siguientes valores:

- `RemoteSigned`: permite ejecutar cualquier script que nosotros creemos, pero si descargamos algún script de Internet solo podremos ejecutarlo si está firmado por un publicador de confianza (trusted Publisher).
- `AllSigned`: todos los scripts, incluso los creados por ti, deben ser firmados por un publicador de confianza.
- `Unrestricted`: todos los scripts, sea cual sea su origen, pueden ser ejecutados.

Por lo tanto, antes de crear nuestros propios scripts debemos verificar la política de ejecución que tiene nuestro equipo y, en caso de tener restringida la ejecución de scripts, debemos introducir la siguiente orden:

```powershell
PS C:\>Set-ExecutionPolicy RemoteSigned
```


## 1.5.- Nomenclatura de cmdlets

Los cmdlets utilizan un sistema de nombres con la estructura **“verbo-sustantivo”**: el nombre de cada cmdlets consta de un verbo estándar y un sustantivo concreto. Los verbos expresan acciones concretas mientras que los sustantivos describen siempre a qué se aplica un comando. La idea detrás de esto es crear un entorno autodescriptivo y uniforme de forma que los comandos sean más fáciles de recordar para los usuarios y que les permita hacerse una idea de su objetivo a partir de su nombre. Por ejemplo, el comando `Stop-Computer` se puede identificar fácilmente como el comando que sirve para apagar el ordenador.

Esta uniformidad en la nomenclatura también es útil para conocer los comandos que afectan a un elemento determinado (nombre) o que realizan una tarea en concreto (verbo). Para ello debemos utilizar el comando `Get-Command` con los parámetros `-Noun` o `–Verb` respectivamente. Tras el parámetro indicamos el nombre o verbo que deseemos y nos mostrará todos los comandos que hay en el sistema que utilizan dicho nombre o verbo.

```powershell
PS C:\>Get-Command –Verb Get
PS C:\>Get-Command –Noun Service
```

Respecto a los parámetros que admiten los cmdlets se fomenta que estén **normalizados**, es decir, que sean descriptivos y que normalmente un parámetro con resultado similar sea el mismo para diferentes cmdlets. Los nombres de los parámetros se utilizan siempre con un guión (-) como prefijo para que Windows los identifique como tales. Por ejemplo, en `Get-Command –Name Clear-Host` el parámetro es `–Name`.

Hay una serie de parámetros que son comunes a todos los comandos. Probablemente el más importante de todos es `-?`, que muestra la ayuda del comando, pero hay otros muchos como: `Whatif`, `Confirm`, `Verbose`, `Debug`, `Warn`, `ErrorAction`, `ErrorVariable`, `OutVariable` y `OutBuffer`.


## 1.6.- Nombres de comando familiares

Microsoft ha querido hacer Powershell fácil de aprender para todos los usuarios, especialmente para aquellos que ya saben utilizar otro intérprete de comandos como MS-DOS o Bash. Por ello, permite crear **alias** para los comandos, de forma que se pueda hacer referencia al mismo comando utilizando diferentes nombres. Por ejemplo, hay comando denominado `Clear-Host` que borra el contenido de la ventana que dispone de dos alias: `cls` y `clear`, que son los nombres de los comandos que realizan esa misma función en MS-DOS y en Bash respectivamente. Cualquiera de las tres formas se puede utilizar indistintamente ya que todas hacen referencia al mismo comando y por tanto producen el mismo resultado.

Algunos comandos de MS-DOS y de Bash que se pueden utilizar en Powershell son:

| 	       |            |		     | 	        | 	       | 	      | 	     |          |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| `cat`	   | `dir`      |	`mount`	 | `rm`	    | `cd`	   | `echo`	  | `move`	 | `rmdir`  |
| `chdir`  | `erase`	| `popd`	 | `sleep`	| `clear`  | `h`	  | `ps`	 | `sort`   |
| `cls`	   | `history`  | `pushd`	 | `tee`	| `copy`   | `kill`	  | `pwd`	 | `type`   |
| `del`	   | `lp`	    | `r`	     | `write`	| `diff`   | `ls`	  | `ren`	 |          |
| 	       |            |		     | 	        | 	       | 	      | 	     |          |


Mientras que los alias descritos anteriormente buscan la compatibilidad de los nombres de otras interfaces, PowerShell dispone de otros alias que buscan la concisión en la escritura que están basados en nombres abreviados de verbos y sustantivos comunes. Por ejemplo, `Get-Item` dispone del alias `gi` mientras que el de `Get-Command` será `gcm`.

También es posible crear nuestros propios alias mediante el comando `Set-Alias` con los parámetros `name` para indicar el nombre del alias y `-Value` para el comando al que hará referencia el alias.

```powershell
PS C:\> Set-Alias –Name gi –Value Get-Item
PS C:\> Set-Alias –Name sl –Value Set-Location
```

Adicionalmente podemos usar el parámetro `-option` para definir el comportamiento y el ámbito del alias. Las diferentes opciones son:

- `None`: es la opción por defecto. Los alias se pueden usar y borrar en cualquier momento.

Otra posibilidad con los alias es definir su comportamiento y ámbito (desde donde pueden ser vistos). Las opciones de ámbito disponibles son:

- **None**: un alias que se puede usar y borrar en cualquier momento. Es la opción por defecto.
- **Constant**: estos alias no pueden ser borrados ni se puede cambiar su valor durante la sesión.
- **ReadOnly**: son similares a los constant pero pueden ser borrados o modificados si se especifica el parámetro Force al borrarlos o modificarlos.
- **Private**: estos alias solo pueden ser vistos en el ámbito actual
- **AllScope**: pueden ser vistos desde cualquier ámbito.

Un ejemplo sería: 

```powershell
PS C:\> New-Item alias:cl –value C:\windows\system32\calc.exe –options “AllScope, Constant”
```

El nombre de un alias puede ser modificado mediante el cmdlet `Rename-Item` simplemente pasándole el parámetro `–NewName` de la forma: 

```powershell
PS C:\> Rename-Item alias:np –newname note
```

Recuerda que cualquier alias que crees durante una sesión es válido solamente para esa instancia de PowerShell. En el momento en que cierres la ventana esos alias dejarán de existir.

Para eliminar un alias antes de cerrar la sesión se puede hacer con el cmdlet `Remove-Item` de la forma: 

```powershell
PS C:\> Remove-Item alias:se
```

Todos los alias del sistema están almacenados en el denominado **dispositivo de alias** (alias drive) que es un dispositivo lógico para almacenar alias. Los **dispositivos lógicos** (logical drive) de PowerShell son similares a las unidades de disco que siempre hemos conocido para acceder a discos físicos, lógicos o de red (C:,…) pero que permiten almacenar otra información: una especie de base de datos de PowerShell. Como decíamos hay uno en concreto que sirve para almacenar todos los alias denominado `alias:`. Para acceder a él introducimos la siguiente orden: 

```powershell
PS C:\> Set-Location alias:
PS alias:\> Get-Childitem *
```

Como a estas alturas ya sabemos utilizar alias estarás pensando que es mucho más fácil realizar la operación anterior de la siguiente manera: 

```powershell
PS C:\> cd alias:
PS alias:\> dir
```

## 1.7.- Obtención de ayuda

Powershell tiene cientos de cmdlets, cada uno con un gran número de parámetros. Esto hace que sea imposible conocerlos todos, por lo que tendremos que recurrir frecuentemente a la ayuda.

El primer sitio para recurrir a la ayuda es en la propia [Web de Microsoft](https://docs.microsoft.com/es-es/powershell/), aquí hay disponibles en castellano guías, manuales y recursos de todo tipo, por lo que es un sitio al que deberás acudir frecuentemente cuando tengas cualquier duda sobre Powershell.

Si queremos obtener información sobre un determinado comando también hay varias opciones para hacerlo desde la línea de comandos.En este caso, lo primero que habría que hacer es actualizar la ayuda para poder tener en nuestro equipo la última versión de la misma. Para ello simplemente debemos ejecutar el comando ```Update-Help``` y esperar unos minutos a que la descargue de Internet y la instale.

Hay tres comandos relativos a la ayuda en Powershell: `Get-Command`, `Get-Help` y `Get-Member`. El tercero lo vamos a dejar para más adelante, así que veamos como funcionan los dos primeros.


### 1.7.1.- Get-Help

Es el comando principal para buscar la ayuda de otro comando. Simplemente necesitamos pasarle el nombre del otro comando con el parámetro ```-name``` para que nos proporcione la ayuda completa de dicho comando.

```powershell
PS C:\> Get-Help -Name Get-Process
```

La información que nos muestra la ayuda de un comando es la siguiente:
- Nombre del comando
- Sinopsis
- Sintaxis
- Descripción 
- Vínculos relacionados
- Comentarios

Aunque eso puede parecer mucha información, la realidad es que las entradas de la ayuda tienen muchos más apartados que 
nos pueden ser interesantes, como para qué sirven los parámetros o ejemplos de utilización. Si queremos una salida completa de la ayuda entonces tendremos que utilizar el parámetro `-full`.

```powershell
PS C:\> Get-Help -Name Get-Process -Full
```

Como se puede apreciar, ahora la ayuda obtenida es bastante más extensa, incluyendo también una explicación detallada de cada parámetro, ejemplos de uso, …

Un comando equivalente pero que muestra la ayuda página a página es `Help`. El resultado es muy similar con la diferencia de que ahora se muestra página a página por pantalla, requiriendo la pulsación de la barra espaciadora para pasar a la siguiente página y pudiendo navegar utilizando las teclas de cursor.

El comando `Get-Help` tiene otros muchos parámetros, lo más útiles son:
- `-Examples`: muestra únicamente ejemplos de uso del comando indicado por name
- `-Online`: abre la ayuda en el avegador Web
- `-ShowWindow`: muestra la ayuda en una ventana emergente.
- `Parameter Nombre`: muestra únicamente del parámetro cuyo nombre se indique

### 1.7.2.- Get-Command

Este comando nos servirá para encontrar otros comandos. Powershell tiene cientos e incluso miles de cmdlets, por lo que es casi imposible conocer el nombre de todos. El comando `Get-Command` nos ayudará a encontrar el comando que estemos buscando.

Si lo ejecutamos directamente, sin parámetros, mostrará todos los comandos disponibles en nuestro sistema, Pero cuando es realmente útil este comando es al utilizar los parámetros `-Verb` y `-Noun`. Hay que recordar que la nomenclatura de todos los cmdlets es consistente, utilizando todos ellos una combinación de verbo y nombre, es decir, qué hace (verbo) y con qué lo hace (nombre). Por ejemplo, el comando `Get-Help` es el comando que obtiene (Get) la ayuda (Help).

Por tanto, podemos obtener una lista de todos los comandos que tengan un nombre o verbo determinado utilizando los parámetros anteriores. Por ejemplo:

```powershell
PS C:\> Get-Command -Noun Process
```

Con el comando anterior podemos ver todos los cmdlets que realizan alguna operación con procesos.


## 1.8.- Ayudas al escribir los comandos

La primera herramienta de la consola para buscar comandos es autocompletar, que consiste en rellenar el comando que queremos al pulsar la **tecla tabulador** después de haber tecleado las primeras letras de un comando. Si hay un único comando que comience con esas teclas lo completará, si hubiera más de uno, completará con el primero en orden alfabético y, tras cada nueva pulsación de la tecla tabulador, irá mostrando el resto de los comandos que comiencen por dichas letras.

El uso del tabulador también es útil si no conocemos los parámetros de un comando, si tras el guión que precede a todos los parámetros pulsamos la tecla tabulador ira iterando sobre todos los parámetros que admite el comando.

Cuando queremos recurrir a algún comando que hayamos introducido recientemente, podemos utilizar las teclas de cursor. Cada vez que tecleemos cursor arriba navegará hacia atrás en el historial mostrando el comando previo al que se ve en pantalla. De forma análoga, el cursor abajo avanzará al siguiente comando.
Pero, si queremos más opciones para trabajar con el historial, tenemos una serie de comando relacionados con el historial.

El primero de ellos es `Get-History`, que mostrará numerados todos los comandos que hemos introducido previamente en la sesión actual. Los parámetros más relevantes para este comando son:

- `-Id`: Si pasamos como identificador un valor numérico correspondiente a una orden del historial nos mostrará únicamente esa orden.
- `-Count`: Por defecto se muestran todos los comandos desde que se inició la sesión. Con este parámetro podemos indicar cuántos comandos se mostrarán, bien desde el último introducido o bien desde el comando indicado si se utiliza en combinación con el parámetro `-Id`.
  
Si queremos volver a ejecutar un comando que ya está en el historial podemos utilizar el cmdlet `Invoke-History`. Podemos ejecutarlo sin parámetros, en cuyo caso volverá a ejecutar el último comando del historial, o utilizar el parámetro `-Id` para indicar el número de entrada del historial que se ejecutará.

Para agilizar la ejecución de comandos del historial podemos utilizar `r`, que es el alias de `Invoke-History`. De igual manera, un alias para `Get-History` es `h`.

Si queremos buscar dentro del historial de una forma rápida, tenemos las combinaciones de teclas `Ctrl-R` y `Ctrl-S`, la primera para buscar hacia atrás y la segunda para buscar hacia adelante. Cuando pulsamos `Ctrl-R` cambiará el prompt y nos pedirá que introduzcamos un texto. Al introducirlo, se mostrará el último comando ejecutado que contenga dicho texto. Con cada nueva pulsación de `Ctrl-R` retrocederemos en el historial buscando más comandos que contengan el texto, mientras que si pulsamos las teclas `Ctrl-S` avanzaremos en el historial.

Algo que hay que destacar respecto a la búsqueda en el historial es que se obtendrán resultados de la propia sesión o de anteriores, mientras que el comando `Get-History` solo muestra las entradas de la sesión actual.

Si queremos borrar el historial de nuestra máquina, solo tenemos que ejecutar el comando `Clear-History` que vacía por completo el historial. Sin embargo, podemos ser más precisos utilizando algunos de los parámetros disponibles. Podemos borrar entradas individuales con el parámetro `-Id`. Por ejemplo, si queremos borrar las entradas con identificador 5 y 9 ejecutaríamos la siguiente orden.

```powershell
PS C:\> Clear-History -Id 5, 9
```

También podemos eliminar un número concreto de entradas con el parámetro `-Count`. En el siguiente ejemplo, se eliminan las 5 entradas anteriores a la que tiene como identificador el número 11, mientras que en el segundo ejemplo se eliminan las últimas 5 entradas al combinarlo con el parámetro `-Newest`.

```powershell
PS C:\> Clear-History -Count 5 -Id 11
PS C:\> Clear-History -Count 5 -Newest
```

Por último, se pueden filtrar las entradas eliminadas según contengan una cadena usando el parámetro `-CommandLine`. En el siguiente ejemplo se eliminan todas las entradas del historial que contienen el texto Help y también las que finalizan con el texto Newest.

```powershell
PS C:\> Clear-History -CommandLine *Help*, *Newest
```

Otra utilidad interesante para realizar con el historial es exportarlo para posteriormente importarlo en una máquina diferente o en la misma máquina en otra sesión. Para conseguir esto debemos exportar el historial a un fichero, que podrá ser en formato XML o CSV y posteriormente importarlo en el otro equipo.

Para exportarlo podemos usar una de las siguientes órdenes, la primera si queremos utilizar el formato XML y la segunda para el caso de querer utilizar el formato CSV.

```powershell
PS C:\> Get-History | Export-Clixml -Path ‘C:\history.xml’
PS C:\> Get-History | Export-CSV -Path ‘C:\history.csv’
```

Si tienes curiosidad, puedes ver el contenido del fichero exportado con el comando `Get-Content`, que muestra el contenido de un archivo de texto.

```powershell
PS C:\> Get-Content ‘C:\history.xml’
```

Para importarlo, el proceso es similar, pero utilizando el comando Add-History de la siguiente forma.

```powershell
PS C:\> Import-Clixml -Path ‘C:\history.xml’ | Add-History
PS C:\> Import-CSV -Path ‘C:\history.csv’ | Add-History
```

***
[Volver al índice principal](index_UT06.md)
