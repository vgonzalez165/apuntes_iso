
# 7.- Gestión de procesos

## 7.1.- Definición de proceso

7.2.- Ciclo de vida
Desde que son creados, los procesos van a pasar por diferentes estados entre los que irá cambiando como respuesta a determinados eventos, que pueden tener orígenes muy variados: parte de la ejecución del propio proceso, interacción con el usuario, eventos producidos por el sistema operativo, …
Los estados que puede tener un proceso son:
	En ejecución: el proceso se está ejecutando en ese momento en el procesador. En cada instante, solo puede haber un proceso en ejecución en el procesador.
	Bloquedado: durante su ejecución, los procesos realizarán operaciones, pero tarde o temprano deberán realizar una operación de entrada y salida. Por ejemplo, leer un dato del disco o esperar la pulsación de una tecla por el usuario.
En estas situaciones, el proceso abandona la CPU para permitir que otro proceso pueda utilizarla y pasa a un estado bloqueado mientras espera a que finalice la operación de E/S.
	Listo: cuando finaliza la operación de E/S a la que está esperando, el proceso ya podrá volver a ejecutarse, pero es muy probable que el procesador esté siendo utilizado en ese momento por otro proceso, por lo que deberá esperar a que quede libre para volver al estado de ejecución. 
Otra posibilidad para que un proceso pase a este estado es por decisión del planificador, un componente del sistema operativo que decide cómo se reparten el uso del procesador entre todos los procesos. Por ejemplo, puede pasar un proceso de ejecución a listo porque lleve demasiado tiempo ejecutándose o porque haya llegado otro proceso con mayor prioridad que quiera ejecutarse.
	Nuevo: cuando se crea un nuevo proceso en el sistema, el sistema operativo le asigna un identificador (PID, process identifier) y crea una serie de estructuras de datos asociadas, pero no tiene por qué incluirlo en el grupo de procesos ejecutables, por ejemplo, porque ya haya demasiados procesos en el sistema. En esta situación se dice que el proceso está en estado nuevo.
	Terminado: tarde o temprano todos los procesos finalizan su ejecución, por lo que el sistema operativo los excluirá de la lista de procesos ejecutables. Sin embargo, es habitual que el sistema conserve durante un tiempo las estructuras de datos asociadas al proceso, por ejemplo, para que los programas de auditoría puedan extraer información de este. 
	Suspendido: cuando ejecutamos un programa, y por tanto se convierte en proceso, es copiado en memoria el código del programa, así como los datos que necesite. De esta forma, cuantos más procesos tengamos en ejecución en el sistema más memoria tendremos ocupada, cuando realmente muchos de esos procesos tal vez estén esperando eventos de E/S que tardarán bastante en finalizar.
Una solución a esto es el intercambio, que consiste en mover procesos de la memoria principal al disco para dejar espacio en memoria para otros procesos. Esto generará dos nuevos estados:
	Bloqueado suspendido: un proceso bloqueado que pase a memoria secundaria pasará a este estado.
	Listo suspendido: cuando finalice el evento al que estaba esperando el proceso, ya podrá ejecutarse, pero antes tendría que volver a la memoria principal. Es decir, es el paso previo a listo cuando un proceso ha sido enviado a la memoria secundaria.
 
7.3.- Planificación de procesos
7.3.1.- El planificador de procesos
Por norma general, un sistema tiene decenas o cientos de procesos compitiendo por los recursos del sistema, en especial por el procesador, ya que únicamente puede haber un proceso en ejecución en un instante determinado.
Una de las tareas del sistema operativo es decidir qué políticas se van a aplicar para decidir qué proceso pasa en cada momento a ejecutarse en el procesador. Esta decisión se toma mediante los algoritmos de planificación, y el componente del sistema operativo encargado de aplicarlos es el planificador o calendarizador.
Los objetivos que debe alcanzar un buen planificador son:
	Maximizar la utilización de la CPU, dado que el procesador es un recurso muy valioso, el objetivo será que esté ocupado el mayor tiempo posible.
	Maximizar la productividad, de forma que se complete el mayor número de procesos por unidad de tiempo.
	Minimizar el tiempo de retorno, que es el intervalo de tiempo que pasa desde la entrada de un proceso en el sistema hasta su finalización.
	Minimizar el tiempo de espera, el tiempo que transcurre desde que un proceso solicita ser ejecutado hasta que comienza su ejecución.
	Minimizar el tiempo de respuesta, especialmente importante en procesos en tiempo real, por ejemplo, los procesos interactivos en los que el usuario espera una respuesta.
