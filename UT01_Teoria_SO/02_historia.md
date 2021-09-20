# 2.- HISTORIA DE LOS SISTEMAS OPERATIVOS

La historia de los ordenadores se descompone en generaciones, que corresponden con grandes avances tecnológicos que han supuesto un punto de inflexión en su evolución. Estas generaciones han sido:

- **Primera generación (1945-1955)**: primeros ordenadores que utilizaban válvulas de vacío. 
- **Segunda generación (1955-1965)**: aparece el transistor, que motiva la aparición de los denominados *mainframes*. Estos ordenadores empiezan a ser accesibles para las grandes multinacionales.
- **Tercera generación (1965-1985)**: con el circuito integrado se reduce aún más el tamaño de los ordenadores, apareciendo los *minicomputadores*, accesibles ya para prácticamente cualquier empresa.
- **Cuarta generación (1985-hoy)**: con la integración a escala muy grande (**VLSI**), los circuitos integrados pueden contener millones o incluso miles de millones de transistores, lo que motiva la aparición de los ordenadores personales. En esta generación, cada persona puede tener su propio ordenador.

De forma paralela a la evolución de los ordenadores así lo han hecho los sistemas operativos que se ejecutan sobre ellos, pudiendo establecer una relación entre las distintas generaciones de ordenadores y los sistemas operativos que han ido surgiendo a lo largo de estos 75 años.


## 2.1.- La primera generación (1945-1955): tubos de vacío y tableros

A mediados de la década de 1940 *Howard Aiken*, en Harvard; *John von Neumman* en Princeton; *Eckert y Mauchely* en Pensilvania y *Konrad Zuse* en Alemania, entre otros, lograron construir máquinas calculadoras. Las primeras empleaban lentos reveladores mecánicas que rápidamente fueron sustituidos por tubos de vacío, lo que dio lugar a los primeros ordenadores.

En estos primeros tiempos un solo grupo de personas diseñaba, construía, programaba, operaba y mantenía cada máquina. Toda la programación se efectuaba en lenguaje de máquina absoluto, a menudo alambrando tableros de conexiones para controlar las funciones básicas de la máquina. Por lo tanto, **no existían sistemas operativos** y los usuarios debían conocer perfectamente el funcionamiento interno de las máquinas.


## 2.2.- La segunda generación (1955-1965): transistores y sistemas por lotes

Con la introducción del **transistor** las computadoras se volvieron lo bastante fiables como para fabricarse y venderse a clientes comerciales. Por primera vez hubo una distinción clara entre diseñadores, constructores, operadores, programadores y personal de mantenimiento.

Un concepto de esta época eran los **sistemas por lotes**, que consistía en ejecutar una serie de programas de forma secuencial. Y este es precisamente el cometido de los sistemas operativos surgidos en esta época. Por ejemplo, **GM-NAA I/O**, desarrollado por General Motors para el IBM 704, era un sistema operativo cuya única función era ejecutar programas por lotes, es decir, uno tras otro.

Otro sistema operativo de la época fue el **IBSYS** para el IBM 7094. Este sistema operativo se caracterizaba porque incluía algunas utilidades tales como compiladores de FORTRAN y COBOL, un ensamblador y algunos programas, como, por ejemplo, un programa de ordenación.

Otro sistema operativo es **FMS (FORTRAN Monitoring System)**, un sistema operativo para el IBM 7090 basado en cinta cuyo propósito era únicamente compilar programas en FORTRAN.


## 2.3.- La tercera generación (1965-80): circuitos integrados y multiprogramación

A principios de los 60 cada ordenador tenía su propio sistema operativo que únicamente funcionaba en dicho ordenador, lo que hacía incompatibles entre sí todos los sistemas operativos, incluso para ordenadores de un mismo fabricante.

