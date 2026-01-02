# Metasploit - Payloads

En el contexto de Metasploit, un **payload** es el código arbitrario que se desea ejecutar en un sistema comprometido. Es la "carga útil" que define la acción que tomará el atacante una vez que el exploit ha abierto una brecha. Esto puede ser tan simple como ejecutar un comando, o tan complejo como establecer una sesión interactiva (como Meterpreter).

Los directorios que ha listado dentro de `/usr/share/metasploit-framework/modules/payloads/` organizan los payloads según su estructura y cómo se entregan y ejecutan en el objetivo:

### Submódulos de Payloads

1. **`adapters`**:
    
    - **Descripción**: Los adaptadores son módulos que "envuelven" payloads existentes para convertirlos a diferentes formatos o para adaptar su método de entrega. No son payloads por sí mismos, sino transformadores que permiten que un payload funcione en un contexto específico. Por ejemplo, un adaptador podría convertir un payload de shellcode binario en un comando de PowerShell o un script SMB.
        
    - **Uso típico**: Adaptar payloads para usar diferentes protocolos de entrega (ej. SMB) o para ser ejecutados por un intérprete específico (ej. PowerShell).
        
2. **`singles`** (también conocidos como payloads "no staged" o "inline"):
    
    - **Descripción**: Son payloads **autocontenidos y completamente independientes**. Significan que todo el código necesario para su funcionalidad (ej. abrir una shell, añadir un usuario) está incluido en una sola pieza de código. No necesitan descargar componentes adicionales una vez que se ejecutan.
        
    - **Ventajas**: Son más estables porque no dependen de una conexión posterior para descargar más código.
        
    - **Desventajas**: Su tamaño puede ser considerable, lo que los hace inadecuados para exploits que tienen restricciones de espacio muy pequeñas para el shellcode inicial.
        
    - **Uso típico**: Cuando se dispone de suficiente espacio en el exploit para el payload completo o cuando se prefiere la simplicidad y estabilidad a la interacción en etapas. Un ejemplo es `windows/shell_bind_tcp`.
        
3. **`stagers`**:
    
    - **Descripción**: Los stagers son payloads **pequeños y minimalistas** cuyo único propósito es establecer una conexión de red con la máquina del atacante y luego descargar el "stage" (la parte principal y más grande del payload). Son la primera parte de un payload "staged" (en etapas).
        
    - **Ventajas**: Su tamaño reducido los hace ideales para exploits con limitaciones de espacio de shellcode. Son muy rápidos de entregar.
        
    - **Desventajas**: Requieren una conexión de red activa entre el objetivo y Metasploit para descargar la siguiente etapa.
        
    - **Uso típico**: En exploits donde el espacio para el shellcode es limitado (ej. desbordamientos de búfer pequeños). Un ejemplo es `windows/shell/reverse_tcp` (donde `reverse_tcp` es el stager).
        
4. **`stages`**:
    
    - **Descripción**: Son los **componentes de payload más grandes y con todas las funciones** que son descargados por los `stagers`. Una vez que el stager ha establecido la conexión, descarga el stage en la memoria del sistema objetivo y transfiere el control a él. Los stages proporcionan funcionalidades avanzadas y completas.
        
    - **Ejemplos Comunes**:
        
        - **Meterpreter**: El payload más avanzado y flexible de Metasploit, que ofrece una shell interactiva muy potente con muchas funcionalidades post-explotación (carga de módulos, captura de pantalla, keylogging, etc.).
            
        - **VNC Injection**: Permite controlar el escritorio remoto del sistema objetivo.
            
        - **Command Shells**: Proporcionan una shell de comandos estándar (ej. `cmd.exe` o `bash`).
            
    - **Ventajas**: Permiten payloads muy grandes y ricos en funciones sin las limitaciones de tamaño del exploit inicial.
        
    - **Uso típico**: Proporcionar control completo y avanzado sobre el sistema comprometido.