Para poder mesurar todos estos objetivos se utilizan una serie de métricas que nos darán una medida del éxito de los diferentes algoritmos en conseguir los objetivos anteriores. Estas métricas son:
	t_i: tiempo de llegada del proceso
	t: tiempo de procesador que necesita el proceso
	t_f: momento en que finaliza el proceso
	T: tiempo total que el proceso está en el sistema. Por lo tanto, T=t_f-t_i
	E: tiempo que el proceso está esperando a que otro proceso le ceda el procesador, es decir, el tiempo que está en estado listo.
	I: índice de servicio, que es la relación entre el tiempo que el proceso está realmente en ejecución y el tiempo total que está en el sistema. Por lo tanto, I=t/T
7.3.2.- Características de los algoritmos de planificación
Como veremos en breve, hay múltiples algoritmos de planificación, cada uno con unas características determinadas con objeto de priorizar unas u otras métricas. A grandes rasgos, se pueden clasificar los algoritmos en dos grandes grupos:
	Algoritmos no apropiativos: son los más sencillos ya que, cuando un proceso comienza a ejecutarse únicamente abandona el procesador por decisión propia. Esto puede ocurrir cuando haya finalizado su ejecución o también cuando tenga que esperar a un evento de entrada/salida. El inconveniente de este tipo de algoritmos es que procesos que requieran largas ráfagas de CPU bloquearán al resto de procesos.
	Algoritmos no apropiativos: la otra posibilidad es que el planificador tenga la posibilidad de expulsar a un proceso de la CPU, por ejemplo, si considera que lleva demasiado tiempo ejecutándose.
Otra posible clasificación es en función de la importancia que tiene cada proceso, en ese caso hay dos tipos diferentes:
	Algoritmos sin prioridades: todos los procesos son iguales y no hay ningún tipo de preferencia. Los procesos se ejecutan por orden de llegada al procesador.
	Algoritmos con prioridades: cada proceso tiene una prioridad asignada de forma que, cada vez que queda libre el procesador, se ejecuta el proceso con mayor prioridad. Por ejemplo, en un sistema se puede dar mayor prioridad a los procesos interactivos (en los que el usuario espera una respuesta rápida) frente a los procesos en segundo plano.
Puede haber dos tipos de prioridades:
	Prioridades estáticas: cuando a un proceso se le asigna una prioridad, ésta se mantiene durante todo su ciclo de vida.
	Prioridades dinámicas: la prioridad puede variar a lo largo de la vida del proceso. Por ejemplo, se puede aumentar la prioridad a un proceso que lleve demasiado tiempo esperando.
