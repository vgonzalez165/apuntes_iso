![Carátula UT06](imgs/caratula_ut06.png)


### Contenidos

1. [Introducción a Powershell](01_introducción.md)
2. [Objetos y el pipeline](02_pipelines.md)
3. [Tipos de datos y variables](03_tipos_datos_y_variables.md)
4. [El sistema de ficheros en Powershell](04_sistema_ficheros.md)
5. [Gestión de Hyper-V desde Powershell](05_hyperv.md)
6. [Gestión de usuarios y grupos](06_usuarios.md)
7. [**Gestión avanzada de usuarios y grupos**](07_usuarios_avanzado.md)
8. [Powershell y el almacenamiento](08_almacenamiento.md)


# 8.- CONEXIONES REMOTAS

Una parte muy importante de Powershell es la posibilidad de ejecutar comandos o scripts de forma remota en otra máquina de la red o en una máquina virtual de Hyper-V sin necesidad de acceder directamente a ella.

En este apartado veremos ambas posibilidades, abordando en primer lugar la ejecución de comandos y scripts en una máquina virtual desde el equipo anfitrión y a continuación los pasos a realizar para establecer una relación de confianza entre dos equipos físicos.


## 8.2.- Conexiones remotas a una máquina virtual

La realización de conexiones remotas de Powershell desde el equipo anfitrión a una de las máquinas virtuales de Hyper-V que se están ejecutando en dicho equipo es muy sencilla gracias a un mecanismo denominado **Powershell Direct**.

Hay dos maneras de ejecutar Powershell Direct:

- Creación y salida de una sesión de Powershell Direct mediante cmdlets PSSession.
- Ejecución de un script o comando con el cmdlet `Invoke-Command`.

Hay una serie de requisitos que se deben cumplir para usar Powershell Direct:

- La máquina virtual debe ejecutarse localmente en el host y estar iniciada.
- Se debe iniciar sesión en el equipo anfitrión como *Administrador de Hyper-V*
- Hay que facilitar unas credenciales de usuario válidas en la máquina virtual
- Tanto el sistema operativo anfitrión como el de la máquina virtual deben ser por lo menos Windows 10 o Windows Server 2016


### 8.2.3.- Sesiones con Powershell Direct

Abrir una sesión con Powershell Direct significa que todos los comandos que introduzcamos en la consola se ejecutarán como si estuviéramos en la máquina virtual.

El comando para abrir la sesión es `Enter-PSSession`, al cual se le deberá pasar o bien el nombre de la máquina mediante el parámetro `-VMName` o bien el identificador de la máquina mediante el parámetro `-VMId`.

```powershell
PS C:\Users\victor> Enter-PSSession -VMName W10

cmdlet Enter-PSSession at command pipeline position 1
Supply values for the following parameters:
Credential
User: vgonzalez
Password for user vgonzalez: ****

[W10]: PS C:\Users\vgonzalez\Documents>
```

Como se puede apreciar en el código anterior, una vez que iniciemos sesión cambia el *prompt* para indicarnos que todas las órdenes que introduzcamos se ejecutarán en la máquina virtual y no en el anfitrión.

Para finalizar la sesión hay que utilizar el comando `Exit-PSSession`.

```powershell
[W10]: PS C:\Users\vgonzalez\Documents> Exit-PSSession
PS C:\Users\victor>
```


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




 

## 8.2.- Conexión remota a un equipo

Lo habitual no es administrar un sistema desde el mismo equipo, sino que el administrador del sistema lo hace mediante conexiones remotas sin moverse de su propio ordenador. Powershell está pensado desde su concepción en facilitar esta tarea, y de hecho, uno de los parámetros generales a todos los comandos es `-ComputerName`, que sirve para hacer referencia al equipo remoto sobre el que se ejecutará el comando.

Dado que la administración remota conlleva una serie de riesgos de seguridad, hay que realizar una serie de pasos previos sobre los equipos para poder administrar de forma remota. 

### 7.2.1.- Habilitar la conexión remota

El primer paso es habilitar la conexión remota, esto debemos hacerlo en cada equipo al que nos queremos conectar de forma remota, es decir, el equipo sobre el que se ejecutaron los comandos. Para ello simplemente debemos abrir una consola de Power Shell en modo administrador y ejecutar el comando `Enable-PSRemoting -Force`.

Este comando inicia el servicio WinRM y lo prepara para que se inicie automáticamente con el sistema, además crea una regla en el firewall para permitir las conexiones entrantes. Sí estamos trabajando en un dominio, esta será la única configuración que tenemos que realizar. Sí en cambio tenemos nuestros equipos en un grupo de trabajo debemos realizar algunas configuraciones adicionales. 

### 7.2.2.- Configuración para grupos de trabajo

Lo primero que tenemos que tener en cuenta si nuestros equipos están en grupos de trabajo es que la red tiene que estar definida como privada y no como pública. Cómo te habrás fijado cada vez que nos conectamos a una red nueva se nos pregunta por el tipo de red de que se trata. Sí en ese momento seleccionamos que la red pública cómo podremos cambiarlo ahora yendo a propiedades del adaptador de red y debajo de perfil de red seleccionar *privado*. 

El siguiente paso es establecer una relación de confianza entre ambos equipos, el que ejecuta el comando y desde el que se lanzó a dicho comando. Esto se consigue con la siguiente línea de código: 

```powershell
PS C:\> Set-Item wsman:\localhost\client\trustedhosts *
```

Con la orden anterior estamos utilizando el asterisco para indicar que nuestro equipo confía en cualquier otro equipo. Pera nuestras prácticas es suficiente, pero hay que tener en cuenta que para un entorno de producción esto sería bastante inseguro. En ese caso lo ideal sería reemplazar el asterisco por las direcciones IP de los equipos en que confiamos separadas por comas. Después de realizar esto debemos reiniciar el servicio WinRM, lo que conseguiremos con la orden `Restart-Service WinRM`.


### 7.2.3.- Probando la conexión remota

Una vez realizados todos los pasos debemos verificar que el servicio de conexión remota funciona perfectamente, para ello disponemos del comando `Test-WsMan` al cual se le pasa como parámetro el nombre o la dirección IP del equipo cuya canción de queramos verificar. 

Llegados a este punto tenemos 3 opciones para realizar la administración remota del otro equipo:

- Iniciar una **sesión remota** en el otro equipo de forma que lo que escribamos nuestra pantalla es como si la escribiéramos en el equipo remoto. 
- Indicar que el comando se va a ejecutar un equipo remoto mediante el parámetro ComputerName , opción que solo está presente en algunos comandos de Powershell. 
- Utilizar el comando Invoke-Command para ejecutar cualquier comando o script que tengamos en el otro equipo.


### 7.2.4.- Iniciar una sesión interactiva

Los comandos para gestionar sesiones remotas son `Enter-PSSession` y `Exit-PSSession` para iniciar y finalizar respectivamente una sesión en otro equipo. Al primero se le debe pasar como parámetro el nombre o dirección IP del equipo con el que queremos establecer la sesión, así como opcionalmente el parámetro `Credentials` para indicar las credenciales con las que queremos iniciar sesión. El segundo comando no requiere ningún tipo de parámetro. 


### 7.2.5.- Ejecutar un comando con el parámetro ComputerName

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

### 7.2.6.- Ejecutar un comando remoto

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



***
[Volver al índice principal](index_UT06.md)