5.- GESTIÓN DE USUARIOS EN LINUX
5.1.- LA SEGURIDAD EN LINUX
El elemento clave para gestionar la seguridad en Linux es la cuenta de usuario. Cada persona que acceda al sistema ha de hacerlo a través de la cuenta de usuario que tenga asignada. Los permisos que tenga entonces serán los mismos que tenga la cuenta con la que se loguee.
De forma similar a Windows, donde cada usuario tiene asignado un SID único, en Linux cada usuario tiene asignado un identificador de usuario, denominado UID único. Sin embargo, el usuario no utiliza este UID para identificarse, sino que utiliza su nombre de usuario o de login, una cadena alfanumérica más comprensible para el usuario.	
Para gestionar la seguridad un sistema Linux utiliza una serie de ficheros y utilidades que veremos a continuación.
5.1.1.- EL FICHERO /ETC/PASSWD
Los sistemas Linux utilizan un fichero especial denominado /etc/passwd para almacenar las correspondencias entre los nombres de usuario y sus correspondientes UIDs. 
 
Un usuario especial que está en todos los sistemas es el usuario root, el cual siempre tiene asignado el UID 0. Además, los sistemas Linux tienen un gran número de usuarios que son utilizados internamente por el sistema denominados cuentas del sistema. Una cuenta del sistema es una cuenta que utilizan determinados servicios que se están ejecutando en el sistema para obtener acceso a los recursos que necesiten. Todos los servicios que se ejecutan en segundo plano tienen que obtener acceso al sistema mediante una cuenta del sistema. Linux reserva los UIDs por debajo de 500 para las cuentas del sistema.
Aparte del nombre de usuario y el UID el fichero passwd muestra mucha más información:
•	El nombre de usuario
•	La contraseña del usuario
•	El UID del usuario
•	El identificador del grupo del usuario (GID)
•	Una descripción de la cuenta del usuario
•	La localización del directorio HOME del usuario
•	El shell por defecto del usuario
5.1.2.- EL FICHERO /ETC/SHADOW
Aunque el campo de contraseña se utilizaba anteriormente para almacenar la función hash de la contraseña del usuario, esto suponía un problema de seguridad porque muchos programas y usuarios debían acceder al directorio /etc/passwd por diversos motivos. Por ello se quitó la contraseña del usuario de este fichero (por ello en todos los usuarios hay un x en su lugar) y ahora se almacenan en un fichero diferente situado en /etc/shadow. El acceso a este fichero está restringido a unos pocos programas, como por ejemplo el de login.
El fichero /etc/shadow solo puede ser accedido por el usuario root y contiene un registro por cada usuario del sistema de la siguiente forma:
 
Los nueve campos de cada fila son:
•	El nombre de login del usuario
•	La función hash de la contraseña
•	El número de días transcurridos desde el 1 de enero de 1970 hasta que la contraseña fue cambiada por última vez
•	El número mínimo de días hasta que la contraseña pueda ser cambiada
•	El número mínimo de días antes de que la contraseña deba ser cambiada
•	El número de días antes de la expiración de la contraseña en los que se avisará al usuario
•	El número de días desde que la contraseña expire hasta que la cuenta sea deshabilitada
•	La fecha (indicada como número de días desde el 01-01-1970) desde que la cuenta de usuario fue deshabilitada
•	Campo reservado

5.2.- AÑADIR UN NUEVO USUARIO
El comando para añadir nuevos usuarios en Linux es useradd. Este comando proporciona un medio sencillo para crear una nueva cuenta de usuario y establecer el directorio HOME del usuario a la vez. El comando useradd utiliza una combinación de valores predefinidos por el sistema y parámetros en la línea de comandos para definir una cuenta de usuario. Para ver los valores predefinidos por el sistema hay invocar el comando con el parámetro –D.
Este comando nos mostrará:
•	El grupo por defecto al que pertenecerá el usuario
•	El directorio donde se creará el directorio home del usuario
•	Los días tras los que se deshabilitará la cuenta desde que la contraseña expire
•	Si la cuenta expirará
•	El shell por defecto que utilizará la nueva cuenta
•	El directorio a partir del cual se construirá el directorio raíz del usuario suele ser /etc/skel
•	Fichero donde el usuario recibirá el correo
Si se desea cambiar alguno de los parámetros por defecto al crear un nuevo usuario se puede hacer mediante la utilización del parámetro –D junto con otro parámetro que representará el valor que cambiará.
5.3.- ELIMINACIÓN DE UN USUARIO
Para eliminar un usuario del sistema el comando que hay que utilizar es userdel. Por defecto, este comando solo elimina la información del usuario del fichero /etc/passwd. No elimina ningún fichero de la cuenta.
Si se utiliza userdel con el parámetro –r también eliminará el directorio HOME del usuario, junto con el directorio de correo del usuario.
5.4.- MODIFICANDO UN USUARIO
Linux proporciona unas pocas utilidades para modificar la información para cuentas de usuario existentes.
5.4.1.- USERMOD
	El comando usermod proporciona opciones para modificar la mayoría de los campos del fichero /etc/passwd. Para hacer esto solo es necesario utilizarlo con el parámetro correspondiente al campo que se quiere modificar. Algunos de estos parámetros son:
