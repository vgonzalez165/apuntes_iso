![Carátula UT07](imgs/caratula_ut07.png)

## Contenidos

1. [Instalación de Windows Server](01_instalación.md)
2. [**Instalación en modo Core**](02_instalación_core.md)
3. [Tareas post-instalación](03_postinstalación.md)
4. [Administración remota del servidor](04_admin_remota.md)
5. [Administración de discos en Windows Server. Grupos de almacenamiento](05_admin_discos.md)


# 2.- INSTALACIÓN CORE

Una decisión que hay que tomar en la instalación es si instalar la versión con entorno gráfico o si es suficiente con la versión **Server Core** en línea de comandos. Hay que tener en cuenta que muchos servidores están dedicados a un rol particular y un entorno gráfico únicamente sirve para consumir recursos sin aportar ninguna ventaja.  

Si escogemos la versión Server Core obtendremos una versión desnuda del sistema operativo: no hay menú de inicio, no hay interfaz gráfica ni consola de administración (MMC, Microsoft Management Console) y, por supuesto, ninguna aplicación gráfica (con algunas excepciones, por ejemplo, podemos acceder al administrador de tareas ejecutando el comando `taskmgr`).

Esta carencia de elementos gráficos nos puede proporcionar numerosas ventajas:

- **Conservación de recursos hardware**. Muchos elementos de la GUI requieren grandes cantidades de memoria, así como capacidad de procesamiento. No tener interfaz gráfica permite dedicar estos recursos a otros servicios más importantes para el servidor.
- **Ahorro de espacio en disco**. La instalación Server Core requiere menos espacio en disco.
- **Reducción de la frecuencia de instalación de actualizaciones**. Los elementos gráficos de Windows son los que más frecuentemente requieren actualizaciones por lo que su eliminación implicará una menor frecuencia de instalación de actualizaciones. Menos actualizaciones conllevan menos reinicios del servidor y por tanto menos tiempo sin servicio (alta disponibilidad).
- **Reducción de la superficie de ataque**. Cuantas menos aplicaciones se estén ejecutando en el sistema menos puntos de entrada habrá para un potencial atacante. 

En Windows Server 2012R2 y 2016, es posible alternar cambiar entre el modo core y el modo con GUI después de haber instalado el sistema operativo. Además, permite un tercer modo denominado **Minimal Server Interface**, que mantiene únicamente algunos elementos de la interfaz gráfica, eliminando aquellos que más recursos consumen.

Entre los elementos eliminados se incluyen Internet Explorer, el escritorio, el explorador de ficheros y algunos elementos del Panel de Control como *Programas y Características*, *Dispositivos e impresoras*, *Windows Update* o *Fuentes*. Algunas de las aplicaciones que se mantienen son la consola de administración MMC (Microsoft Management Console), el Administrador de Dispositivos y toda la interfaz de Windows PowerShell.

Algo importante es que **en Windows Server 2019 se eliminó la opción de alternar entre los modos en un sistema operativo ya instalado**, por lo que es una decisión irrevocable que hay que tomar durante la instalación del sistema.

Hay dos formas de alternar estos dos modos (sólo válido para Windows Server 2012R2 y 2016): desde el Administrador del Servidor o mediante Powershell.


## 2.1.- Habilitar Server Core desde el Administrador del servidor

Para alternar entre interfaz con GUI, Server Core y Minimal Server Interface lo podemos hacer desde *Administrador de servidor -> Administrar -> Quitar roles y funciones ->Infraestructura e interfaces de usuario*. Ahí tenemos estas opciones:

- **Infraestructura y herramientas de administración**: corresponde a la interfaz mínima por lo que es el único checkbox que habría que dejar marcado.
- **Shell gráfico de servidor**: habilitar esta casilla implica la instalación de la interfaz gráfica completa.
- **Experiencia de escritorio**: incluye características propias de Windows 8 tales como Windows Media o los temas de escritorio.
 

## 2.2.- Habilitar la GUI desde la línea de comandos con Powershell

Realizar el proceso contrario es algo más complicado ya que debes tener en cuenta que no tendrás entorno gráfico por lo que deberá realizarse a través de la línea de comandos.  Para ello utilizaremos PowerShell.

Para entrar en PowerShell utilizamos introducimos la siguiente orden:

```powershell
C:\>Powershell
```

Esto nos llevará a PowerShell. Fíjate que la única diferencia que apreciarás es que aparecen los caracteres **PS** al principio del prompt. La orden para añadir la interfaz gráfica es:

```powershell
PS C:\>Add-WindowsFeature Server-Gui-Shell, Server-Gui-Mgmt-Infra
```

Como habrás podido deducir la orden `Add-WindowsFeature` sirve para instalar características a Windows. Los parámetros que se pasan corresponden a las características que queremos instalar. El primero corresponde a la interfaz gráfica completa y el segundo a la interfaz mínima.

Por ejemplo, en la siguiente captura se va a pasar de Server Core a la interfaz mínima.

```powershell
C:\>powershell
Windows PowerShell
Copyright (C) 2013 Microsoft Corporation. Todos los derechos reservados.

PS C:\> Add-WindowsFeature Server-Gui-Mgmt-Infra

Success Restart Needed Exit Code       Feature Result
------- -------------- ---------       --------------
True    Yes            Success Rest... (Infraestructura y herramientas de adm...
ADVERTENCIA: Debe reiniciar este servidor para finalizar el proceso de
instalación.
ADVERTENCIA: La actualización automática de windows no está habilitada. Para
asegurarse de que la característica o el rol recién instalados se actualicen
automáticamente, active Windows Update en el Panel de control.
```

Tras ejecutar la orden debemos reiniciar el servidor. La orden para reiniciar el servidor en PowerShell es:

```powershell
PS C:\>Shutdown –r –t 0
```

Algunos parámetros útiles del comando Shutdown:

- `-r`: indica que el equipo debe reiniciarse tras el apagado.
- `-s`: el equipo se apaga.
- `-t` X: especifica el tiempo que ha de pasar desde que se ejecuta la orden hasta que se apaga el equipo. Esto es útil en entornos en producción ya que así se da tiempo a los usuarios conectados para que cierren las aplicaciones de forma que no pierdan los datos.




***
[Volver al índice principal](index_UT07.md)