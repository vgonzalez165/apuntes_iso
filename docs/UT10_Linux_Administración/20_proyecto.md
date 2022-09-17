# Proyecto EV3. Configuración de un servidor Linux

El objetivo final de este proyecto será preparar un servidor Ubuntu Linux y configurarlo según las instrucciones que se indican a continuación.

## 1. Máquina de partida (3 puntos)

Para evitar el tedioso paso de clonar máquinas virtuales y realizar las configuraciones iniciales, la máquina de partida será el servidor que creaste en la práctica PR1101 de esta misma unidad. Recuerda que esta práctica tiene que tener las siguientes características:

- Tres adaptadores de red: dos conectados a dos conmutadores virtuales en modo interno y otro conectado al *default switch* de Hyper-V.
- El rango de direcciones IP que tienes asignado es 172.20.X.0/24 donde X corresponde a tu número de equipo en el aula. De esta red tienes que extraer **dos subredes**, una para cada uno de los conmutadores en modo interno a que está conectado.
- El servidor funcionará como **enrutador**.

## 2. Creación de usuarios (2 puntos)

Vamos a suponer que este servidor va a ser utilizado por todos tus compañeros de clase, por lo que tendrás que crear cuentas para cada uno de ellos. Tienes que tener en cuenta lo siguiente:

- Tienes que crear cuentas para por lo menos **cinco** de tus compañeros.
- El nombre de cada cuenta será la primera letra del nombre seguida del primer apellido (por ejemplo, `vgonzalez`).
- La contraseña en todos los casos será `paso`
- Además, crearás un grupo llamado `iso` al que pertenecerán todos los usuarios que has creado.

## 3. Carpeta compartida (2 puntos)

En este servidor vamos a añadir un segundo disco duro de 50 gigas, teniendo en cuenta lo siguiente:

- El disco estará formateado en `ext4` y se montará de forma permanente en la carpeta `/iso`.
- Esta carpeta será accesible para todos los usuarios que has creado con permisos de lectura y escritura. Estos mismos permisos también los tendrá el usuario `root`.
- Ningún otro usuario del sistema tendrá acceso a la carpeta.


## 4. Carpetas personales

Ahora vamos a asignar a cada uno de los usuarios una carpeta de almacenamiento fuera de su directorio personal. Para ello tienes que añadir otros disco duro más a la máquina virtual con una capacidad de 500GB. En este disco:

- Crea una partición que ocupe todo el disco y aplica formato `ext4`.
- Monta permanentemente el disco en el directorio `/datos`
- Dentro de `/datos` crea un subdirectorio para cada uno de los usuarios.
- Asigna permisos a esos subdirectorios para que solo el usuario correspondiente pueda acceder a cada uno.
- Crea un **enlace simbólico** en el directorio personal de cada usuario que le lleve a su directorio personal. Puedes crear un enlace simbólico con la orden `ln -s /mnt/datos datos`


## 5. Conexión SSH (2 puntos)

Configura la conexión SSH para todos los usuarios que has creado. Para ello necesitarás que tus compañeros te faciliten sus claves de forma que puedas configurar la conexión transparente sin contraseña. Aunque no es algo que se haría en la vida real, será necesario que también te faciliten la clave privada para que puedas comprobar que funciona.

Aunque puedes intercambiar las claves de la forma que quieras, una buena opción sería que uno cree un repositorio en Github al que pertenezcáis todos y que luego cada uno añada sus claves.


# Criterios de calificación

El proyecto se corregirá directamente sobre el equipo del alumno, en concreto sobre las máquinas virtuales correspondientes al servidor y un cliente. Solo se valorarán los criterios conseguidos en su totalidad, los criterios no conseguidos o con una consecución parcial tendrán una valoración de 0 puntos.

| Criterio                                                                                      | Valor | Obtenido | Nota |
| --------------------------------------------------------------------------------------------- | ----- | -------- | ---- |
| Se ha configurado correctamente la red                                                        |  1.0  |          |      |
| El servidor enruta los paquetes de los clientes                                               |  1.0  |          |      |
| Los clientes tienen resolución de nombres                                                     |  0.5  |          |      |
| Se han creado las cuentas de usuario                                                          |  0.5  |          |      |
| Se ha creado el grupo y añadido los usuarios                                                  |  0.5  |          |      |
| Se ha montado el disco de 50GB en la carpeta `/iso` mediante el fichero `fstab`               |  0.5  |          |      |
| Únicamente los usuarios creados y `root` tienen permisos de lectura escritura sobre `/iso`    |  1.5  |          |      |
| Se ha preparado y montado el segundo disco en `/datos`                                        |  0.5  |          |      |
| Se han creado los subdirectorios de `/datos` con los permisos correctos                       |  1.5  |          |      |
| Se han creado los enlaces simbólicos a las carpetas personales de los usuarios                |  0.5  |          |      |
| Se ha configurado la conexión SSH mediante el par de claves pública-privada a los usuarios    |  2.0  |          |      |