IBM intentó resolver ambos problemas con la introducción del **System/360**, una familia de ordenadores que incluía desde pequeñas computadoras hasta grandes mainframes. Lo relevante de esta familia de ordenadores es que compartían la misma **arquitectura**, difiriendo únicamente en el desempeño y el precio. El utilizar la misma arquitectura permitió usar el mismo sistema operativo en todos ellos, denominado **OS/360**, un sistema enorme y complejo, pero que permitía algún tan común actualmente como intercambiar programas y datos entre diferentes ordenadores, es decir, la **compatibilidad** entre sistemas y la **portabilidad** de aplicaciones.

Además, este sistema introdujo técnicas tan comunes en la actualidad como:

- **Multiprogramación**: hasta este momento, los programas solo se podían ejecutar de uno en uno. Cuando se estaba ejecutando un programa había que esperar a que finalizara su ejecución antes de dar paso a otro programa. Esto era muy ineficiente porque los programas habitualmente tienen largos tiempos de espera en los que no hacen nada (por ejemplo, cuando envían una solicitud de lectura a un disco duro o cuando esperan algún tipo de entrada del usuario por teclado). La multiprogramación soluciona este problema permitiendo que varios programas se alojen en memoria y ejecutarlos alternativamente aprovechando los tiempos en que estén esperando algún evento.
- **Spooling (*Simultaneous Paripheral Operation Online*)**: que permitía almacenar los trabajos en disco de forma que nada más finalizar uno se pudiera comenzar a ejecutar el siguiente.
  
En esta generación también apareció el **tiempo compartido**, una variante de la multiprogramación, en la que cada usuario tiene una terminal en línea desde la que pueden trabajar simultáneamente, incluso mientras la computadora trabajaba en segundo plano con trabajos de lotes grandes aprovechando los periodos inactivos de la CPU. El primer sistema de tiempo compartido fue el **CTSS (Compatible Time Sharing System)**, desarrollado en el MIT para la máquina 7094.

Después del éxito del CTSS, MIT, Bell Labs y General Electric decidieron emprender el desarrollo de un “*servicio de computadora*”, una máquina que atendiera a cientos de usuarios por tiempo compartido en un sistema que se podría considerar análogo al sistema de telefonía fija. Este sistema se llamó **MULTICS (MULTiplexed Information and Computing System)**.

Desde el punto de vista comercial, MULTICS tuvo un éxito muy limitado, aunque es cierto que sus usuarios eran muy fieles al sistema, existiendo ordenadores con este sistema operativo instalado hasta mediados de los 90.
Sin embargo, este sistema tuvo una gran relevancia en la historia de los sistemas operativos ya que algunas de sus características influyeron enormemente en posteriores sistemas operativos  que han acabado desembocando en los sistemas operativos actuales.

Otro adelanto durante la tercera generación fue el gran crecimiento de las **minicomputadoras**, como la serie **DEC-PDP**. En una de estas minicomputadoras, junto con MULTICS, podemos marcar el germen que ha acabado desembocado en los sistemas operativos actuales. Esto hecho vino de la mano de dos científicos de computación de Bell Labs que había trabajado en el proyecto MULTICS, **Ken Thompson** y **Dennis Ritchie**. Ambos usaron una vieja DEC PDP-7 que nadie estaba usando para crear un lenguaje de programación denominado **C** (precursor de prácticamente todos los lenguajes de programación que hay en la actualidad) y un sistema operativo que pretendía ser una versión austera de MULTICS para un único usuario. A este sistema lo llamaron **UNIX**.

El sistema operativo UNIX tuvo una rápida expansión entre universidades, donde gran número de colaboradores contribuyeron a ampliarlo y mejorarlo. Con el tiempo se escindió en dos versiones: el **System V de AT&T** y el **BSD de la Universidad de Berkeley**.

El System V ha dado lugar a las versiones comerciales de UNIX de la actualidad, como el IBM AIX, HP-UX o Solaris de Oracle. En cambio, BSD evolución en otro sentido, hacia versiones de software libre, como NetBSD, FreeBSD y OpenBSD; y también en un sistema operativo ampliamente conocido en la actualidad: el Mac OS X de Apple.


