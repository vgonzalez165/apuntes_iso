![Carátula UT06](imgs/caratula_ut06.png)


### Contenidos

1. [Introducción a Powershell](01_introducción.md)
2. [Objetos y el pipeline](02_pipelines.md)
3. [Tipos de datos y variables](03_tipos_datos_y_variables.md)
4. [El sistema de ficheros en Powershell](04_sistema_ficheros.md)
5. [Gestión de Hyper-V desde Powershell](05_hyperv.md)
6. [**Gestión de usuarios y grupos**](06_usuarios.md)
7. [Gestión avanzada de usuarios y grupos](07_usuarios_avanzado.md)
8. [Conexión remota](08_conexion_remota.md)
9. [Powershell y el almacenamiento](08_almacenamiento.md)


# 6.- USUARIOS Y GRUPOS DESDE POWERSHELL

Powershell dispone de comandos para automatizar fácilmente la administración de usuarios y grupos en un sistema. Estos comandos se pueden dividir en tres grupos: los relacionados con los usuarios, los relacionados con los grupos, y los relacionados con la pertenencia a grupos, que se identifican respectivamente por los nombres `LocalUser`, `LocalGroup` y `LocalGroupMember`.

## 6.1.- Usuarios

En este caso los comandos disponibles son:

- `New-LocalUser`: crea un nuevo usuario.
- `Set-LocalUser`: modifica las propiedades de un usuario existente.
- `Remove-LocalUser`: elimina un usuario local.
- `Get-LocalUser`: recoge información de los usuarios locales.
- `Rename-LocalUser`: cambia el nombre de un usuario.
- `Enable-LocalUser` y Disable-LocalUser: habilita o deshabilita una cuenta de usuario.

Todos estos comandos trabajan con objetos de la clase `LocalUser`, cuyas propiedades coinciden con los valores que ya conocemos del diálogo de creación de usuarios del MMC: Name, FullName, SID, …


### 6.1.1.- Gestión de la contraseña con variables

Según lo que hemos visto en el apartado anterior, la forma de crear un usuario nuevo sería con la siguiente orden:

```powershell
PS C:\> New-LocalUser 'Nombre' -Password '123456' -Fullname 'Nombre completo' -Description 'Descripción'
```

Aunque esta orden es sintácticamente válida, nos mostrará un mensaje de error, a la vez que salta a la vista que supone un importante problema de seguridad, ya que estamos escribiendo la contraseña del usuario de una forma bastante visible, y que, además, quedaría almacenada en el historial.

Para que no nos de errores sintácticos tenemos que introducirla de la siguiente forma, aunque seguimos teniendo problemas de seguridad.

```powershell
PS C:\> New-LocalUser 'Nombre' -Password (ConvertTo-SecureString -AsPlainText -String 'paso' -force) -Fullname 'Nombre completo' -Description 'Descripción'
```

Para ser más cuidadosos y evitar estos problemas podemos hacer uso del comando `Read-Host`. Este comando es muy sencillo, ya que su cometido es pedir al usuario que introduzca un valor mediante el teclado. Además, si acudimos a su ayuda veremos que únicamente tiene dos parámetros:

- `-Prompt`: un texto que se muestra en pantalla para decirle al usuario qué esperamos que introduzca.
- `-AsSecureString`: para indicar que el texto que va a introducir el usuario será una contraseña, y, por tanto, la tratará como tal.
  
Ahora nos falta un detalle, y es que debemos decidir donde guardar la contraseña que introduzca el usuario. Para ello, podemos definir **variables** en Powershell, las cuales están identificadas por un nombre y pueden contener un valor, ya sea un valor simple (cadena, número, …) o un objeto (devuelto por un comando de Powershell).

Las variables en Powershell se identifican por un nombre que siempre debe comenzar por el símbolo dólar, por ejemplos, `$password`.

Para almacenar un valor en una variable debemos utilizar el operador igual (=), indicando después de este el valor que queremos que contenga la variable, ya sea indicando directamente el valor o con un comando que lo devolverá. 

```powershell
PS C:\> $mes='octubre'
PS C:\> $mes
octubre
```
 
