# Estructura interna
## ¿Dónde Encontrar el Directorio de Metasploit?

La ubicación del directorio de Metasploit Framework depende de cómo lo hayas instalado y del sistema operativo que estés utilizando.

- Kali Linux y otras distribuciones de pentesting:
    
    En sistemas como Kali Linux, donde Metasploit viene preinstalado, la ruta más común es:
    
    ```
    /usr/share/metasploit-framework/
    ```
    
- Instalación manual desde GitHub:
    
    Si clonaste el repositorio directamente desde GitHub, el directorio estará en la ubicación donde ejecutaste el comando git clone. Por ejemplo, si lo clonaste en tu carpeta de inicio, la ruta sería:
    
    ```
    ~/metasploit-framework/
    ```
    
- Instalación con el instalador oficial:
    
    Si usaste el instalador proporcionado por Rapid7 (la empresa detrás de Metasploit), la ubicación suele ser:
    
    - **Linux:** `/opt/metasploit-framework/`
        
    - **Windows:** `C:\metasploit-framework\`
        
    - **macOS:** `/opt/metasploit-framework/`
        

### Comando para encontrarlo

Si no estás seguro de la ubicación, puedes usar el siguiente comando en Linux o macOS para buscar el directorio:

```shell
find / -type d -name "metasploit-framework" 2>/dev/null
```

Este comando buscará en todo el sistema de archivos un directorio llamado `metasploit-framework`. ¡Ten paciencia, ya que puede tardar un poco!



### Estructura interna

- **`/modules`**: Este es el corazón del framework. Contiene el código fuente de todos los módulos que utilizas. Está organizado en subdirectorios según el tipo de módulo:
    
    - **`/exploits`**: Contiene los _exploits_, que son los códigos que aprovechan una vulnerabilidad específica en un sistema. Se organizan por sistema operativo (Windows, Linux, etc.) y luego por el servicio o aplicación vulnerable.
        
    - **`/payloads`**: Contiene los _payloads_, que es el código que se ejecuta en el sistema objetivo después de que un _exploit_ ha tenido éxito. Pueden ser desde una simple shell hasta el avanzado Meterpreter.
        
    - **`/auxiliary`**: Incluye una variedad de herramientas que no son _exploits_ directos. Aquí encontrarás escáneres de puertos, _fuzzers_, sniffers y otras utilidades para recopilar información.
        
    - **`/post`**: Contiene módulos que se ejecutan después de haber comprometido un sistema (post-explotación). Sirven para escalar privilegios, recopilar información adicional o pivotar a otros sistemas en la red.
        
    - **`/encoders`**: Se utilizan para codificar los _payloads_ y _exploits_ para evitar ser detectados por sistemas de seguridad como los antivirus (AV) o los sistemas de detección de intrusos (IDS).
        
    - **`/nops`** (No OPeration): Generadores de NOPs (instrucciones que no hacen nada) que se usan para rellenar la memoria y asegurar que el _payload_ se ejecute correctamente.
        
    - **`/evasion`**: Módulos diseñados específicamente para evadir software de seguridad.
        
- **`/data`**: Almacena archivos de datos adicionales que los módulos pueden necesitar. Esto incluye listas de palabras para ataques de fuerza bruta, plantillas para informes y otros recursos como los binarios de Meterpreter.
    
- **`/lib`**: Contiene las bibliotecas principales del framework. Es el motor que hace que todo funcione, manejando desde la comunicación de red hasta la implementación de los diferentes protocolos.
    
- **`/scripts`**: Aquí se guardan varios scripts, principalmente los de Meterpreter, que son scripts post-explotación que puedes ejecutar una vez que tienes una sesión activa en la máquina víctima.
    
- **`/tools`**: Incluye herramientas y utilidades de línea de comandos que son útiles para el desarrollo de _exploits_ y otras tareas relacionadas con la seguridad.
    
- **`/plugins`**: Permite extender la funcionalidad de Metasploit. Puedes cargar plugins al inicio para integrar herramientas externas o añadir nuevos comandos a la consola.
