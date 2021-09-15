
# 1.- INTRODUCCIÓN

## 1.1.- CONCEPTOS IMPORTANTES DE WINDOWS POWERSHELL

El diseño de Windows PowerShell integra conceptos de muchos entornos distintos. Algunos de estos conceptos son:

- Los comandos no están basados en texto: los cmdlets están diseñados para usar objetos: información estructurada que es más que una simple cadena de caracteres que se muestra en pantalla.
- El conjunto de comandos es ampliable: las interfaces como cmd.exe no proporcionan al usuario una manera de ampliar directamente el conjunto de comandos integrados. Los comandos nativos de PowerShell, denominados cmdlets, se pueden ampliar con cmdlets que crees o agregues mediante complementos o módulos.
- PowerShell controla la entrada y la presentación de la consola: cuando se escribe un comando la información es procesada y aplica un formato a los resultados que se muestren en pantalla. Esto es importante porque el usuario siempre puede hacer las cosas de la misma manera independientemente del cmdlet utilizado.
- PowerShell utiliza sintaxis del lenguaje C#: incluye palabras clave y funciones de sintaxis muy parecidas a las que se usan en el lenguaje C# por lo que su aprendizaje facilitaría el aprendizaje de C#.


## 1.2.- POWERSHELL VS POWERSHELL CORE

En el año 2018 Microsoft anunció una nueva edición de Powershell que denominó Powershell Core de forma que en la actualidad coexisten ambas ediciones. La diferencia más reseñable es que, mientras que Powershell es exclusivo de Windows, Powershell Core puede ser instalado en otros sistemas, como Mac OS X o Linux, lo que permite crear scripts compatibles para múltiples plataformas.

|               | Powershell        | Powershell Core   |
|---------------|-------------------|-------------------|
| Versiones     | 1.0 a 5.1         | 6.0 a 7.1         |
| Plataforma    | Solo Windows      | Windows, MAC OS y Linux |
| Dependencia   | .NET Framework    | .NET Core         |
| Comando       | powershell.exe    | pwsh.exe          |
| Políticas de actualización    | Solo correcciones de errores críticos | Todas las actualizaciones |


## 1.3.- PREPARACIÓN DEL ENTORNO