Teniendo esto en cuenta, utilizaremos una variable para almacenar el valor de la contraseña introducida por el usuario. Observa en la siguiente imagen que el valor guardado no es el texto de contraseña que se ha introducido, sino que por seguridad se almacena en un objeto de tipo `SecureString` que la almacena.

```powershell
PS C:\> $pass=Read-Host -Prompt "Introduce la contraseña:" -AsSecureString
Introduce la contraseña:: ****
PS C:\> $pass
System.Security.SecureString
```
 
Ahora que ya tenemos la contraseña podemos crear el usuario de forma segura utilizando la variable que acabamos de crear para pasar la contraseña al comando `New-LocalUser`.

```powershell
PS C:\> $pass=Read-Host -Prompt "Introduce la contraseña:" -AsSecureString
Introduce la contraseña:: ****
PS C:\> New-LocalUser "MiUsuario" -Password $pass

Name      Enabled Description
----      ------- -----------
MiUsuario True
```
 
Algo que hay que tener en cuenta cuando creamos usuarios en Powershell es que el comando `New-LocalUser` crea el usuario, pero **no lo añade a ningún grupo**, ni tan siquiera al grupo Usuarios, por lo que es una tarea que tendremos que realizar después de crear cada usuario.


### 6.1.2.- Gestión de la contraseña con paréntesis

Hay una segunda alternativa que nos permite obtener el mismo resultado sin utilizar variables, y es mediante el uso de los paréntesis. Los **paréntesis** sirven para procesar un comando cuyo resultado se pasará como parámetro a otro comando.

Veamos primero como quedaría y luego analizamos su funcionamiento:

```powershell
PS C:\> New-LocalUser "MiUsuario2" -Password (Read-Host -Prompt "Pass:" -AsSecureString)
Pass: ****

Name       Enabled Description
----       ------- -----------
MiUsuario2 True

```

Como se puede ver, el comando `Read-Host` está rodeado entre paréntesis, lo que indica que es lo primero que se tiene que ejecutar, en este caso preguntándonos por la contraseña. Una vez ejecutado y ya obtenido el valor que devuelve es cuando se ejecuta el otro comando (`New-LocalUser`), obteniendo en el parámetro `-Password` el valor antes obtenido.

Otros ejemplos del uso de paréntesis son:

1.	Obtener el tiempo que ha transcurrido desde una fecha hasta hoy.

```powersell
PS C:\> (Get-Date) – (Get-Date -Day 7 –Month 7 -Year 1973)
```

2.	Reiniciar remotamente todos los equipos listados en un fichero

```powershell
PS C:\> Restart-Computer -ComputerName (Get-Content C:\computers.txt) -Force
```


## 6.2.- Grupos

### 6.2.1.- Gestión de grupos

La gestión de grupos con Powershell es análoga a la gestión de usuarios, con comandos muy similares, pero utilizando el nombre **LocalGroup**. Estos nos permitirán obtener información de los grupos existentes en el sistema (`Get-LocalGroup`), crear un nuevo grupo (`New-LocalGroup`), eliminar un grupo existente (`Remove-LocalGroup`), renombrar un grupo (`Rename-LocalGroup`) y cambiar las propiedades de un grupo (`Set-LocalGroup`).

### 6.2.2.- Pertenencia a grupos

Ya solo nos queda un grupo de comandos, y es el que permite relacionar los usuarios con los grupos a los que pertenecen. Todos estos comandos tienen el nombre LocalGroupMember y son los siguientes:

- `Add-LocalGroupMember`: añade un usuario a un grupo
- `Get-LocalGroupMember`: devuelve los usuarios que pertenecen a un grupo determinado
- `Remove-LocalGroupMember`: elimina la pertenencia de un usuario a un grupo.
- 
Si te fijas, no hay ningún comando que nos permita saber a qué grupos pertenece un usuario determinado, solamente en sentido contrario, es decir, a partir del grupo obtener los usuarios que pertenecen a él.

Si en algún momento necesitaras esta información lo deberías de hacer a través del comando `net`, una especie de supercomando previo a Powershell (para la consola cmd) que permitía todo tipo de tareas: desde gestionar los usuarios del sistema hasta compartir recursos en red .

La forma de obtener toda la información de un usuario, incluyendo los grupos a los que pertenece, es:

```powershell
PS C:\> net user usuario
```


***
[Volver al índice principal](index_UT06.md)
