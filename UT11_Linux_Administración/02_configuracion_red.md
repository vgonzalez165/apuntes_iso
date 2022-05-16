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




# 2.- CONFIGURACIÓN DE RED EN UBUNTU SERVER

La versión 17.04 de Ubuntu trajo consigo un cambio en la gestión de las conexiones de red. El fichero `/etc/interfaces`, que había servido tradicionalmente para configurar las interfaces de red sigue estando presente pero simplemente para notificar que ahora la configuración de red se realiza mediante **netplan**.

**Netplan** es una utilidad que permite configurar fácilmente los adaptadores de red, y aunque las ventajas para configuraciones sencillas apenas se aprecian, sí que es bastante mejor cuando se realizan configuraciones más complicadas.

Netplan utiliza ficheros **YAML (*YAML Ain’t Markup Language*)** que se pueden encontrar en diversas localizaciones:

- `/run/netplan/*.yaml`
- `/etc/netplan/*.yaml`
- `/lib/netplan/*.yaml`

Si hay varios ficheros, las configuraciones nuevas se añadirán y las que tengan el mismo nombre se reemplazarán siguiendo los siguientes principios de precedencia:

- El orden en que se leen los directorios es primero `/lib`, luego `/etc` y finalmente `/run`, por lo que la configuración que prevalecerá en caso de contener las mismas claves es la que se encuentra en el directorio `/run`.
- Igualmente, los ficheros se ordenan alfabéticamente, teniendo mayor prioridad los últimos que se encuentren en este orden alfabético.


## 2.1.- El fichero de configuración

Un ejemplo de fichero YAML de Netplan puede ser el siguiente:
 
```bash
vgonzalez@ubuntu:~$ cat /etc/netplan/00-installer-config.yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    eth0:
      addresses:
      - 10.0.0.201/8
      nameservers:
        addresses: []
        search: []
    eth1:
      dhcp4: yes
  version: 2
```

Los ficheros YAML tienen una estructura muy clara, con una serie de claves que pueden o bien tener un valor o bien contener a su vez diferentes subclaves. Para indicar que una subclave se está contenida dentro de una clave se utiliza la **indentación**, por lo que **es importante respetarla**. Asimismo, no se puede utilizar el carácter de tabulado, sino que **hay que utilizar el espacio** para la indentación.

Si una clave tiene un único valor se pone a continuación de esta. Si espera una secuencia de valores deben indicarse en sucesivas líneas precediendo cada valor con el carácter guion.

```
clave1: valor
clave2: 
   subclave21: valor
   subclave22: valor
clave_secuencia1:
   - valor1
   - valor2
clave_secuencia2: [valor1, valor2]
```

Las primeras líneas son siempre las mismas, correspondiendo estas a la **versión** y el ***renderer***. La primera línea que se puede cambiar es la que indica el *renderer* que se utilizará. Las opciones son **NetworkManager** y **networkd**, siendo este último el deseable ya que permite configuraciones más avanzadas de la red, como, por ejemplo, las tablas de enrutamiento.

A continuación, se indican los diferentes bloques de tipos de dispositivos (ethernet, wifi, …), indicando dentro de cada uno una entrada para cada dispositivo. Los nombres de las claves serán **ethernets**, **wifis** y **bonds**, correspondiendo con las interfaces claveadas, inalámbricas y los enlaces agregados respectivamente.


### 2.1.1.- Interfaces cableadas

Las interfaces cableadas se identifican mediante la clave `ethernets`, dentro de la cual se indicará una subclave para cada una de las interfaces del equipo. Estas subclaves podrán tener las siguientes propiedades:

- `addresses`: Añade direcciones estáticas a la interfaz que se suman a las recibidas a través de DHCP. Cada entrada de la secuencia se indica en notación CIDR de la forma `addr/prefixlen`. Ten en cuenta que una misma interfaz puede tener diversas direcciones IP indicadas como una secuencia de claves.
- `dhcp4` y `dhcp6`: el valor `true` en una de estas claves indica que se le asignará una IP de forma dinámica mediante un servidor DHCP.
- `gateway4`, `gateway6`: indica la dirección de la puerta de enlace de la red. Requiere que se establezca también la clave `addresses`.
- `nameservers`: Establece los servidores DNS y los dominios de búsqueda para la configuración manual. Soporta dos campos:
    - `addresses`: una lista de direcciones IPv4 e IPv6
    - `search`: lista de dominios de búsqueda

