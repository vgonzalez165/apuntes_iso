![Carátula UT02](imgs/caratula_ut02.png)

## Contenidos

1. [Conceptos básicos de virtualización](01_conceptos_básicos.md)
2. [Tipos de máquinas virtuales](02_tipos_MV.md)
3. [Tipos de hipervisores](03_tipos_hipervisores.md)
4. [Oracle VirtualBox](04_virtualbox.md)
5. [Microsoft Hyper-V](05_hiper-v.md)

# 5. MICROSOFT HYPER-V


## 5.1.- Habilitar Hyper-V en Windows 10

Hyper-V es una herramienta que inicialmente estaba orientada a las versiones de servidor de Windows, pero recientemente ha sido incluido como una característica en Windows 10. Los requisitos que debe tener el ordenador son:

- Windows 10 Enterprise, Pro o Education
- Procesador de 64 bits con traducción de direcciones de segundo nivel (SLAT).
- Compatibilidad de CPU con la extensión del modo monitor de la máquina virtual (VT-c en CPU de Intel).
- Mínimo de 4 GB de memoria.

La edición de Windows instalada se puede ver en *Panel de Control -> Sistema*, donde también se muestra si la arquitectura del equipo y el sistema son de 64 bits. Sin embargo, puede ser más complicado saber si el procesador dispone de SLAT y del juego de instrucciones de virtualización. Hay diversas formas de verificarlo, pero lo más sencillo es recurrir al conjunto de herramientas que Microsoft denomina [SysInternals](https://docs.microsoft.com/en-us/sysinternals/), que son varias utilidades en línea de comandos que realizan tareas muy concretas sobre el sistema operativo. 

La que interesa en este caso es [**CoreInfo**](https://docs.microsoft.com/en-us/sysinternals/downloads/coreinfo), que extrae información sobre las características del microprocesador. Esta herramienta debe ser ejecutada en la consola, y si lo hacemos con el parámetro `-v` mostrará únicamente las características del procesador relativas a la virtualización.

```powershell
PS C:\Users\Victor\Desktop> .\Coreinfo64.exe -v

Coreinfo v3.52 - Dump information on system CPU and memory topology
Copyright (C) 2008-2021 Mark Russinovich
Sysinternals - www.sysinternals.com


Note: Coreinfo must be executed on a system without a hypervisor running for
accurate results.

AMD Ryzen 5 2600 Six-Core Processor
AMD64 Family 23 Model 8 Stepping 2, AuthenticAMD
Microcode signature: 00000000
HYPERVISOR      *       Hypervisor is present
SVM             -       Supports AMD hardware-assisted virtualization
NP              -       Supports AMD nested page tables (SLAT)
```

**HACER CAPTURA CON EL PORTÁTIL, QUE ES INTEL**

Si se cumplen todas las condiciones será posible habilitar Hyper-V en el ordenador. Hyper-V es una **característica**, programas o utilidades incluidas en el sistema operativo pero que se pueden habilitar o deshabilitar a voluntad. Para ello, hay que ir a *Panel de Control -> Programas -> Activar o desactivar las características de Windows* y marcar la entrada que pone Hyper-V. Opcionalmente, se pueden instalar independientemente los diversos componentes de que dispone:

- **Herramientas de administración de Hyper-V**: tiene dos componentes, el primero es la interfaz gráfica para administrar Hyper-V mientras que el segundo es el módulo de Powershell que incluye todos los comandos necesarios para interactuar con las máquinas virtuales. Estos componentes se pueden instalar de forma aislada si, por ejemplo, se va a administrar de forma remota un hipervisor que se encuentra ubicado en otra máquina.
- **Plataforma de Hyper-V**: es el hipervisor propiamente dicho.

![Instalación de Hyper-V](imgs/hyperv_install.png)

La instalación de estas características requiere del reinicio del sistema ya que Hyper-V es un **hipervisor de tipo 1**. Esto quiere decir que no se instala sobre un sistema operativo, sino que el hipervisor es el propio sistema operativo. Lo que hace Windows cuando habilitamos Hyper-V es cargar el hipervisor y hacer que el sistema operativo que tenemos en nuestro ordenador pase a ejecutarse sobre una máquina virtual.

[Interfaz de Hyper-V](imgs/hyperv_interfaz.png)


## 5.2.- Creación de una máquina virtual

La creación de una máquina virtual es muy sencilla, únicamente hay que ir a *Nuevo -> Máquina virtual* y seguir los pasos del asistente. Algunos datos que se piden son:

- **Nombre**: Cada máquina virtual está identificada por un nombre. Es deseable que el nombre sea suficientemente descriptivo y que no sea muy complicado ya que, si administramos las máquinas virtuales desde Powershell, haremos referencia a ella por el nombre.
- **Generación**: La generación de la máquina virtual hace referencia al hardware virtual de la misma y determina algunas de sus funcionalidades. Lo más destacable probablemente es que las máquinas de generación 1 permiten instalar sistemas operativos tanto de 32 como de 64 bits, mientras que las máquinas de 64 bits únicamente soportan sistemas operativos de 64 bits. Si quieres un listado completo de las diferencias entre ambas generaciones puedes consultarlas [aquí](https://docs.microsoft.com/es-es/windows-server/virtualization/hyper-v/Hyper-V-feature-compatibility-by-generation-and-guest).
- **Memoria RAM**: La cantidad de memoria RAM que se asignará a la máquina virtual y que por supuesto, tomará de la RAM de la máquina cuando la máquina virtual esté en ejecución.
- **Funciones de red**: Aquí se escoge el adaptador de red que tendrá la máquina. Más adelante se verá en detalle el funcionamiento de los adaptadores de red.
- **Disco duro virtual**: De forma análoga a como se hacía con VirtualBox, se puede crear un disco duro, reutilizar uno existente o no conectar ningún disco. Si vamos a reutilizar un disco existente, el formato nativo de discos virtuales de Hyper-V es `.vhd` y `.vhdx`.
- **Disco de arranque**: Donde se podrá seleccionar una imagen ISO desde la que instalar el sistema operativo.

Y con estas opciones ya estaría creada la máquina virtual. Ahora ya se puede arrancar, pero antes hay que diferenciar entre dos términos u operaciones diferentes que se pueden realizar con la máquina:

- **Iniciar**: Esta operación arranca la máquina virtual, pero no muestra una interfaz para interactuar con ella. La máquina se ejecuta haciendo lo que tenga que hacer, pero nosotros no veremos una ventana que nos la muestre.
- **Conectar**: Cuando hacemos click en *conectar* se abre una ventana que nos muestra la máquina virtual. Esto no implica la ejecución de la máquina, que no se producirá hasta que no pulsemos en *iniciar*.
- **Desconectar**: Esta acción cierra la ventana de la máquina virtual, pero si esta se encontrara en ejecución no la detendrá, sino que se mantendrá ejecutándose en segundo plano.
- **Apagar**: Envía una señal de apagado a la máquina virtual.
- 

## 5.4.- Cambios en la configuración de una máquina virtual

Tras crear la máquina virtual se puede afirnar su configuración haciendo click derecho sobre ella y seleccionando *Configuración*. Las opciones más relevantes son:

- **BIOS**: aquí se indica en qué orden se buscarán dispositivos arrancables cuando iniciamos la máquina.
- **Memoria**: aquí es posible modificar el valor de memoria RAM de la máquina virtual. Aquí también se puede configurar la **memoria dinámica**. Si está habilitada se pueden indicar los siguientes valores:
  - *RAM mínima*: es la cantidad de RAM que tendrá inicialmente la máquina y el valor al que tenderá.
  - *RAM máxima*: a medida que la MV vaya requiriendo RAM, esta irá creciendo hasta el valor que hayamos puesto en este campo.
  - *Ponderación de memoria*: si hay varias máquinas en ejecución y todas demandan RAM, será este valor el que determinará cuáles tienen prioridad sobre otras.
- **Procesador**: aquí lo más relevante es el número de procesadores virtuales que se pueden asignar a la máquina virtual.
- **Controladora**: las controladores (IDE o SCSI), son el punto de conexión de los discos duros virtuales, así como de las unidades ópticas. Las controladoras IDE únicamente pueden tener conectados dos dispositivos, mientras que las controladores SCSI permiten hasta 64.
- **Servicios de integración**: son diversos servicios que permiten de alguna manera la interacción entre el anfitrión y el invitado. Por ejemplo, el *latido* sirve para permitir que el anfitrión sepa si la máquina virtual está funcionando o no. En la web de Microsoft hay información completa sobre todos los [servicios de integración](https://docs.microsoft.com/es-es/virtualization/hyper-v-on-windows/reference/integration-services).
- **Puntos de control**:


## 5.5.- Gestión de la red en Hyper-V



*** 

[Volver al índice](index_UT02.md)