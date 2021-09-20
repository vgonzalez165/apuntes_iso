# 1. CONCEPTOS BÁSICOS DE VIRTUALIZACIÓN

## 1.1.- Definición de máquina virtual

Una máquina virtual es un software que permite reproducir una máquina con la misma arquitectura que la máquina sobre la que se está ejecutando. Una máquina virtual se apoya en el hardware 
subyacente para realizar algunas de las operaciones.

Arquitectura de computadoras: hace referencia al diseño interno del microprocesador, sus instrucciones al más bajo nivel y su estructura operacional.

Las más habituales:
• x86-64: es la que utilizan todos los procesadores de Intel y AMD.
• ARM: usada en dispositivos móviles. Algunos fabricantes que la utilizan son Qualcomm, Mediatek, Samsung, ..


Esto quiere decir que un ordenador de sobremesa solo puede tener máquinas virtuales basadas en la arquitectura x86.
Por ejemplo, en una máquina virtual no podríamos instalar un Android. Para hacer esto, necesitaríamos un emulador

En un emulador la máquina que simula el software tiene una arquitectura totalmente diferente. Por ello todas las operaciones se simulan mediante software.

Ejemplos de emulador:
• BlueStacks: permite emular Android en nuestro ordenador.
• ePSXe: en este caso es un emulador de la PlayStation, que también tiene una arquitectura diferente.

Hay gran número de aplicaciones de virtualización en el 
mercado, entre las que destacan:
• JavaVM
• VirtualBox
• VMWare
• .Net
• Parallels
• Xen
• Hyper-V


## 1.2.- Nomenclatura

**¿algo de host, anfitrión y todo eso?¿????????????????????????**


## 1.3.- Ventajas e inconvenientes de la virtualización

Ventajas 

Reducción de costes y mejor gestión de los recursos hardware. Cada máquina física se puede usar para varios propósitos a la vez.
Las MV pueden ser creadas, clonadas y destruidas a demanda.
Posibilidad de vender capacidad de cálculo a otras empresas (Virtual Private Servers)
Mayor facilidad para restablecer sistemas de mantenimiento, disponibilidad o recuperación de datos tras un desastre.
Simplificación de los sistemas de copia de seguridad.
Permiten guardar el estado
Permiten rápida incorporación de nuevos recursos
Entornos de aprendizaje y pruebas
Compatibilidad de programas. Podemos utilizar programas que solo estén disponibles para otro sistema operativo.
Entornos controlados, se pueden probar los efectos de programas en los que no confiamos
Fácil migración de unos ordenadores a otros.
Más ecológicas ya que requieren menor cantidad de recursos duplicados como ventiladores, … y por tanto reducen el consumo energético y el ruido.

Inconvenientes

Ralentizan el sistema ya que hay que pasar a través de la máquina virtual para ejecutar el sistema.
Se vuelve inestables a nivel de velocidad si hay varias máquinas ejecutándose simultáneamente (aunque estables a nivel de resultados)
Los usuarios pueden instalar programas no permitidos en el anfitrión.


[**Volver** Menú de la UT01](index_UT02.md)

[**Siguiente>**: 2. Tipos de máquinas virtuales](02_tipos_MV.md)