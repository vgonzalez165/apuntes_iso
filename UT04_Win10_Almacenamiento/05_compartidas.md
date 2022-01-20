<link rel="stylesheet" href="../styles.css">

![Carátula UT04](imgs/caratula_ut04.png)

## Contenidos

1. [Organización del almacenamiento](01_organización.md)
2. [Configuración de discos en Windows](02_configuración_discos.md)
3. [Sistemas de archivos](03_sistemas_archivos.md)
4. [El sistema de ficheros NTFS](04_ntfs.md)
5. [**Carpetas compartidas**](05_compartidas.md)


# 5.- CARPETAS COMPARTIDAS

Windows permite compartir carpetas locales a través de la red con otros equipos mediante las **carpetas compartidas**.


## 5.1.- Compartir una carpeta

Para compartir una carpeta en Windows hemos de acceder a la pestaña *Compartir* del diálogo *Propiedades* de dicha carpeta. Hay dos formas de compartir una carpeta, mediante un asistente o bien mediante el Uso compartido avanzado. Por defecto, el asistente está habilitado, pero si no lo estuviera será necesario habilitarlo para utilizarlo. Para ello, en cualquier ventana del explorador de archivos, vamos a *Vista -> Opciones -> Ver -> Configuración avanzada* y marcamos la casilla *Usar asistente para compartir*.

### 5.1.1.- Compartir utilizando el asistente

La forma más sencilla es mediante el asistente, pero nos deja pocas opciones a la hora de determinar cómo queremos compartir la carpeta. El asistente se lanza desde la pestaña *Compartir* y ahí únicamente debemos añadir los usuarios que queremos que tengan acceso a la carpeta indicando qué tipo de acceso deben tener: solo lectura o lectura escritura.

Algo que hay que tener en cuenta es que, para que un usuario pueda acceder a un recurso compartido en la red, _debe tener contraseña_. Es decir, la contraseña del usuario que vaya a acceder de forma remota a un recurso no puede estar en blanco.

Otro aspecto a tener en cuenta es que el usuario al que se otorgan permisos tiene que ser un usuario local del equipo que comparte la carpeta, no del equipo desde el que se accede a la carpeta. Es decir, los usuarios pertenecen al mismo equipo al que pertenece la carpeta. Sin embargo, es posible que ambos equipos tengan un usuario con el mismo nombre y la misma contraseña. En ese caso, cuando se acceda a la carpeta compartida no se pedirán las credenciales de usuario.

Para acceder a una carpeta compartida es posible hacerlo desde el explorador de archivos, en el árbol que pone *Red*. Sin embargo, hay ocasiones en que el descubrimiento de equipos de Windows no funciona muy bien y no veremos ahí los recursos compartidos en la red.

En estos casos será necesario acceder utilizando la denominada **Convención de Nomenclatura Universal (UNC)**, que indica la estructura que debe tener la dirección de un recurso de red en una red Windows.

En UNC, las rutas de red tienen la forma:

```
\\nombreDeEquipo\nombreDeRecursoCompartido
```

El nombre del equipo puede ser sustituido por la dirección IP del mismo. Esta dirección se puede introducir en el cuadro de texto *Ejecutar* o bien en cualquier ventana del explorador de Windows.


### 5.1.2.- Uso compartido avanzado

Si optamos por el **Uso compartido avanzado** dispondremos de mayor número de opciones a la hora de compartir la carpeta. Las opciones que hay son:

- **Nombre del recurso compartido**: podemos escoger el nombre por el que se conocerá la carpeta compartida en la red. Este nombre no tiene por qué ser el mismo que el de la carpeta local que compartimos. Una característica del nombre es que si su último carácter es el símbolo dólar (`$`) la carpeta compartida _no será visible en la red_. Es decir, únicamente será accesible introduciendo directamente el nombre completo del recurso compartido ya que no aparecerá en el explorador de red.
- **Límite de usuario**: podemos establecer el número máximo de usuarios que se conectarán simultáneamente a la carpeta compartida.
- **Permisos**: ahora disponemos de una ACL similar a la que teníamos en los permisos NTFS locales donde podemos añadir usuarios y grupos y especificar qué permisos van a tener exactamente sobre dicha carpeta.
  
Hay que tener en cuenta que los permisos compartidos pueden no coincidir con los permisos NTFS.  En esos casos, siempre se aplicarán los permisos **más restrictivos**. Así, si por ejemplo, un usuario tiene permiso compartido de *Control total* pero solo tiene permiso de *Lectura* en NTFS, cuando acceda por la red solo tendrá permiso de lectura.


## 5.2.- Asignar unidad de red

Si accedemos frecuentemente a una carpeta compartida desde un equipo hay una opción mucho más cómoda que teclear la dirección UNC cada vez que queramos acceso, y es asignando la carpeta compartida a una **unidad de red**.

Al hacerlo, tendremos la carpeta compartida en *Este Equipo* como si fuera un dispositivo de almacenamiento más. Los pasos que hay que realizar son:

- Hacemos click derecho sobre *Este equipo* y seleccionamos la opción **Conectar a unidad de red**.
- En el cuadro de diálogo que nos sale rellenamos los campos solicitados:
    - Letra de unidad que queremos asignarle a la carpeta compartida.
    - Dirección UNC de la carpeta compartida
    - Marcamos la casilla Conectar de nuevo al iniciar sesión si queremos que la unidad sea persistente y se mantenga después de reiniciar el ordenador.
    - Por defecto, nos conectaremos a la carpeta compartida con el mismo usuario y misma contraseña que el usuario local que estemos utilizando. Si deseamos acceder con unas credenciales diferentes deberemos marcar la casilla Conectar con otras credenciales.
    - Tras aceptar, ya tendremos la unidad de red disponible.


## 5.3.- Eliminar las credenciales de red almacenadas en caché

Si has practicado con lo visto en los apartados anteriores te habrás dado cuenta de que el sistema recuerda las credenciales con las que te has registrado en una carpeta compartida. El problema radica cuando queremos cambiar el usuario con el que iniciamos sesión en la carpeta. Para solucionar este problema tenemos que recurrir a la línea de comandos y utilizar el comando `net use`.

Si ejecutamos directamente el comando `net use` nos mostrará todas las conexiones que tenemos abiertas en el sistema.

```powershell
PS C:\> net use

Estado         Local      Remoto                    Red
---------------------------------------------------------------------------------------
Conectado                 \\172.16.1.76\datos       Microsoft Windows Network
Conectado                 \\172.16.1.76\IPC$        Microsoft Windows Network
Se ha completado el comando correctamente.
```
 
Para cerrar todas las conexiones que tenemos abiertas hay que ejecutar el comando net use * /del.

```powershell
PC C:\> net use * /del
Tiene estas conexiones remotas:

                    \\172.16.1.76\datos
                    \\172.16.1.76\IPC$
Si continúa, se cancelarán las conexiones.

¿Desea continuar esta operación? (S/N) [N]: s
Se ha completado el comando correctamente.
```
 


***
[Volver al índice principal](index_UT04.md)