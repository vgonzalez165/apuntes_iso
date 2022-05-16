<link rel="stylesheet" href="../styles.css">

![Carátula UT11](imgs/caratula_ut11.png)

### Contenidos

1. [Gestión del software en Linux](01_gestion_software.md)
2. [Configuración de red en Ubuntu Server ](02_configuracion_red.md)
3. [Conexión remota al servidor mediante SSH](03_ssh.md)
4. [Administración del almacenamiento](04_almacenamiento.md)
5. [Gestión de usuarios en Linux](05_usuarios.md)
6. [Gestión de procesos](06_procesos.md)
7. [Arranque del sistema con `systemd`](07_systemd.md)
8. [El directorio `/proc`](08_directorio_proc.md)


# 3.- CONEXIÓN REMOTA AL SERVIDOR MEDIANTE SSH

## 3.1.- Secure Shell (SSH)

**SSH (Secure Shell)** es un protocolo que facilita las comunicaciones seguras entre dos sistemas utilizando una arquitectura cliente/servidor y que permite a los usuarios conectarse a un host remotamente. Su diferencia con respecto a otros protocolos, como Telnet o FTP, es que encripta la sesión de conexión, haciendo imposible que alguien pueda obtener contraseñas. Por defecto utiliza el **puerto 22**.

### 3.1.1.- Conexión mediante SSH

SSH es un protocolo cuya función principal es el acceso remoto a un servidor por medio de un canal seguro en el que toda la información está encriptada. Algunas de las posibilidades de SSH son:

- Acceso a una terminal cifrada en otros equipos.
- Copiar datos de forma segura (tanto archivos sueltos como simular sesiones FTP cifradas)
- Gestionar claves RSA para no tener que escribir contraseñas al conectar a los dispositivos
- Pasar los datos de cualquier otra aplicación por un canal seguro tunelizado mediante SSH
- Redirigir el tráfico del sistema X Window para poder ejecutar programas gráficos remotamente.


### 3.1.2.- Preparación del servidor SSH

Para que un equipo acepte conexiones SSH debe tener instalado el servicio, por lo que el primer paso será instalar el paquete `openssh-server`. 

Los ficheros de configuración SSH se encuentran todos en el directorio `/etc/ssh`.
 
```bash
    victor@DESKTOP-483UVTV:~$ ls /etc/ssh
    moduli  ssh_config  ssh_config.d  ssh_import_id  sshd_config  sshd_config.d
```

El fichero que necesitamos para configurar el servidor es `sshd_config`. Ten cuidado no lo confundas con el fichero `ssh_config`, de nombre muy similar pero que sirve para configurar el cliente.

Por seguridad, es buena idea crear una copia del fichero de configuración antes de modificarlo para poder revertir los cambios en caso de realizar una configuración incorrecta.

Como tantos otros ficheros de configuración en Linux, dispone de un gran número de opciones la mayoría de las cuales están por defecto comentadas para que no se apliquen. Algunas de estas opciones que nos pueden interesar son:

- `Port`: un valor diferente a 22 cambiará el puerto por el que escucha el servidor.
- `ListenAddress`: si el equipo tiene varias interfaces de red, se puede especificar exactamente por cuáles responderá a las peticiones SSH entrantes. Si se va a indicar más de una dirección se indicará una en cada línea.
- `LoginGraceTime`: el servidor desconectará después del tiempo indicado si el usuario no ha iniciado sesión con éxito. Si el valor es 0 no tendrá límite.
- `PermitRootLogin`: si el valor está establecido en no, el usuario root no podrá acceder de forma remota.
- `PasswordAuthentication`: especifica si está permitida la autenticación por contraseña.
- `PermitEmptyPasswords`: si está habilitada la autenticación por contraseña, indica si el servidor permite iniciar sesión a cuentas sin contraseña. El valor por defecto es no.
- `AllowUser`: toma como valor una lista de nombres de usuario, permitiendo el inicio de sesión únicamente a los usuarios cuyo nombre esté en dicha lista. Es posible indicar el usuario de la forma USER@HOST para permitir a usuarios de determinados equipos.
- `DenyUsesr`: de forma análoga al anterior, contiene una lista de usuarios a los que no se les permitirá iniciar sesión remota.
- `Banner`: toma como valor el nombre de un fichero (por defecto /etc/issue.net). El contenido de este fichero es mostrado en el equipo que se intenta conectar antes de la autenticación
  
Recuerda que, como siempre que cambiamos un fichero de configuración, debemos reiniciar el servicio para que se apliquen los cambios.