•	-c para cambiar el campo de comentario
•	-e para cambiar la fecha de expiración
•	-g para cambiar el grupo de login por defecto
•	-l para cambiar el nombre de login del usuario
•	-p para cambiar la contraseña del usuario
•	-L para bloquear la cuenta
•	-U para desbloquear la cuenta.
5.4.2.- PASSWD Y CHPASSWD
La forma más rápida de cambiar la contraseña de un usuario es mediante el comando passwd. Si no se le indican parámetros modificará la contraseña del usuario actual. Si se le indica un nombre de usuario cambiará la contraseña de dicho usuario. Únicamente el usuario root puede cambiar la contraseña del resto de usuarios.
El parámetro –e forzará al usuario a modificar su contraseña la próxima vez que se inicie sesión en el equipo. Es útil para dar una contraseña sencilla a los usuarios cuando se creen sus cuentas y luego obligarles a modificarla.
Si necesitas modificar la contraseña de un gran número de usuario la mejor opción es el comando chpasswd. Este comando lee una lista de pares nombre de usuario – contraseña (separados por una coma) de la entrada estándar y automática asigna cada contraseña a cada usuario.
5.5.- GRUPOS EN LINUX
Las cuentas de usuario son útiles para controlar la seguridad de usuarios individuales, pero no son muy útiles a la hora de permitir a grupos de usuarios compartir recursos. Para esta labor Linux dispone del concepto de grupos.
Cada grupo tiene su propio GID, que, al igual que las UIDs es un valor único en el sistema.
La información relativa a los grupos de usuarios se almacena en el fichero /etc/group. La información de este fichero es:
•	El nombre del grupo
•	La contraseña del grupo sirve para que los usuarios que no pertenecen a este grupo puedan ser miembros del mismo temporalmente.
•	El GID del grupo. Los grupos utilizados por cuentas del sistema tienen GIDs por debajo del número 500, mientras que los grupos de usuarios tienen el GID por encima de este valor.
•	Lista de las cuentas de usuario que pertenecen a este grupo.
El comando para añadir un nuevo grupo al sistema es groupadd. Cuando se crea un grupo nuevo no se le añaden usuarios por defecto y dicho comando no permite hacer esto. Para ello debemos utilizar el comando usermod.
 
El comando usermod también permite cambiar otra información de los grupos. El parámetro –g permitirá modificar el GID del grupo mientras que el parámetro –n permite cambiar el nombre del grupo.
5.6.- PERMISOS
Como se ha visto anteriormente, al ejecutar el comando ls con el modificador –l se nos muestra una serie de caracteres al principio de cada línea. El primero de estos caracteres define el tipo del objeto.
•	- para ficheros
•	d para directorios
•	l para enlaces
•	c para dispositivos de caracteres
•	b para dispositivos de bloque
•	n para dispositivos de red
A continuación, hay tres conjuntos de tres caracteres. Cada conjunto de caracteres define tres tipos de permisos:
•	r para el permiso de lectura del objeto
•	w para el permiso de escritura del objeto
•	x para el permiso de ejecución del objeto
Si un permiso está negado se mostrará con el símbolo guion en su posición. Los tres conjuntos de caracteres definen los tres niveles de seguridad del objeto:
•	El propietario del objeto
•	El grupo al que pertenece el objeto
•	El resto de los usuarios del sistema

 
Cuando creamos un fichero nuevo este tiene unos permisos por defectos predefinidos. Estos permisos predefinidos vienen dados por el comando umask, el cual establece los permisos por defecto para cualquier fichero o directorio que crees.	
5.6.1.- EL COMANDO UMASK
El comando umask nos servirá para modificar los permisos por defecto que se asignarán a cada nuevo fichero o directorio creado en el sistema. Si lo ejecutamos veremos que nos muestra por pantalla 4 dígitos octales que representan los permisos por defecto. El primero corresponde al denomina sticky bit. Los otros tres son los permisos por defecto para el usuario que crea el objeto, el grupo del usuario y el resto de los usuarios respectivamente en la representación octal.
Sin embargo, el comando umask no muestra los permisos que se asignan tal cual, sino que lo hace mediante una máscara, es decir, los permisos indicados por este comando serán los que se restarán de los permisos máximos que puede tener el objeto. Los permisos máximos de un objeto serán:
•	Para los ficheros los permisos 666 (lectura y escritura para todos)
•	Para los directorios los permisos 777 (lectura, escritura y ejecución, es decir, recorrido, para todos)
Esto quiere decir que si tenemos una máscara definida como 022 los permisos efectivos que se aplicarán a un fichero serán los resultantes de quitar los permisos de la máscara (022) a los permisos máximos (666), quedando unos permisos efectivos de 644 (lectura/escritura para el usuario y lectura para el resto).	
5.6.2.- MODIFICANDO PERMISOS
El comando chmod es el que nos permitirá modificar los permisos asociados a un fichero o directorio. El formato de este comando es:
chmod opciones modo fichero
El parámetro modo permite establecer los permisos utilizando una notación simbólica u octal. La notación octal consiste en asignar a cada grupo de permisos un dígito octal (3 bits). Cada bit de ese dígito corresponderá a cada uno de los permisos diferentes disponibles. Por ejemplo, el permiso de lectura y escritura (rw-) correspondería con la secuencia de bits 110, cuyo valor en octal es el dígito 6.
 
