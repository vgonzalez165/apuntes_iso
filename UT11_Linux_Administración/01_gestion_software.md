2.- GESTIÓN DE SOFTWARE EN LINUX
2.1.- INTRODUCCIÓN
Como cualquier otro sistema operativo, Linux dispone de una serie de programas preinstalados, lo cual depende de la distribución escogida, y además podemos instalar nuevos programas, para lo que hay diversos mecanismos.
Pero antes de ver las diversas formas de instalar programas en Linux conviene aclarar algo de terminología que será muy útil para entender los diversos métodos que veremos.
•	Código fuente: cuando un programador desarrolla un programa lo hace utilizando un lenguaje de programación determinado (C, Java, Python, …). El programa en este lenguaje, que puede ser entendido por las personas, se denomina código fuente. 
Con el código fuente podemos saber exactamente cómo funciona internamente el programa.
 
•	Compilar: aunque el código fuente es comprensible para las personas, no lo es para los ordenadores. Por ello se debe realizar un proceso, denominado compilación, que convierte el código fuente en código objeto, que es el que ejecutará el ordenador. Normalmente se distribuye el código objeto de los programas, no el código fuente.
•	Biblioteca o librería: se puede definir como un código reutilizable que puede ser invocado desde otros programas. De esta forma un programador no tiene que volver a codificar una determinada función, sino que puede invocar la de la librería.
Normalmente, al compilar el programa, se incluían en el código objeto las bibliotecas que sean necesarias, aunque eso ha cambiado con las bibliotecas de enlace dinámico.
•	Biblioteca de enlace dinámico (Dynamic link Library): son bibliotecas que se cargan bajo demanda de un programa por parte del sistema operativo. De esta forma, no es necesario incluirlas en el código objeto y por tanto éste tendrá un tamaño inferior.
Así, si varios programas utilizan la misma biblioteca, únicamente habrá una copia de esta gestionada por el sistema operativo, mientras que, si fueran incluidas en el código objeto, habría una copia por cada programa que las utilizara.
•	Paquete: en Linux se denomina paquete a un programa o software empaquetado y preparado para su instalación. Es decir, es el código objeto del programa más información relativa al mismo, como, por ejemplo, las bibliotecas que necesita.
La instalación de un paquete requiere de una herramienta para gestionarlos. La herramienta utilizada por Ubuntu es dpkg o apt, pero hay otras como rpm o YaST.
•	Repositorio: dado que Linux es software libre y por tanto todo el software puede ser copiado y distribuido libremente, tiene sentido agrupar en un único sitio todo el software disponible. Estos servidores donde se agrupan los paquetes se denominan repositorios.
•	Dependencias: cuando instalamos un paquete puede ocurrir que éste necesite que otros paquetes sean instalados previamente. Estos otros paquetes habitualmente son bibliotecas de enlace dinámico, pero pueden ser otros programas. 
2.2.- EL GESTOR DE PAQUETES DE DEBIAN: DPKG
Una solución a los problemas derivados de compilar directamente los programas es el uso de herramientas de empaquetado que automaticen todo el proceso que debíamos realizar manualmente en el caso de la compilación.
Ubuntu utiliza la herramienta dpkg (Debian PacKaGe). Esta herramienta trabaja programas ya compilados que se facilitan en forma de paquetes, ficheros con extensión .deb.
Aunque veremos que esta herramienta no se utiliza directamente, es importante conocerla, ya que todas las aplicaciones para instalar software en Ubuntu se basan en dkpg.
Algunas operaciones que podemos realizar con dpkg son:
•	dpkg -i paquete.deb: instala el paquete .deb.
•	dpkg -l: muestra todos los paquetes .deb que están instalados en el sistema.
•	dpkg -l patrón: muestra todos los paquetes instalados que contienen un determinado patrón.
•	dpkg -r paquete: desinstala un paquete instalado eliminando todos sus ficheros (salvo los de configuración).
•	dpkg --contents paquete.deb: muestra los ficheros que hay en un paquete. También se puede usar la versión corta (-c)
•	dpkg --info paquete.deb: muestra información sobre un paquete tal como la versión, la arquitectura o las dependencias del mismo.

