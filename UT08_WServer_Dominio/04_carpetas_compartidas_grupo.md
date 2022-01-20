<link rel="stylesheet" href="../styles.css">

![Carátula UT08](imgs/caratula_ut08.png)

# UT08. WINDOWS SERVER. ADMINISTRACIÓN DEL DOMINIO

PONER EL RESUMEN DE LA UNIDAD

### Contenidos

1. [Active Directory](01_active_directory.md)
2. [Usuarios y grupos del dominio](02_usuarios_dominio.md)
3. [Carpetas personales en AD](03_carpetas_personales.md)
4. [**Carpetas compartidas por un grupo**](04_carpetas_compartidas_grupo.md)
5. [Perfiles de usuario: locales, móviles, obligatorios, temporales y super-obligatorios](05_perfiles_usuarios.md)
6. [Asignación de derechos a usuarios y grupos](06_derechos_usuarios.md)
7. [Group Policy Objects (GPOs) y Group Policy Preferences (GPPs)](07_gpo_gpp.md)
8. [Despliegue remoto de aplicaciones](08_despliegue_aplicaciones.md)
9. [Segundo controlador de dominio](09_segundo_dc.md)
10. [DFS](10_dfs.md)


# 4.- CARPETAS COMPARTIDAS POR UN GRUPO

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