### 3.1.3.- Preparación del cliente

En la mayoría de las distribuciones de Linux ya está instalado el cliente. El comando se llama `ssh` y espera como parámetros el nombre de usuario, la IP del equipo remoto y opcionalmente el puerto (modificador `-p`)
 
### 3.1.4.- Acceso SSH con clave pública

El problema del proceso anterior es que **solicita la contraseña** cada vez que conectamos. Este proceso se puede simplificar si configuramos el acceso SSH con clave pública.

Este proceso comprende tres pasos:

- Generación de un par de claves pública/privada en el cliente
- Envío de la clave pública al servidor
- Deshabilitar el acceso al servidor con contraseña
- 
El primer paso es **generar un par de claves** para lo que utilizaremos el comando `ssh-keygen`. A este comando se le puede pasar el parámetro `-b` para indicar el tamaño en bit de la clave que queremos generar.

```
vgonzalez@ubuntu:~$ ssh-keygen -b 1024
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vgonzalez/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/vgonzalez/.ssh/id_rsa
Your public key has been saved in /home/vgonzalez/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:B1XXDy9v1vmrTkqm3//oeABHzMKotQi3EJsE13k+6g0 vgonzalez@ubuntu
The key's randomart image is:
+---[RSA 1024]----+
|  ..+o . o.+. .. |
|   ooo+ +.o +.. .|
|    o+ B.. o   o.|
|      + +.. . . o|
|       .S..o   oo|
|      E  .  .  .=|
|     . o   o o o.|
|      . . + +....|
|         ..oo*=o+|
+----[SHA256]-----+
```

Por defecto, las claves se guardan en el directorio `~/.ssh`. En este directorio verás dos ficheros, uno que contiene la **clave pública** (`id_rsa.pub`) y el otro con la **clave privada** (`id_rsa`). 

Podemos ver el contenido de ambos ficheros ya que la clave, que es un valor binario, está protegida por una **armadura ASCII** para que pueda mostrarse en modo texto.

```
vgonzalez@ubuntu:~$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQDeHkF6GFQyMlK44ReOjzfTXS68lHfCEkBtmBYKtezgzrJ7dGHTMJ5rrpcMm/za+f2Kvz4VE0m9KpfKKcYhvxLa3P5N2pWAUMUIfPIHayG1ueQgD8P7GJFgq2Q7Bf5vGYW9rGpIn45koGWMnyqc99jMH31229aQ30pHGAtJpaJMsw== vgonzalez@ubuntu
```

El segundo paso que hay que realizar es **enviar la clave pública al servidor**. Esto lo podemos hacer por cualquier medio (correo electrónico, una memoria USB o incluso tecleándolo), pero vamos a ver como enviarlo directamente de un equipo a otro utilizando el comando `scp`, el cual permite transferir ficheros entre dos equipos de la red.
 
```
SCP(1)                                                 BSD General Commands Manual                                                SCP(1)

NAME
     scp — OpenSSH secure file copy

SYNOPSIS
     scp [-346BCpqrTv] [-c cipher] [-F ssh_config] [-i identity_file] [-J destination] [-l limit] [-o ssh_option] [-P port] [-S program]
         source ... target

DESCRIPTION
     scp copies files between hosts on a network.  It uses ssh(1) for data transfer, and uses the same authentication and provides the
     same security as ssh(1).  scp will ask for passwords or passphrases if they are needed for authentication.

     The source and target may be specified as a local pathname, a remote host with optional path in the form [user@]host:[path], or a
     URI in the form scp://[user@]host[:port][/path].  Local file names can be made explicit using absolute or relative pathnames to
     avoid scp treating file names containing ‘:’ as host specifiers.
```

El comando `scp` utiliza el protocolo SSH para enviar los ficheros, por lo que es conveniente que hayamos comprobado que funciona con contraseña antes de enviar los ficheros. Se le deben indicar como parámetros el nombre del fichero que queremos transferir y el nombre del fichero de destino, pero con el siguiente formato: `USER@HOST:FILENAME`
 
TODO: Falta captura de la conexión

Una vez subidas vamos al servidor donde debemos incluirlas en el fichero `~/.ssh/authorized_keys`.  Lo normal es que no exista ni el fichero ni el directorio que lo contiene, por lo que deberemos crearlo.

TODO: Falta captura

Finalmente copiamos la clave y borramos el archivo copiado al servidor.

TODO: Falta captura
 
Para aumentar la seguridad podemos deshabilitar el acceso SSH con contraseña. Para ello editamos el fichero `/etc/ssh/sshd_config` y modificar la siguiente línea. 
 