2.3.- SOLUCIONANDO EL PROBLEMA DE LAS DEPENDENCIAS: APT
La utilización de dpkg facilita mucho la instalación de programas, pero sigue teniendo algunos inconvenientes. El principal de todos ellos es que es responsabilidad del usuario gestionar las dependencias, debiendo instalarlas antes de poder instalar el paquete. 
La solución es apt (Advanced Package Tool), una aplicación que utiliza dpkg pero añadiendo dos ventajas muy importantes:
•	Mientras que con dpkg el usuario es responsable de descargar los ficheros .deb, en apt se introduce el concepto de repositorio, un servidor donde se almacenan los ficheros .deb para ser descargados automáticamente.
•	Además, apt determina las dependencias del paquete y procede a instalarlas automáticamente.
Para usar apt tenemos dos comandos: apt-get y apt-cache. Algunas de las opciones más comunes son:
•	apt-get update: actualiza el listado de paquetes disponibles en el repositorio. Es conveniente ejecutar esta orden antes de instalar cualquier paquete.
•	apt-get upgrade: actualiza todos los paquetes instalados en el sistema a su última versión disponible. 
•	apt-cache search patrón: muestra todos los paquetes cuyo nombre o descripción contenga el patrón indicado.
•	apt-get install paquete: instala el paquete indicado.
•	apt-get remove paquete: desinstala el paquete indicado.
•	apt-get clean: limpia los paquetes descargados e instalados.
•	apt-cache show paquete: muestra información detallada de un paquete

2.4.- MEJORANDO APT CON APTITUDE
Aunque con apt-get ya podemos gestionar fácilmente el software instalado en nuestro sistema hay una evolución del mismo, denominado aptitude, que incluye algunas ventajas. Estas son:
•	Las sintaxis de aptitude es la misma, pero utilizando siempre el comando aptitude (que reemplazaría a apt-get y apt-caché)
•	Cuando desinstalamos un paquete con apt-get solo se elimina ese paquete del sistema, pero no sus dependencias, por lo que quedarían paquetes en el sistema que no se utilizan. En cambio, aptitude desinstala también todas las dependencias de un paquete que no estén siendo utilizadas por ningún otro paquete.
•	Con apt-get también se puede hacer esto, pero indicándolo explícitamente, para ello debemos ejecutarlo con la orden autoremove.
•	La orden aptitude puede ser invocada sin argumentos, lo que hará que se muestre un shell interactivo.






2.5.- GESTIÓN DE REPOSITORIOS
Tanto Ubuntu como Mint dividen los repositorios en 4 tipos, cada uno con una serie de paquetes:
•	Main: es el repositorio principal. Todos los paquetes incluidos son software libre y son mantenidos por Canonical, el cual garantiza el mantenimiento de los paquetes, así como las actualizaciones de seguridad.
•	Universe: también son paquetes de software libre, pero a diferencia de Main, son mantenidos por la comunidad, por lo que Canonical no garantiza su mantenimiento. 
•	Restricted: básicamente incluye drivers privativos para diversos dispositivos hardware (tarjetas WiFi, gráficas, …). Es mantenido por Canonical, pero con las limitaciones inherentes al software privativo.
•	Multiverse: contiene software no libre y open source con restricciones legales. Son paquetes mantenidos por la comunidad. Algunos ejemplos pueden ser Adobe Flash Plugin o nautilus-dropbox.
Por defecto, únicamente está habilitado el repositorio main, por lo que si queremos instalar un paquete que se encuentre en alguno de los otros repositorios debemos añadirlos. Esto se consigue con la orden:
sudo add-apt-repository {repositorio}
Recuerda que una vez hecho esto debes actualizar los repositorios realizando un update.
2.6.- GESTORES GRÁFICOS DE PAQUETES
Las versiones de escritorio disponen de aplicaciones gráficas para la instalación de software. Algunas de las más comunes son:
•	Synaptic: básicamente es una interfaz gráfica para apt.
•	Centro de Software de Ubuntu/Gestor de Software de Mint: cada distribución dispone de una herramienta aún más simple para instalar aplicaciones
•	Gdebi: otra interfaz gráfica para apt
2.7.- SNAPS
Todos los sistemas que hemos visto hasta ahora (salvo la compilación del código fuente) están basados en última instancia en dpkg y por tanto heredan unos mismos problemas:
•	Si tenemos una versión relativamente antigua del sistema y queremos instalar una versión reciente de un programa puede ocurrir que las dependencias de este programa aún no se encuentren en la versión requerida en los repositorios.
•	El otro problema es que necesitamos una conexión a Internet para poder instalar los paquetes, siendo prácticamente imposible descargar los paquetes en un ordenador para posteriormente instalarlos en otro.
La solución a estos problemas está en los paquetes Snap, que están disponibles a partir de Ubuntu 16.04LTS. Estos paquetes contienen todos los archivos y dependencias necesarias para instalar un determinado programa, siguiente un enfoque similar al de Windows donde todos los ficheros de instalación son autocontenidos.
Para usar los Snaps debemos tener instalado el demonio snapd. Podemos comprobar si está en ejecución con la orden sudo service snapd status. Si no lo estuviera lo podemos instalar como un programa cualquiera.
 
Algunas de las operaciones que podemos realizar son:
•	snap find patrón: busca todos los paquetes cuyo nombre coincida con el patrón.
•	snap install paquete: instala el paquete indicado
•	snap list: muestra un listado de los paquetes que hay instalados
•	snap refresh: actualiza todos los paquetes instalados.
•	snap remove paquete: elimina un paquete




