![Carátula UT05](imgs/caratula_ut05.png)

# UT05. WINDOWS 10. ADMINISTRACIÓN

### Contenidos

1. Gestión de procesos
2. El administrador de tareas
3. Servicios
4. El monitor de recursos
5. El visor de eventos
6. Gestión de energía
7. Directivas de grupo local
8. Programador de tareas


## 2.- EL ADMINISTRADOR DE TAREAS

El **Administrador de tareas** es una herramienta de supervisión que permite controlar la actividad del sistema en tiempo real y visualizar la información del estado del procesador, de la memoria de las aplicaciones y de los procesos.

Hay múltiples forma de llegar al *Administrador de tareas*:

- Desde el menú contextual de la barra de tareas
- Pulsando `Ctrl+Mayus+Esc`
- Pulsando `Ctrl+Alt+Supr` y seleccionando *Administrador de tareas*
- Ejecutando el comando `taskmgr.exe`.

La primera vez que se accede al *Administrador de tareas* se ve una ventana que apenas muestra información, pero si pulsamos en *Más detalles* pasará a otro modo en el que tenemos gran candidad de información repartida en siete pestañas.

### 2.1.- Pestaña Procesos

![Pestaña Procesos del Administrador de tareas1](imgs/taskmgr_procesos.png)

Esta primera pestaña muestra información sobre todos los procesos que se están ejecutando en el sistema. Los datos relativos a cada proceso se encuentran en una serie de columnas que se pueden personalizar haciendo click derecho sobre el encabezado de una de ellas y seleccionando cuáles queremos mostrar.

Los campos más relevantes son:

- **CPU, Memoria y Disco**: muestra el porcentaje de uso de la CPU, la cantidad de memoria RAM que ocupa el mismo y su tasa de utilización del disco duro. Estos tres componentes (CPU, RAM y disco) son los principales cuellos de botella en el ordenador, por lo que en situaciones en que haya una bajada brusca del rendimiento del sistema debemos acudir aquí para localizar el proceso que tiene un consumo excesivo de alguno de estos recursos.
- **Estado**: muestra el estado del proceso, aunque únicamente muestra un valor cuando el proceso está *suspendido*. En las últimas versiones de Windows 10 muestra esta situación mediante el icono de una hoja.
- **PID**: cada proceso en Windows tiene asociado un identificador único denominado **PID** (**Process IDentifier**) y esto es lo que se muestra en esta columna. Cada vez que ejecutamos un programa y se genera el proceso correspondiente el sistema le otorgará un PID único. Por ello, podemos tener diferentes instancias de un mismo programa con diferentes PID, ya que son procesos diferentes.
- **Consumo de energía**: da una medida del impacto energético que tiene la ejecución de cada proceso en el sistema.
- **Línea de comandos**: la ruta del programa que se ejecutó para crear este proceso y los parámetros de ejecución de dicho programa. Puede ser útil en el caso de procesos sospechosos para identificar el origen de los mismos.

Además, en la parte inferior derecha se encuentra el botón **Finalizar tarea**, que envía una señal de apagado al proceso que tengamos seleccionado, interesantes en situaciones en la que no podamos cerrar algún programa, por ejemplo, porque se haya colgado.


### 2.2.- Pestaña Rendimiento

![Pestaña Rendimiento del Administrador de tareas](imgs/taskmgr_rendimiento.png)

Esta pestaña muestra información visualmente sobre el consumo de recursos en el sistema a lo largo del tiempo, siendo estos recursos la CPU, la memoria RAM, las unidades de almacenamiento, los adaptadores de red y la tarjeta gráfica. Se puede ver información más específica de cada uno de los recursos seleccionándolo en la parte izquierda.

La información que podemos encontrar en cada apartado es:

- **CPU**: en la gráfica vemos el porcentaje de uso de la CPU. Además, se muestra la siguiente información:
  - Modelo de procesador y frecuencia del mismo
  - Uso y Velocidad: el porcentaje en forma numérica del uso del procesador por parte de los procesos y la frecuencia


***
[Volver al índice principal](index_UT05.md)