

8.- EL DIRECTORIO /PROC
Cada sistema operativo ofrece un mecanismo para que el administrador de sistemas investigue las interioridades del sistema operativo y para configurar los parámetros cuando lo necesite. En Linux este mecanismo es el sistema de archivos /proc.
El directorio /proc fue creado para mejorar la comunicación entre los usuarios y el kernel. Este sistema es en realidad interesante porque realmente no existe del todo en disco; es una abstracción de la información del kernel. Todos los archivos del directorio corresponden a una función o a un conjunto de variables del kernel.
Por ejemplo, para ver un informe sobre el tipo de procesador de sus servidores, podemos leer el archivo /proc/cpuinfo con el comando cat de la forma siguiente:
 
El kernel creará dinámicamente el informe mostrando la información del procesador y devolviéndole a cat para que podamos verlo.
Otra ventaja de este sistema es que el flujo de información no es unidireccional, sino que también sirve para enviar información al kernel. Por ejemplo, un vistazo al directorio /proc/sys/net/ipv4 nos mostrará un montón de ficheros con permiso de escritura.
La mayoría de los ficheros de este fichero contienen solamente un número, pero su modificación puede suponer cambios importantes en el comportamiento de la pila TCP/IP.
Por ejemplo, el archivo /proc/sys/net/ipv4/ip_forward contiene un 0 por defecto. Esto le dice a Linux que no realice IP Forwarding cuando tenemos varias interfaces en el sistema. Pero si queremos habilitar esta capacidad solo tenemos que modificar el valor de ese fichero por 1 con la orden:
echo “1” > /proc/sys/net/ipv4/ip_forward
Con esto conseguiremos que el servidor realice funciones de enrutamiento.
Veamos ahora algunas entradas interesantes de este directorio:
Nombre	Contenido
/proc/cpuinfo	Información sobre la CPU del sistema
/proc/interrupts	Uso de las IRQ en el sistema
/proc/meminfo	Uso de la memoria
/proc/swaps	Información sobre las particiones de intercambio
/proc/version	Número de versión actual del kernel, la máquina en que se compiló y la fecha y hora de compilación.
/proc/net/dev	Información sobre cada dispositivo de red (contador de paquetes, contador de errores, etc..)
/proc/net/sockstat	Estadísticas de utilización de sockets de red
/proc/sys/net/ipv4/
icmp_echo_ignore_broadcasts	Por defecto es 0. Esto permite que se devuelvan las peticiones ICMP a las direcciones de broadcast. Esto supone un peligro frente a los ataques de denegación de servicio.
/proc/sys/net/ipv4/ip_forward	Ya lo vimos antes, habilita el encaminamiento IP
/proc/sys/net/ipv4/syn_cookies	Por defecto es 0. Al cambiarlo a 1 se activa la protección del sistema frente a ataques SYN FLOOD.


