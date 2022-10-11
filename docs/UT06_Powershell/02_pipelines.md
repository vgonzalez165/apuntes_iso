---
layout: default
title: Control de la salida con pipelines
parent: UT06. Powershell
nav_order: 2
---

# 2.- Control de la salida con pipelines

## 2.1.- La canalización o pipeline

El **pipeline** o la **canalización**, representado por el símbolo barra vertical (`|`), es utilizado para combinar diversos cmdlets de forma que la salida de uno es enviada a la entrada del siguiente, de forma muy similar a cómo se hace en Linux. Esto permite enlazar varios comandos en una especie de flujo de datos en el que cada uno de los comandos realiza algún tipo de operación sobre los datos generados por el comando anterior. 

La sintaxis es de la forma:

```powershell
Comando1 | Comando2 | Comando3
```

En este ejemplo, la salida de `Comando1`es enviada a `Comando2`, que realizará algún tipo de operación con esos datos y enviará la salida generada a `Comando3`. Como este ya es el último comando, su salida será enviada a la consola para ser mostrada al usuario.

Veamos un ejemplo sencillo de utilización de la canalización.

```powershell
C:\> Get-Process notepad | Stop-Process
```

Aquí, el primer comando de la canalización buscará todos los procesos que hay en el sistema que se llamen `notepad`, es decir, todas las instancias del bloc de notas que tengamos abiertas. Este listado de procesos lo enviará al siguiente comando de la canalización, `Stop-Process`, cuyo cometido es finalizar un proceso. Por tanto, tomará cada proceso del listado que recibe y lo cerrará.



## 2.2.- Objetos en Powershell

Un aspecto importante al trabajar con la canalización, sobre todo si has trabajado antes con Linux, es que Powershell es un Shell **orientado a objetos**, y, por tanto, la información que se intercambia entre los comandos no son meras cadenas de caracteres, sino que son objetos.

Un **objeto** se puede entender como una abstracción de un elemento, por ejemplo, un usuario, un proceso o un fichero. Cada objeto tiene una serie de **propiedades**, que son las características intrínsecas de dicho objeto, y una serie de **métodos**, que son las acciones que se pueden realizar sobre dicho objeto.

Por ejemplo,supongamos que MS-DOS ejecutamos la orden:

```powershell
C:\> dir c:\datos\*.txt
```

Cuando ejecutamos esa orden el sistema mira el contenido del directorio `C:\datos` buscando los ficheros que tengan extensión `txt` y, a partir del mismo, genera una salida por pantalla, es decir, una secuencia de caracteres, que muestran el contenido de dicho directorio. Si quisiéramos realizar algún tipo de operación automatizada con la salida de este comando, tendremos que trabajar con esa secuencia de caracteres que ha generado. Por ejemplo, podríamos guardar todos esos caracteres en un fichero, o filtrarlos para que muestre todas las líneas que contengan una determinada cadena, pero solo operaciones que trabajen con esa cadena de texto.

Veamos ahora la orden equivalente en Powershell, que será:

```powershell
C:\> Get-Item C:\datos\*.txt
```

Si la ejecutamos veremos que la salida es muy similar a la del comando anterior, pero, sin embargo, lo que hace es muy diferente. El comando obtiene el listado de ficheros del disco, pero lo hace como una **lista de objetos**, cada uno de los cuales representa un fichero. Una vez que tiene esta lista de objeto mira a ver que hay que hacer con ellos: si tiene que enviárselos a otro comando mediante el pipeline (como veremos un poco más adelante) le enviará la lista de objetos; en cambio, si tiene que mostrar la salida por pantalla tomará esos objetos y elegirá la representación más adecuada para mostrársela al usuario, momento en el que dejarán de ser un conjunto de objetos para ser una secuencia de caracteres.

Podemos saber qué tipo de objetos devuelve un comando de Powershell, así como el listado completo de propiedades y métodos del mismo,  canalizando la salida al comando `Get-Member`.

```powershell
PS C:\> Get-LocalUser -Name Victor| Get-Member

   TypeName: Microsoft.PowerShell.Commands.LocalUser

Name                   MemberType Definition
----                   ---------- ----------
Clone                  Method     Microsoft.PowerShell.Commands.LocalUser Clone()
Equals                 Method     bool Equals(System.Object obj)
GetHashCode            Method     int GetHashCode()
GetType                Method     type GetType()
ToString               Method     string ToString()
AccountExpires         Property   System.Nullable[datetime] AccountExpires {get;set;}
Description            Property   string Description {get;set;}
Enabled                Property   bool Enabled {get;set;}
FullName               Property   string FullName {get;set;}
LastLogon              Property   System.Nullable[datetime] LastLogon {get;set;}
...

```

