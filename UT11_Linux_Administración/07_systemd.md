

7.- ARRANQUE DEL SISTEMA CON SYSTEMD
Systemd es un bloque de construcción básico que gestiona el sistema de inicio de Linux y que también permite gestionar todos los servicios que se están ejecutando en el sistema. El objetivo de systemd es reemplazar el sistema de inicio init de los antiguos sistemas Linux. 
El principal cometido de un sistema de inicio es inicializar los componentes que deben arrancarse una vez que el kernel de Linux haya sido iniciado. El sistema de inicio también es utilizado para administrar los servicios y demonios que hay en el sistema en cualquier momento mientras el sistema esté en ejecución.
En systemd, el objetivo de la mayoría de las acciones son las unidades (units), que son recursos que systemd sabe cómo administrar. Las unidades se categorizan por el tipo de recurso que representan y son definidos mediante ficheros conocidos como ficheros de unidad (unit files). El tipo de cada unidad puede ser deducido del sufijo del nombre del fichero.
Para las tareas de administración de servicios, la unidad objetivo tiene que ser una unidad de servicio (sufijo service). Sin embargo, es posible omitir el sufijo al invocar la orden.
7.1.- INICIANDO Y PARANDO SERVICIOS
El comando para interactuar con los servicios es systemctl y su sintaxis es:
$ sudo systemctl orden unit_file
Las órdenes que se le pueden pasar son:
•	status: muestra información sobre el servicio (recuerda que se puede omitir el sufijo del servicio)
 
•	start/stop: arranca y para respectivamente el servicio.
•	restart: para y vuelve a arrancar el servicio.
•	reload: hay servicios que pueden recargar los ficheros de configuración sin necesidad de ser parados.
•	reload-or-restart: si no tienes muy claro si un servicio se puede recargar o hay que reiniciarlo se puede usar esta opción. Si el servicio lo permite recargará únicamente los ficheros de configuración, si no es posible, reiniciará el servicio.
•	enable/disable: los comandos anteriores son útiles para arrancar y parar servicios durante una sesión. Si se quiere que los servicios se lancen automáticamente al iniciar el sistema hay que habilitarlos.
Al habilitar un servicio se crea un enlace simbólico de la copia del sistema del fichero del servicio (normalmente en /lib/systemd/system o /etc/systemd/system) en la localización del disco donde systemd busca los ficheros en el arranque (habitualmente en /etc/systemd/system/some_target.target.wants)
 
•	is-active/is-enabled/is_failed: estas opciones permiten comprobar el estado de un servicio devolviendo un estado de salida acorde con el resultado de la comprobación.
7.2.- ANALIZANDO EL ESTADO DEL SISTEMA
Las órdenes anteriores permiten operar con servicios individuales, pero no son muy útiles para explorar el estado del sistema. Si queremos una visión más global, podemos usar las siguientes opciones de systemctl.
•	list-units: muestra un listado de todas las unidades activas.
 
La información que muestra es:
o	UNIT: el nombre de la unidad
o	LOAD: si la configuración de la unidad está cargada en memoria
o	ACTIVE: si está activa o no
o	SUB: este campo muestra información más precisa sobre el estado de la unidad, sus valores varían en función del tipo de unidad.
o	DESCRIPTION: una breve descripción de la unidad.
Algunos modificadores que admite son:
o	--all: muestra también las unidades que no están activas.
o	--state=: muestra las unidades con un determinado estado. Por ejemplo, --state=inactive.
o	--type=: también se puede filtrar por tipo. Por ejemplo, --type=service.
•	list-unit-files: la orden anterior solo muestra las unidades cargadas en memoria. Se pueden mostrar los ficheros de las unidades para poder ver todas las disponibles.
 
Las unidades marcadas como estáticas son unidades que no pueden ser habilitadas, es decir, que no es posible añadirlas para que se ejecuten automáticamente al inicio.
7.3.- AJUSTANDO EL ESTADO DEL SISTEMA CON TARGETS
Los targets son ficheros de unidad especiales que describen un estado del sistema o un punto de sincronización:
Incompleto. Más documentación en:
•	https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units
•	https://www.digitalocean.com/community/tutorials/systemd-essentials-working-with-services-units-and-the-journal#basic-unit-management
•	https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
•	https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs

 
