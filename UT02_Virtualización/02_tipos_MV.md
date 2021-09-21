![Carátula UT02](imgs/caratula_ut02.png)

# 2.- TIPOS DE MÁQUINAS VIRTUALES

En base a su funcionalidad podemos distinguir dos tipos de máquinas virtuales:
Máquinas virtuales de sistema 
Máquinas virtuales de proceso

Máquinas virtuales de proceso
El objetivo de estas máquinas es ejecutar el código nativo de los procesos para los que fue diseñada.

La más conocida es la máquina virtual de Java (Java Virtual Machine o JavaVM), disponible para múltiples sistemas operativo como Windows, Linux, Solaris, MAC OS X,…


Máquinas virtuales de sistema
La máquina física se replica en varias máquinas virtuales, cada una con su sistema operativo.
Entre las más populares se encuentran:
VirtualBox
VMware
Parallels
QEMU
Xen
VirtualPC
En este caso el software de virtualización proporciona una réplica del ordenador anfitrión con un subconjunto de sus recursos.
Esto permite tratar a la máquina virtual como un ordenador y, por tanto, instalar sistemas operativos en ella.

Entre sus utilidades destacan:
Coexistencia de varios sistemas operativos
Simulación de hardware
Implantación de servidores
Virtualización de equipos

Como curiosidad, la primera máquina virtual surgió a finales de los 60 con el sistema CP/CMS de IBM

En la actualidad la virtualización está cobrando gran importancia, por lo que los dos grandes fabricantes de procesadores de PC incluyen un juego específico de instrucciones para la virtualización en sus procesadores.

IVT (Intel Virtualization Technology)
AMD-V (AMD Virtualization)


Que un procesador no disponga de estos juegos de instrucciones no quiere decir que no pueda ejecutar software de virtualización, pero el rendimiento será peor.
Por norma general estos juegos de instrucciones hay que habilitarlos en la BIOS (se accede a ella pulsando Supr, F2 o alguna similar durante el arranque del sistema)


[**< Anterior**: 1. Conceptos básicos de virtualización](01_conceptos_básicos.md)

[**Volver** Menú de la UT02](index_UT02.md)

[**Siguiente>**: 3. Tipos de hipervisores](03_tipos_hipervisores.md)