Si queremos utilizar la habitual secuencia de caracteres (rwx), el comando chmod toma una aproximación diferente. El formato para especificar un permiso en el modo simbólico es:
 
El primer grupo de caracteres define a quien se le aplicarán los nuevos permisos:
•	u para el usuario
•	g para el grupo
•	o para otros
•	a para todos los anteriores
El siguiente carácter se utiliza para indicar si quieres añadir el permiso a los existentes (+), quitar el permiso de los existentes (-), o establecer el permiso a un valor (=).
Finalmente, el último símbolo es el permiso que se va a modificar. Hay muchas opciones, pero las más comunes son:
•	r para permisos de lectura
•	w para permiso de escritura
•	x para permiso de ejecución
•	s para establecer el SUID o el SGID
5.6.3.- CAMBIO DE PROPIETARIO
Para cambiar el propietario de un fichero Linux dispone de dos comandos. El comando chown para cambiar el usuario propietario y el comando chgrp para cambiar el grupo propietario del objeto.
El formato de chown es:
 
Se puede especificar tanto el nombre de usuario como su UID para referenciar al usuario. Además, este comando también nos permite modificar el grupo propietario.
 
Incluso yendo más lejos se podría modificar únicamente el grupo propietario del objeto aplicando el comando de la siguiente forma:
 
El funcionamiento del comando chgrp es muy similar al anterior, con la única excepción de que el valor modificado es el grupo propietario y no el usuario propietario.

5.7.- COMPARTICIÓN DE FICHEROS 
El mecanismo que hemos visto para compartir fichero en Linux hasta ahora se basa en el establecimiento de permisos para usuarios y grupos. Así si queremos que un determinado usuario pueda acceder a un objeto podemos incluirlo en el grupo propietario de ese objeto y así obtener los permisos que tenga ese grupo.
Sin embargo, en entornos con múltiples usuarios este proceso puede ser complicado y nada cómodo de utilizar. Para ello Linux dispone de una solución muy simple que viene ligada a 3 bits adicionales que almacena para cada fichero y directorio.
•	El set user id (SUID): cuando un fichero es ejecutado por un usuario, el programa se ejecuta bajo los permisos del propietario.
•	El set group id (SGID): en fichero el programa se ejecuta utilizando los permisos del grupo propietario del fichero. Para directorios, los nuevos ficheros creados bajo ese directorio utilizan el grupo del directorio como grupo por defecto.
•	El sticky bit: el fichero permanece en memoria una vez que el proceso ha finalizado.
El bit SGID es importante para compartir ficheros. Habilitando el bit SGID, puedes forzar que todos los ficheros creados en un directorio compartido pertenezcan al grupo del directorio y no al grupo del usuario individual que lo ha creado.
El bit SGID se puede establecer con el comando chmod. Se puede añadir anteponiéndolo al valor de 3 dígitos octales de los permisos (quedando en ese caso 4 dígitos), o se puede utilizarse la notación simbólica.
 
Así, para crear un directorio compartido que mantenga la propiedad de los ficheros creados dentro de él para el grupo propietario del directorio podríamos hacer lo siguiente:
 


 