7.3.3.- FCFS (First Come, First Served)
El algoritmo FCFS es uno de los algoritmos de planificación más sencillos. A medida que van llegando procesos al sistema se van añadiendo al final de una cola de ejecución, y, cuando el procesador queda libre, se escoge el primero proceso de la lista para pasar a ejecutarse.
Este algoritmo es justo en el sentido de que todos los procesos tienen la misma oportunidad para ejecutarse, sin embargo, tiene el problema de que los procesos cortos pueden verse afectados por otros procesos que requieran mucho tiempo de procesador y hayan llegado antes. A esto se le denomina efecto convoy.
7.3.4.- FCFS con prioridades
Es una alternativa al algoritmo FCFS en el que cada proceso tiene asignada una prioridad, normalmente representada mediante un valor numérico. De esta forma, se puede asignar una prioridad mayor a los procesos cortos para que sean seleccionados antes en la cola, evitando así el efecto convoy.
Pero este algoritmo tiene otro problema, y es que puede ocurrir que un proceso con poca prioridad esté indefinidamente en la cola porque están llegando continuamente otros procesos con más prioridad que se ejecutarán antes que él. A este problema se le denomina bloqueo indefinido o inanición.
Hay una solución, denominada envejecimiento, que consiste en incrementar cada cierto tiempo las prioridades de todos los procesos que están en la cola, de forma que tarde o temprano, cualquier proceso llegará a tener la suficiente prioridad como para poder ejecutarse.
7.3.5.- SJF (Shortest Job Firs)
Una alternativa para evitar que los procesos muy largos hagan esperar a los procesos cortos es la propuesta en el algoritmo SJF, en la que cada vez que el procesador queda libre, se selecciona entre todos los procesos que están esperando aquel que menos tiempo de procesador vayan a requerir.
Con este algoritmo se consigue que los procesos largos no demoren excesivamente los procesos cortos, pero tiene el inconveniente de que requiere que los procesos sepan de antemano el tiempo que van a estar ejecutándose, lo cual no siempre es posible.
7.3.6.- SRTF (Shortest Remaining Time First)
Mientras que FCFS y SJF son no apropiativos, el algoritmo SRTF es la versión apropiativa del algoritmo SJF: cada vez que llega un proceso nuevo a la cola, se compara el tiempo que va a requerir de procesador con el tiempo que le queda al proceso que se está ejecutando. Si va a necesitar menos tiempo que el proceso que el que le queda de ejecución al proceso actual, éste es expulsado (pasando directamente al estado de listo) y el recién llegado pasa a ejecutarse.
Este algoritmo presenta un desempeño excelente, con un buen índice de servicio y una alta eficiencia, pero tiene el inconveniente de que es injusto en el sentido de que un proceso muy largo puede ser repetidamente expulsado de la CPU.
7.3.7.- Round Robin
Este algoritmo apropiativo es ampliamente utilizado por su buen desempeño general. En este caso se define un tiempo denominado quantum, que será el tiempo máximo que un proceso puede estar ejecutándose en la CPU. Transcurrido ese tiempo será expulsado del procesador para dar paso a otro proceso, aunque no haya finalizado su ejecución.
Los procesos pendientes de ejecutarse esperan en una cola en la que están colocados según orden de llegada. Cuando un proceso es expulsado del procesador se coloca al final de esta cola.
Un factor muy importante en el rendimiento de este algoritmo es la elección del tiempo de quantum, y esto se debe al denominado cambio de contexto. Cuando la CPU cambia de un proceso a otro, la transición no es inmediata, sino que hay que llevar los datos del proceso saliente a la memoria y cargar los datos del proceso entrante en los registros del procesador (contador de programa, registro de instrucción, …). Este proceso se denomina cambio de contexto y requiere de algunos ciclos de reloj. De esta forma, según el tamaño del quantum pueden darse las siguientes situaciones:
	Un quantum muy corto provocará muchos cambios de contexto, que ocuparán gran parte del tiempo del procesador.
	Un quantum muy largo hará que la mayoría de los procesos puedan finalizar su ejecución dentro de ese quantum, por lo que el algoritmo degenerará en un algoritmo FCFS.
7.3.8.- Colas multinivel

7.3.9.- MLFQ (Colas multinivel con realimentación)



