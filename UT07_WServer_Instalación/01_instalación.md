![Carátula UT07](imgs/caratula_ut07.png)

## Contenidos

1. [**Instalación de Windows Server**](01_instalación.md)
2. [Instalación en modo Core](02_instalación_core.md)
3. [Tareas post-instalación](03_postinstalación.md)
4. [Administración remota del servidor](04_admin_remota.md)
5. [Administración de discos en Windows Server. Grupos de almacenamiento](05_admin_discos.md)


# 1.- INSTALACIÓN DE WINDOWS SERVER

## 1.1.- Introducción

Windows Server es la familia de sistemas operativos de servidor de Microsoft y su evolución ha sido más o menos paralela a la de la familia de sistemas operativos de escritorio, compartiendo tanto herramientas como aspectos visuales o modos de trabajo. De esta forma Windows Server 2012R2 comparte muchos aspectos con Windows 8 mientras que Windows Server 2016 y 2019 son muy similares a Windows 10.

Cada nueva versión de Windows Server mantiene los elementos más relevantes de las versiones anteriores a la vez que añade nuevas funcionalidades. Los elementos más destacables añadidos en cada una de estas versiones son:

- **Windows Server 2012R2**: lanzado en 2013 como una evolución de Windows Server 2012, añade un gran número de funcionalidades a este, tales como:
    - **Opciones de instalación**: permite alternar entre una instalación Server Core sin interfaz gráfica y una instalación con GUI sin necesidad de reinstalar. Además, dispone de una tercera opción que incluye la Consola de Administración de Microsoft (MMC) y la Consola de Administración del Servidor, pero sin el resto de los componentes del entorno gráfico.
    - **Nuevo Administrador de servidor**:  esta herramienta ha sido rediseñada para administración fácilmente múltiples servidores.
    - **Powershell**: se le da más importancia a Powershell con la inclusión del modo Core y la inclusión de más de 2300 cmdlets.
    - **Hyper-V**: incluye una nueva versión del hipervisor de Microsoft que incluye características como virtualización de redes, pools de recursos de almacenamiento, …
    - **Espacios de almacenamiento**: incorpora un nuevo sistema de gestión del almacenamiento que permite trabajar con pools de recursos de almacenamiento de forma sencilla.
- **Windows Server 2016**: entre sus nuevas funcionalidades se pueden destacar:
    - **Versión Nano Server** : se incluye un nuevo tipo de instalación optimizado para trabajar en la nube y que carga de forma local únicamente el núcleo del sistema operativo, reduciendo así aún más la superficie expuesta que en las versiones Core.
    - **Contenedores** : los contenedores son una tecnología que proporcionan un entorno ligero y aislado y que permiten empaquetar y ejecutar aplicaciones en diversos entornos locales y en la nube.
    - **Virtualización anidada**: a partir de esta versión es posible ejecutar máquinas virtuales dentro de otras máquinas virtuales.
    - **Powershell Direct**: con esta opción tenemos la posibilidad de ejecutar comandos de Powershell sobre una máquina virtual enviándolos directamente desde el anfitrión.
- **Windows Server 2019**: esta es la última versión de Windows Server, incluyendo funcionalidades muy orientadas a la virtualización y a la nube. Algunas de estas funcionalidades son:
    - **Storage Spaces Direct**: incluye mejoras en los espacios de almacenamiento, dotándolos de mayor velocidad, estabilidad y escalabilidad.
    - **Centro de Administración de Windows**: esta nueva herramienta permite administrar los servidores mediante una interfaz Web.
    - **Hybrid Cloud y Azure**: en esta versión se profundiza en la integración entre los servidores locales y los servidores en la nube de Azure, integrándolos mediante una conexión VPN.


## 1.2.- Tareas previas y posteriores a la instalación

Antes de instalar un servidor es necesario realizar una serie de tareas previas:

