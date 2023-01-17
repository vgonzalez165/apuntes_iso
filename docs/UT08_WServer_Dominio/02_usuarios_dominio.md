---
layout: default
title: Usuarios y grupos del dominio
parent: UT07. Windows Server. Administración del dominio
nav_order: 2
---

# 2.- Usuarios y grupos del dominio

## 2.1.- Creación de usuarios en el dominio



## 2.2.- Creación de carpetas personales

La primera tarea de administración de red que realizaremos en el servidor será la creación de un **espacio de almacenamiento** para cada usuario en el servidor. El objetivo es que, dado que los usuarios podrán iniciar sesión en cualquier equipo de la red, tengan un espacio de almacenamiento alojado en una carpeta compartida de forma que pueda estar siempre accesible, independientemente de la ubicación desde la que se haya conectado el usuario. Este espacio se mostrará al usuario como una **unidad de red**.

La realización de este trabajo se puede dividir en tres tareas:

- Creación de la carpeta compartida en el servidor y que contendrá todas las carpetas de los usuarios.
- Configuración de la carpeta compartida en la cuenta de cada usuario.
- Acceder desde el equipo cliente a la carpeta compartida.


### 2.2.1.- Creación de la carpeta compartida

Podemos crear la carpeta compartida de la misma forma en que se crean en Windows 10 a través del menú contextual de la carpeta, tal como se vio en el apartado [Carpetas compartidas](../UT04_Win10_Almacenamiento/05_compartidas.md) de la unidad de trabajo 4.

La otra posibilidad es utilizar la opción *Recursos compartidos* que se encuentra en el rol *Servicios de archivos y almacenamiento*.

![Recursos compartidos](imgs/02_02_recursos_compartidos.jpg)

Este rol es el único que está habilitado por defecto en el sistema tras la instalación del mismo, pero no con toda su funcionalidad. Si accedes a *Agregar roles y características* podrás ver todas las funcionalidades que hay disponibles, pero por ahora solo vamos a agregar la denominada **Administrador de recursos del servidor de archivos**.

Una vez instalado ya podremos ir a *Servicios de archivos y almacenamiento* desde donde tendremos una vista general de todos las carpetas que hay compartidas en el sistema, y también podremos lanzar el asistente para crear una nueva carpeta haciendo *click* en *Tareas -> Nuevo recurso compartido*. 

Las opciones que nos preguntará el asistente son:

- **SMB o NFS**: el protocolo SMB (Server Message Block) es el protocolo utilizado en sistemas Windows para compartir carpetas en red, mientras que NFS (Network File System) es el utilizado en sistemas Linux. Por tanto, esta elección dependerá del sistema operativo de los equipos que tengamos en la red y que vayan a acceder al recurso compartido.
- **Rápido o avanzado**: si deseamos una configuración estándar prefijada o queremos especificar más detalladamente la configuración del recurso compartido.
- **Ubicación del recurso compartido**: en la siguiente ventana del asistente hay que indicar la ubicación de la carpeta compartida, tanto el servidor en que la vayamos a ubicar como el volumen. Por defecto, la creará en una carpeta denominada `\shares` dentro del volumen que hayamos indicado, pero también tenemos la opción de personalizar la ubicación.
- **Nombre del recurso compartido**: lo siguiente será indicar el nombre que tendrá el recurso compartido. Recuerda que, al igual que pasa con Windows 10, si finalizamos el nombre del recurso compartido con el símbolo `$` conseguiremos que dicho recurso no sea visible en la red, siendo únicamente accesible para aquellos que conozcan su nombre.
- **Parámetros de configuración**: en la siguiente ventana del asistente configuramos los siguientes valores:
  - *Habilitar enumeración basada en el acceso*: si lo habilitamos, los usuarios únicamente verán las carpetas y archivos para los que tiene permiso de acceso. Cualquier otro recurso al que no tenga permisos será totalmente invisible para él.
  - *Permitir almacenamiento en caché para el recurso compartido*: esta opción permite que los cliente habiliten una **caché** de archivos de forma que tengan disponibles el contenido de la carpeta compartida aunque no tenga conexión con el equipo que aloja dicha carpeta.
  - *Cifrar acceso a datos*: habilitar esta opción obliga a que todo el tráfico por la red del contenido de esta carpeta esté cifrado.
- **Permisos**: de forma análoga a la configuración en Windows 10, en la siguiente ventana podremos configurar la Lista de Control de Acceso del recurso compartido.
- **Cuota**: aquí podríamos habilitar una **cuota** para establecer un límite de capacidad que el usuario puede utilizar en la carpeta compartida.








