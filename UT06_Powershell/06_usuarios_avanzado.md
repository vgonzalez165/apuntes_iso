![Carátula UT06](imgs/caratula_ut06.png)

# 4.- FUNCIONES AVANZADAS EN LA GESTIÓN DE USUARIOS

Ahora que hemos aprendido a trabajar con usuarios y grupos en Powershell, vamos a utilizarlo como excusa para seguir avanzando en nuestro conocimiento de Powershell. En concreto, vamos a:

- Aprender a crear usuarios y grupos automáticamente, con lo que veremos cómo leer los datos que hay en un fichero CSV y seleccionar los que nos interesen.
- Añadir usuarios y grupos a un equipo diferente al nuestro estableciendo una conexión remota con el mismo.


## 4.1.- CREACIÓN AUTOMÁTICA DE USUARIOS Y GRUPO

Vamos a ver primero como haríamos para automatizar la creación de usuarios en nuestro sistema partiendo de un fichero con los datos de los usuarios que deseamos crear. La idea fundamental es muy sencilla, partimos de un fichero en formato CSV con información de los usuarios, extraemos los datos de cada usuario de dicho fichero y los utilizamos para dar de alta el usuario. Para esto necesitaremos un nuevo comando que nos permita iterar sobre todos los objetos devueltos por un comando, el comando `foreach`, que explicaremos un poco más adelante.

Además, también aprovecharemos para crear un script que 


### 4.1.1.- IMPORTACIÓN DEL FICHERO CSV

El primer paso es crear un fichero CSV con la información que queramos añadir a los usuarios que serán creados. El acrónimo CSV viene de *Comma Separated Values*, y es precisamente eso en lo que consiste, en un fichero donde tenemos una serie de filas con valores separados por un carácter especial, habitualmente comas.

Estos ficheros son muy útiles cuando queremos almacenar datos de forma tabulada en un formato altamente portable y fácilmente comprensible tanto por máquinas como por usuarios.

Habitualmente, la **primera fila** del fichero CSV contiene los encabezados que indican qué es cada uno de los valores según su posición, mientras que el **resto de las filas** corresponden a cada uno de los elementos almacenados representados por su secuencia de valores.

