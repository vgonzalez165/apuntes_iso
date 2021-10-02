![Carátula UT02](imgs/caratula_ut02.png)

## Contenidos

1. [Conceptos básicos de virtualización](01_conceptos_básicos.md)
2. [**Tipos de máquinas virtuales**](02_tipos_MV.md)
3. [Tipos de hipervisores](03_tipos_hipervisores.md)
4. [Oracle VirtualBox](04_virtualbox.md)
5. [Microsoft Hyper-V](05_hiper-v.md)


# 2.- TIPOS DE MÁQUINAS VIRTUALES

Si nos fijamos en la funcionalidad del software de virtualización, podemos distinguir dos grandes tipos:
- Máquinas virtuales de proceso
- Máquinas virtuales de sistema


## 2.1.- Máquinas virtuales de proceso

Este tipo de máquinas virtuales se salen de la idea que tenemos de virtualización donde se simula un ordenador completo. Las máquinas virtuales de proceso tienen como objetivo ejecutar código nativo de los procesos para los que fueron diseñadas. Es decir, su objetivo es únicamente ejecutar código diseñado para una arquitectura específica, no relacionado con ninguna máquina física.

Probablemente la máquina más conocida es la máquina virtual de Java (JVM). Los programas en Java se convierten en un código denominado **bytecodes** que se ejecuta en dicha máquina virtual. Como la máquina virtual de Java se puede instalar en múltiples sistemas operativos (Windows, Linux, Mac OS) conseguimos que el código Java sea multiplataforma.


## 2.2.- Máquinas virtuales de sistema

En este tipo de sistemas, la máquina física se replica en varias máquinas virtuales, cada una con su sistema operativo. Hay múltiples soluciones de virtualización de este tipo, como 
- VirtualBox
- VMware
- Parallels
- QEMU
- Xen

En este caso el software de virtualización proporciona una réplica del ordenador físico con un subconjunto de sus recursos. Al ordenador físico que ejecuta el software se le denomina **anfitrión**, mientras que las máquinas virtuales se les suele denominar **invitados**. Esto permite tratar a la máquina virtual como un ordenador y, por tanto, instalar sistemas operativos en ella.

Entre sus utilidades destacan:
- Coexistencia de varios sistemas operativos
- Simulación de hardware
- Implantación de servidores
- Virtualización de equipos

Como curiosidad, aunque puede parecer que la virtualización es algo reciente, la primera máquina virtual surgió a finales de los 60 con el sistema CP/CMS de IBM, aunque no ha sido hasta estos últimos años cuando los sistemas virtuales se han popularizado.

Actualmente la virtualización está cobrando gran importancia, por lo que los dos grandes fabricantes de procesadores de PC incluyen un juego específico de instrucciones para la virtualización en sus procesadores.

- **Intel VT** (Intel Virtualization Technology)
- **AMD-V** (AMD Virtualization)

Aunque difieren en el nombre, ambas tecnologías son similares y su presencia en el microprocesador permite ejecutar máquinas virtuales de forma mucho más eficiente. Incluso algunas funcionalidades de las máquinas virtuales no estarán disponibles si no disponemos de alguno de estos juegos de instrucciones, por ejemplo, la capacidad de ejecutar máquinar virtuales de 64 bits o utilizar la virtualización anidada (crear máquinas virtuales dentro de otras máquinas virtuales).

Que un procesador no disponga de estos juegos de instrucciones no quiere decir que no pueda ejecutar software de virtualización, pero el rendimiento será peor.

Por norma general estos juegos de instrucciones hay que habilitarlos en la BIOS (se accede a ella pulsando Supr, F2 o alguna similar durante el arranque del sistema)


*** 

[Volver al índice](index_UT02.md)