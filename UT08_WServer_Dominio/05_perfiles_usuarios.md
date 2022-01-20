<link rel="stylesheet" href="../styles.css">

![Carátula UT08](imgs/caratula_ut08.png)

# UT08. WINDOWS SERVER. ADMINISTRACIÓN DEL DOMINIO

PONER EL RESUMEN DE LA UNIDAD

### Contenidos

1. [Active Directory](01_active_directory.md)
2. [Usuarios y grupos del dominio](02_usuarios_dominio.md)
3. [Carpetas personales en AD](03_carpetas_personales.md)
4. [Carpetas compartidas por un grupo](04_carpetas_compartidas_grupo.md)
5. [**Perfiles de usuario: locales, móviles, obligatorios, temporales y super-obligatorios**](05_perfiles_usuarios.md)
6. [Asignación de derechos a usuarios y grupos](06_derechos_usuarios.md)
7. [Group Policy Objects (GPOs) y Group Policy Preferences (GPPs)](07_gpo_gpp.md)
8. [Despliegue remoto de aplicaciones](08_despliegue_aplicaciones.md)
9. [Segundo controlador de dominio](09_segundo_dc.md)
10. [DFS](10_dfs.md)


# 5.- PERFILES DE USUARIOS: LOCALES, MÓVILES, OBLIGATORIOS, TEMPORALES Y SUPER-OBLIGATORIOS

## 5.1.- Perfiles de usuario

Los perfiles de usuario contienen la configuración del entorno de trabajo de cada usuario del equipo local.
Entre otros, contiene aspectos como:
Preferencias de configuración de pantalla
Escritorio
Conexiones de red
Configuración de impresoras

Esta información se organiza en archivos que se crean en el momento es que el usuario inicia sesión por primera vez.

Los perfiles de los usuarios se almacenan en la carpeta C:\Usuarios, dentro de la cual hay una subcarpeta con el nombre de cada usuario conteniendo su perfil.

El perfil del usuario contiene a su vez carpetas, algunas de las cuales son:
Datos de programa: datos particulares de algunos programas, p.e. diccionarios de LibreOffice
Escritorio: los ficheros del Escritorio del usuario
Configuración local: algunos datos que necesitan almacenar los programas, como archivos temporales de Internet
Mis documentos: documentos del usuario.

En un entorno de Active Directory hay diferentes tipos de perfil:
Perfil de usuario local: 
Se guarda en el disco duro local del equipo cliente.
Todas las modificaciones que se realicen serán específicas del ordenador en que se han establecido.
Perfil de usuario móvil:
Se almacena en una carpeta compartida en el servidor.
Está asociada a la cuenta del dominio
Estará disponible de forma independiente al ordenador concreto en que inicie sesión el usuario.

Perfil de usuario obligatorio: 
Se podrían definir como perfile móviles de solo lectura.
Solo los administradores del dominio pueden realizar cambios en dichos perfiles.
Perfil de usuario temporal:
Se aplican cuando se produce un error que impide cargar un perfil móvil o un perfil obligatorio.
Cuando el usuario cierra sesión , este perfil se elimina perdiéndose todas las modificaciones realizadas por el usuario.

Perfil de usuario super-obligatorio:
Incorporado a partir de Windows Server 2008
Similar al perfil obligatorio pero si se produce un error que impida cargar dicho perfil el usuario no podrá iniciar sesión.


## 5.2.- Creación de un perfil móvil


## 5.3.- Creación de un perfil obligatorio