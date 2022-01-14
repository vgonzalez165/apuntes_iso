![Carátula UT08](imgs/caratula_ut08.png)

### Contenidos

1. [**Active Directory**](01_active_directory.md)
2. [Usuarios y grupos del dominio](02_usuarios_dominio.md)
3. [Carpetas personales en AD](03_carpetas_personales.md)
4. [Carpetas compartidas por un grupo](04_carpetas_compartidas_grupo.md)
5. [Perfiles de usuario: locales, móviles, obligatorios, temporales y super-obligatorios](05_perfiles_usuarios.md)
6. [Asignación de derechos a usuarios y grupos](06_derechos_usuarios.md)
7. [Group Policy Objects (GPOs) y Group Policy Preferences (GPPs)](07_gpo_gpp.md)
8. [Despliegue remoto de aplicaciones](08_despliegue_aplicaciones.md)
9. [Segundo controlador de dominio](09_segundo_dc.md)
10. [DFS](10_dfs.md)


# 1.- ACTIVE DIRECTORY

## 1.1.- Introducción a Active Directory

### 1.1.1.- Conceptos básicos de Active Directory

Un **directorio** es una lista que permite organizar y localizar elementos.

En términos informáticos los dos elementos de un directorio son:
- **El almacén de datos**: contiene los objetos tales como aplicaciones, bases de datos, impresoras, usuarios,…
- **Los servicios** que actúan sobre los datos. Lleva a cabo funciones como: replicación, medidas de seguridad, distribución de datos,…

**Se puede definir Active Directory como un almacén de objetos y un servicio basado en la red que ubica y administra  recursos, poniéndolos al alcance de grupos y usuarios**.

Algo importante que hay que tener en cuenta al trabajar con Active Directory es que **todo es un objeto**, ya sean usuarios, servidores, estaciones de trabajo, documentos o impresoras. Y, como todo objeto, todo lo que se puede encontrar en AD tiene una serie de **propiedades** o **atributos** que le define, así como una **Lista de Control de Acceso (ACL)** que determina qué usuarios y grupos pueden acceder a dicho recurso, así como los permisos con los que puede acceder.

Active Directory dispone de dos elementos estructurales más un tercer elemento que sería el esquema:

- **Estructura lógica**: está compuesta por los objetos así como sus atributos asociados. Los objetos de AD se organizan según un modelo jerárquico de **dominio**.
- **Estructura física**: esta estructura presenta mecanismos para la replicación y comunicación de datos. Sus dos aspectos fundamentales son:
  - La definición de elemento estructural de la subred IP, constituido por los **sitios** de Active Directory.
  - El **servidor físico** que almacena y replica datos de AD.
- **Esquema**: contiene las definiciones de todos los objetos que se crean y almacenan en Active Directory y sus propiedades. El esquema es responsabilidad del **controlador de dominio**, aunque una copia se replica a todos los controladores de dominio del bosque para asegurar la integridad y coherencia de los datos en todo el bosque.


### 1.1.2.- Bloques organizativos de Active Directory

Los bloques organizativos disponibles en la estructura lógica son:
Dominios
Árboles de dominio
Bosques de dominio
Unidades organizativas

#### Dominio

El **dominio** consta de los sistemas de equipo y los recursos de red que comparte un límite de seguridad lógica. En ocasiones se crean para definir límites funcionales como aquellos entre las unidades administrativas. 

También se les considera agrupaciones de recursos o servidores que utilizan un nombre de dominio común, conocido como **espacio de nombres**.

#### Árbol de dominios

Cuando múltiples dominios comparten un esquema, relaciones de confianza de seguridad y un catálogo global, se crea un árbol de dominio, definido por espacios de nombre común y contiguo.
Ejemplo:  hijo1.raiz.com, hijo2.raiz.com e hijo3.raiz.com pueden formar parte de un árbol, ya que comparten una nomenclatura contigua y pueden tener un esquema y catálogo global comunes.
El primer dominio creado se conoce como dominio raíz y contiene la configuración y los datos de esquema para el árbol.

Algunos de los motivos para crear diferentes dominios en un árbol son:
Se pueden gestionar organizaciones diferentes y proporcionar identidades de unidad.
Se pueden reforzar distintos límites de seguridad y directivas de contraseña
Proporciona una mejor gestión de un gran número de objetos administrados
Descentraliza la administración

#### Bosque de dominios

Cuando se añade un nuevo dominio a un árbol se establecen unas relaciones de confianza transitivas. 
Pero las relaciones de confianza también se pueden establecer entre árboles de dominio con distintos espacios de nombres. Cuando esto ocurre se crean distintos espacios de nombres.
Todos los árboles de un bosque comparten una serie de atributos, incluyendo el catálogo global, la configuración y el esquema.
Un bosque no tiene un nombre propio.

#### Unidades organizativas
Los dominios se pueden dividir de manera interna en subestructuras administrativas conocidas como Unidades Organizativas.
Pueden considerarse como objetos de contenedor. 
Las UO pueden anidarse dentro de otras UO, pudiendo crear una estructura jerárquica dentro del dominio.


# 2.- Instalación de Active Directory