```yaml
nerwork:
    version: 2
    renderer: networkd
    ethernets:
        enp3s0:
            addresses:
                - 10.10.10.2/24
            gateway4: 10.10.10.1
            nameservers:
                search: [mydomain, otherdomain]
                addresses: [10.10.10.1, 1,1,1,1]
```
- `optional`: Un dispositivo opcional no es requerido para el arranque. Normalmente, networkd esperará un cierto a que un dispositivo se configure antes de continuar con el arranque. Sin embargo, si un dispositivo está marcado como `opcional`, networkd no esperará a que se configure. Solo está soportado por networkd.
- `routes`: cuando tenemos varias direcciones de red asignadas a una misma interfaz podemos escoger el enrutamiento deseado mediante la clave `routes`. Esta clave tendrá una serie de subclaves to de la siguiente forma:

```yaml
ethernets:
    enp3s0:
        addresses:
            - 9.0.0.9/24
            - 10.0.0.10/24
            - 11.0.0.11/24
        #gateway4: # deshabilitado ya que se configuran a continuación las rutas
        routes:
            - to: 0.0.0.0/0
                via: 9.0.0.1
                metric: 100
            - to: 0.0.0.0/0
                via: 10.0.0.1
                metric: 100
            - to: 0.0.0.0/0
                via: 11.0.0.1
                metric: 100
```
 
Después de `to`: indicamos las redes de destino que corresponden a esta ruta, con `via`: donde se indica la puerta de enlace para esas redes. Observa que la puerta de enlace debe estar en la misma subred que alguna de las interfaces. Por ejemplo, en la imagen anterior la puerta de enlace `9.0.0.1` está en la misma subred que la interfaz con la IP `9.0.0.9` por lo que los paquetes destinados a esta puerta de enlace se enviarán por dicha interfaz.

Con la clave `metric` se indica la **métrica**, que puede ser útil cuando la tabla de enrutamiento incluye varias rutas para el mismo destino. En ese caso se utiliza la métrica para determinar cuál de ellas tendrá preferencia.
Observa también que, si utilizamos tablas de enrutamiento, no hay que indicar la opción puerta de enlace.


### 2.1.2.- Interfaces inalámbricas

Las opciones de configuración de las interfaces inalámbricas tienen una configuración muy similar a las interfaces cableadas, requiriendo una asignación dinámica o estática de la dirección IP. Sin embargo, también es necesario indicarle un método de autenticación para poder conectarse con el punto de acceso, lo cual se consigue con la clave **access-points**.

Dentro de esta clave tendremos una subclave con el SSID de cada una de las redes inalámbricas a las que nos queramos conectar, indicando la contraseña con la subclave **password**. Si fuera una red abierta (sin contraseña) indicaríamos como valor del punto de acceso la cadena {}.
 
 ```yaml
 network:
    version: 2
    renderer: networkd
    wifis:
        wlp2s0b1:
            dhcp4: no
            dhcp6: no
            addresses: [192.168.0.21/24]
            gateway4: 192.168.0.1
            nameservers:
                addresses: [192.168.01., 8.8.8.8]
            access-points:
                "network_ssid_name":
                    password: "********"
        wl0:
            access-points:
                opennetwork: {}
            dhcp4:
 
 ```
 
### 2.1.3.- Interfaces agregadas

En la unidad anterior ya vimos cómo crear interfaces agregadas (*bonding*) desde el instalador de Ubuntu Server. Si queremos realizar esta operación una vez instalado Ubuntu, deberemos recurrir al fichero de configuración de netplan. Las interfaces agregadas se indican dentro de la clave `bond`, que tendrá una subclave para cada uno de los enlaces agregados que queramos crear.

Podemos ver un ejemplo a continuación:
 
```yaml
network:
    version: 2
    renderer: networkd
    bonds:
        bond0:
            dhcp4: yes
            interfaces:
                - enp3s0
                - enp4s0
            parameters:
                mode: active-backup
                primary: enp3s0
```

Las subclaves que tendrá cada enlace agregado pueden ser:

