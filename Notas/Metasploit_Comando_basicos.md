# Comandos Básicos

#### **1. Comandos de Gestión e Informativos del Entorno**

Estos comandos permiten gestionar el estado del framework y obtener información general sobre el entorno de trabajo.

- **`banner`**
    
    - **Descripción Técnica:** Despliega el banner de inicio de Metasploit. Este banner contiene información de estado crítico, incluyendo la versión del framework y un resumen cuantitativo de los módulos cargados (exploits, auxiliares, payloads, encoders, nops, etc.). Es útil para una verificación rápida del estado del entorno.
    - **Sintaxis:**
        
        ```bash
        msf> banner
        ```
        
- **`version`**
    
    - **Descripción Técnica:** Devuelve la versión específica del Metasploit Framework y del `msfconsole` instalados. Verificar la versión es crucial para asegurar la compatibilidad con módulos específicos, reproducir hallazgos o diagnosticar problemas relacionados con el framework.
    - **Sintaxis:**

        
        ```bash
        msf> version
        ```
        
- **`help`**
    
    - **Descripción Técnica:** Proporciona acceso a la documentación interna del framework. Ejecutado sin argumentos, lista todos los comandos principales. Si se especifica un comando como argumento (`help <comando>`), ofrece una descripción detallada de su sintaxis y opciones.
    - **Ejemplo Práctico:** Si no estás seguro de cómo utilizar los filtros avanzados del comando `search`, puedes ejecutar `msf> help search` para obtener una lista completa de los parámetros de búsqueda disponibles (`cve:`, `platform:`, `type:`, etc.).
- **`save`**
    
    - **Descripción Técnica:** Persiste la configuración actual del entorno de Metasploit. Esto incluye todas las variables globales y locales establecidas (ej. `LHOST`, `RHOSTS`), rutas de red para pivoteo, y el historial de la base de datos. Al reiniciar `msfconsole`, esta configuración se carga automáticamente, evitando la reconfiguración manual.
    - **Ejemplo Práctico:** Imagina que has configurado 5 variables globales para una red objetivo y has establecido una ruta de pivote a través de una sesión comprometida. En lugar de reconfigurar todo al día siguiente, simplemente ejecutas `msf> save`. El estado se guardará y restaurará en el próximo inicio.

---

#### **2. Comandos de Interacción con Módulos**

Estos comandos son el núcleo para encontrar, analizar y configurar los módulos del framework.

- **`search`**
    
    - **Descripción Técnica:** Es un potente motor de búsqueda para localizar módulos dentro de la base de datos del framework. Permite filtrar por nombre, tipo de módulo, plataforma, autor, número de CVE, entre otros.
    - **Sintaxis:**
        
        ```bash
        msf> search <palabra_clave:valor>
        ```
        
    - **Ejemplo Práctico:** Para encontrar un exploit específico para la vulnerabilidad "BlueKeep" en sistemas Windows, en lugar de una búsqueda genérica, puedes usar un filtro preciso: `msf> search type:exploit platform:windows cve:2019-0708`. Esto reduce drásticamente el ruido y acelera la selección del módulo.
- **`info`**
    
    - **Descripción Técnica:** Muestra información detallada sobre un módulo específico. Proporciona datos cruciales como el ranking de fiabilidad del exploit (`Rank`), las arquitecturas y plataformas objetivo, los privilegios que otorga, las opciones de configuración requeridas, y referencias a fuentes externas como CVEs y URLs de advisories.
    - **Sintaxis:**
        
        ```bash
        msf> info <ruta_del_modulo>
        ```
        
- **`show`**
    
    - **Descripción Técnica:** Comando contextual que, una vez que un módulo ha sido seleccionado con `use`, permite visualizar sus componentes. Se utiliza principalmente para listar las opciones de configuración (`show options`), los payloads compatibles (`show payloads`), los objetivos (`show targets`) o los módulos de un tipo específico (`show exploits`).
    - **Ejemplo Práctico:** Después de cargar un exploit con `use exploit/windows/smb/ms17_010_eternalblue`, el comando `msf> show targets` te listará los sistemas operativos y arquitecturas específicas que el exploit puede atacar, permitiéndote seleccionar el objetivo correcto para maximizar la probabilidad de éxito.

---

#### **3. Comandos de Red y Manejo de Sesiones**

Estos comandos son esenciales para interactuar con los sistemas objetivo y gestionar las conexiones post-explotación.

