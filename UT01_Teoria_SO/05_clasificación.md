![Carátula UT01](imgs/caratula_ut01.png)

# UT01. TEORÍA DE SISTEMAS OPERATIVOS

### Contenidos

1. [Introducción a los sistemas operativos](01_introducción.md)
2. [Historia de los sistemas operativos](02_historia.md)
3. [Componentes de un sistema operativo](03_componentes.md)
4. [Arquitectura de los sistemas operativos](04_arquitectura.md)
5. [Clasificación de los sistemas operativos](05_clasificación.md)


## 5.- CLASIFICACIÓN DE LOS SISTEMAS OPERATIVOS

Hay diversos métodos de clasificación de sistemas operativos en función del aspecto en que nos fijemos. Hay que tener en cuenta también que muchas clasificaciones se han ido diluyendo a lo largo del tiempo habiendo una separación muy difusa entre las diferentes categorías.


### 5.1.- En función del número de usuarios

- **Monousuario**: solo soportan el trabajo de un usuario que dispondrá de todos los recursos del sistema en exclusiva, evitándose los problemas típicos de contienda. Ejemplos: MS/DOS, Windows XP
- **Multiusuario**: son capaces de dar servicio a varios usuarios de forma simultánea de manera que compartan los recursos del sistema: CPU, memoria, … Ejemplos: UNIX, Linux, Windows Server


### 5.2.- En función del número de procesos simultáneos

- **Monotarea o monoprogramación**: solo pueden ejecutar una tarea cada vez y cuando terminan pasan a la siguiente.
- **Multitarea o multiprogramación**: se pueden ejecutar varias tareas de forma simultánea. Esta concurrencia es únicamente aparente ya que las tareas se van turnando en el uso del procesador.


### 5.3.- En función del número de procesadores simultáneos
	
- **Monoproceso**: trabajan con un único procesador sobre el que se van alternando todos los trabajos.
- **Multiproceso**: trabajan con varios procesadores y, por tanto, pueden desarrollar varias tareas simultáneamente.


### 5.4.- En función de los requerimientos temporales

- **Procesos por lotes (batch)**: cada trabajo ocupa completamente la CPU y cuando finaliza se pasa al siguiente. Esto permite la ejecución de varios programas sin supervisión directa del usuario.
- **Procesos con respuesta en tiempo**: de alguna manera es relevante el tiempo en que se deben ejecutar las aplicaciones. Según el tipo de restricción temporal pueden ser:
    - **Tiempo real**: su parámetro clave es el tiempo dado que muchas operaciones deben cumplirse en plazos estrictos. A su vez pueden ser de **tiempo real riguroso**, si el plazo es ineludible, o de **tiempo real no riguroso**, si es aceptable no cumplir de vez en cuando con un plazo. Ejemplos: VXWorks o QNX
	- **Sistemas interactivos**: también deben responder en tiempo a las peticiones del usuario, pero el condicionante temporal no es tan vital. Utilizan técnicas de multiprogramación para conseguir atender varias peticiones de forma simultánea. Ejemplos: Linux, Windows, MacOS, …


### 5.5.- En función del modo de ofrecer los servicios
	
- **Sistemas centralizados**: un ordenador se encarga de todo el trabajo de procesamiento y a él se conectan terminales desde los que el usuario envía sus trabajos. Ejemplos: MULTICS, OS/390
- **Sistemas en red**: mantienen varios equipos conectados entre sí para compartir recursos e información, pero manteniendo cada una autonomía en cuando a capacidad de proceso. Ejemplos: Novell Netware, Windows Server, Banyan VINES, …
- **Sistemas distribuidos**: conjunto de equipos interconectados que se reparten los diferentes trabajos de forma transparente para el usuario. Ejemplos: Amoeba, Sprite, …


***

[**< Anterior**: 4. Arquitectura de los sistemas operativos](01_introducción.md)

[**Volver** Menú de la UT01](index_UT01.md)
