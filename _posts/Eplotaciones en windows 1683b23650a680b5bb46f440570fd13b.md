---

layout: post
title: Eplotaciones en windows
date: December 26, 2024
tags: ciberseguridad
toc: true

---
# Introducción a Windows

## Kernel

Windows en vezde compad embeded

El soporte oficial empieza en 2023 (antes tenía soporte de microsoft)

Kernel NT → suele ir emparejado windows server a otro windows

- Cómo funciona
    - Es una pieza de software extremadamente compleja
    - Driver → Explica al ordenador cómo comunicarse con otros complementos que no hablan el mismolenguaje
    - Tienen el mismo SO los drivers y los kernel
    - Un fallo en kernel es catastrofico (se encarga de proteger al resto del ordenador)
    - Microsoft está intentando sacar a la gente del kernel (porque kernel tiene los mayores privilegios)
- Partes
    - Servicios ejecutivos
        - Diferentes partes del kernel que se supone que son independientes
    - Administrador de objetos
        - No es Programación orientada a objetos como tal
        - Permisos que tiene cada uno, etc
- Si algo quiere interactuar entre el hardware y el software hay que comunicarse con el kernel

Separación entre usuario-kernel

- CPU moderna → sabe diferenciar que hay dos modos de ejecución (user/kernel)
- Forzar al modo usuario (conectarse con dispositivos conectados al ordenador) a comunicarse con kernel
- Modo especial para conectar user y hardware → SYSCALL
    - Llama a un método especial del kernel
    - Que funcionalidad estamos interesados de ejecuta
    - Despachar
- Librerias
    - DLLs → librerías estándar que no se pueden ejecutar ellos solos
    - Exponen un montón de métodos para comunicarse con el kernel
    - API de alto nivel → + normal
- Cuando se hace la llamada al kernel hay un sistema de dispatch que ejecuta la llamada que les han hecho (MOV EAX, 129)
- Llamadas directamente al kernel → muy incómodo (por eso se hace por DLL)
- Malware si tiene syscall (funciona)
    - Hay contramedidas: revisando desde dónde se está haciendo la syscall
    - Ver qué modificaciones se han hecho los EDRs (revertir esas modificaciones o evitarlas de alguna forma) → Red Team
    - Si se pasa sin pasar por la DLL es bastante sospechoso
- Subsistemas
    - Win32 → utilizan las DLLs normales de windows (gran parte de las apps de windows)
    - OS-2: viejo
    - Aplicaciones POSIX
        - Intentar ser compartible con Linux
        - A partir de Windows 8 cambiaron el windows → ~Windows System for Linux

## Procesos

- 

# Autenticación y autorización local

# Kerberos

**Local Security Policy comon panel de control** 

PRIMERO TERMINAREMOS EL TEMA DE WINDOWS
Queda la parte del UAC y la elevación de privilegios
Popup de permitir que un programa haga cambios en el dispositivo. El sistema sabes si tienes permisoss de administración porque se efectúa un segundo token de confirmación para c. El nivel de confianza de un usuario en el sistema por defecto es confianza medio por defecoto. El nivel de confianza de administradores es alto.
Para elevar privilegios en windows pasamos por user account control.
En el panel de control se puede modificar eso otorgando más y menos seguridad. Incrementar cuándo se notifica la elevación de privilegios es un método de hardening, si no con UACMe sería muy sencillo saltarse el control.

EMPEZAMOS TEMA 6, MÉTODOS DE EXPLOTACIÓN EN LINUX
Linux se basa en Unix, y al referirnos al Unix nos referimos al núcleo. El kernel es Unix para todos, pero se van haciendo distribuciones que cambian las capas exteriores, el kernel sigue siendo igual. La parte común a todos los sistemas es generalmente la más importante, porque es la que se encarga de hablar directamente con el hardware y gestionar procesos y syscalls.
El kernel de linux tiene una gran cantidad de capas. Lo podemos traducir a que el kernel es una barrera entre aplicaciones y cpu, memoria y hardware.
Para interactuar con el Kernel se pueden usar aplicaciones como ZSH.

Cómo se estructura un SO basado en Linux:
El sistema virtual de ficheros está entre el kernel y el hardware, y hace diferentes interpretaciones para diferentes formatos. A veces hay que formatear por cómo habla el kernel con el fichero.
En Linux todo es o un proceso o un fichero. Los procesos son ficheros con programas de ejecución. Por otro lado, los ficheros, son todos los archivos que no tienen la capacidad de ejecutarse. Dentro de ese sistema, los ficheros y directorios están en forma de árbol conocido como rutas. Un directorio es una carpeta, es un fichero con información de otros ficheros. Los ficheros pueden tener cualquier secuencia de bytes. La estructuración va con barras.
Los permisos en Linux defienden las restricciones de uso y acceso, distinguido en niveles de propietario, grupo y otros. El principio del mínimo privilegio es imporante para gestionar sistemas. Hay tres permisos principalesm en Linux: rwx (read, write, xecute). El sistema operativo se basa en esta precedencia de permisos para autorizar acciones o no. Se leen permisos de izquierda a derecha. Para comprobar  se lee propiedad del fichero, se lee la pertenencia del usuario a grupo, escanean sus permisos y listo.
whoami, ls -l, echo
los grupos se suelen usar para blacklist
Hay dos permisos extra, el SGID y el SUID (set group id o set user id) relacionados con los grupos de nuestro sistema. No me he enterado muy bien de esta parte en clase. Expandir y estudiar.
Página para ver ampliación de derechos en las presentaciones: Buscar
La asignatura ofrece teoría casi puramente, es un trozo de tierra que poco a poco va a ir conectando cosas. La práctica es para hacer un contacto con un ataque.

Tanto el kernel como la shell utilizan variables para comunicarse con los procesos de usuario. Pueden ser de entorno globales durante toda la sesiónd del usuario, o locales que durarán mientras la shell esté abierta.