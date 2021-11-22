![Carátula UT06](imgs/caratula_ut06.png)


### Contenidos

1. [Introducción a Powershell](01_introducción.md)
2. [Objetos y el pipeline](02_pipelines.md)
3. [Tipos de datos y variables](03_tipos_datos_y_variables.md)
4. [El sistema de ficheros en Powershell](04_sistema_ficheros.md)
5. [Gestión de Hyper-V desde Powershell](05_hyperv.md)
6. [Gestión de usuarios y grupos](06_usuarios.md)
7. [Gestión avanzada de usuarios y grupos](07_usuarios_avanzado.md)
8. [Conexión remota](08_conexion_remota.md)
9. [Powershell y el almacenamiento](08_almacenamiento.md)




# 4.- CONTROLANDO HYPER-V CON POWERSHELL

Powershell dispone de un módulo específico para trabajar con máquinas virtuales de Hyper-V el cual se puede instalar cuando habilitamos la característica de Administración de Hyper-V.

Podemos ver todos los comandos incluidos en el módulo con la siguiente orden, que nos muestra más de 200 comandos relacionados con máquinas virtuales.

```powershell
PS C:\> Get-Command -Module hyper-v
```

Veamos los comandos que más útiles nos pueden ser:


## 4.1.- Listado de máquinas virtuales

Con el comando `Get-VM` obtendremos un listado de las máquinas virtuales actualmente administradas en nuestro equipo.

```powershell
PS C:\> Get-VM
```

Podemos utilizar un filtro para mostrar solamente las máquinas que nos interesen. En las siguientes órdenes se muestran únicamente las máquinas virtuales que se encuentran en ejecución en un momento determinado. Observa que ambas sintaxis son equivalentes y se pueden utilizar indistintamente una u otra.

```powershell
PS C:\> Get-VM | Where-Object {$_.State -eq ‘Running’}
PS C:\> Get-VM | Where-Object State -eq ‘Running’
```


## 4.2.- Iniciar y apagar máquinas virtuales

Como seguro que ya te imaginas, los comandos para iniciar y apagar máquinas virtuales son `Start-VM` y `Stop-VM`. Ambas utilizan el parámetro `-Name` para hacer referencia a la máquina que se va a iniciar o apagar. 

```powershell
PS C:\> Start-VM -Name “Windows 10”
```

También podemos utilizar el pipeline para realizar arranques o paradas de todas las máquinas virtuales o de un conjunto de ellas, por ejemplo, con la siguiente orden se paran todas las máquinas que estén arrancadas:

```powershell
PS C:\> Get-VM | Where-Object State -eq ‘Running’ | Stop-VM
```

Si por algún motivo la máquina virtual se hubiera colgado y no respondiera a la orden de parada se podría utilizar el parámetro -Force para forzar el apagado. Ten en cuenta que esto es equivalente a apagar una máquina física tirando del cable de corriente, por lo que puede haber pérdida de datos o tener alguna inconsistencia en el sistema de ficheros.


# 4.3.- Suspender e hibernar máquinas virtuales

De forma análoga a como podemos hacer con una máquina física, es posible suspender e hibernar las máquinas virtuales.
Suspender una máquina virtual supone congelar su ejecución en el momento actual y quedará en este estado hasta que la volvamos a iniciar con el comando `Resume-VM`.

```powershell
PS C:\> Suspend-VM -Name ‘Win10’
PS C:\> Resume-VM -Name ‘Win10’
```

La diferencia cuando guardamos una máquina virtual (el equivalente a hibernar) es que se congela la máquina y además el contenido de su memoria se vuelca al disco, por lo que se liberará la memoria que esté ocupando la máquina virtual. El comando para realizar esta tarea es `Save-VM`, mientras que se debe restaurar con `Start-VM`.

```powershell
PS C:\> Save-VM -Name ‘Win10’
PS C:\> Start-VM -Name ‘Win10’
```


## 4.4. Medir el consumo de recursos de la máquina virtual

En entornos con múltiples máquinas virtuales puede ser útil hacer un seguimiento de los recursos consumidos por cada una de las máquinas. Para hacer esto primero hay que habilitar la medición de recursos en las máquinas virtuales con la siguiente orden:

```powershell
PS C:\> Enable-VMResourceMetering -VMName ‘Win10’
```

Una vez habilitada la medición de recursos podemos comprobar el consumo de recursos con el comando:

```powershell
PS C:\> Measure-VM -Name ‘Win10’
```


## 4.5.- Obtener los recursos de anfitrión

En ocasiones nos puede interesar conocer los recursos disponibles en el equipo que está ejecutando Hyper-V, por ejemplo, para saber cuanta memoria RAM tiene o el número de núcleos del procesador. Esto lo podemos conseguir con el comando Get-VMHost. 

```powershell
PS C:\> Get-VMHost
```

Recuerda que puedes combinarlo con el comando `Select-Object` para seleccionar las propiedades que se mostrarán. También puedes utilizar el comodín (*) para indicar que se muestren todas las propiedades.

```powershell
PS C:\> Get-VMHost | Select-Object *
```


## 4.6.- Otros comandos para trabajar con Hyper-V

Hay otros muchos comandos que nos permiten realizar cualquier función que nos imaginemos sobre las máquinas virtuales. En especial, hay una serie de comandos que nos permitirán crear tanto discos como máquinas virtuales directamente desde la línea de comandos, pero que se salen del ámbito de estudio de este curso.

Si estás muy interesado, puedes obtener más información en las páginas de ayuda de Hyper-V con Powershell de Microsotf o también hay un manual bastante detallado en https://statemigration.com/save-time-on-hyper-v-using-powershell/.



***
[Volver al índice principal](index_UT06.md)