## 2.4.- La cuarta generación (1980-1995): computadoras personales

Esta generación está caracterizada por los ordenadores personales y, con ellos, una gran variedad de sistemas operativos que, en muchos casos, establecieron las bases para los sistemas operativos actuales.

### 2.4.1.- Primeros sistemas operativos orientados a computadoras personales

El primer sistema operativo para microcomputadoras fue **CP/M (Control Program for Microcomputers)**, desarrollado por Gary Kildall para el procesador Intel 8080.  Posteriormente fue reescrito para otros procesadores, como el Zilog Z80 o el Motorola 68000, lo que le permitió dominar el mundo de la microcomputación durante cinco años. Algunos ordenadores que utilizaron este sistema operativo son el ZX Spectrum, Atari ST, MSX o el Apple II.

El otro gran sistema operativo de la época fue el **MS-DOS**. Este sistema tiene su origen en el sistema DOS (Disk Operating System) de Seatle Computer Products. Este fue adquirido por Bill Gates, que lo renombró y le añadió el intérprete BASIC de Microsoft para su uso en el PC original de IBM. Este sistema pronto dominó el mercado de PCs. Un factor clave fue la decisión de vender el MS-DOS a compañías de computadoras para incluirlo con el hardware, en lugar de venderlo directamente al usuario como se hacía con CP/M.

Hasta este momento, la interfaz de usuario de todos los sistemas operativos consistía en una terminal de texto en la que el usuario introducía comandos u órdenes. Pero eso cambió gracias a las investigaciones hechas por **Doug Engelbart** en el Stanford Research Institute donde inventó la **GUI (Graphical User Interface)**, provista de ventanas, iconos, menús y ratón. Los investigadores de Xerox PARC adoptaron estas ideas y las incorporaron en las máquinas que fabricaban, aunque no les llegaron a dar aplicación comercial ya que no fueron capaces de ver su potencial.
Quien sí lo vio fue Steve Jobs después de una visita a Xerox PARC, lo que llevó a la construcción de Lisa, la primera computadora con GUI, que fracasó por ser demasiado cara. El segundo intento, la **Apple Macintosh**, fue un enorme éxito, no sólo porque era más económica, sino porque era amigable para el usuario, lo cual la hacía apta para usuarios sin conocimientos de informática.


### 2.4.2.- MS-DOS y Windows

Cuando Microsoft buscó un sucesor para MS-DOS en 1985 se basó en el éxito de la Macintosh, por lo que produjo un sistema basado en GUI que llamó **Windows** y que originalmente se ejecutaba encima de MS-DOS, es decir, era un **shell** y no un verdadero sistema operativo. En 1995 Microsoft sacó **Windows 95**, que ya se podía considerar un sistema operativo que integraba Windows y MS-DOS. Este evolucionó con Windows 98 y posteriormente con el malogrado **Windows Me (Millenium Edition)**.

Pero estos sistemas seguían estando construidos sobre MS-DOS, lo que acarreaba múltiples deficiencias y fallos, lo que se puso de manifiesto sobre todo con Windows Me. Por ello, Microsoft reescribió Windows desde cero y creó un sistema operativo gráfico de 32 bits que denominó **Windows NT** y que posteriormente evolucionó hacia **Windows 2000**.

El objetivo de Microsoft era que todos los usuarios de Windows 95, 98 y Me se pasaran a este sistema, ya que era un sistema muy superior y mucho más estable, pero no llegó a conseguirlo, quedando relegado a entornos corporativos.
Microsoft siguió intentándolo y llegó al que sería su gran éxito, el sistema operativo que combinaba la estabilidad de Windows 2000 con la familiaridad para el usuario de Windows Me. Este sistema se denominó **Windows XP** y llegó a vender más de 1000 millones de copias hasta su desaparición en 2014.

