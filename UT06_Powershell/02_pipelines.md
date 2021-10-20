![Carátula UT06](imgs/caratula_ut06.png)

# 2.- CONTROL DE LA SALIDA CON PIPELINES

El **pipeline**, representado por el símbolo barra vertical (|), es utilizado para combinar diversos cmdlets de forma que la salida de uno es enviada a la entrada del siguiente, de forma muy similar a cómo se hace en Linux. 
 

## 2.1.- Objetos en Powershell

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

Con este enfoque, podemos enviar la salida del comando **xxxxxxxxxxx**



Podemos saber qué tipo de objetos devuelve un comando de Powershell canalizando la salida al comando Get-Member, que muestra información por pantalla del objeto que se le envía.
 
En la imagen anterior ejecutamos el comando `Get-LocalUser -Name Victor` que devuelve un objeto de tipo Microsoft.Powershell.Commands.LocalUser que se corresponde con el usuario cuyo nombre es Victor. Este objeto contiene toda la información sobre dicho usuario, la cual se almacena en las propiedades del mismo. Por ejemplo, la fecha de caducidad de la cuenta del usuario se almacena en la propiedad AccountExpires. 


En la imagen anterior podemos ver que el comando `Get-LocalUser` devuelve objetos del tipo Microsoft.Powershell.Commands.LocalUser, y, a continuación, se nos muestran los métodos y propiedades de estos objetos. Por ejemplo, el objeto usuario tiene un método denominado Clone() que, como su nombre indica, servirá para clonar el usuario al que corresponde ese objeto; o, una propiedad denominada Description que almacenará una cadena de texto con la descripción introducida cuando se creó el usuario.


## 2.2.- El pipeline

Cuando hablamos del **pipeline** en Powershell nos referimos a la capacidad de enviar la salida de un comando (que recordemos que por norma general es uno o varios objetos) a otro comando para que haga algo con ellos. De esta forma podremos encadenar diversos comandos de forma que cada uno de ellos toma como entrada la salida del comando anterior, lo manipula de alguna manera, y el resultado obtenido lo envía a la entrada del siguiente comando.

La sintaxis para indicar esta operación es mediante el carácter barra vertical (|), que ha de situarse entre un comando y el siguiente.

Algo importante que hay que tener en cuenta es que el tipo de objeto que genera el primer comando como salida tiene que coincidir con el tipo de objeto que admite el segundo comando como entrada. Por ejemplo, no tendría sentido enviar la salida del comando ```Get-LocalUser```, que devuelve una lista de objetos que representan usuarios, al comando ```Kill-Process```, que requiere como entrada objetos que representan procesos para eliminarlos.



## 2.3.- Redirección a otro comando

Un uso del pipeline es obtener la referencia a algún objeto para utilizar como entrada de otro comando. Por ejemplo, supongamos que queremos terminar el proceso correspondiente a la calculadora, para lo cual necesitamos el comando Stop-Process. 
Mirando la ayuda de este comando veremos que requiere el identificador del proceso que queremos terminar, por lo que sería necesario conocer antes este número, por ejemplo, mediante el comando anterior.
 
Sin embargo, es más sencillo enlazar ambos comandos en una línea de la siguiente forma:
 
Observa que esto es útil incluso si tenemos varias instancias de un proceso. En el siguiente ejemplo se ve que hay ocho instancias del proceso Teams, las cuales son recogidas todas por el comando Get-Process y enviadas a Stop-Process.
 

## 2.4.- Obtener propiedades del objeto

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

## 2.5.- Ordenar, agrupar y medir la salida de un comando

Hay comandos que proporcionan un gran número de elementos al ejecutarse por lo que puede ser conveniente manipular dicha salida para que sea más fácilmente comprensible. Para ello utilizaremos los cmdlets `Sort-Object`, `Group-Object` y `Measure-Object`, que ordenan, agrupan y cuentan respectivamente.

### 2.3.1.- Ordenando con el comando `Sort-Object`

El comando `Sort-Object` admite un conjunto de objetos como entrada y devuelve es mismo conjunto de objetos pero **ordenados** según el valor de la propiedad que se indique. 

Por ejemplo, para ordenar los procesos que se están ejecutando actualmente en el sistema ordenados por uso de CPU ejecutaríamos la orden:

```powershell
PS C:\> get-process | Sort-Object cpu

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     12     3,74       2,68       0,00    1616   0 amdfendrsr
     19     6,27       9,71       0,00    1908   0 svchost
     13     3,62       6,50       0,00    1964   0 svchost
     14    17,88      20,35       0,00    2064   0 svchost
...
```

El parámetro más relevante de este comando es `-Descending`, que ordena la salida de mayor a menor.

### 2.3.2.- Agrupando con `Group-Object`

El comando `Group-Object` recoge un conjunto de objetos y, en lugar de mostrarlos todos secuencialmente, los agrupa en función del valor de la propiedad que indiquemos.

Veámoslo con un ejemplo. El comando `Get-Service` nos devuelve todos los servicios que hay en el sistema de la siguiente forma:

```powershell
PS C:\> Get-Service

Status   Name               DisplayName
------   ----               -----------
Stopped  AarSvc_1f230e      Agent Activation Runtime_1f230e
Running  AdobeARMservice    Adobe Acrobat Update Service
Stopped  AJRouter           Servicio de enrutador de AllJoyn
Stopped  ALG                Servicio de puerta de enlace de nivel…
```

Si quisiéramos saber cuántos están actualmente en ejecución tendríamos que contarlos manualmente. La mejor alternativa es utilizar `Group-Object` para agruparlos por la propiedad `status`, lo que los agrupará en función del valor de estado de cada servicio y además nos indicará cuántos hay en cada grupo.

