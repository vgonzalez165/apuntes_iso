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
- Contendrá un subdirectorio para cada uno de los usuarios que has creado y en el cual el usuario correspondiente podrá leer y escribir.
- Además, habrá otro subdirectorio llamado `general` en el que todos los usuarios que has creado tendrán permisos de lectura y escritura.

## 4. Conexión SSH (2 puntos)


## 5. 