En 2005, cuando se empezó a hacer patente que Windows XP necesitaba ceder el paso a otro sistema operativo más evolucionado, Microsoft anunció que estaba desarrollando el sucesor de XP, denominado con el nombre clave Longhorn. Este sistema operativo prometía un gran número de funcionalidades totalmente revolucionarias, pero, a medida que fue avanzando su desarrollo, diversos problemas hicieron que estas fantásticas funcionalidades prometidas se esfumaran de forma que el producto que finalmente apareció, denominado **Windows Vista**, fue un auténtico fracaso.

Microsoft rápidamente sacó un nuevo sistema operativo, denominado **Windows 7** que es el que consiguió que los usuarios abandonaran Windows XP. Tras un intento fallido con Windows 8 llegamos al sistema operativo más utilizado en la actualidad, **Windows 10**.

Paralelamente a todos estos sistemas orientados al mercado doméstico, Microsoft han mantenido una familia de sistemas operativos orientados al mercado de servidores, denominados **Windows Server**.


### 2.4.3.- Linux

El desarrollo de **Linux** es, en cierto modo, paralelo al de Windows, y su origen tiene un nombre propio y una fecha: **Richard Stallman**, un programador que, en 1983, ante un entorno tecnológico cada vez más comercializado, añoraba los viejos tiempos en los que el software era creado en universidades y no tenía propietario, los programas se distribuían libremente y cualquiera con conocimientos podía contribuir a su desarrollo. Por ello, en ese año decidió acuñar el concepto de **software libre**, el cual se resume en cuatro libertades esenciales:

- Libertad de ejecutar un programa para cualquier propósito.
- Libertad para estudiar como funciona un programa, para lo que es necesario, por tanto, el acceso al código fuente.
- Libertad de redistribuir copias de un programa.
- Libertad de distribuir copias de sus versiones modificadas a terceros.

Además, al tiempo que acuñó este concepto, inició el **proyecto GNU** (GNU is not Unix), con el que pretendía crear un sistema operativo similar y compatible con Unix, pero de software libre. En su camino hacia este objetivo, en 1985 creó la Fundación del Software Libre (FSF) y desarrolló la Licencia General de GNU (GNU GPL)

A mediados de los 90 había avanzado mucho en su objetivo de construir un sistema operativo libre. Tenía un gran número de utilidades, pero se había encontrado con grandes problemas al diseñar el componente más importante de un sistema operativo: el kernel, que es el componente encargado de comunicarse directamente con el hardware.

Y aquí dejamos a Richard Stallman para viajar a Holanda, allí, un profesor de Universidad llamado **Andrew Tannenbaum** había escrito un libro (*Operating Systems: Design and Implementation*) orientado a sus alumnos de sistemas operativos donde desarrollaba la construcción de un sistema operativo, al que denominaba Minix por ser una versión muy limitada de Unix. Este libro tuvo una gran repercusión entre estudiantes de todo el mundo. Uno de estos estudiantes fue **Linux Torvald**¸ un estudiante finés que decidió partir de Minix para crear un sistema operativo más funcional. El 25 de agosto de 1991 publicó un mensaje en Usenet anunciando la creación de este núcleo y dejando el proyecto a la colaboración de cualquiera que quiera aportar algo. Este llamamiento tuvo un éxito enorme y muy pronto había un gran número de colaboradores contribuyendo a este sistema, al cual decidió llamar **Linux**.

Aunque al principio tenía una licencia propia que restringía la actividad comercial, en 1992 lo vinculó a la licencia GNU GPL que había desarrollado Richard Stallman, lo que permitió completar aquello que faltaba en el proyecto GNU: el kernel. El resultado es el sistema operativo que conocemos actualmente como **GNU/Linux**.



[**< Anterior**: 1. Introducción a los Sistemas Operativos](01_introducción.md)

[**Volver** - Menú de la UT01**](index_UT01.md)

[**Siguiente>**: 3. Componentes de un sistema operativo](03_componentes.md)
