---
layout: default
title: Carpetas compartidas por un grupo
parent: UT07. Windows Server. Administración del dominio
nav_order: 4
---

# 4.- Carpetas compartidas por un grupo

Vamos a crear una carpeta que en esta ocasión estará compartida por un grupo de usuarios.

Además realizaremos los pasos de configuración necesarios para que la carpeta se vea como una unidad de red.

Para este ejemplo tenemos un grupo que llamaremos Alumnos que contiene los dos usuarios de la presentación anterior.

Creamos la carpeta y en su menú contextual seleccionamos Usuarios específicos dentro de Compartir con.

Creamos la carpeta y en su menú contextual seleccionamos Usuarios específicos dentro de Compartir con.

Podemos acceder a esta carpeta desde el servidor utilizando su dirección de red.

En esta carpeta creamos un fichero que llamaremos conecta.bat.
Como sabrás los ficheros .bat son ficheros por lotes o scripts.

Añadimos el siguiente código en el fichero que hemos creado y lo guardamos.

Recuerda que la sintaxis para indicar la carpeta es la misma que utilizamos en la práctica anterior.

Sólo nos queda conseguir que se ejecute el script en el inicio de sesión de cada usuario. Para ello vamos al complemento Usuarios y equipos de Active Directory.

Como hicimos anteriormente seleccionamos todos los usuarios que pertenezcan al grupo y en el menú contextual seleccionamos Propiedades.

En la pestaña Perfil marcamos la opción Script de inicio de sesión e indicamos ahí el nombre que le hemos puesto al script.


