![Carátula UT02](imgs/caratula_ut02.png)

### Contenidos

1. [Conceptos básicos de virtualización](01_conceptos_básicos.md)
2. [Tipos de máquinas virtuales](02_tipos_MV.md)
3. [Tipos de hipervisores](03_tipos_hipervisores.md)
4. [Oracle VirtualBox](04_virtualbox.md)
5. [Microsoft Hyper-V](05_hiper-v.md)


## 3.- TIPOS DE HIPERVISORES

La solución software que permite crear máquinas virtuales se denomina **hipervisor**. Hay multiples hipervisores, pero en general se pueden dividir en dos grandes grupos:

- Hipervisores tipo 1
- Hipervisores tipo 2

Adicionalmente, hay otro enfoque diferente en la virtualización que no se podría incluir en ninguno de los dos tipos , que son los contenedores.


### 3.1.- Hipervisores de tipo 1

También se denominan **unhosted**, **nativos** o **bare metal**. Este tipo de hipervisores no se instalan sobre ningún sistema operativo, sino que se ejecutan directamente sobre el hardware. En realidad, lo normal es que el propio hipervisor incluya su propio sistema operativo, el cual únicamente incluye las funciones estrictamente necesarias para ejecutar el hipervisor. Por ejemplo, VMWare ESXi incluye se propio kernel, llamado VMkernel, y algunas aplicaciones básicas de Linux (como un servidor `ssh` o un intérprete bash)


Normalmente son sistemas operativos muy ligeros que únicamente contienen el software de virtualización.
La administración se suele hacer de forma remota desde otro equipo (p.e. a través de una interfaz web)

Ventajas:
Buen rendimiento: no están sujetos a las limitaciones de tener que ejecutarse sobre un sistema operativo, lo que redunda en un mayor rendimiento.
Alta seguridad: al no tener sistema operativo, se minimizan los posibles agujeros de seguridad.
Por estas ventajas, son los hipervisores utilizados en entornos profesionales.

Algunos ejemplos son:
VMware eESXi
Xen
Citrix Xen Server
Microsoft Hyper-V

Hipervisores tipo 2
También denominados hosted
El software de virtualización es un programa que se ejecuta sobre un sistema operativo.
Son más ineficientes, ya que hay una capa más entre la máquina virtual y el hardware.
Al ser un programa normal, la administración se suele realizar desde el propio equipo.

Ventajas:
Administración más simple: en general son más sencillos de administrar.
Útiles para realización de pruebas: no requieren de la misma infraestructura que los hipervisores de tipo 1, por lo que son muy útiles para realizar pruebas a nivel personal.

Algunos ejemplos son:
VirtualBox
KVM
Vmware Workstation
Virtual PC

Contenedores
También denominado virtualización del sistema operativo.
Más que un tipo de hipervisor se podría considerar un tipo de virtualización.
Mientras que los otros tipos de hipervisores virtualizan el sistema operativo completo, los contenedores únicamente virtualizan el kernel y las librerías necesarias para realizar una tarea.

Por tanto, no son un sistema operativo virtual como tal.
Se pueden entender como paquetes que se ejecutan aislados sobre el sistema operativo principal.

El software más popular de contenedores es Docker.


Hipervisores más populares

Oracle Virtual Box
Pertenece a Oracle, pero es software libre
Es un hipervisor de tipo 2
Está disponible para múltiples sistemas operativos: Windows, Linux, Solaris, OpenBSD, …

VMWare
Es la empresa de referencia en el mundo de la virtualización.
Dos líneas de negocio:
Versiones de escritorio: 
VMware Workstation: comercial
VMware Fusion: para Apple
VMware Player: versión gratuita

Versión empresarial
VMware vSphere: Es una suite de virtualización que integra diferentes herramientas para la virtualización.
Vmware vSphere Hypervisor: es una versión gratuita con funcionalidad limitada del hipervisor de esta compañía.

Microsoft Hyper-V
Solución de virtualización de Microsoft
Integrada como rol en Windows Server y también disponible en Windows 10 Pro

Parallels Desktop
Virtualización en ordenadores Apple con arquitectura Intel
Hipervisor de tipo 2 que permite usar máquinas Windows y Linux en ordenadores Apple.


[**< Anterior**: 2. Tipos de máquinas virtuales](02_tipos_MV.md)

[**Volver** Menú de la UT02](index_UT02.md)

[**Siguiente>**: 4. Oracle VirtualBox](04_virtualbox.md)

