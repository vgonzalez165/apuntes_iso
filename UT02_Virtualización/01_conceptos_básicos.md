<link rel="stylesheet" href="../styles.css">

![Carátula UT02](imgs/caratula_ut02.png)

## Contenidos

1. [**Conceptos básicos de virtualización**](01_conceptos_básicos.md)
2. [Tipos de máquinas virtuales](02_tipos_MV.md)
3. [Tipos de hipervisores](03_tipos_hipervisores.md)
4. [Oracle VirtualBox](04_virtualbox.md)
5. [Microsoft Hyper-V](05_hiper-v.md)


# 1. CONCEPTOS BÁSICOS DE VIRTUALIZACIÓN

## 1.1.- Definición de máquina virtual

Una **máquina virtual** es un software que permite reproducir una máquina con la misma arquitectura que la máquina sobre la que se está ejecutando. Es decir, podemos considerarla como un programa que simula un ordenador completo de forma que podemos hacer sobre esa máquina virtual cualquier tarea que podríamos realizar sobre una máquina física.

Las máquinas virtuales no simulan todo el hardware, sino que se apoyan en el hardware subyacente haciendo en muchas ocasiones de puente entre el sistema virtualizado y la máquina física. Por este motivo, la máquinas virtuales siempre tienen que tener la misma **arquitectura** que la máquina física en la que se están ejecutando.

Cuando hablamos de arquitectura nos referimos al diseño interno del procesador, sus instrucciones al más bajo nivel y su estructura operacional.

Las arquitecturas más habituales son:

- **x86-64**: es la que utilizan todos los procesadores de Intel y AMD. 
- **ARM**: usada en dispositivos móviles. Algunos fabricantes que la utilizan son Qualcomm, Mediatek, Samsung, ..

Esto implica que un ordenador de sobremesa con Windows, que solo se ejecuta sobre arquitecturas x86, solo podrá tener máquinas virtuales basadas en dicha arquitectura. Así, podrá instalar un Windows, un Linux o incluso un BeOS, pero nunca podría instalar un Android (en realidad se podría, pero serían imágenes preparadas para ejecutarse sobre x86).

Los programas que permiten instalar sistemas operativos de arquitecturas diferentes se llaman **emuladores** y son mucho más lentos que las máquinas virtuales, ya que absolutamente todas las funciones del hardware se tienen que simular por software.

Algunos ejemplos de emulador sería:

- **BlueStacks**: permite emular Android en nuestro ordenador, es decir, es un software que emula completamente una arquitectura ARM.
- **ePSXe**: en este caso es un emulador de la PlayStation, que también tiene una arquitectura completamente diferente.

En la actualidad la virtualización es una tecnología madura que está ampliamente extedida en todos los ámbitos, desde pequeños sistemas de virtualización en equipos personales hasta sistemas en la nube capaces de virtualizar cientos de servidores. 

Algunas de las aplicaciones de virtualización más conocidas son:

- Oracle VirtualBox
- VMWare, con soluciones de virtualización para escritorio y también para grandes servidores
- Xen
- Microsoft Hyper-V


## 1.2.- Ventajas e inconvenientes de la virtualización

La amplia difusión de las tecnologías de virtualización se deben a las múltiples ventajas que tiene, entre las que destacan:

- **Reducción de costes y mejor gestión de los recursos hardware**. Se pueden agrupar todos los recursos hardware en una misma máquina que ejecuta múltiples máquinas virtuales, pudiendo tener un reparto más eficiente de los recursos. 
- **Las MV pueden ser creadas, clonadas y destruidas a demanda**. La creación, clonación y destrucción de máquinas es un proceso casi instantáneo, pudiendo crear fácilmente máquinas a medida que las vamos necesitando.
- **Posibilidad de vender capacidad de cálculo a otras empresas (VPS, Virtual Private Servers)**. Hay múltiples empresas que proporcionan servidores virtuales en la nube con los recursos hardware que deseemos, como [Digital Ocean](https://www.digitalocean.com) o [Vultr](https://www.vultr.com/). ESte tipo de servicio se denomina **IaaS (Infrastruture as a Service)**.
- **Mayor facilidad para restablecer sistemas de mantenimiento, disponibilidad o recuperación de datos tras un desastre**. Dado que se puede levantar una máquina virtual en pocos segundos, cuando un servidor cae se puede levantar una máquina virtual que lo reemplace de forma casi instantánea.
- **Simplificación de los sistemas de copia de seguridad**. 
- **Permiten guardar el estado**. Al ser todo virtual, se puede almacenar el estado exacto en que está en una máquina virtual para recuperarlo cuando deseemos. Así, por ejemplo, podemos guardarlo antes de aplicar las actualizaciones y, en caso de que algo vaya mal, recuperar el estado anterior para dejar la máquina como estaba.
- **Permiten rápida incorporación de nuevos recursos**. Es muy sencillo añadir más memoria RAM, más discos duros o más núcleos virtuales a una máquina virtual siempre que la máquina física disponga de esos recursos.
- **Entornos de aprendizaje y pruebas**. Las máquinas virtuales son ideales para probar otros sistemas operativos sin realizar ningún cambio permanente en nuestro equipo.
- **Compatibilidad de programas**. Podemos utilizar programas que solo estén disponibles para otro sistema operativo. Por ejemplo, si tenemos un programa antiguo que solo funciona en Windows XP podemos instalar este sistema en una máquina virtual y ejecutar ahí el programa.
- **Entornos controlados**. Se pueden probar los efectos de programas en los que no confiamos, por ejemplo si sospechamos que tienen algún tipo de software malicioso o si preveemos que pueden perjudicar nuestro ordenador de alguna forma.
- **Fácil migración de unos ordenadores a otros**. Mover una máquina virtual entre equipos es tan fácil como mover un archivo.
- **Más ecológicas**. Requieren menor cantidad de recursos duplicados como ventiladores, monitores, … y por tanto reducen el consumo energético y el ruido.

Aunque son muy pocos, las máquinas virtuales también tienen algunos **inconvenientes**.

- **Ralentizan el sistema**. Hay que pasar a través de la máquina virtual para ejecutar el sistema. Sin embargo, este inconveniente es cada vez menos perceptible, sobre todo desde que los procesadores incluyen las extensiones del juego de instrucciones de virtualización.
- **Se vuelve inestables a nivel de velocidad si hay varias máquinas ejecutándose simultáneamente**. El reparto de recursos entre las máquinas virtuales puede no ser todo lo equitativo que debiera, provocando incrementos de latencia temporales. Aunque hay que tener en cuenta que esto no afectará a la estabilidad a nivel de resultados.
- **Los usuarios pueden instalar programas no permitidos en el anfitrión**.


*** 

[Volver al índice](index_UT02.md)