En el ejemplo anterior se puede ver que el comando `Get-LocalUser` devuelve objetos de tipo `Microsoft.Powershell.Commands.LocalUser`. Estos objetos representan cada uno de los usuarios del sistema, y por tanto, cada objeto tendrá una serie de propiedades que definen al usuario y también de métodos que permiten realizar operaciones sobre dicho usuario. 

Por ejemplo, la propiedad `Enabled` almacenará un valor booleano (que puede ser verdadero o falso) que indicará si la cuenta está habilitada o no. Hay muchas formas de ver el valor de la propiedad de un objeto, una de ellas es rodeando el comando que genera el objeto entre paréntesis y poniendo a continuación el nombre de la propiedad que queremos obtener separándola del comando con el carácter punto (`.`).


```powershell
PS C:\> (Get-LocalUser -Name Victor).Enabled
True
```
 
Aparte de las propiedades, los objetos en Powershell pueden tener otros tipos de elementos. El tipo de cada elemento se puede ver en la columna `MemberType`, siendo los más relevantes:

- **Property**: son las propiedades del objeto, por ejemplo, el nombre del proceso o su identificador.
- **Method**: los métodos permiten realizar algún tipo de operación con el objeto, por ejemplo, el método ```kill()``` mataría el objeto. 
- **AliasProperty**: un nombre alternativo para una propiedad, normalmente más conciso, por ejemplo, **Name** contiene el mismo valor que la propiedad **ProcessName**.

Es posible mostrar únicamente los elementos de un determinado tipo con el parámetro `-MemberType` del comando `Get-Member`.

```powershell
PS C:\> Get-LocalUser -Name Victor | Get-Member -MemberType Properties

   TypeName: Microsoft.PowerShell.Commands.LocalUser

Name                   MemberType Definition
----                   ---------- ----------
AccountExpires         Property   System.Nullable[datetime] AccountExpires {get;set;}
Description            Property   string Description {get;set;}
Enabled                Property   bool Enabled {get;set;}
FullName               Property   string FullName {get;set;}
LastLogon              Property   System.Nullable[datetime] LastLogon {get;set;}
...
```


Si nos fijamos vemos que los objetos devueltos por el comando ```Get-LocalUser``` tienen varias decenas de propiedades. Sin embargo, cuando lo ejecutamos únicamente podemos ver por pantalla unas pocas: el nombre, si está habilitado el usuario y la descripción.

```powershell
PS C:\Users\victor> Get-LocalUser

Name               Enabled Description
----               ------- -----------
Administrador      False   Cuenta integrada para la administración del equipo o dominio
DefaultAccount     False   Cuenta de usuario administrada por el sistema.
Invitado           False   Cuenta integrada para el acceso como invitado al equipo o dominio
victor             True
WDAGUtilityAccount False   Una cuenta de usuario que el sistema administra y usa para escenarios de Protección de apli…

```

Esto se debe a que Powershell solo nos muestra por pantalla las propiedades que considera más relevantes para aumentar la legibilidad. Si queremos decidir qué propiedades queremos, debemos hacer uso del comando ```Select-Object``` que permite filtrar un objeto seleccionando únicamente un subconjunto de las propiedades.

```powershell
PS C:\> Get-LocalUser | Select-Object Name, SID

Name               SID
----               ---
Administrador      S-1-5-21-236780772-618523378-4141520374-500
DefaultAccount     S-1-5-21-236780772-618523378-4141520374-503
Invitado           S-1-5-21-236780772-618523378-4141520374-501
victor             S-1-5-21-236780772-618523378-4141520374-1001
WDAGUtilityAccount S-1-5-21-236780772-618523378-4141520374-504
```

Si lo que se desea es ver todas las propiedades de los objetos se puede utilizar el carácter comodín (`*`), que fuerza a que se muestren todas las propiedades.

```powershell
PS C:\> Get-LocalUser -Name Victor | Select-Object *

AccountExpires         :
Description            :
Enabled                : True
FullName               :
PasswordChangeableDate : 29/09/2021 16:26:41
PasswordExpires        :
UserMayChangePassword  : True
PasswordRequired       : False
PasswordLastSet        : 29/09/2021 16:26:41
LastLogon              : 27/10/2021 10:30:53
Name                   : victor
SID                    : S-1-5-21-236780772-618523378-4141520374-1001
PrincipalSource        : Local
ObjectClass            : User
```


