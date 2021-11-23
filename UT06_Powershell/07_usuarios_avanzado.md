![Carátula UT06](imgs/caratula_ut06.png)


### Contenidos

1. [Introducción a Powershell](01_introducción.md)
2. [Objetos y el pipeline](02_pipelines.md)
3. [Tipos de datos y variables](03_tipos_datos_y_variables.md)
4. [El sistema de ficheros en Powershell](04_sistema_ficheros.md)
5. [Gestión de Hyper-V desde Powershell](05_hyperv.md)
6. [Gestión de usuarios y grupos](06_usuarios.md)
7. [**Gestión avanzada de usuarios y grupos**](07_usuarios_avanzado.md)
8. [Conexión remota](08_conexion_remota.md)
9. [Powershell y el almacenamiento](08_almacenamiento.md)


# 7.- FUNCIONES AVANZADAS EN LA GESTIÓN DE USUARIOS

Ahora que hemos aprendido a trabajar con usuarios y grupos en Powershell, vamos a utilizarlo como excusa para seguir avanzando en nuestro conocimiento de Powershell. En concreto, vamos a:

- Aprender a crear usuarios y grupos automáticamente, con lo que veremos cómo leer los datos que hay en un fichero CSV y seleccionar los que nos interesen.
- Añadir usuarios y grupos a un equipo diferente al nuestro estableciendo una conexión remota con el mismo.


## 7.1.- Creación automática de usuarios y grupos

Vamos a ver primero como haríamos para automatizar la creación de usuarios en nuestro sistema partiendo de un fichero con los datos de los usuarios que deseamos crear. La idea fundamental es muy sencilla, partimos de un fichero en formato CSV con información de los usuarios, extraemos los datos de cada usuario de dicho fichero y los utilizamos para dar de alta el usuario. Para esto necesitaremos un nuevo comando que nos permita iterar sobre todos los objetos devueltos por un comando, el comando `foreach`, que explicaremos un poco más adelante.

Además, también aprovecharemos para crear un script que 


### 7.1.1.- Importación del fichero CSV

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


### 7.1.2.- El comando ForEach

El comando `foreach` tiene múltiples usos y varias formas de ser utilizado, pero por ahora únicamente veremos como se puede utilizar dentro del pipeline tomando como entrada la salida de otro comando.

La sintaxis en este caso es de la siguiente manera:

```powershell
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


### 7.1.3.- Juntando todo

Ahora que ya tenemos claro cómo funcionan las herramientas que vamos a utilizar ya solo nos queda juntarlo todo. El código sería el siguiente:

```powershell
PS C:\> Import-Csv ‘C:\users.csv’ | foreach { 
		New-LocalUser -Name $_.nombre_usuario -FullName $_.nombre_completo
		}
```

Si lo ejecutas, verás que creará todos los usuarios que tenemos en el fichero CSV, aunque, como no hemos puesto la contraseña nos la pedirá para cada usuario que creemos. Ya te queda para cuando hagamos la práctica correspondiente incluir un campo con la contraseña en el fichero CSV.


### 7.1.4.- Nuestro primer script

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
 

***
[Volver al índice principal](index_UT06.md)