TODO: Falta captura


## 3.2- Conexión gráfica con SSH

Otra posibilidad es realizar una conexión a un entorno gráfico utilizando un túnel SSH. Para conseguir esto debemos configurar primero el servidor para que tenga habilitado el *X11 Forwarding*, que permite enviar la interfaz gráfica a través de la red usando SSH.

Para ello, únicamente debemos asegurarnos de que el fichero de configuración del servidor tiene habilitada la opción X11Forwarding.

```bash
#GatewayPorts no
X11Forwarding yes
#X11DisplayOffset 10
```

 
### 3.2.1.- Conexión desde un cliente Linux

Lo más sencillo es configurar la conexión desde una máquina Linux, en este caso simplemente hay que utilizar el modificador -X al invocar el comando ssh desde el cliente.
 
Al ejecutar este comando nos mostrará la terminal del servidor, pero la diferencia es que si ejecutamos un programa del servidor que requiera entorno gráfico, lo lanzará en una ventana en la máquina cliente. Por ejemplo, si queremos acceder al sistema de ficheros en un Linux Mint podemos invocar el administrador de archivos Nemo.
 


3.2.2.- CONEXIÓN GRÁFICA DESDE UN CLIENTE WINDOWS
Si queremos conectarnos a un entorno gráfico desde un cliente Windows, debemos hacerlo con Putty, pero necesitamos un programa adicional, Xming, que puedes descargar de Sourceforge (https://sourceforge.net/projects/xming/).
Tras descargarlo e instalarlo, se puede configurar yendo a Inicio -> XLaunch.
Con esta aplicación podrás seleccionar cómo quieres que se muestren las ventanas, como se va a lanzar el programa o como ser va a gestionar el portapapeles.
Una vez instalado y configurado a nuestro gusto hay que utilizar Putty para iniciar la conexión SSH. Al igual que desde Linux teníamos que habilitar el IP Forwarding. En este caso hay que ir a Connection -> SSH -> X11 y marcar la casilla Enable IP Forwarding.
 
Tras hacerlo, establecer una conexión tal y como la hemos hecho otras veces. Al igual que cuando nos conectamos desde Linux, accederemos a una terminal en la que, si introducimos un comando que requiera entorno gráfico, abrirá una ventana en nuestra máquina Windows cliente.
3.3.- SSH CON CHROOTED JAIL (OPCIONAL)
Algo en lo que te habrás fijado al configurar el acceso SSH a un servidor para diferentes usuarios es que cualquier usuario que se conecte mediante SSH podrá ver todos los ficheros del sistema de ficheros, incluso los de otros usuarios. Indudablemente, este comportamiento no es deseable en entornos en que múltiples usuarios comparten el acceso a un mismo servidor, por ejemplo, en un servidor web.
La técnica que permite solventar este problema se denomina jaula chroot (chroot jail) y se basa en el comando chroot, el cual sirve para ejecutar un proceso bajo un directorio raíz simulado, de manera que dicho proceso no pueda acceder a ficheros fuera de ese directorio.
Veamos los pasos para conseguir esto .

3.3.1.- CREAR EL SSH CHROOT JAIL
Comenzamos creando el directorio al que restringiremos el acceso al usuario. En este caso el usuario que tendrá acceso remoto se llamará test.
 
Con el modificador -p simplemente indicamos que si no existiera el directorio padre lo cree. 
En el fichero de configuración sshd_config, hay una entrada denominada ChrootDirectory que es la que utilizaremos. Podemos ir a la página del manual de sshd_config y buscar la parte correspondiente a esta entrada.
 
Si te fijas, nos indica que el directorio que indiquemos debe contener, por lo menos, un shell y los nodos básicos de /dev tales como los dispositivos null, zero, stdin, stdout, stderr y tty.
Busquemos primero estos ficheros en el directorio /dev.
 
De la anterior imagen nos interesan los números que se encuentran entre el grupo propietario y la fecha de creación. Estos números se denominan número mayor y menor y son propios de los ficheros de dispositivo. 
•	El número mayor identifica el driver asociado con el dispositivo. Por ejemplo, el dispositivo /dev/null utiliza el driver 1. El kernel utiliza el número mayor cuando abre el dispositivo para ejecutar el driver correspondiente.
•	El número menor es utilizado únicamente por el driver especificado por el número mayor. Esto se debe a que un driver suele controlar diversos dispositivos y este número le permite saber con cuál de ellos está tratando.
Una vez que conocemos los números mayor y menor de los dispositivos que queremos copiar tenemos que crear estos ficheros de dispositivos dentro de nuestro directorio. Para ello utilizaremos el comando mknod, que permite crear ficheros especiales de bloques o de caracteres. 
La sintaxis de este comando es:
 
Donde tipo puede ser c para indicar un dispositivo de caracteres y b si es un dispositivo de bloques. También usaremos el modificador -m para indicar los permisos que tendrá el fichero (en este caso todos).
 
A continuación, hay que asignar permisos a la jaula chroot. Fíjate que la jaula chroot y sus subdirectorios deben pertenecer al usuario root, y no deben tener permisos de escritura para ningún otro usuario normal o grupo.
 
3.3.2.- CONFIGURAR EL SHELL INTERACTIVO
Ahora vamos a crear el directorio bin copiar en él los ficheros /bin/bash de la siguiente forma:
 
Hay que tener en cuenta que Bash utiliza librerías para su funcionamiento, por lo que tendremos que copiarlas en el directorio lib. Para saber qué librerías utiliza un programa tenemos el comando ldd.
 
3.3.3.- CREAR Y CONFIGURAR EL USUARIO SSH
Ahora vamos a crear el comando useradd para crear el usuario. Ten en cuenta que este comando difiere del comando adduser en que no hace nada que no le indiquemos explícitamente en los parámetros (crear el directorio personal, crear la contraseña, …). Por tanto, para ponerle contraseña, debemos utilizar también el comando passwd.
 
También debemos guardar una copia de los ficheros de configuración en nuestra jaula, en concreto, los ficheros /etc/passwd y /etc/group,
 
Nota: cada vez que se añada un usuario SSH nuevo tienes que copiar estos ficheros actualizados.
3.3.4.- CONFIGURAR SSH PARA QUE UTILICE LA JAULA SSH
Ahora vamos al fichero de configuración sshd_config y añadimos las siguientes líneas:
 
Tras ello solo queda reiniciar el servicio SSH.
3.3.5.- CREAR EL DIRECTORIO HOME DEL USUARIO SSH Y AÑADIR COMANDOS
Ahora ya podemos probar a conectarnos con el usuario mediante SSH. Si nos conectamos veremos que lo podemos hacer sin ningún problema, pero unas pocas pruebas nos mostrarán que hay algunos comandos de Linux que no funcionan en nuestro entorno.
 
Esto se debe a que, como el usuario está encerrado en la jaula chroot únicamente podrá acceder a comandos internos (builtin) de Bash, tales como pwd, cd, … pero no a comandos externos como ls o date. En un sistema normal, estos comandos se encuentran en el directorio /bin, y, como es un directorio fuera de la jaula, el usuario no podrá acceder a él.
Pero antes de eso vamos a crear un directorio personal para el usuario dentro de la jaula chroot. Esto lo hacemos con las siguientes órdenes:
 
Una vez creado el directorio personal vamos a crear el directorio bin y copiar en él los comandos que queramos que tenga el usuario.
 
Al igual que nos pasó con bash, estos comandos también pueden requerir librerías externas, por lo que tendremos que copiarlas a nuestra jaula. Recuerda que el comando para verificar qué librerías requiere un comando es ldd.
 
 
Si probamos ahora ya veremos que el usuario puede acceder sin problemas mediante SSH y utilizar cualquier comando integrado de Bash o los comandos externos que hayamos añadido manualmente.
3.3.6.- PERMITIR ÚNICAMENTE CONEXIONES SFTP
El protocolo SFTP (SSH File Transfer Protocol) es un protocolo que permite realizar operaciones sobre ficheros (acceso, transferencia y administración) a través de la red sobre un túnel SSH. Hay que tener en cuenta que, a pesar del nombre, SFTP no es un FTP que se ejecuta sobre SSH, sino que es un protocolo nuevo.
Comparado con el protocolo SCP, que únicamente permite transferencias de ficheros, el protocolo SFTP un mayor rango de operaciones sobre los ficheros remotos. Un cliente SFTP incluye características extra como retomar transferencias interrumpidas, listado de directorios o eliminación remota de ficheros.
El protocolo SFTP lo podemos utilizar para permitir que un usuario remoto utilice SSH para transferir ficheros, pero sin tener la posibilidad de acceder a una terminal SSH. 
El primer paso para ello será cambiar la siguiente línea del fichero de configuración /etc/ssh/ssd_config.
 
Tras reiniciar el servicio, si intentamos acceder mediante SSH obtendremos un mensaje de error.
 
Y sólo podremos tener acceso mediante el comando sftp.
 