- `interfaces`: lista de las interfaces de red que se agregarán a este enlace.
- `parameters`: los parámetros con que se configurará el enlace. Tiene múltiples subclaves que puedes consultar en la ayuda, pero la más importante es **mode**, que indica el modo de funcionamiento del enlace. Las opciones son:
  - balance-rr (por defecto), 
  - active-backup, 
  - balance-xor, broadcast, 
  - 802.3ad, 
  - balance-tlb y 
  - balance-alb.



### 2.1.5.- Más información

Si quieres información más detallada sobre el fichero de configuración de netplan y sus múltiples opciones de configuración puedes recurrir a los siguientes recursos:

•	[Referencia de netplan](https://netplan.io/reference/)
•	[Ejemplos de uso de netplan](https://netplan.io/examples/)



## 2.2.- Aplicando los cambios

Para aplicar los cambios realizados en el fichero de configuración ya no es necesario reiniciar el servicio de red, sino que únicamente hay que invocar el comando `netplan` con el parámetro `apply`.


## 2.3.- El comando `ip`

Con la desaparición del fichero de configuración `/etc/network/interfaces`, también desaparecerá el comando `ipconfig`. Su sustituto será el comando `ip`. Este comando es bastante similar a `ipconfig` pero mucho más potente, con muchas funcionalidades asociadas.

Veamos algunas de las operaciones más comunes que se pueden realizar con el comando `ip`.

### 2.3.1.- Mostrar información de las interfaces de red

La orden para mostrar información de las interfaces de red es:

```
$ ip addr show
```

Muestra información tal como dirección IP, subred, … Se le puede pasar opcionalmente el identificador de una interfaz para que únicamente muestre información sobre dicha interfaz. También se puede abreviar como `ip a s`


### 2.3.2.- Habilitar y deshabilitar interfaces de red

Para habilitar o deshabilitar una interfaz de red utilizamos la siguiente orden.

```
$ ip link set {id} up|down
```


### 2.3.3.- Asignar dirección IP a una interfaz

Asignamos una IP con:

```
$ ip addr add {address}/{masc} dev {id}
```

Esta orden añade una nueva dirección IP al dispositivo de red cuyo identificador se indique. Hay que tener en cuenta que esta IP no reemplaza a la anterior, sino que la interfaz pasará a tener una dirección IP más.

También podemos indicar la dirección de broadcast de la red.

```
$ ip addr add broadcast {broadcast_address} dev {id}
```

O indicar ambas de la forma:

```
$ ip addr add {address}/{masc} brd + dev {id}
```

### 2.3.4.- Eliminar una dirección IP de la interfaz

Para eliminar una IP de una interfaz utilizamos el siguiente comando.

```
$ ip addr del {address}/{masc} dev {id}
```

### 2.3.5.- Información de la puerta de enlace

Para mostrar información sobre la puerta de enlace ejecutamos la siguiente orden.

```
$ ip route show
```

El resultado muestra información de enrutamiento para todas las interfaces de red. Podemos obtener información de enrutamiento para una IP particular con la orden.

```
$ ip route get {address}
```

### 2.3.6.- Comprobar las entradas ARP

Podemos también obtener el contenido de la **caché ARP** del equipo con el siguiente comando.

```
$ ip neigh
```

### 2.3.7.- Modificar las entradas ARP

Aunque la cache ARP se genera automáticamente, podemos editar manualmente las entradas de la misma para eliminar entradas existentes o añadir nuevas. Para eliminar una entrada lo hacemos de la forma:

```
$ ip neigh del {ip_address} dev {id}
```

Si queremos añadir una nueva entrada los hacemos con el comando:

```
$ ip neigh add {ip_address} lladdr {mac_address} dev {id} nud {value}
```

Donde `nud` significa estado vecino y sus valores pueden ser:

- `perm`: permanente, solo puede ser eliminado por el administrador.
- `noarp`: la entrada es válida, pero puede eliminarse una vez que caduque la vida útil
- `stale`: la entrada es válida pero no se sabe si es alcanzable
- `reachable`: la entrada es válida y alcanzable


### 2.3.8.- Obtener las estadísticas de red

Por último, podemos obtener las estadísticas de red con la siguiente orden.

```
$ ip -s link
```


***
[Volver al índice principal](index_UT11.md)

