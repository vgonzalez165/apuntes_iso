![Carátula UT05](imgs/caratula_ut05.png)

## Contenidos

1. Gestión de procesos
2. El administrador de tareas
3. Servicios
4. El monitor de recursos
5. El visor de eventos
6. Gestión de energía
7. Directivas de grupo local
8. Programador de tareas


# 7.- INTRODUCCIÓN A LAS DIRECTIVAS DE GRUPO LOCAL

Las **directivas de grupo** son un mecanismo de Windows que permite configurar muchos aspectos importantes del equipo o de la red. Con las directivas de grupo se pueden realizar multitud de tareas como:

- Limitar qué aplicaciones pueden instalar los usuarios
- Deshabilitar dispositivos portátiles como memorias USB
- Deshabilitar protocolos de red como TLS 1.0 para forzar a que se use una versión más actual y segura.
- Limitar las configuraciones que el usuario puede realizar en el *Panel de Control*
- Establecer una imagen predeterminada como fondo de escritorio.

Para acceder al *Editor de Políticas Locales* simplemente hay que ejecutar el complemento del MMC `gpedit.msc`.

![Editor de Directivas de Grupo](imgs/gpo_vista_global.png)

## 7.2.- Directivas de Grupo Local

Las *Directivas de Grupo* se organizan en una estructura arborescente que parte de dos raices:

- **Configuración de equipo**: son configuraciones que se aplican a todo el equipo, independiente del usuario que haya iniciado sesión. Hacen efecto durante el arranque del sistema operativo, antes de que se muestre la pantalla de inicio de sesión.
- **Configuración de equipo**: en este caso son configuraciones que se aplican a usuarios determinados, y por tanto, hacen efecto después de que el usuario haya iniciado sesión.




***
[Volver al índice principal](index_UT05.md)