## 2.3.- Ordenar, agrupar y medir la salida de un comando

Hay comandos que proporcionan un gran número de elementos al ejecutarse por lo que puede ser conveniente manipular dicha salida para que sea más fácilmente comprensible. Para ello utilizaremos los cmdlets `Sort-Object`, `Group-Object` y `Measure-Object`, que ordenan, agrupan y cuentan respectivamente.


### 2.3.1.- Ordenando con el comando `Sort-Object`

El comando `Sort-Object` admite un conjunto de objetos como entrada y devuelve ese mismo conjunto de objetos pero **ordenados** según el valor de la propiedad que se indique. 

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


## 2.4.- Filtrado de objetos

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
- La variable `$_` hace referencia a cada uno de los objetos que recibe. Es decir, el comando `Get-Process` devuelve un conjunto de objetos, los cuales comprueba mediante el filtro de `Where-Object` en sucesivas iteraciones. La forma de hacer referencia a cada uno de los objetos en cada iteración es mediante la variable `$_`.


La otra forma de hacerlo se muestra en el siguiente código, donde se puede ver que no es necesario incluir llaves ni ningún parámetro:

```powershell
PS C:\> Get-Process | Where-Object Name -eq “Calculator”

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     31    24,09      42,77       0,78   20804   1 Calculator
```

Respecto a qué operadores usar en cada situacion, los operadores de comparación (mayor y menor) se utilizan con **propiedades numéricas**. Por ejemplo, si queremos obtener la lista de objetos cuyo consumo de CPU ha sido superior a los 120 segundos deberíamos introducir la siguiente orden:

```powershell
PS C:\> Get-Process | Where-Object CPU -gt 100

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     23     7,12       8,47     137,81    2120   1 AdobeCollabSync
     19     7,87      13,88     167,69    7528   1 ctfmon
    126   189,64     143,66   1.053,48    7828   1 explorer
     62   188,28     128,72     101,16    3732   1 firefox
```

Los operadores `-like` y `-notlike` se utilizan para realizar comparaciones de cadenas utilizando comodines. Podemos utilizar dos comodines:

- **Asterisco**: representa una cadena de cualquier longitud que también puede ser la cadena vacía.
- **Interrogación**: representa un único carácter.
- 
Por ejemplo, si queremos localizar el proceso correspondiente al programa *Word* y no sabemos exactamente como se llama podemos hacer una búsqueda de la siguiente forma, representando cadenas que contengan la cadena *word* en su interior y que pueden tener cualquier cadena tanto antes como después.

```powershell
PS C:\> Get-Process | Where-Object Name -like *word*

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     67   112,89     154,90      86,31   16732   1 WINWORD
```


## 2.5.- Formateando la salida

Cuando ejecutamos un comando en Powershell vemos un texto con la salida del comando, pero en realidad cualquier comando devuelve uno o varios objetos y lo que se nos muestra por pantalla son las propiedades más relevantes de dichos objetos en un formato predeterminado. Sin embargo, es posible modificar tanto las propiedades que se nos muestran de cada objeto, como el formato en que se hace.
Powershell tiene cinco cmdlets para el **formato de la salida** de comandos, de los que nosotros veremos `Format-List`, `Format-Table` y `Format-Wide`.


### 2.5.1.- Format-Wide

Este comando muestra únicamente una propiedad de cada objeto, mostrando todos estos valores en una tabla. Como alternativa más breve se puede utilizar el alias `fw`.
Los parámetros más destacables de este comando son:
•	`-Property`: para indicar si queremos que se muestre una propiedad diferente a la que muestra por defecto.
•	`-Column`: que mediante un valor numérico señalará cuántas columnas tendrá la tabla con los resultados mostrados.

### 2.5.2.- Format-Table

Formatea la salida del comando redireccionado en forma de tabla donde mostrará un objeto en cada fila y las propiedades más relevante en las columnas. El alias para este comando es `ft`.
Un parámetro útil con `Format-Table` es `-AutoSize`, que adapta el tamaño de la salida para que se ajuste al tamaño disponible en la pantalla y así evitar que se recorte la salida.


### 2.5.3.- Format-List

Por último, `Format-List` muestra la salida del comando como una lista de propiedades, indicando cada una de estas propiedades en una línea diferente.


 
***
[Volver al índice principal](index_UT06.md)