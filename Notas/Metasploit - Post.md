# Metasploit - Post

Los **módulos `post`** son scripts y utilidades que se ejecutan **después de haber comprometido exitosamente un sistema**, es decir, tras obtener una sesión (como Meterpreter o una shell interactiva).

No explotan vulnerabilidades, sino que **permiten realizar acciones internas** dentro del sistema víctima para extraer información, establecer persistencia o escalar privilegios.
## ¿Para qué se utilizan los módulos Post?

Algunas de las acciones comunes que permiten:

- **Escalada de privilegios:** identificar vectores para obtener privilegios de administrador o root.
    
- **Exfiltración de información sensible:**
    
    - Credenciales almacenadas (hashes, tokens).
        
    - Archivos, bases de datos, configuraciones.
        
- **Keylogging y espionaje:** registrar pulsaciones de teclas o copiar la memoria de procesos.
    
- **Persistencia:** insertar puertas traseras para mantener acceso tras reinicios.
    
- **Ejecución remota de scripts o comandos:** como PowerShell, bash, VBScript.
    
- **Reconocimiento interno:** escaneo de red local, rutas, usuarios conectados, shares.


## ¿Cuántos módulos Post existen?

El **Metasploit Framework tiene más de 250 módulos `post`**, y están organizados según la **plataforma o entorno objetivo**.

## Categorías de módulos Post

|Categoría|Descripción|
|---|---|
|**Linux**|Módulos para sistemas Linux comprometidos (escalada, recolecta de credenciales, reconocimiento).|
|**Windows**|Tareas específicas para Windows: volcado de SAM, ejecución de PowerShell, UAC bypass, etc.|
|**OS X**|Post-exploitation en macOS: extracción de llaveros, historial, persistencia.|
|**Cisco**|Extracción de configuraciones, contraseñas y archivos desde dispositivos Cisco.|
|**Solaris**|Post-explotación en Solaris, muy útil en entornos legacy corporativos.|
|**Firefox**|Robo de cookies, historial, credenciales guardadas del navegador.|
|**Aix**|Utilidades post-exploitation para IBM AIX.|
|**Android**|Recolección de contactos, SMS, GPS, fotos, y más desde dispositivos Android comprometidos.|
|**Multi**|Módulos genéricos que aplican a múltiples plataformas (ej. hashdump, persistence, enum).|
|**Zip**|Posiblemente módulos asociados a extracción o manipulación de archivos ZIP encontrados.|
|**PowerShell**|Ejecución de scripts y comandos PowerShell en sesiones Windows.|
|**Juniper**|Post-exploitation para dispositivos o entornos con tecnología Juniper (firewalls, switches, etc.).|

## Ejemplo de uso de un módulo Post

Después de explotar un objetivo y obtener una sesión Meterpreter:

```bash
msf > sessions

[*] Active sessions
  Id  Name  Type                   Information                    Connection
  --  ----  ----                   ------------                    ----------
  1         meterpreter x86/win32  NT AUTHORITY\SYSTEM @ victim   192.168.1.10:4444 → 192.168.1.20:5000

msf > use post/windows/gather/enum_logged_on_users
msf post(enum_logged_on_users) > set SESSION 1
msf post(enum_logged_on_users) > run
```

Este módulo enumerará los usuarios conectados en la máquina Windows comprometida.
