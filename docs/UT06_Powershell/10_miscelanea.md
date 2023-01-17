---
layout: default
title: Miscelánea
parent: UT06. Powershell
nav_order: 10
---

# 10.- Miscelánea

## 10.1.- Ejecución de scripts

Cuando intentamos ejecutar un script de PowerShell realizado por nosotros la primera vez nos dará un mensaje de error. Esto se debe a la política de ejecución que está definida y que determina cómo se ejecutan los scripts.  Por defecto la política de ejecución está definida como *Restricted* lo que significa que los scripts creados por ti no tienen permisos de ejecución. Se puede comprobar la política que tenemos con la orden `Get-ExecutionPolicy`.

Podemos cambiar la política que se va a aplicar en el sistema con el cmdlet `Set-ExecutionPolicy` que admite uno de los siguientes valores:

- `RemoteSigned`: permite ejecutar cualquier script que nosotros creemos, pero si descargamos algún script de Internet solo podremos ejecutarlo si está firmado por un publicador de confianza (trusted Publisher).
- `AllSigned`: todos los scripts, incluso los creados por ti, deben ser firmados por un publicador de confianza.
- `Unrestricted`: todos los scripts, sea cual sea su origen, pueden ser ejecutados.

Por lo tanto, antes de crear nuestros propios scripts debemos verificar la política de ejecución que tiene nuestro equipo y, en caso de tener restringida la ejecución de scripts, debemos introducir la siguiente orden:

```powershell
PS C:\>Set-ExecutionPolicy RemoteSigned
```


## 10.2.- Alias

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