Al igual que pasa con muchos ficheros de texto, las líneas que comienzan por el carácter **almohadilla (#)** son **comentarios** que la máquina ignorará y que sirven para mostrar información al usuario.

El siguiente sería un ejemplo de fichero CSV que almacena dos elementos, cada uno tres valores diferentes.
 
```csv
# Usuarios para dar de alta en el sistema
nombre_usuario, nombre_completo, descripcion
victor, Victor J. González, Este será el primer usuario
Gonzalo, Gonzalo González, Y este el segundo usuario
```

Como ya hemos visto, el comando de Powershell para leer un fichero CSV es `Import-Csv`. Si lo ejecutamos veremos que nos devuelve el contenido del fichero formateado, pero si utilizamos el comando `Get-Member` podemos ver más claro que lo que estamos obteniendo es un conjunto de objetos con tantas propiedades como valores tenga nuestro fichero CSV y cuyo nombre es el nombre indicado en la primera fila del fichero CSV.

```powershell
PS C:\> Import-Csv .\users.csv

nombre_usuario nombre_completo    descripcion
-------------- ---------------    -----------
victor         Victor J. González Este será el primer usuario
Gonzalo        Gonzalo González   Y este el segundo usuario

PS C:\> Import-Csv .\users.csv | Get-Member

   TypeName: System.Management.Automation.PSCustomObject

Name            MemberType   Definition
----            ----------   ----------
Equals          Method       bool Equals(System.Object obj)
GetHashCode     Method       int GetHashCode()
GetType         Method       type GetType()
ToString        Method       string ToString()
descripcion     NoteProperty string descripcion=Este será el primer usuario
nombre_completo NoteProperty string nombre_completo=Victor J. González
nombre_usuario  NoteProperty string nombre_usuario=victor
```

Ahora necesitamos **iterar** sobre cada uno de estos objetos para utilizar los datos para crear los usuarios, y esto lo haremos con el comando `foreach`.


### 4.1.2.- EL COMANDO FOREACH

El comando `foreach` tiene múltiples usos y varias formas de ser utilizado, pero por ahora únicamente veremos como se puede utilizar dentro del pipeline tomando como entrada la salida de otro comando.

La sintaxis en este caso es de la siguiente manera:

```
comando | foreach { otro_comando }
```

El primer comando debe devolver una secuencia de objetos mientras que el segundo comando se ejecutará una vez por cada objeto que haya devuelto el primero. Para ello necesitamos poder referenciar de alguna manera al objeto que corresponde a cada una de las iteraciones, y esto lo conseguimos mediante la variable especial representada por `$_`. Donde pongamos esta variable será reemplazada en cada iteración por el objeto correspondiente.

Normalmente no querremos acceder a todo el objeto, sino a alguna de sus propiedades, y eso lo conseguimos con el carácter punto (.) tras la variable seguido por el nombre de la propiedad. Por ejemplo: 

```powershell
PS C:\> Get-LocalUser | foreach { Write-Output $_.name }
```

En esta línea hacemos lo siguiente:

- Obtenemos una serie de objetos que representan los usuarios locales del sistema. Supongamos que nos devuelve 5 usuarios.
- Para cada uno de esos 5 usuarios ejecutará el código entre llaves, por lo que el comando `Write-Output` se ejecutará 5 veces. Este comando simplemente muestra por pantalla lo que se le pase como parámetro.
- En la primera iteración `$_` hará referencia al primer usuario, en la segunda iteración al segundo usuario, y así sucesivamente.
- Como estamos poniendo `$_.name`, solo nos quedamos con una propiedad de cada objeto, la que corresponde al nombre del usuario, que por tanto es la que se mostrará por pantalla.


### 4.1.3.- JUNTANDO TODO

Ahora que ya tenemos claro cómo funcionan las herramientas que vamos a utilizar ya solo nos queda juntarlo todo. El código sería el siguiente:

```powershell
PS C:\> Import-Csv ‘C:\users.csv’ | foreach { 
		New-LocalUser -Name $_.nombre_usuario -FullName $_.nombre_completo
		}
```

Si lo ejecutas, verás que creará todos los usuarios que tenemos en el fichero CSV, aunque, como no hemos puesto la contraseña nos la pedirá para cada usuario que creemos. Ya te queda para cuando hagamos la práctica correspondiente incluir un campo con la contraseña en el fichero CSV.


### 4.1.4.- NUESTRO PRIMER SCRIPT

El comando anterior funciona, pero si tenemos que ejecutar frecuentemente puede ser bastante tedioso tener que introducirlo por teclado. Sobre todo, teniendo en cuenta que hemos omitido un gran número de parámetros que sí que convendría incluir en caso de querer crear usuarios en un entorno real. 

En esta situación, sería realmente muy interesante poder almacenar nuestros comandos en un fichero, por ejemplo, para poder utilizarlos cuando queramos. Y esto es precisamente lo que para lo que sirven los scripts. A grandes rasgos, los scripts son ficheros de texto que contienen una serie de comandos que se van a ejecutar secuencialmente cuando ejecutemos dicho script. 

Lo primero que debemos tener en cuenta cuando vamos a ejecutar scripts en nuestro sistema, es que coma por defecto Windows no permite ejecutar scripts que no estén firmados como medida de seguridad. Si vamos a crear nuestros propios scripts, necesitaremos decirle al sistema que permita la ejecución de dichos scripts. 

Esto se determina mediante la política de ejecución, la cual podemos ver si ejecutamos el comando `Get-ExecutionPolicy`. Las opciones son:

- `RemoteSigned`: permite ejecutar cualquier script que nosotros creemos, pero si descargamos algún script de Internet solo podremos ejecutarlo si está firmado por un publicador de confianza (Trusted Publisher).
- `AllSigned`: todos los scripts, incluso los creados por ti, deben ser firmados por un publicador de confianza.
- `Unrestricted`: todos los scripts, sea cual sea su origen, pueden ser ejecutados.
  
Podemos establecer la política que deseemos con el comando `Set-ExecutionPolicy`.

Para crear el script podemos utilizar cualquier editor de texto, como en Notepad, o el IDE incluido en Windows para la creación de scripts en Powershell, llamado **Powershell ISE**. Lo único con lo que hay que tener precaución es con que el fichero se guarde en texto plano, por lo que no son válidos procesadores de texto (como Word o el Wordpad), que añaden formato al texto.

El contenido del fichero para nuestro script sería el siguiente, siendo aconsejable guardar el script como un fichero con **extensión ps1**.
 
```powershell
# Crea varios usuarios a partir de un fichero CSV
Write-Output "Creando usuarios"
Import-Csv 'E:\users.csv'  |  foreach {
    New-LocalUser -Name $_.nombre_usuario -FullName nombre_completo
}
```

Para ejecutar el script simplemente debemos introducir su nombre en la línea de comandos teniendo cuidado de preceder el nombre con la ruta completa donde se encuentra. Por ejemplo, en mi caso el script se encuentra en el directorio raíz del disco E:\, por lo que la orden debería ser la siguiente:

```powershell
PS C:\> E:\crea_usuarios.ps1
Creando usuarios

cmdlet New-LocalUser en la posición 1 de la canalización de comandos
Proporcione valores para los parámetros siguientes:
Password:
```
 

## 4.2.- CONEXIÓN REMOTA A UN EQUIPO

Lo habitual no es administrar un sistema desde el mismo equipo, sino que el administrador del sistema lo hace mediante conexiones remotas sin moverse de su propio ordenador. Powershell está pensado desde su concepción en facilitar esta tarea, y de hecho, uno de los parámetros generales a todos los comandos es ComputerName, que sirve para hacer referencia al equipo remoto sobre el que se ejecutará el comando.

Dado que la administración remota conlleva una serie de riesgos de seguridad, hay que realizar una serie de pasos previos sobre los equipos para poder administrar de forma remota. 

### 4.2.1.- HABILITAR LA CONEXIÓN REMOTA

El primer paso es habilitar la conexión remota, esto debemos hacerlo en cada equipo al que nos queremos conectar de forma remota, es decir, el equipo sobre el que se ejecutaron los comandos. Para ello simplemente debemos abrir una consola de Power Shell en modo administrador y ejecutar el comando `Enable-PSRemoting -Force`.

Este comando inicia el servicio WinRM y lo prepara para que se inicie automáticamente con el sistema, además crea una regla en el firewall para permitir las conexiones entrantes. Sí estamos trabajando en un dominio, esta será la única configuración que tenemos que realizar. Sí en cambio tenemos nuestros equipos en un grupo de trabajo debemos realizar algunas configuraciones adicionales. 

### 4.2.2.- CONFIGURACIÓN PARA GRUPOS DE TRABAJO

Lo primero que tenemos que tener en cuenta si nuestros equipos están en grupos de trabajo es que la red tiene que estar definida como privada y no como pública. Cómo te habrás fijado cada vez que nos conectamos a una red nueva se nos pregunta por el tipo de red de que se trata. Sí en ese momento seleccionamos que la red pública cómo podremos cambiarlo ahora yendo a propiedades del adaptador de red y debajo de perfil de red seleccionar *privado*. 

El siguiente paso es establecer una relación de confianza entre ambos equipos, el que ejecuta el comando y desde el que se lanzó a dicho comando. Esto se consigue con la siguiente línea de código: 

```powershell
PS C:\> Set-Item wsman:\localhost\client\trustedhosts *
```

Con la orden anterior estamos utilizando el asterisco para indicar que nuestro equipo confía en cualquier otro equipo. Pera nuestras prácticas es suficiente, pero hay que tener en cuenta que para un entorno de producción esto sería bastante inseguro. En ese caso lo ideal sería reemplazar el asterisco por las direcciones IP de los equipos en que confiamos separadas por comas. Después de realizar esto debemos reiniciar el servicio WinRM, lo que conseguiremos con la orden `Restart-Service WinRM`.


### 4.2.3.- PROBANDO LA CONEXIÓN REMOTA

Una vez realizados todos los pasos debemos verificar que el servicio de conexión remota funciona perfectamente, para ello disponemos del comando `Test-WsMan` al cual se le pasa como parámetro el nombre o la dirección IP del equipo cuya canción de queramos verificar. 

Llegados a este punto tenemos 3 opciones para realizar la administración remota del otro equipo:

- Iniciar una **sesión remota** en el otro equipo de forma que lo que escribamos nuestra pantalla es como si la escribiéramos en el equipo remoto. 
- Indicar que el comando se va a ejecutar un equipo remoto mediante el parámetro ComputerName , opción que solo está presente en algunos comandos de Powershell. 
- Utilizar el comando Invoke-Command para ejecutar cualquier comando o script que tengamos en el otro equipo.


### 4.2.4.- INICIAR UNA SESIÓN INTERACTIVA

Los comandos para gestionar sesiones remotas son `Enter-PSSession` y `Exit-PSSession` para iniciar y finalizar respectivamente una sesión en otro equipo. Al primero se le debe pasar como parámetro el nombre o dirección IP del equipo con el que queremos establecer la sesión, así como opcionalmente el parámetro `Credentials` para indicar las credenciales con las que queremos iniciar sesión. El segundo comando no requiere ningún tipo de parámetro. 


### 4.2.5.- EJECUTAR UN COMANDO CON EL PARÁMETRO COMPUTERNAME

Hay una serie de comandos que admiten directamente el parámetro con `-ComputerName` de forma que simplemente incluyendo este parámetro seguido de un nombre de equipo o su dirección IP, se ejecutarán en el equipo remoto. 

Con la orden de la siguiente imagen puedes ver todos los comandos que lo permiten. 

```powershell
PS C:\> Get-Command | Where { $_.parameters.keys -Contains "ComputerName" -and $_.parameters.keys -notcontains "Session"}

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Add-Computer                                       3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Clear-EventLog                                     3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Get-EventLog                                       3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Get-HotFix                                         3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Get-Process                                        3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Get-PSSession                                      3.0.0.0    Microsoft.PowerShell.Core
...
```

### 4.2.6.- EJECUTAR UN COMANDO REMOTO

Si has ejecutado el comando anterior, te habrás fijado los comandos relativos a la gestión de usuarios locales no están incluidos en los conventos devueltos, por lo que no es una opción válida para crear usuarios remotos en otro equipo. Lo mismo nos pasará con otros muchos comandos o con scripts creados por nosotros queremos ejecutar remotamente. En estos casos tenemos que utilizar el comando `Invoke-Command`.

Los parámetros qué podemos utilizar con este comando son: 

- `ComputerName`: el equipo en el que se ejecutará el comando. Se puede indicar una serie de equipos separando sus nombres o direcciones IP mediante comas. 
- `ScriptBlock`: Detrás de este parámetro tenemos que introducir el comando que queramos ejecutar en el equipo remoto, sin embargo, hay que tener en cuenta que este comando tiene que estar rodeado por los símbolos de llaves. 
- `FilePath`: Si lo que queremos ejecutar en el equipo remoto es un script debemos indicar su ruta mediante este parámetro. Hay que tener en cuenta que la ruta hace referencia al equipo desde el que se está invocando el comando y no sobre qué se va a ejecutar.
  
A continuación, tienes dos ejemplos de uso este comando. En el primero obtenemos el listado de usuarios locales de 2 equipos de la red, mientras que en el segundo estamos ejecutando un script en el equipo cuya IP se indica. 

```powershell
PS C:\> Invoke-Command -ComputerName Server01, Server02 -ScriptBlock {Get-LocalUser}
PS C:\> Invoke-Command -ComputerName 192.168.1.1 -FilePath c:\Scripts\DiskCollect.ps1
```


### 4.2.7.- CONEXIONES REMOTAS A UNA MÁQUINA VIRTUAL

Todos los pasos anteriores son válidos para conectarse a un equipo remoto, ya sea un equipo físico o una máquina virtual que se está ejecutando en otro equipo. Pero si queremos conectarnos a una máquina virtual de Hyper-V alojada en el propio equipo hay un mecanismo, denominado Powershell Direct, que permite realizar la conexión independientemente de la configuración de administración remota o la configuración de red. Con Powershell Direct podemos tanto iniciar una sesión interactiva como ejecutar un comando o script mediante el comando Invoke-Command.

Los únicos requisitos necesarios para esta conexión son:

- La máquina virtual debe ejecutarse localmente en el host.
- La máquina virtual debe estar activada y en funcionamiento con al menos un perfil de usuario configurado.
- Hay que realizar la conexión como administrador de Hyper-V
- Deben facilitarse credenciales de usuario válidas para la máquina virtual

Para iniciar una sesión interactiva hay que hacerlo de forma análoga a como hacíamos al conectarnos a un equipo remoto con el comando `Enter-PSSession`, pero pasando el nombre de la máquina virtual o su identificador.

```powershell
PS C:\> Enter-PSSession -VMName <Nombre_MV>
PS C:\> Enter-PSSession -VMId <ID_MV>
```

La ejecución remota de un comando o script es equivalente, usando los mismos parámetros para indicar el nombre de la máquina virtual o su identificador.
