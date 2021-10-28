![Carátula UT05](imgs/caratula_ut05.png)

## Contenidos

1. Gestión de procesos
2. El administrador de tareas
3. Servicios
4. El monitor de recursos
5. El visor de eventos
6. Gestión de energía
7. Directivas de grupo local
8. Programador de tareas


# 3.- SERVICIOS

En UNIX y en Linux, estos programas se llaman **demonios**, y en Windows, se llaman **Servicios**. Son programas que pueden iniciarse automáticamente cuando se inicia Windows y proporcionan alguna función esencial, dando, como su nombre indica, algún servicio a los usuarios.

Los servicios se ejecutan independientemente de que un usuario haya iniciado sesión ya que su propietario son cuentas de usuario especiales del sistema que les proporcionan los permisos necesarios para realizar su cometido.

El sistema tiene una serie de servicios disponibles, pero no es necesario que se encuentren todos en ejecución en todo momento. Normalmente se pueden configurar para:

- Que se inicien **automáticamente** cuando se inicia Windows.
- Iniciarlos **manualmente**, bien cuando se requiera o cuando lo requiera un programa. Por ejemplo, el servicio `msiserver` es el encargado de instalar los paquetes con extensión `.msi` (Windows Installer) en el sistema. Por defecto, este servicio está apagado, pero en el momento en que hacemos doble click en un fichero con extensión `.msi` se pondrá en ejecución para realizar la instalación.
- Estar **deshabilitados**, en el caso de servicios que no queremos que estén en ejecución.



Cuando Windows se inicia, se ejecuta el programa Controlador de servicios services.exe, que inicia los controladores de dispositivos y los servicios de manera ordenada.
Dependencias: muchos servicios dependen de que otros funcionen. Windows rastrea esas dependencias e inicia automáticamente estos servicios antes que sus dependientes.

Los servicios de Windows se pueden lanzar desde:
Archivos ejecutables independientes (.exe)
Archivos .dll (bibliotecas de vínculo dinámico, conjuntos de funciones). Los archivos .dll se cargan y se invocan con svchost.exe. 

svchost.exe no es un servicio. Es un programa que se encarga de ejecutar el servicio que está implementado en las librerías .dll. 
Cada ejecución de svchost.exe puede administrar varios servicios. 

El Administrador de tareas nos puede mostrar que servicios corresponden a cada instancia de svchost.exe


Para administrar los servicios de forma gráfica vamos a Herramientas administrativas -> Servicios.	


Para ver información más detallada de cada servicio hacemos doble click sobre el mismo.	

En la pestaña General vemos información básica sobre el servicio, así como la ruta del ejecutable que lo lanzó.

Automático: el servicio se inicia automáticamente cuando Windows se inicia. Los servicios sin dependencias se inician primero, seguidos de los servicios que dependen de ellos.
Manual: el servicio no se inicia a no ser que lo solicite alguna aplicación, otro servicio o se inicie de forma manual.
Deshabilitado: el servicio no se iniciará bajo ninguna circunstancia. Al deshabilitar un servicio se deshabilitan los servicios que dependen de él.	

Aquí podemos cambiar el estado del servicio.

En la pestaña Iniciar sesión se especifica el usuario al que pertenece el servicio (normalmente usuarios del sistema).

Cuentas más utilizadas con servicios:
Local System: utilizada por la mayoría de los servicios. Nivel de privilegios máximo (es la versión para servicios de la cuenta Administrador)
Local Service: Nivel de privilegios mínimo. Los servicios que utilizan esta cuenta acceden a otros equipos de la red con métodos de acceso anónimos.
Network Service: Parecida a Local Service. Los servicios que utilizan esta cuenta acceden a otros equipos de la red con la cuenta del equipo local

En la siguiente pestaña podemos indicar qué debe hacer el sistema en caso de que el servicio falle. Las opciones son:
Parar el servicio
Ejecutar un programa
Reiniciar el equipo

La pestaña Dependencias permitirá ver de qué servicios depende el servicio seleccionado y cuáles son los servicios que dependen de él.




***
[Volver al índice principal](index_UT05.md)