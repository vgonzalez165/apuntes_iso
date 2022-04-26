<link rel="stylesheet" href="../styles.css">

![Carátula UT02](imgs/caratula_ut02.png)

## Contenidos

1. [Conceptos básicos de virtualización](01_conceptos_básicos.md)
2. [Tipos de máquinas virtuales](02_tipos_MV.md)
3. [**Tipos de hipervisores**](03_tipos_hipervisores.md)
4. [Oracle VirtualBox](04_virtualbox.md)
5. [Microsoft Hyper-V](05_hiper-v.md)


# 3.- TIPOS DE HIPERVISORES

La solución software que permite crear máquinas virtuales se denomina **hipervisor**. Hay multiples hipervisores, pero en general se pueden dividir en dos grandes grupos:

- Hipervisores tipo 1
- Hipervisores tipo 2

Adicionalmente, hay otro enfoque diferente en la virtualización que no se podría incluir en ninguno de los dos tipos , que son los **contenedores**.


## 3.1.- Hipervisores de tipo 1

También se denominan **unhosted**, **nativos** o **bare metal**. Este tipo de hipervisores no se instalan sobre ningún sistema operativo, sino que se ejecutan directamente sobre el hardware. En realidad, lo normal es que el propio hipervisor incluya su propio sistema operativo, el cual únicamente incluye las funciones estrictamente necesarias para ejecutar el hipervisor. Por ejemplo, VMWare ESXi incluye se propio kernel, llamado VMkernel, y algunas aplicaciones básicas de Linux (como un servidor `ssh` o un intérprete bash)

El que estos hipervisores estén ellos solos a la máquina, unido al hecho de que este tipo de soluciones profesionales de virtualización están orientadas a su instalación en grandes servidores que no suelen estar dotados de teclado ni de monitor, hace que si acceso y administración siempre se realice de forma remota, normalmente por SSH, RDP o a través de portales web.

La principal **ventaja** de los hipervisores de tipo 1 es que al eliminar la capa de sistema operativo entre el hipervisor y el hardware subyacente se consigue un mejor rendimiento. Además, al eliminar el sistema operativo de base se consigue una menor superficie de ataque que redunda en una menor probabilidad de que haya agujeros de seguridad.

Como se ha dicho, estos hipervisores están orientados a entornos profesionales. Algunos de los más populares son:
- VMware eESXi
- Xen
- Citrix Xen Server
- Microsoft Hyper-V

## 3.2 Hipervisores de tipo 2

También denominados **hosted**, se caracterizan porque son programas que se ejecutan sobre un sistema operativo. Obviamente, esto introduce un alto nivel de ineficiencia, ya que cualquier operación que realice el sistema operativo instalado en la máquina virtual deberá ser procesado primero por el hipervisor, luego por el sistema operativo de base, para finalmente llegar al hardware.

Como son aplicaciones normales, su interfaz con el usuario es como el de una aplicación normal, siendo accesible desde el entorno de ventanas de Windows o del sistema operativo en el que esté instalado el hipervisor.

La mayor **ventaja** de los hipervisores de tipo 2 es que tienen una funcionalidad bastante limitada que se traduce en una fácil administración. Esto hace que sean útiles para la realización de pruebas a nivel personal o en entornos académicos, en los que prima la facilidad de manejo frente a la capacidad de realizar un gran número de funciones.

Algunos de los hipervisores de tipo 2 más utilizados son:

- Oracle VirtualBox
- KVM
- Vmware Workstation


## 3.3.- Contenedores

Este tipo de virtualización difiere de los tipos anteriores, siendo incluso discutible el que se trate de virtualización. Los contenedores también se denominan **virtualización del sistema operativo**. Mientras que los hipervisores que hemos visto buscan reproducir una máquina hardware mediante software, los contenedores únicamente virtualizan o duplican el kernel del sistema operativo y las librerías necesarias para realizar una determinada tarea.

Los contenedores no contienen sistemas operativos como las máquinas virtuales, sino que por norma general contienen servicios. Es decir, únicamente la parte del sistema que necesita para lanzar un servicios determinado (por ejemplo, un servidor Web), la cual se ejecuta en un entorno aislado a disposición únicamente de los servicios que ejecuta.

La principal ventaja de los contenedores es que son extremadamente livianos. Esto hace que su creación sea muy rápida y con muy poco impacto en el sistema operativo en que se ejecutan.

Al ser una tecnología reciente no hay muchas soluciones de contenedores, siendo la más popular **Docker**.



*** 

[Volver al índice](index_UT02.md)