```powershell
PS C:\> Get-Service | Group-Object -Property status

Count Name                      Group
----- ----                      -----
  168 Stopped                   {AarSvc_1f230e, AJRouter, ALG, AppIDSvc…}
  103 Running                   {AdobeARMservice, AMD Crash Defender Service, AMD External Events Utility, Appinfo…}
```

Incluso podríamos indicar más de una propiedad para que agrupe en función de las diferentes combinaciones de los valores de dicha propiedad.

```powershell
PS C:\> Get-Service | Group-Object -Property Status,StartType

Count Name                      Group
----- ----                      -----
    6 Stopped, Automatic        {dbupdate, edgeupdate, gpsvc, gupdate…}
  150 Stopped, Manual           {AarSvc_1f230e, AJRouter, ALG, AppIDSvc…}
   11 Stopped, Disabled         {AppVClient, DialogBlockingService, MsKeyboardFilter, NetTcpPortSharing…}
   61 Running, Automatic        {AdobeARMservice, AMD Crash Defender Service, AMD External Events Utility, AudioEndpoi…
   43 Running, Manual           {Appinfo, AppXSvc, BthAvctpSvc, cbdhsvc_1f230e…}
```

### 2.3.3.- Midiendo con el `Measure-Object`

Este comando realiza cálculos con los valores de las propiedades de un objeto. En el caso de propiedades numéricas puede calcular el mínimo, el máximo, la suma y el promedio. En el caso de propiedades de tipo texto puede contar y calcular el número de líneas, palabras y caracteres.

```powershell
PS C:\> Get-ChildItem | Measure-Object -Property Length -Minimum -Maximum -Average

Count             : 4
Average           : 1242
Sum               :
Maximum           : 3572
Minimum           : 58
StandardDeviation :
Property          : Length
```

Por ejemplo, el comando anterior muestra el tamaño máximo, mínimo y medio de todos los ficheros que se encuentran en un directorio.


# 2.4.- FILTRADO DE OBJETOS

Muchos comandos disponen de parámetros para filtrar la salida en función del valor de alguna propiedad. Por ejemplo, se puede utilizar el parámetro `-Name` del comando `Get-Process` para que solo nos devuelva los procesos que tengan un nombre determinado.

```powershell
PS C:\> Get-Process -Name calculator

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     30    21,46      28,66       0,73   14060   1 Calculator
```

Pero esta opción solo está disponible en algunos comandos y para propiedades muy determinadas. Cuando se desea un mayor control en el filtrado de los objetos proporcionados por un comando lo ideal es enviar dicha salida por el pipeline al comando `Where-Object`, el cual admite un gran número de criterios para filtrar.

Los operadores de comparación que soporta el comando se muestran en la siguiente tabla.

| Operador        | Significado       | Operador        | Significado       |
|-----------------|-------------------|-----------------|-------------------|
| `-eq`           | Es igual a        | `-ne`           | No es igual a     |
| `-lt`           | Es menor que      | `-gt`           | Es mayor que      |
| `le`            | Es menor o igual que  | `-ge`       | Es mayor o igual que    |
| `-like`         | Es como           | `-notlike`      | No es como        |

Se pueden utilizar dos sintaxis diferentes para indicar el filtro en este comando: mediante *scriptblocks* o indicando directamente las comparaciones en los parámetros de `Where-Object`. La primera opción es válida para todas las versiones de Powershell, aunque tiende a hacer menos legible el código, la segunda opción es la recomendada porque aumenta la legibilidad, pero solo está disponible a partir de la versión 3 de Powershell.

Veamos como mostraríamos todos los procesos que corresponden a la calculadora con ***scriptblocks**.

```powershell
PS C:\> Get-Process | Where-Object -FilterScript { $_.Name -eq “Calculator” }
```

Lo más destacable de esta sintaxis es:

- Las comparaciones se incluyen entre llaves, en lo que se llama un bloque de script
- El bloque de script se indica mediante el parámetro `-FilterScript`, sin embargo, esto es opcional y podríamos omitir el nombre del parámetro.
- La variable `$_` hace referencia a cada uno de los objetos que recibe. Es decir, el comando `Get-Process` devuelve un conjunto de objetos y, para cada uno de esos


Este comando tiene dos sintaxis o formas de utilizarlo equivalentes. A continuación, hay dos ejemplos de filtrado que tienen el mismo resultado y que muestran aquellos procesos cuyo nombre sea Calculator.

```powershell
PS C:\> Get-Process | Where-Object Name -eq “Calculator”
PS C:\> Get-Process | Where-Object {$_.Name -eq “Calculator”}
```

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

Este comando muestra únicamente una propiedad de cada objeto, mostrando todos estos valores en una tabla. Como alternativa más breve se puede utilizar el alias `fw`.
Los parámetros más destacables de este comando son:
•	`-Property`: para indicar si queremos que se muestre una propiedad diferente a la que muestra por defecto.
•	`-Column`: que mediante un valor numérico señalará cuántas columnas tendrá la tabla con los resultados mostrados.

### 2.5.2.- FORMAT-TABLE

Formatea la salida del comando redireccionado en forma de tabla donde mostrará un objeto en cada fila y las propiedades más relevante en las columnas. El alias para este comando es `ft`.
Un parámetro útil con `Format-Table` es `-AutoSize`, que adapta el tamaño de la salida para que se ajuste al tamaño disponible en la pantalla y así evitar que se recorte la salida.


### 2.5.3.- FORMAT-LIST

Por último, `Format-List` muestra la salida del comando como una lista de propiedades, indicando cada una de estas propiedades en una línea diferente.


 
***
[Volver al índice principal](index_UT06.md)