Si vamos a trabajar con Powershell es conveniente preparar el entorno de trabajo. Por defecto, Windows 10 incluye Powershell, pero si queremos utilizar Powershell Core deberemos instalarlo manualmente. La última versión de esta herramienta siempre la podremos encontrar en el repositorio oficial disponible en GitHub, accesible en [web del proyecto de Github](https://github.com/PowerShell/PowerShell).

Tras instalarlo podremos acceder a uno u otro entorno ejecutando los comandos `powershell.exe` o `pwsh.exe`. Como trabajaremos con diversos terminales o intérpretes de comandos, una opción recomendable es instalar **Windows Terminal** (accesible desde la tienda), que permite integrar diversos terminales en diferentes pestañas.

Si quieres personalizar más Windows Terminal tienes múltiples guías por internet, por ejemplo [aquí](https://terminaldelinux.com/terminal/wsl/configurar-windows-terminal/) o [aquí](https://www.developerro.com/2020/11/18/custom-windows-terminal/).

Un complemento de Powershell es el editor **Powershell ISE**, un editor que ya viene instalado en Windows 10 y que permite el trabajo con scripts de Powershell. Sin embargo, este editor es bastante limitado y además, solo compatible con Powershell (no con Core). Por ello, en caso de querer realizar scripts de Powershell, lo ideal es utilizar el editor [**Visual Studio Code**](https://code.visualstudio.com/) con el [plugin de Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell). En la [documentación de Microsoft](https://docs.microsoft.com/es-es/powershell/scripting/dev-cross-plat/vscode/using-vscode?view=powershell-7.1) se explican los pasos a realizar para configurar este editor.


## 1.3.- EJECUCIÓN DE SCRIPTS

Cuando intentamos ejecutar un script de PowerShell realizado por nosotros la primera vez nos dará un mensaje de error. Esto se debe a la política de ejecución que está definida y que determina cómo se ejecutan los scripts.  Por defecto la política de ejecución está definida como *Restricted* lo que significa que los scripts creados por ti no tienen permisos de ejecución. Se puede comprobar la política que tenemos con la orden `Get-ExecutionPolicy`.

Podemos cambiar la política que se va a aplicar en el sistema con el cmdlet `Set-ExecutionPolicy` que admite uno de los siguientes valores:

- `RemoteSigned`: permite ejecutar cualquier script que nosotros creemos, pero si descargamos algún script de Internet solo podremos ejecutarlo si está firmado por un publicador de confianza (trusted Publisher).
- `AllSigned`: todos los scripts, incluso los creados por ti, deben ser firmados por un publicador de confianza.
- `Unrestricted`: todos los scripts, sea cual sea su origen, pueden ser ejecutados.

Por lo tanto, antes de crear nuestros propios scripts debemos verificar la política de ejecución que tiene nuestro equipo y, en caso de tener restringida la ejecución de scripts, debemos introducir la siguiente orden:

```powershell
PS C:\>Set-ExecutionPolicy RemoteSigned
```


## 1.4.-NOMENCLATURA DE CMDLETS

Los cmdlets utilizan un sistema de nombres con la estructura “verbo-sustantivo”: el nombre de cada cmdlets consta de un verbo estándar y un sustantivo concreto. Los verbos expresan acciones concretas mientras que los sustantivos describen siempre a qué se aplica un comando. La idea detrás de esto es crear un entorno autodescriptivo y uniforme de forma que los comandos sean más fáciles de recordar para los usuarios y que les permita hacerse una idea de su objetivo a partir de su nombre. Por ejemplo, el comando `Sopt-Computer` se puede identificar fácilmente como el comando que sirve para apagar el ordenador.

Esta uniformidad en la nomenclatura también es útil para conocer los comandos que afectan a un elemento determinado (nombre) o que realizan una tarea en concreto (verbo). Para ello debemos utilizar el comando `Get-Command` con los parámetros `-Noun` o `–Verb` respectivamente. Tras el parámetro indicamos el nombre o verbo que deseemos y nos mostrará todos los comandos que hay en el sistema que utilizan dicho nombre o verbo.

```powershell
PS C:\>Get-Command –Verb Get
PS C:\>Get-Command –Noun Service
```

Respecto a los parámetros que admiten los cmdlets se fomenta que estén normalizados, es decir, que sean descriptivos y que normalmente un parámetro con resultado similar sea el mismo para diferentes cmdlets. Los nombres de los parámetros se utilizan siempre con un guión (-) como prefijo para que Windows los identifique como tales. Por ejemplo, en `Get-Command –Name Clear-Host` el parámetro es `–Name`.

Hay una serie de parámetros que son comunes a todos los comandos. Probablemente el más importante de todos es `-?`, que muestra la ayuda del comando, pero hay otros muchos como: `Whatif`, `Confirm`, `Verbose`, `Debug`, `Warn`, `ErrorAction`, `ErrorVariable`, `OutVariable` y `OutBuffer`.


## 1.5.- NOMBRES DE COMANDOS FAMILIARES

Microsoft ha querido hacer Powershell fácil de aprender para todos los usuarios, especialmente para aquellos que ya saben utilizar otro intérprete de comandos como MS-DOS o Bash. Por ello, permite crear **alias** para los comandos, de forma que se pueda hacer referencia al mismo comando utilizando diferentes nombres. Por ejemplo, hay comando denominado `Clear-Host` que borra el contenido de la ventana que dispone de dos alias: `cls` y `clear`, que son los nombres de los comandos que realizan esa misma función en MS-DOS y en Bash respectivamente. Cualquiera de las tres formas se puede utilizar indistintamente ya que todas hacen referencia al mismo comando y por tanto producen el mismo resultado.

Algunos comandos de MS-DOS y de Bash que se pueden utilizar en Powershell son:

| 	       |            |		     | 	        | 	       | 	      | 	     |          |
|----------|------------|------------|----------|----------|----------|----------|----------|
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

El nombre de un alias puede ser modificado mediante el cmdlet ```Rename-Item``` simplemente pasándole el parámetro ```–newname``` de la forma: 

```powershell
PS C:\> Rename-Item alias:np –newname note
```

Recuerda que cualquier alias que crees durante una sesión es válido solamente para esa instancia de PowerShell. En el momento en que cierres la ventana esos alias dejarán de existir.

Para eliminar un alias antes de cerrar la sesión se puede hacer con el cmdlet ```Remove-Item``` de la forma: 

```powershell
PS C:\> Remove-Item alias:se
```

Todos los alias del sistema están almacenados en el denominado **dispositivo de alias** (alias drive) que es un dispositivo lógico para almacenar alias. Los **dispositivos lógicos** (logical drive) de PowerShell son similares a las unidades de disco que siempre hemos conocido para acceder a discos físicos, lógicos o de red (C:,…) pero que permiten almacenar otra información: una especie de base de datos de PowerShell. 

Como decíamos hay uno en concreto que sirve para almacenar todos los alias denominado alias:. Para acceder a él introducimos la siguiente orden: 

```powershell
PS C:\> Set-Location alias:
PS alias:\> Get-Childitem *
```

Como a estas alturas ya sabemos utilizar alias estarás pensando que es mucho más fácil realizar la operación anterior de la siguiente manera: 

```powershell
PS C:\> cd alias:
PS alias:\> dir
```

## 1.6.- OBTENCIÓN DE AYUDA

Powershell tiene cientos de cmdlets, cada uno con un gran número de parámetros. Esto hace que sea imposible conocerlos todos, por lo que tendremos que recurrir frecuentemente a la ayuda.

El primer sitio para recurrir a la ayuda es en la propia [Web de Microsoft](https://docs.microsoft.com/es-es/powershell/), aquí hay disponibles en castellano guías, manuales y recursos de todo tipo, por lo que es un sitio al que deberás acudir frecuentemente cuando tengas cualquier duda sobre Powershell.

Si queremos obtener información sobre un determinado comando también hay varias opciones para hacerlo desde la línea de comandos.En este caso, lo primero que habría que hacer es actualizar la ayuda para poder tener en nuestro equipo la última versión de la misma. Para ello simplemente debemos ejecutar el comando ```Update-Help``` y esperar unos minutos a que la descargue de Internet y la instale.

Hay tres comandos relativos a la ayuda en Powershell: ```Get-Command```, ```Get-Help``` y ```Get-Member```. El tercero lo vamos a dejar para más adelante, así que veamos como funcionan los dos primeros.


### 1.6.1.- GET-HELP

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
nos pueden ser interesantes, como para qué sirven los parámetros o ejemplos de utilización. Si queremos una salida completa de la ayuda entonces tendremos que utilizar el parámetro ```-full```.

```powershell
PS C:\> Get-Help -Name Get-Process -Full
```

Como se puede apreciar, ahora la ayuda obtenida es bastante más extensa, incluyendo también una explicación detallada de cada parámetro, ejemplos de uso, …

Un comando equivalente pero que muestra la ayuda página a página es ```Help```. El resultado es muy similar con la diferencia de que ahora se muestra página a página por pantalla, requiriendo la pulsación de la barra espaciadora para pasar a la siguiente página y pudiendo navegar utilizando las teclas de cursor.

El comando ```Get-Help``` tiene otros muchos parámetros, lo más útiles son:
- ```-Examples```: muestra únicamente ejemplos de uso del comando indicado por name
- ```-Online```: abre la ayuda en el avegador Web
- ```-ShowWindow```: muestra la ayuda en una ventana emergente.
- -```Parameter Nombre```: muestra únicamente del parámetro cuyo nombre se indique

### 1.6.2.- GET-COMMAND

Este comando nos servirá para encontrar otros comandos. Powershell tiene cientos e incluso miles de cmdlets, por lo que es casi imposible conocer el nombre de todos. El comando ```Get-Command``` nos ayudará a encontrar el comando que estemos buscando.

Si lo ejecutamos directamente, sin parámetros, mostrará todos los comandos disponibles en nuestro sistema, Pero cuando es realmente útil este comando es al utilizar los parámetros ```-Verb``` y ```-Noun```. Hay que recordar que la nomenclatura de todos los cmdlets es consistente, utilizando todos ellos una combinación de verbo y nombre, es decir, qué hace (verbo) y con qué lo hace (nombre). Por ejemplo, el comando ```Get-Help``` es el comando que obtiene (Get) la ayuda (Help).

Por tanto, podemos obtener una lista de todos los comandos que tengan un nombre o verbo determinado utilizando los parámetros anteriores. Por ejemplo:

```powershell
PS C:\> Get-Command -Noun Process
```

Con el comando anterior podemos ver todos los cmdlets que realizan alguna operación con procesos.

## 1.7.- AYUDAS AL ESCRIBIR LOS COMANDOS

La primera herramienta de la consola para buscar comandos es autocompletar, que consiste en rellenar el comando que queremos al pulsar la tecla tabulador después de haber tecleado las primeras letras de un comando. Si hay un único comando que comience con esas teclas lo completará, si hubiera más de uno, completará con el primero en orden alfabético y, tras cada nueva pulsación de la tecla tabulador, irá mostrando el resto de los comandos que comiencen por dichas letras.

Cuando queremos recurrir a algún comando que hayamos introducido recientemente, podemos utilizar las teclas de cursor. Cada vez que tecleemos cursor arriba navegará hacia atrás en el historial mostrando el comando previo al que se ve en pantalla. De forma análoga, el cursor abajo avanzará al siguiente comando.
Pero, si queremos más opciones para trabajar con el historial, tenemos una serie de comando relacionados con el historial.

El primero de ellos es ```Get-History```, que mostrará numerados todos los comandos que hemos introducido previamente en la sesión actual. Los parámetros más relevantes para este comando son:

- ```-Id```: Si pasamos como identificador un valor numérico correspondiente a una orden del historial nos mostrará únicamente esa orden.
- ```-Count```: Por defecto se muestran todos los comandos desde que se inició la sesión. Con este parámetro podemos indicar cuántos comandos se mostrarán, bien desde el último introducido o bien desde el comando indicado si se utiliza en combinación con el parámetro ```-Id```.
  
Si queremos volver a ejecutar un comando que ya está en el historial podemos utilizar el cmdlet ```Invoke-History```. Podemos ejecutarlo sin parámetros, en cuyo caso volverá a ejecutar el último comando del historial, o utilizar el parámetro ```-Id``` para indicar el número de entrada del historial que se ejecutará.

Para agilizar la ejecución de comandos del historial podemos utilizar ```r```, que es el alias de ```Invoke-History```. De igual manera, un alias para ```Get-History``` es ```h```.

Si queremos buscar dentro del historial de una forma rápida, tenemos las combinaciones de teclas ```Ctrl-R``` y ```Ctrl-S```, la primera para buscar hacia atrás y la segunda para buscar hacia adelante. Cuando pulsamos ```Ctrl-R``` cambiará el prompt y nos pedirá que introduzcamos un texto. Al introducirlo, se mostrará el último comando ejecutado que contenga dicho texto. Con cada nueva pulsación de ```Ctrl-R``` retrocederemos en el historial buscando más comandos que contengan el texto, mientras que si pulsamos las teclas ```Ctrl-S``` avanzaremos en el historial.

Algo que hay que destacar respecto a la búsqueda en el historial es que se obtendrán resultados de la propia sesión o de anteriores, mientras que el comando ```Get-History``` solo muestra las entradas de la sesión actual.

Si queremos borrar el historial de nuestra máquina, solo tenemos que ejecutar el comando ```Clear-History``` que vacía por completo el historial. Sin embargo, podemos ser más precisos utilizando algunos de los parámetros disponibles. Podemos borrar entradas individuales con el parámetro ```-Id```. Por ejemplo, si queremos borrar las entradas con identificador 5 y 9 ejecutaríamos la siguiente orden.

```powershell
PS C:\> Clear-History -Id 5, 9
```

También podemos eliminar un número concreto de entradas con el parámetro ```-Count```. En el siguiente ejemplo, se eliminan las 5 entradas anteriores a la que tiene como identificador el número 11, mientras que en el segundo ejemplo se eliminan las últimas 5 entradas al combinarlo con el parámetro ```-Newest```.

```powershell
PS C:\> Clear-History -Count 5 -Id 11
PS C:\> Clear-History -Count 5 -Newest
```

Por último, se pueden filtrar las entradas eliminadas según contengan una cadena usando el parámetro ```-CommandLine```. En el siguiente ejemplo se eliminan todas las entradas del historial que contienen el texto Help y también las que finalizan con el texto Newest.

```powershell
PS C:\> Clear-History -CommandLine *Help*, *Newest
```

Otra utilidad interesante para realizar con el historial es exportarlo para posteriormente importarlo en una máquina diferente o en la misma máquina en otra sesión. Para conseguir esto debemos exportar el historial a un fichero, que podrá ser en formato XML o CSV y posteriormente importarlo en el otro equipo.

Para exportarlo podemos usar una de las siguientes órdenes, la primera si queremos utilizar el formato XML y la segunda para el caso de querer utilizar el formato CSV.

```powershell
PS C:\> Get-History | Export-Clixml -Path ‘C:\history.xml’
PS C:\> Get-History | Export-CSV -Path ‘C:\history.csv’
```

Si tienes curiosidad, puedes ver el contenido del fichero exportado con el comando ```Get-Content```, que muestra el contenido de un archivo de texto.

```powershell
PS C:\> Get-Content ‘C:\history.xml’
```

Para importarlo, el proceso es similar, pero utilizando el comando Add-History de la siguiente forma.

```powershell
PS C:\> Import-Clixml -Path ‘C:\history.xml’ | Add-History
PS C:\> Import-CSV -Path ‘C:\history.csv’ | Add-History
```

# 2.- CONTROL DE LA SALIDA CON PIPELINES

El **pipeline**, representado por el símbolo barra vertical (|), es utilizado para combinar diversos cmdlets de forma que la salida de uno es enviada a la entrada del siguiente, de forma muy similar a cómo se hace en Linux. 
 

## 2.1.- OBJETOS EN POWERSHELL

Un aspecto importante al trabajar con el pipeline, sobre todo si has trabajado antes con Linux, es que Powershell es un Shell **orientado a objetos**, y, por tanto, la información que se intercambia entre los comandos son objetos.

Un **objeto** se puede entender como una abstracción de un elemento, por ejemplo, un usuario, un proceso o un fichero. Cada objeto tiene una serie de **propiedades**, que son las características intrínsecas de dicho objeto, y una serie de **métodos**, que son las acciones que se pueden realizar sobre dicho objeto.

Por ejemplo, cuando en MS-DOS ejecutamos la orden:

```powershell
C:\> dir c:\datos\*.txt
```

Cuando ejecutamos esa orden el sistema mira el contenido del directorio ```C:\datos``` buscando los ficheros que tengan extensión txt y, a partir del mismo, genera una salida por pantalla, es decir, una secuencia de caracteres, que muestran el contenido de dicho directorio. Si quisiéramos realizar algún tipo de operación automatizada con la salida de este comando, tendremos que trabajar con esa secuencia de caracteres que ha generado. Por ejemplo, podríamos guardar todos esos caracteres en un fichero, o filtrarlos para que muestre todas las líneas que contengan una determinada cadena, pero solo operaciones que trabajen con esa cadena de texto.

Veamos ahora la orden equivalente en Powershell, que será:

```powershell
C:\> Get-Item C:\datos\*.txt
```

Si la ejecutamos veremos que la salida es muy similar a la del comando anterior, pero sin embargo lo que hace es muy diferente. El comando obtiene el listado de ficheros del disco, pero lo hace como una **lista de objetos**, cada uno de los cuales representando cada fichero. Una vez que tiene esta lista de objeto mira a ver que hay que hacer con ellos: si tiene que enviárselos a otro comando mediante el pipeline (como veremos un poco más adelante) le enviará la lista de objetos; en cambio, si tiene que mostrar la salida por pantalla tomará esos objetos y verá cuál es la representación más adecuada para mostrársela al usuario, momento en el que dejarán de ser un conjunto de objetos para ser una secuencia de caracteres.

Con este enfoque, podemos enviar la salida del comando xxxxxxxxxxx



Podemos saber qué tipo de objetos devuelve un comando de Powershell canalizando la salida al comando Get-Member, que muestra información por pantalla del objeto que se le envía.
 
En la imagen anterior ejecutamos el comando Get-LocalUser -Name Victor que devuelve un objeto de tipo Microsoft.Powershell.Commands.LocalUser que se corresponde con el usuario cuyo nombre es Victor. Este objeto contiene toda la información sobre dicho usuario, la cual se almacena en las propiedades del mismo. Por ejemplo, la fecha de caducidad de la cuenta del usuario se almacena en la propiedad AccountExpires. 


En la imagen anterior podemos ver que el comando Get-LocalUser devuelve objetos del tipo Microsoft.Powershell.Commands.LocalUser, y, a continuación, se nos muestran los métodos y propiedades de estos objetos. Por ejemplo, el objeto usuario tiene un método denominado Clone() que, como su nombre indica, servirá para clonar el usuario al que corresponde ese objeto; o, una propiedad denominada Description que almacenará una cadena de texto con la descripción introducida cuando se creó el usuario.


## 2.2.- EL PIPELINE

Cuando hablamos del **pipeline** en Powershell nos referimos a la capacidad de enviar la salida de un comando (que recordemos que por norma general es uno o varios objetos) a otro comando para que haga algo con ellos. De esta forma podremos encadenar diversos comandos de forma que cada uno de ellos toma como entrada la salida del comando anterior, lo manipula de alguna manera, y el resultado obtenido lo envía a la entrada del siguiente comando.

La sintaxis para indicar esta operación es mediante el carácter barra vertical (|), que ha de situarse entre un comando y el siguiente.

Algo importante que hay que tener en cuenta es que el tipo de objeto que genera el primer comando como salida tiene que coincidir con el tipo de objeto que admite el segundo comando como entrada. Por ejemplo, no tendría sentido enviar la salida del comando ```Get-LocalUser```, que devuelve una lista de objetos que representan usuarios, al comando ```Kill-Process```, que requiere como entrada objetos que representan procesos para eliminarlos.



## 2.1.- REDIRECCIÓN A OTRO COMANDO

Un uso del pipeline es obtener la referencia a algún objeto para utilizar como entrada de otro comando. Por ejemplo, supongamos que queremos terminar el proceso correspondiente a la calculadora, para lo cual necesitamos el comando Stop-Process. 
Mirando la ayuda de este comando veremos que requiere el identificador del proceso que queremos terminar, por lo que sería necesario conocer antes este número, por ejemplo, mediante el comando anterior.
 
Sin embargo, es más sencillo enlazar ambos comandos en una línea de la siguiente forma:
 
Observa que esto es útil incluso si tenemos varias instancias de un proceso. En el siguiente ejemplo se ve que hay ocho instancias del proceso Teams, las cuales son recogidas todas por el comando Get-Process y enviadas a Stop-Process.
 

# 2.2.- OBTENER PROPIEDADES DEL OBJETO

Como hemos dicho, la salida de los comandos de Powershell son objetos, cada uno de los cuales tiene una serie de propiedades, aunque en la salida por defecto únicamente es muestran las más importantes. Si queremos ver todas las propiedades de un objeto lo podemos hacer con el comando ```Get-Member``` de la siguiente forma:

```powershell
PS C:\> Get-Process -Name Calculator | Get-Member

   TypeName: System.Diagnostics.Process

Name                       MemberType     Definition
----                       ----------     ----------
Handles                    AliasProperty  Handles = Handlecount
Name                       AliasProperty  Name = ProcessName
NPM                        AliasProperty  NPM = NonpagedSystemMemorySize64
PM                         AliasProperty  PM = PagedMemorySize64
SI                         AliasProperty  SI = SessionId
...
```

Lo primero que podemos ver es que el comando ```Get-Process``` devuelve objetos del tipo **System.Diagnostics.Process**. Podemos saber qué comandos admiten ese objeto como entrada utilizando el comando ```Get-Command```.

```powershell
PS C:\> get-command -ParameterType Process

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Debug-Process                                      7.0.0.0    Microsoft.PowerShell.Management
Cmdlet          Enter-PSHostProcess                                7.1.4.0    Microsoft.PowerShell.Core
Cmdlet          Get-Process                                        7.0.0.0    Microsoft.PowerShell.Management
Cmdlet          Get-PSHostProcessInfo                              7.1.4.0    Microsoft.PowerShell.Core
Cmdlet          Stop-Process                                       7.0.0.0    Microsoft.PowerShell.Management
Cmdlet          Wait-Process                                       7.0.0.0    Microsoft.PowerShell.Management
```

Volviendo al comando ```Get-Member```, tras el tipo de objeto devuelto nos mostrará un listado de todas las propiedades y métodos de dicho objeto. En la segunda columna se indica el tipo de miembro, los más relevantes son:

- **Property**: son las propiedades del objeto, por ejemplo, el nombre del proceso o su identificador.
- **Method**: los métodos permiten realizar algún tipo de operación con el objeto, por ejemplo, el método ```kill()``` mataría el objeto. 
- **AliasProperty**: un nombre alternativo para una propiedad, normalmente más conciso, por ejemplo, **Name** contiene el mismo valor que la propiedad **ProcessName**.

Si nos fijamos vemos que los objetos devueltos por el comando ```Get-Process``` tienen varias decenas de propiedades. Sin embargo, cuando lo ejecutamos únicamente podemos ver por pantalla unas pocas.

```powershell
PS C:\> Get-Process -name calculator

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     30    23,82      69,59       0,69   17396   1 Calculator
```

Esto se debe a que Powershell solo nos muestra por pantalla las propiedades que considera más relevantes para aumentar la legibilidad. Si queremos decidir qué propiedades queremos, debemos hacer uso del comando ```Select-Object``` que permite filtrar un objeto seleccionando únicamente un subconjunto de las propiedades.

```powershell
PS C:\> Get-Process -name calculator | Select-Object name, cpu, StartTime

Name            CPU StartTime
----            --- ---------
Calculator 0,734375 14/09/2021 22:45:57
```



# 2.3.- ORDENAR, AGRUPAR Y MEDIR LA SALIDA DE UN COMANDO

Hay comandos que proporcionan un gran número de elementos al ejecutarse por lo que puede ser conveniente manipular dicha salida para que sea más fácilmente comprensible. Para ello utilizaremos los cmdlets Sort-Object, Goup-Object y Measure-Object, que ordenan, agrupan y cuentan respectivamente.
Al comando Sort-Object hay que pasarle como parámetro la propiedad por la que queramos que nos ordene la salida, por ejemplo, podemos ordenador todos los procesos del sistema por uso de CPU. Admite algunos parámetros que podemos ver en la ayuda (Get-Help Sort-Object), pero probablemente el más utilizado sea -Descending para mostrar los resultados ordenados en orden descendente.
Además, aprovecharemos este ejemplo para mostrar cómo se pueden enlazar varios comandos mediante pipelines, para ello obtenemos todos los procesos del sistema, seleccionamos qué propiedades queremos que se muestren y finalmente los ordenamos según el consumo de CPU en orden descendente, es decir, que los primeros sean los que mayor consumo de CPU tengan.

PS C:\> Get-Process | Select-Object Name, Id, CPU | Sort-Object CPU -Descending

El siguiente comando para reorganizar la salida es Group-Object, que agrupa los objetos según la propiedad que se especifique.
PS C:\> Get-Process | Group-Object Name, CPU
Y por último, podemos utilizar el comando Measure-Object para contar los objetos que proporciona el comando del que obtiene la salida. Por ejemplo, con la siguiente orden contaremos el número de procesos que hay en el sistema.
PS C:\> Get-Process | Measure-Object


# 2.4.- FILTRADO DE OBJETOS

La mayoría de los comandos devuelven un conjunto de objetos que normalmente queremos filtrar. Por ejemplo, el comando Get-Process permite filtrar la salida por nombre indicando el parámetro -Name, de forma que únicamente nos muestre aquellos procesos cuyo nombre se ajuste a nombre que hemos indicado como parámetros.
Si queremos más opciones de filtrado, tenemos a nuestra disposición el comando Where-Object, el cual recibe un conjunto de objetos (normalmente pasados mediante pipeline) y los filtra mediante algún criterio, de forma que en su salida proporciona únicamente los objetos que cumplan el criterio especificado.
Para usar este comando, lo primero que tenemos que conocer son los operadores de comparación, que son los siguientes:
Operador	Significado	Operador	Significado
-eq	Es igual a	-ne	no es igual a
-lt	Es menor que	-gt	Es mayor que
-le	Es menor o igual que	-ge	Es mayor o igual que
-like	Es como	-notlike	No es como
Este comando tiene dos sintaxis equivalentes, siendo indiferente el uso de una u otra. A continuación, hay dos ejemplos de filtrado que tienen el mismo resultado y que muestran aquellos procesos cuyo nombre sea Calculator.
PS C:\> Get-Process | Where-Object Name -eq “Calculator”
PS C:\> Get-Process | Where-Object {$_.Name -eq “Calculator”}
Los operadores de comparación (mayor y menor) se utilizan con propiedades numéricas. Por ejemplo, si queremos obtener la lista de objetos cuyo consumo de CPU ha sido superior a los 120 segundos deberíamos introducir la siguiente orden:
PS C:\> Get-Process | Where-Object CPU -gt 100
Los operadores -like y -notlike se utilizan para realizar comparaciones de cadenas utilizando comodines. Podemos utilizar dos comodines:
•	Asterisco: representa una cadena de cualquier longitud que también puede ser la cadena vacía.
•	Interrogación: representa un único carácter.
Por ejemplo, si queremos localizar el proceso correspondiente al programa Word y no sabemos exactamente como se llama podemos hacer una búsqueda de la siguiente forma, representando cadenas que contengan la cadena word en su interior y que pueden tener cualquier cadena tanto antes como después.
PS C:\> Get-Process | Where-Object Name -like *word*


# 2.5.- FORMATEANDO DE LA SALIDA

Cuando ejecutamos un comando en Powershell vemos un texto con la salida del comando, pero en realidad cualquier comando devuelve uno o varios objetos y lo que se nos muestra por pantalla son las propiedades más relevantes de dichos objetos en un formato predeterminado. Sin embargo, es posible modificar tanto las propiedades que se nos muestran de cada objeto, como el formato en que se hace.
Powershell tiene cinco cmdlets para el formato de la salida de comandos, de los que nosotros veremos Format-List, Format-Table y Format-Wide.


### 2.5.1.- FORMAT-WIDE

Este comando muestra únicamente una propiedad de cada objeto, mostrando todos estos valores en una tabla. Como alternativa más breve se puede utilizar el alias fw.
Los parámetros más destacables de este comando son:
•	-Property: para indicar si queremos que se muestre una propiedad diferente a la que muestra por defecto.
•	-Column: que mediante un valor numérico señalará cuántas columnas tendrá la tabla con los resultados mostrados.

### 2.5.2.- FORMAT-TABLE

Formatea la salida del comando redireccionado en forma de tabla donde mostrará un objeto en cada fila y las propiedades más relevante en las columnas. El alias para este comando es ft.
Un parámetro útil con Format-Table es -AutoSize, que adapta el tamaño de la salida para que se ajuste al tamaño disponible en la pantalla y así evitar que se recorte la salida.


# 2.5.3.- FORMAT-LIST

Por último, Format-List muestra la salida del comando como una lista de propiedades, indicando cada una de estas propiedades en una línea diferente.


 
# 3.- CONTROLANDO HYPER-V CON POWERSHELL

Powershell dispone de un módulo específico para trabajar con máquinas virtuales de Hyper-V el cual se puede instalar cuando habilitamos la característica de Administración de Hyper-V.
Podemos ver todos los comandos incluidos en el módulo con la siguiente orden, que nos muestra más de 200 comandos relacionados con máquinas virtuales.
PS C:\> Get-Command -Module hyper-v
Veamos los comandos que más útiles nos pueden ser:


## 3.1.- LISTADO DE MÁQUINAS VIRTUALES

Con el comando Get-VM obtendremos un listado de las máquinas virtuales actualmente administradas en nuestro equipo.
PS C:\> Get-VM
Podemos utilizar un filtro para mostrar solamente las máquinas que nos interesen. En las siguientes órdenes se muestran únicamente las máquinas virtuales que se encuentran en ejecución en un momento determinado. Observa que ambas sintaxis son equivalentes y se pueden utilizar indistintamente una u otra.
PS C:\> Get-VM | Where-Object {$_.State -eq ‘Running’}
PS C:\> Get-VM | Where-Object State -eq ‘Running’


## 3.2.- INICIAR Y APAGAR MÁQUINAS VIRTUALES

Como seguro que ya te imaginas, los comandos para iniciar y apagar máquinas virtuales son Start-VM y Stop-VM. Ambas utilizan el parámetro -Name para hacer referencia a la máquina que se va a iniciar o apagar. 
PS C:\> Start-VM -Name “Windows 10”
También podemos utilizar el pipeline para realizar arranques o paradas de todas las máquinas virtuales o de un conjunto de ellas, por ejemplo, con la siguiente orden se paran todas las máquinas que estén arrancadas:
PS C:\> Get-VM | Where-Object State -eq ‘Running’ | Stop-VM
Si por algún motivo la máquina virtual se hubiera colgado y no respondiera a la orden de parada se podría utilizar el parámetro -Force para forzar el apagado. Ten en cuenta que esto es equivalente a apagar una máquina física tirando del cable de corriente, por lo que puede haber pérdida de datos o tener alguna inconsistencia en el sistema de ficheros.


# 3.3.- SUSPENDER E HIBERNAR MÁQUINAS VIRTUALES

De forma análoga a como podemos hacer con una máquina física, es posible suspender e hibernar las máquinas virtuales.
Suspender una máquina virtual supone congelar su ejecución en el momento actual y quedará en este estado hasta que la volvamos a iniciar con el comando Resume-VM.
PS C:\> Suspend-VM -Name ‘Win10’
PS C:\> Resume-VM -Name ‘Win10’
La diferencia cuando guardamos una máquina virtual (el equivalente a hibernar) es que se congela la máquina y además el contenido de su memoria se vuelca al disco, por lo que se liberará la memoria que esté ocupando la máquina virtual. El comando para realizar esta tarea es Save-VM, mientras que se debe restaurar con Start-VM.
PS C:\> Save-VM -Name ‘Win10’
PS C:\> Start-VM -Name ‘Win10’


## 3.4. MEDIR EL CONSUMO DE RECURSOS DE UNA MÁQUINA VIRTUAL

En entornos con múltiples máquinas virtuales puede ser útil hacer un seguimiento de los recursos consumidos por cada una de las máquinas. Para hacer esto primero hay que habilitar la medición de recursos en las máquinas virtuales con la siguiente orden:
PS C:\> Enable-VMResourceMetering -VMName ‘Win10’
Una vez habilitada la medición de recursos podemos comprobar el consumo de recursos con el comando:
PS C:\> Measure-VM -Name ‘Win10’


## 3.5.- OBTENER LOS RECURSOS DEL ANFITRIÓN

En ocasiones nos puede interesar conocer los recursos disponibles en el equipo que está ejecutando Hyper-V, por ejemplo, para saber cuanta memoria RAM tiene o el número de núcleos del procesador. Esto lo podemos conseguir con el comando Get-VMHost. 
PS C:\> Get-VMHost
Recuerda que puedes combinarlo con el comando Select-Objetc para seleccionar las propiedades que se mostrarán. También puedes utilizar el comodín (*) para indicar que se muestren todas las propiedades.
PS C:\> Get-VMHost | Select-Object *


## 3.6.- OTROS COMANDOS PARA TRABAJAR CON HYPER-V

Hay otros muchos comandos que nos permiten realizar cualquier función que nos imaginemos sobre las máquinas virtuales. En especial, hay una serie de comandos que nos permitirán crear tanto discos como máquinas virtuales directamente desde la línea de comandos, pero que se salen del ámbito de estudio de este curso.
Si estás muy interesado, puedes obtener más información en las páginas de ayuda de Hyper-V con Powershell de Microsotf o también hay un manual bastante detallado en https://statemigration.com/save-time-on-hyper-v-using-powershell/.




# 4.- EL SISTEMA DE FICHEROS

Tabla resumen de comandos

| Comando           | Alias         | Descripción                                   |
|-------------------|---------------|-----------------------------------------------|
| `Add-Content`     | ac            | Añade contenido a un fichero                  |
| `Clear-Host`      | cls, clear    | Borra la pantalla                             |
| `Clear-Item`      | cli           | Borra el contenido de un fichero, pero no el propio fichero |
| `Copy-Item`       | copy, cp, cpi | Copia un fichero o directorio                 |
| `Get-ChildItem`   | dir, ls, gci  | Muestra el contenido de un directorio         |
| `Get-Content`     | type, cat, gc | Muestra el contenido de un fichero            |
| `Get-Item`        | gi            | ????????????????????????                      |
| `Get-ItemProperty`| gp            | Obtiene una propiedad de un fichero o directorio |
| `Join-Path`       | -             | Une dos partes de una ruta, p.e. una unidad de disco y un nombre de fichero |
| `Move-Item`       | mi, mv, move  | Mueve ficheros y directorios                  |
| `New-Item`        | ni            | Crea un fichero o directorio                  |
| `Remove-Item`     | ri, rm, rmdir, del, erase, rd | Borra un fichero o directorio |
| `Rename-Item`     | ren, rni      | Renombra un fichero o directorio              |
| `Set-Location`    | cd, chdir, sl | Cambia el directorio de trabajo al directorio especificado |
| `Test-Path`       | -             | Devuelve True si la ruta indicada existe      |