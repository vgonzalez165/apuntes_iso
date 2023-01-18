---
layout: default
title: Perfiles de usuario en Active Directory
parent: UT08. Windows Server. Administración del dominio
nav_order: 5
---

# 5.- Perfiles de usuario en Active Directory

## 5.1.- Perfiles de usuario

Un **perfil de usuario** en Windows es el conjunto de archivos y directorios que contienen la configuración del entorno de trabajo del usuario, así como todos los archivos y documentos que pertenecen al mismo.

Ejemplos de información contenida en el perfil del usuario con:

- Preferencias de configuración de pantalla
- Escritorio
- Conexiones de red
- Configuración de impresoras
- Todo lo que hay en *Mis documentos*, *Mis imágenes*, ...

Esta información se organiza en archivos que se crean en el momento es que el usuario inicia sesión por primera vez.

Los perfiles de los usuarios se almacenan en la carpeta C:\Usuarios, dentro de la cual hay una subcarpeta con el nombre de cada usuario conteniendo su perfil.

El perfil del usuario contiene a su vez carpetas, algunas de las cuales son:
Datos de programa: datos particulares de algunos programas, p.e. diccionarios de LibreOffice
Escritorio: los ficheros del Escritorio del usuario
Configuración local: algunos datos que necesitan almacenar los programas, como archivos temporales de Internet
Mis documentos: documentos del usuario.

En un entorno de Active Directory hay cinco tipos diferentes de perfil:
- **Perfil de usuario local**: Todos los datos del perfil se guardan en el disco duro local del equipo cliente. Cualquier modificación que se realice será específica del ordenador en que se han establecido. Esto implica que el usuario tendrá un perfil diferente en cada uno de los equipos del dominio que utilice.
- **Perfil de usuario móvil**: En este caso el perfil se almacena en una carpeta compartida en el servidor que está asociada a la cuenta del dominio. De esta forma, estará disponible y será siempre el mismo independientemente del ordenador desde el que se conecte el usuario.
- **Perfil de usuario obligatorio**: Se podrían definir como perfiles móviles de solo lectura. Está disponible desde cualquier equipo de la red, ya que, al igual que os perfiles móviles, se almacena en una carpeta compartida, pero, aunque el usuario puede realizar cambios en él, estos cambios no perdurarán una vez que cierre sesión.
- **Perfil de usuario temporal**: Hay ocasiones en que no se puede obtener un perfil móvil y obligatorio, por ejemplo, por problemas de conectividad o de falta de disponibilidad de recurso compartido en que se almacenan. En estos casos Windows permitirá al usuario iniciar sesión utilizando un perfil temporal que será eliminado una vez que haya finalizado la sesión.
- **Perfil de usuario super-obligatorio**: Este tipo de perfil fue incorporado en Windows Server 2008. Es similar al perfil obligatorio pero, si se produce un error que impida cargar dicho perfil, el usuario no podrá iniciar sesión.


## 5.2.- Creación de un perfil móvil


## 5.3.- Creación de un perfil obligatorio