- **Verificar si el hardware cumple los requerimientos mínimos**. Los requisitos mínimos varían según la versión escogida, pero parten de un procesador con arquitectura de 64 bits a 1,4 GHz y 512 GB de RAM, aunque indudablemente a medida que se añadan funciones al servidor estos requisitos se incrementarán.
- **Verificar la lista de compatibilidad de hardware**: el hardware del servidor ha de ser compatible con Windows Server ya que hay que tener en cuenta que, mientras que prácticamente todo el hardware es compatible con las versiones de escritorio de Windows, hay muchos dispositivos hardware que no serán compatibles con la versión de servidor. En el la [web de Microsoft](https://www.windowsservercatalog.com) se pueden consultar las listas de compatibilidad de hardware de las diferentes versiones de Windows Server.
- **Decidir si se hará una instalación nueva o una actualización**, cuando queremos instalar Windows Server en un sistema que ya está ejecutando un sistema operativo es importante decidir si queremos actualizar desde la instalación anterior o comenzar desde cero iniciando una nueva instalación.
- **Determinar si se hará una instalación interactiva o automatizada**: ya veremos en futuras unidades que es posible automatizar la instalación del sistema operativo.
- **Determinar el tipo de instalación**: Windows Server permite la instalación en modo Core donde se omite la interfaz gráfica (GUI), con toda la sobrecarga que supone la misma. Esto puede ser lo deseable por ejemplo si el servidor se va a administrar de forma remota.
- **Definir cuál será la partición de instalación**: el número de discos duros y la forma de particionarlos debe ser algo estudiado de forma previa a la instalación del sistema operativo.
- **Determinar los parámetros de configuración de la red**: hay que conocer de antemano el esquema de direccionamiento IP de la red. Normalmente los servidores tienen una asignación estática de la IP.
- **Decidir si el equipo formará parte de un dominio o de un grupo de trabajo**: esto va a permitir establecer las relaciones entre todos los equipos de la red. 
  
Por otro lado, una vez instalado el sistema hay que realizar una serie de comprobaciones y configuraciones adicionales:

- **Verificar el estado de los dispositivos**: a través del Administrador de Dispositivos podremos saber si algún dispositivo no ha sido reconocido.
- **Verificar la configuración TCP/IP**: hay que verificar que la conexión a la red está correctamente configurada.
- **Verificar los registros de eventos**: mediante el Visor de Eventos comprobaremos que no haya ningún error ni advertencia.
- **Verificar las particiones del disco**: mediante Administración de Equipos verificamos las particiones.
- **Reiniciar el sistema**: aunque el sistema no lo haya requerido es conveniente reiniciar el sistema para verificar que el servidor arranca normalmente.
- **Copia de seguridad**: al terminar la instalación es conveniente realizar una copia de seguridad y programar una copia de seguridad periódica.


## 1.3.- Instalación de Windows Server

Durante el proceso de instalación de Windows Server 2012 debemos seguir los siguientes pasos:

- **Preparar el equipo para arrancar desde CD/DVD**. Es necesario arrancar desde el CD de instalación por lo que será necesario indicarle a la BIOS el CD/DVD será el primer dispositivo de arranque. Por defecto, muchos ordenadores ya tienen como primer dispositivo de arranque el CD, pero si no fuera así habría que acceder a la BIOS para indicarlo. Para ello, durante el POST, hay que pulsar la tecla `F2` o  `Supr`  (depende de la placa base), lo que nos llevará a la BIOS o la UEFI. Una vez allí hay que modificar el orden de las unidades de arranque para que el CD se encuentre antes que el disco duro.
- **Ejecutar el programa de instalación**. Una vez configurada la BIOS solo habrá que insertar el CD de instalación y reiniciar el ordenador para que se ejecute el instalador presente en el DVD.
- **Seleccionar el tipo de instalación**. A partir de Windows Server 2012, Microsoft decidió potenciar la instalación en modo Core donde se omite la interfaz gráfica con todas las ventajas que eso supone. Por defecto se instalará en modo Core, por lo que si deseamos la interfaz gráfica deberemos estar pendientes durante el proceso de instalación para indicarlo.
- **Particionar el disco duro**. Según las características de nuestro sistema y nuestras necesidades será necesario realizar particiones en el disco duro. Las particiones se pueden realizar con anterioridad al proceso de instalación con un programa independiente o realizarlas utilizando el programa de particionado que se nos muestra durante el proceso de instalación. Sin embargo, es conveniente utilizar el programa de particionado de Microsoft ya que este creará automáticamente una **partición reservada para el sistema**. Esta partición varía en tamaño según la versión del sistema operativo (100MB para Windows 7, 350MB para Windows Server2012 y 500MB para Windows 10) y no será accesible desde el sistema una vez instalado ya que no tiene asignada letra de unidad. 
- Esta partición no es obligatoria. Su contenido es:
  - El gestor de arranque y los datos de configuración del arranque: Cuando el equipo se inicia, el gestor de arranque de Windows se inicia también y lee los datos de inicio de los datos de configuración de proceso de arranque (BCD) almacenados. El equipo inicia el gestor de arranque de la partición reservada para el sistema, Windows se inicia de acuerdo con los datos de configuración almacenados en la partición.
  - Los archivos de inicio se utilizan para el cifrado de unidad BitLocker: Si alguna vez decide cifrar la unidad del disco duro con BitLocker, la partición reservada para el sistema contiene los archivos necesarios para iniciar el equipo. El equipo inicia el sistema sin cifrar la partición reservada, esta partición será la encargada para descifrar la partición de arranque y el sistema de archivo de Windows. Si no se creara la partición no se podría utilizar BitLocker.
- **Introducir la contraseña del Administrador**. Antes de poder iniciar sesión es necesario asignar la contraseña del Administrador. Hay que tener en cuenta que Windows Server tiene establecidos unos requisitos de complejidad para la contraseña por lo que esta deberá constar de mayúsculas, minúsculas y dígitos.
- **Configuración de la red**. Cada ordenador que se encuentra conectado a una red debe tener asignada una dirección IP. Esta dirección IP puede ser asignada de dos maneras diferentes:
    - **IP estática**: es el usuario el que asigna la dirección IP al ordenador, estando garantizado así que siempre tendrá dicha IP.
    - **IP dinámica**: en este caso hay un servidor DHCP en la red que se encarga de asignar direcciones IP automáticamente a los equipos que lo soliciten. 
- La asignación dinámica de IPs simplifica mucho la gestión de la red, pero tiene el inconveniente de que no garantiza que un equipo siempre tenga la misma IP, lo cual no es deseable en un equipo que va a realizar las funciones de servidor. Por ello el primer paso en el sistema es cambiar la configuración de red para asignarle una dirección IP estática. Para ello hay que ir a *Conexiones de red -> Propiedades de la interfaz que deseamos configurar -> Protocolo de Internet versión 4 (TCP/IPv4) -> Propiedades* y ahí introducir la dirección IP, la máscara de red, la puerta de enlace (dirección del router) y las direcciones IP de los servidores DNS.
- **Instalar todas las actualizaciones y parches disponibles**: en todos los sistemas operativos está publicando continuamente actualizaciones para añadir funcionalidades nuevas y parches para solucionar problemas de seguridad. El proceso de instalar actualizaciones y parches es imprescindible y se debe realizar a lo largo de todo el ciclo de vida del sistema, pero cobra especial importancia tras la instalación ya que el número de actualizaciones disponibles será proporcional a la antigüedad del CD de instalación. La gestión de actualizaciones se realiza mediante Windows Update. El primer paso será elegir cómo gestionará Windows las actualizaciones en Cambiar configuración. Las opciones disponibles son:
    - Instalar actualizaciones automáticamente
    - Descargar actualizaciones, pero permitirme elegir si deseo instalarlas
    - Buscar actualizaciones, pero permitirme elegir si deseo descargarlas e instalarlas
    - No buscar actualizaciones
- Tras elegir la opción deseada podemos ir a Buscar actualizaciones para forzar la búsqueda de las actualizaciones disponibles.


## 1.4.- Instalación de aplicaciones 

La forma de instalar y gestionar aplicaciones en Windows Server difiere bastante de la forma de hacerlo en las versiones de escritorio. Mientras que en las versiones de escritorio es habitual obtener el instalador de las aplicaciones en CD o descargándolo para instalarlas, en entornos de servidor es una tarea desaconsejada ya que son entornos más controlados y la instalación de aplicaciones externas puede traer problemas de estabilidad y de seguridad.

Por ello Windows Server ya incluye las aplicaciones que se pueden necesitar en un servidor por medio de los **roles** y **características**.


### 1.4.1.- Roles

Los **roles** se utilizan para organizar la funcionalidad del sistema operativo. Designan las funciones principales del servidor. Con cada rol pueden ser necesarios servicios específicos denominados **servicios de rol**.

Por ejemplo, los roles DNS o DHCP no requieren ningún servicio adicional, pero el rol servicio de ficheros se compone de otros servicios adicionales. La idea es que según el rol elegido se carguen exclusivamente los programas necesarios.

Algunos ejemplos de roles son:

- Acceso remoto
- Active Directory
- Hyper-V
- Servidor DHCP
- Servidor Web
- Windows Server Update Services


### 1.4.2.- Características

Además de los roles y servicios de roles, están las características. Se trata de componentes independientes de las funciones del servidor pero que pueden ser necesarios para dar apoyo a los roles. 

Por ejemplo, el Directorio Activo es un rol, pero para determinadas instalaciones puede ser necesaria la instalación de un componente independiente llamado WINS, que es una característica. Algunos ejemplos de características son:
- Microsoft .NET Framework
- Windows PowerShell
- Cifrado de unidad BitLocker
- Cliente TFTP
- Cliente NFS
- Copia de seguridad de Windows Server


### 1.4.3.- Instalación de roles y características

La instalación de roles y características se puede realizar desde el Administrador del servidor, en la opción *Administrar -> Agregar roles y características*.
 


***
[Volver al índice principal](index_UT07.md)