- **`connect`**
    
    - **Descripción Técnica:** Implementa un cliente TCP/IP básico, similar a Netcat o Telnet, directamente desde la consola. Es una herramienta eficaz para realizar sondeos de puertos rápidos (port scanning) y captura de banners (banner grabbing) sin necesidad de salir del entorno de Metasploit.
    - **Sintaxis:**
        
        ```bash
        msf> connect <IP> <puerto>
        ```
        
    - **Ejemplo Práctico:** Antes de lanzar un exploit contra un servidor web, puedes verificar si el puerto 80 está abierto y qué software está corriendo. `msf> connect 10.10.10.5 80`. Si la respuesta es `HTTP/1.1 200 OK Server: nginx/1.18.0`, has confirmado que el puerto está abierto y has identificado el software del servidor, lo que te permite ajustar tu ataque.
- **`sessions`**
    
    - **Descripción Técnica:** Lista y gestiona las sesiones activas post-explotación (ej. Meterpreter, shell). Permite interactuar con una sesión específica, ejecutar comandos en todas las sesiones simultáneamente, o finalizar conexiones.
    - **Sintaxis:**
        
        ```bash
        msf> sessions [opciones]
        ```
        
    - **Ejemplo Práctico:** Si has comprometido tres servidores, `sessions -l` te mostrará las sesiones con IDs 1, 2 y 3. Para interactuar con el segundo servidor, usas `sessions -i 2`. Para comprobar qué usuario está activo en todos los sistemas comprometidos a la vez, puedes ejecutar `sessions -C whoami`.
- **`route`**
    
    - **Descripción Técnica:** Gestiona la tabla de enrutamiento del framework para realizar técnicas de **pivoteo**. Permite dirigir el tráfico de Metasploit a través de un host comprometido para alcanzar redes internas que no son directamente accesibles.
    - **Ejemplo Práctico:** Has comprometido un servidor en la DMZ (`192.168.1.100`) que tiene una segunda interfaz de red a la red interna (`10.0.0.0/24`). Tras obtener una sesión (ID 1), ejecutas `route add 10.0.0.0 255.255.255.0 1`. A partir de ese momento, cualquier módulo de Metasploit que configures para atacar el rango `10.0.0.0/24` será canalizado automáticamente a través del host comprometido.

---

#### **4. Comandos de Automatización, Scripting y Registro**

Estos comandos están diseñados para automatizar tareas repetitivas y mantener un registro de las actividades realizadas, lo cual es fundamental para la generación de informes y la reproducibilidad.

- **`spool`**
    
    - **Descripción Técnica:** Redirige toda la salida de la consola a un archivo de registro en tiempo real. Es indispensable para la documentación y auditoría de un pentest, creando una transcripción completa de todos los comandos ejecutados y sus resultados.
    - **Sintaxis:**
        
        ```bash
        msf> spool <ruta_del_archivo>
        ```
        
- **`makerc`**
    
    - **Descripción Técnica:** Guarda el historial de comandos ejecutados en la sesión actual en un archivo de recurso (`.rc`). A diferencia de `spool`, que guarda la salida, `makerc` guarda únicamente los comandos. Este archivo puede ser ejecutado posteriormente para automatizar una cadena de ataque completa.
    - **Sintaxis:**
        
        ```bash
        msf> makerc <ruta_del_archivo.rc>
        ```
        
    - **Ejemplo Práctico:** Después de explotar un sistema con una secuencia de 10 comandos, ejecutas `makerc exploit_flow.rc`. Para repetir el mismo ataque en otro momento, simplemente inicias Metasploit con `msfconsole -r exploit_flow.rc`, y todos los comandos se ejecutarán automáticamente.
- **`irb`**
    
    - **Descripción Técnica:** Inicia una sesión de **Interactive Ruby (IRB)** dentro del contexto de Metasploit. Esto otorga acceso directo a la API del framework y a los objetos de la sesión activa (como el objeto `client` de Meterpreter) para ejecutar scripts de Ruby complejos y realizar tareas de post-explotación altamente personalizadas que no están cubiertas por módulos existentes.
    - **Ejemplo Práctico:** Si necesitas exfiltrar datos de una manera muy específica o interactuar con una aplicación custom en el host comprometido, puedes usar `irb` para escribir un script Ruby sobre la marcha que manipule el sistema de archivos o la red a través de la API de Meterpreter.