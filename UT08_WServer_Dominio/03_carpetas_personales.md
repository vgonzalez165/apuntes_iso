<link rel="stylesheet" href="../styles.css">

![Carátula UT08](imgs/caratula_ut08.png)

# UT08. WINDOWS SERVER. ADMINISTRACIÓN DEL DOMINIO

PONER EL RESUMEN DE LA UNIDAD

### Contenidos

1. [Active Directory](01_active_directory.md)
2. [Usuarios y grupos del dominio](02_usuarios_dominio.md)
3. [**Carpetas personales en Active Directory**](03_carpetas_personales.md)
4. [Carpetas compartidas por un grupo](04_carpetas_compartidas_grupo.md)
5. [Perfiles de usuario: locales, móviles, obligatorios, temporales y super-obligatorios](05_perfiles_usuarios.md)
6. [Asignación de derechos a usuarios y grupos](06_derechos_usuarios.md)
7. [Group Policy Objects (GPOs) y Group Policy Preferences (GPPs)](07_gpo_gpp.md)
8. [Despliegue remoto de aplicaciones](08_despliegue_aplicaciones.md)
9. [Segundo controlador de dominio](09_segundo_dc.md)
10. [DFS](10_dfs.md)


# 3.- CARPETAS PERSONALES EN ACTIVE DIRECTORY

La primera tarea de administración de red que realizaremos en el servidor será la creación de un espacio de almacenamiento para cada usuario en el servidor.

Para ello crearemos una carpeta en el servidor que el usuario verá en su ordenador como una unidad de red.

Dividiremos el trabajo en tres tareas:
Creación de una carpeta compartida en el servidor que contenga todas las carpetas de los usuarios.
Configuración de una carpeta compartida en la cuenta de cada usuario.
Acceder desde el equipo cliente a la carpeta compartida.

Primero creamos una carpeta en el servidor

Compartimos la carpeta de forma similar a como lo hacemos en Windows 7
Vamos a Propiedades de la carpeta y en la pestaña Compartir hacemos click en Uso compartido avanzado.

Marcamos la casilla de Compartir esta carpeta y ponemos el nombre por el que queremos que se muestre la carpeta en la red.

Al igual que pasa con Windows 10, si queremos que la carpeta no se muestre a los usuarios debemos poner el símbolo $ al final de su nombre.

Por defecto tiene asignado únicamente permisos de lectura para el grupo Todos.

Este grupo es demasiado genérico, por lo que vamos a reemplazarlo por el grupo Usuarios del dominio, además le daremos permisos de Control total. 

Los permisos tienen que quedar de esta manera.

El siguiente paso es configurar las cuentas de cada usuario para que la utilicen como lugar de almacenamiento en red.
Esto lo hacemos desde la herramienta Usuarios y equipos de Active Directory

Seleccionamos todos los usuarios a los que queramos asignar una carpeta y vamos a Propiedades

Observa que como hemos seleccionado varios usuarios únicamente vemos las propiedades que pueden ser comunes a todos ellos.

Vamos a la pestaña Perfil y marcamos la casilla Carpeta particular.

Aquí tenemos dos opciones:
Ruta de acceso local: esta opción indica que el usuario verá la carpeta compartida como una carpeta dentro de su propio sistema de ficheros.
Conectar: en este caso verá la carpeta compartida como una unidad de red. Será el caso que configuraremos ahora.

Cuando queremos conectar la carpeta como unidad de red debemos indicar una letra de unidad e indicar la ruta de la carpeta en la red.
La sintaxis es la siguiente:

`\\servidor\carpeta_contenedora\carpeta_compartida`

Por lo tanto lo pondremos de la siguiente forma:
Observa cómo hemos indicado el nombre de la carpeta (que se llamará igual que el usuario)

Seguro que recuerdas que en Windows las cadenas rodeadas por el símbolo por ciento (%) son variables de entorno.
En concreto la variable %username% es la que almacena el nombre del usuario.
Aquí utilizamos esta variable porque estamos trabajando con varios usuarios a la vez y no podríamos poner un texto normal porque entonces las carpetas de todos los usuarios se llamarían igual.