8.- Gestión de memoria
El sistema de gestión de memoria es un componente muy importante del sistema operativo. Mientras que el procesador únicamente puede ser utilizado por un único proceso en un momento determinado, debiendo turnarse para utilizarlo, la memoria puede ser utilizada simultáneamente por todos los procesos, siendo necesario en este caso establecer mecanismos que permitan repartírsela entre los diferentes procesos.
Los primeros sistemas multiprogramados utilizaban sistemas de intercambio, que podían ser de particiones fijas o particiones dinámicas. Estos sistemas ya están obsoletos y ningún sistema operativo actual los utiliza, pero su estudio nos ayudará a comprender los problemas inherentes a la gestión de memoria, facilitándonos el entendimiento de los actuales sistemas de memoria virtual.
5.1.- Intercambio
El intercambio o swaping consiste en traer a la memoria un proceso entero, ejecutarlo durante un rato, y, si es necesario hacer espacio en memoria para otros procesos, llevarlo a memoria secundaria de donde podrá volver a la memoria principal cuando el sistema decida que puede continuar su ejecución.
Hay dos posibles implementaciones en función de cómo sean los espacios en memoria donde se guardará el proceso: particiones fijas y particiones variables.
5.1.1.- Particiones fijas 
En este caso la memoria se encuentra dividida en una serie de particiones cuyo tamaño es fijo, y es el mismo para todas.
Cuando llega un proceso nuevo al sistema es cargado en la primera partición libre. Si estuvieran todas las particiones ocupadas, será necesario mover algún proceso a la memoria secundaria, utilizando alguno de los algoritmos de reemplazo que veremos más adelante.
Este método fue el primer sistema de intercambio que hubo y era muy útil ya que permitía tener en el sistema más procesos de los que cabían en memoria, pero tiene dos inconvenientes:
	No se podían ejecutar procesos cuyo tamaño fuera superior al tamaño de la partición en el disco.
	Tenía un uso muy ineficiente de la memoria, cualquier proceso, por pequeño que fuera, ocuparía una partición completa, malgastando el espacio restante. A este problema se le denomina fragmentación interna.
Una forma de solucionar los inconvenientes anteriores es utilizar particiones de diferentes tamaños, de forma que cada proceso es cargado en la partición que mejor se adapte a su tamaño. Con esto se conseguía minimizar los problemas anteriores, aunque no los solucionaba.
5.1.2.- Particiones dinámicas
La alternativa que surgió a las particiones fijas fue el particionamiento dinámico. En este caso las particiones se crean a medida de los procesos que llegan, asignándoles a cada uno exactamente la cantidad de memoria que necesita. Este método aprovecha al máximo la memoria, pero también complica su asignación, liberación y control.
Aparte de la mayor complejidad en la gestión, este sistema tiene el inconveniente de que, con el tiempo, la memoria se va llenando de huecos demasiados pequeños en los que no cabe ningún proceso. Este fenómeno se denomina fragmentación externa, y únicamente se puede solucionar mediante la compactación de memoria, que consiste en reubicar todas las particiones para dejar todo el espacio libre en un único hueco, con el coste de CPU que eso requiere.
5.2.- Memoria virtual
Los sistemas de memoria virtual abordan la gestión de memoria desde otro enfoque diferente: en lugar de cargar el proceso entero en memoria se divide en fragmentos y únicamente se cargan en la memoria principal los fragmentos que necesite para ejecutarse, manteniendo el resto en memoria secundaria.
Con esto se consigue un mayor aprovechamiento de la memoria, ya que los procesos grandes ya no necesitan estar completos en memoria y por tanto podrá haber más procesos simultáneamente. Otra ventaja es que es posible ejecutar programas cuyo tamaño exceda el de la memoria.
Hay dos técnicas principales de memoria virtual: la paginación y la segmentación, así como una combinación de ambas: la segmentación con paginación.
5.2.1.- Paginación
En este sistema la memoria se divide en particiones de tamaño fijo, denominadas marcos de página. Cuando un programa se ejecuta, el espacio de direcciones de memoria de ese proceso es dividido en fragmentos denominados páginas, cuyo tamaño es el mismo que el de los marcos de página.
Cuando el proceso comienza a ejecutarse, únicamente las páginas que requiera para la ejecución son cargadas en la memoria, cada una en un marco de página, manteniendo el resto en la memoria secundaria. Indudablemente, durante la ejecución puede que sea necesario algún dato que se encuentre en una página que no esté en memoria. En ese caso se produce lo que se denomina un fallo de página, y el sistema responderá buscando la página correspondiente en la memoria secundaria y cargándola en un marco de página que se encuentre libre en memoria.
Si no hubiera ningún marco de página libre se liberaría alguno según el algoritmo de reemplazo de páginas que utilice el sistema (XXXXXXXXXX).
5.2.2.- Segmentación
Los sistemas vistos hasta ahora tratan el espacio de direcciones de memoria de un proceso como unidimensional
 

