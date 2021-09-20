# 1.- INDICE DE CONTENIDOS DEL CURSO

## UT01.- Teoría de Sistemas Operativos

### Contenidos

Esta unidad es una breve introducción a los sistemas operativos. Los apartados son:

1. [Introducción a los sistemas operativos](UT01_Teoria_SO/01_introducción.md)
2. [Historia de los sistemas operativos](UT01_Teoria_SO/02_historia.md)
3. [Componentes de un sistema operativo](UT01_Teoria_SO/03_componentes.md)
4. [Arquitectura de los sistemas operativos](UT01_Teoria_SO/04_arquitectura.md)
5. [Clasificación de los sistemas operativos](UT01_Teoria_SO/05_clasificación.md)

### Prácticas

- PR0101: Cronología de los ordenadores
- PR0102: Comparativa de sistemas operativos


## UT02.- Virtualización

### Contenidos

En esta unidad se va a proporcionar una visión global de la virtualización. Comenzamos con una breve introducción a la virtualización, terminología, ventajas e inconvenientes y evolución. A continuación, hablaremos de tipos de máquinas virtuales y tipos de hipervisores.

Para cerrar la unidad nos centraremos en dos hipervisores, uno de tipo uno (Hyper-V) y otro de tipo dos (VirtualBox). El primero es el que utilizaremos para las unidades relativas a Windows 10 y Server, mientras que el segundo es el que usaremos en el último trimestre con Linux.

1. Conceptos básicos. Definición. Ventajas e inconvenientes de la virtualización
2. Tipos de máquinas virtuales
3. Tipos de hipervisores
4. Oracle VirtualBox
5. Microsoft Hyper-V

### Prácticas

- PR0201: Creación de MV con Hyper-V. Almacenamiento y puntos de control
- PR0202: MV con Hyper-V. Compartición, clonación de discos físicos.
- PR0203: MV con VirtualBox


## UT03.- Windows 10. Instalación y primeros pasos

### Contenidos

- Usuarios y grupos en Powershell (**Mover**)
- Herramientas de administración
- Panel de control. Sistema
- Panel de control. Otras opciones
- Gestión de usuarios y grupos

### Prácticas

- PR0301: Comandos básicos de Powershell
- PR0302: Configuración básica de Windows (I). MMC, Hostname y administrador de dispositivos.
- PR0303: Configuración básica de Windows (II). Rendimiento, memoria virtual, variables de entorno y acceso remoto.
- PR0304: Gestión de energía, usuarios y registro
- PR0405: Usuarios y grupos con Powershell.


## UT04.- Windows 10. Gestión del almacenamiento

### Contenidos

- Organización del almacenamiento (estructura física y lógica, gestores de archivos)
- Configuración de discos en Windows (discos básicos y dinámicos, el comando diskpart)
- El sistema de archivos NTFS (permisos NTFS)
- Carpetas compartidas (permisos compartidos)
- Sistema de archivos con Powershell (**Mover**)

### Prácticas

- PR0401: Sistema de ficheros FAT
- PR0402: Discos básicos y dinámicos.
- PR0403: Diskpart
- PR0404: Permisos NTFS
- PR0405: Carpetas compartidas
- PR0406: Archivos y carpetas con Powershell (I)
- PR0407: Archivos y carpetas con Powershell (II)


## UT05.- Windows 10. Administración

### Contenidos

- Gestión de procesos (Quitamos planificación?)
- Gestión de memoria (¿¿¿también sobra????)
- Monitorización del sistema
  - El administrador de tareas
  - Servicios
  - Monitor de recursos 
  - Visor de eventos
- Gestión de energía
- Directivas de grupo local
- Programador de tareas
- Administración del sistema con Powershell (¿¿¿¿¿Hacer y mover????)

### Prácticas

- PR0501: Directivas de seguridad
- PR0502: Directiva de equipo local

## UT06.- Powershell

## UT07.- Windows Server. Instalación

### Contenidos

- Introducción a Windows Server. Modo Core y Experiencia de usuario
- Tareas de post-instalación
- Administración remota del servidor
- Grupos de almacenamiento

### Prácticas

- PR0701: Preparación del servidor
- PR0702: Grupos de almacenamiento

## UT08.- Windows Server. Gestión del dominio

### Contenidos

- Active Directory
- Usuarios y grupos del dominio
- Carpetas personales en AD
- Carpetas compartidas por un grupo
- Perfiles de usuario: locales, móviles, obligatorios, temporales y super-obligatorios
- Asignación de derechos a usuarios y grupos
- Group Policy Objects (GPOs) y Group Policy Preferences (GPPs)
- Despliegue remoto de aplicaciones (**Esto se puede quitar o integrar en GPOs**)
- Segundo controlador de dominio
- DFS (**Yo creo que no**)

### Prácticas

- PR0801: Usuarios en el dominio
- PR0802: GPOs
- PR0803: GPOs (II)
- PR0804: Segundo controlador de dominio

## UT09.- Gestión de identidades en la nube. Azure Active Directory y Microsoft 365

### Contenidos

- Servicios de MS en la nube: Azure Active Directory, Azure y Microsoft365
- Usuarios en Azure AD
- Migración de usuarios a la nube: sincronización entre AD y Azure AD

### Prácticas

## UT10.- Linux. Instalación

### Contenidos

- Introducción a Linux
- Instalación de Ubuntu Server
- Bonding de adaptadores de red (**????**)
- El sistema de ficheros en Linux
  - Navegación por el sistema de ficheros
  - Creación de particiones
  - Configuración de discos RAID
  - Configuración de discos LVM
- Navegación por el sistema de ficheros (**¿aquí o atrás**)
- Comandos para el sistema de ficheros
- Comandos avanzados del shell Bash
  - Monitorización del espacio en disco
  - Trabajando con ficheros de datos
  - Redireccionamiento
- El editor `sed` y expresiones regulares

### Prácticas

- PR1001: Instalación de Linux (I)
- PR1002: Introducción a Bash
- PR0803: Filtros y redireccionamiento
- PR0804: Comando `sed` y expresiones regulares (I)
- PR0805: Comando `sed` y expresiones regulares (II)

## UT11.- Linux. Administración

### Contenidos

- Gestión del software en Linux
- Configuración de red en Ubuntu Server
- Conexión remota al servidor mediante SSH
- Administración del almacenamiento
- Gestión de usuarios en Linux
- Gestión de procesos
- Arranque del sistema con `systemd`
- El directorio `/proc`

### Prácticas

- PR1101: Configuración de la red
- PR1102: Conexión SSH
- PR1103: Permisos en Linux


## UT12.- Contenedores. Automatización y orquestación

### Contenidos

- Virtualización de servicios con contenedores. Docker.
- Despliegue automatizado de máquinas virtuales. Vagrant.
- Herramientas de gestión de la configuración. Ansible. 

### Prácticas

- PR1201: Vagrant