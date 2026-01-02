# Metasploit - Auxiliary
No todos los módulos en Metasploit están diseñados para obtener acceso no autorizado. Los auxiliares se centran más en **escaneo, enumeración, recolección de información, fuzzing**, entre otros.

## Submódulos Auxiliares de Metasploit

1. **`admin`**:
    
    - **Descripción**: Contiene módulos diseñados para realizar diversas tareas administrativas o de post-explotación en sistemas comprometidos o con credenciales conocidas. Esto puede incluir el control de servicios, manipulación de registros, gestión de usuarios, o la ejecución de comandos con privilegios.
        
    - **Uso típico**: Gestión de sistemas tras la explotación, interacción con sistemas de gestión de bases de datos, o ejecución de comandos específicos en sistemas remotos.
        
2. **`analyze`**:
    
    - **Descripción**: Módulos dedicados al análisis de datos recopilados o la realización de tareas de análisis forense ligero. Pueden ayudar a procesar logs, volcado de memoria o paquetes de red para extraer información útil.
        
    - **Uso típico**: Procesamiento de información de inteligencia recopilada durante una operación.
        
3. **`bnat`**:
    
    - **Descripción**: Relacionado con la traducción de direcciones de red (NAT) y técnicas para eludir o interactuar con dispositivos NAT/firewalls. Puede contener módulos para identificar configuraciones NAT o para el "port forwarding" a través de NAT.
        
    - **Uso típico**: Evaluación de la configuración de red y la capacidad de atravesar dispositivos NAT.
        
4. **`client`**:
    
    - **Descripción**: Módulos que interactúan con aplicaciones del lado del cliente. Estos módulos suelen apuntar a vulnerabilidades en navegadores web, clientes de correo electrónico, reproductores multimedia u otras aplicaciones de usuario final.
        
    - **Uso típico**: Creación de archivos maliciosos (ej. PDF, Office) o configuración de servidores web que sirven contenido diseñado para explotar vulnerabilidades en clientes.
        
5. **`cloud`**:
    
    - **Descripción**: Módulos enfocados en la interacción y evaluación de la seguridad de servicios y entornos basados en la nube (ej. AWS, Azure, Google Cloud). Pueden buscar configuraciones erróneas, credenciales expuestas o vulnerabilidades específicas de plataformas en la nube.
        
    - **Uso típico**: Auditoría de configuraciones de seguridad en entornos de nube.
        
6. **`crawler`**:
    
    - **Descripción**: Módulos utilizados para "rastrear" o indexar sitios web y aplicaciones web, similar a un spider de motor de búsqueda. Recopilan información sobre la estructura del sitio, enlaces, archivos y posibles puntos de entrada para ataques.
        
    - **Uso típico**: Recopilación de inteligencia sobre aplicaciones web antes de intentar la explotación.
        
7. **`docx`**:
    
    - **Descripción**: Módulos específicos para la creación o manipulación de documentos en formato DOCX (Microsoft Word) con fines maliciosos, a menudo para incrustar exploits o macros maliciosas.
        
    - **Uso típico**: Preparación de documentos para ataques de ingeniería social.
        
8. **`dos`**:
    
    - **Descripción**: Contiene módulos diseñados para realizar ataques de Denegación de Servicio (DoS). Estos módulos buscan explotar vulnerabilidades que pueden hacer que un servicio o sistema se bloquee, se ralentice drásticamente o deje de responder, agotando sus recursos.
        
    - **Uso típico**: Prueba de la resiliencia de los servicios ante ataques de agotamiento de recursos o fallos específicos.
        
9. **`fileformat`**:
    
    - **Descripción**: Módulos que generan archivos maliciosos en varios formatos (ej. PDF, Office, imágenes) que, al ser abiertos por una víctima, pueden explotar vulnerabilidades en las aplicaciones que los manejan.
        
    - **Uso típico**: Creación de cargas útiles para ataques de ingeniería social dirigidos a aplicaciones específicas.
        
10. **`fuzzers`**:
    
    - **Descripción**: Módulos auxiliares que realizan tareas de fuzzing básicas. Inyectan datos aleatorios o malformados en entradas de aplicaciones o servicios para descubrir fallos o vulnerabilidades que pueden llevar a un crash o a un comportamiento inesperado.
        
    - **Uso típico**: Descubrimiento preliminar de fallos en servicios de red o aplicaciones.
        
11. **`gather`**:
    
    - **Descripción**: Una categoría muy amplia de módulos dedicada a la **recopilación de información** (reconocimiento) sobre el objetivo. Esto puede incluir escaneo de puertos, enumeración de usuarios, recopilación de banners, análisis de configuración, etc. Es uno de los submódulos más utilizados en la fase inicial de un pentest.
        
    - **Uso típico**: Enumeración de objetivos, recopilación de credenciales, información de red y del sistema.
        
12. **`parser`**:
    
    - **Descripción**: Módulos que se utilizan para analizar o "parsear" diferentes tipos de datos o formatos de archivo, extrayendo información relevante. Pueden ser útiles para procesar logs, volcados de memoria o formatos específicos para encontrar datos de interés.
        
    - **Uso típico**: Extracción de datos de configuraciones, hashes o credenciales de archivos o flujos de datos.
        
13. **`pdf`**:
    
    - **Descripción**: Módulos específicos para la creación o manipulación de documentos PDF, a menudo para incrustar payloads maliciosos o exploits que se activan al abrir el archivo con un lector de PDF vulnerable.
        
    - **Uso típico**: Ataques de ingeniería social que distribuyen PDF maliciosos.
        
14. **`scanner`**:
    
    - **Descripción**: Contiene una vasta colección de módulos utilizados para realizar escaneos de red, descubrir servicios, identificar versiones de software, enumerar puertos, buscar vulnerabilidades específicas (sin explotarlas directamente) y realizar otras tareas de reconocimiento activo.
        
    - **Uso típico**: Detección de servicios vulnerables, escaneo de puertos, descubrimiento de dispositivos en la red.
        
15. **`server`**:
    
    - **Descripción**: Módulos que implementan servicios de servidor (ej. HTTP, FTP, SMB) dentro de Metasploit, a menudo para alojar payloads maliciosos, realizar ataques de "man-in-the-middle" o recolectar información de clientes que se conectan.
        
    - **Uso típico**: Configuración de servidores para entregar payloads, o para ataques de phishing o recolección de credenciales.
        
16. **`sniffer`**:
    
    - **Descripción**: Módulos diseñados para capturar y analizar el tráfico de red. Pueden ser utilizados para interceptar credenciales, datos sensibles o para comprender el comportamiento de la red.
        
    - **Uso típico**: Escucha pasiva de redes para recopilar información.
        
17. **`spoof`**:
    
    - **Descripción**: Módulos que realizan ataques de suplantación (spoofing), como suplantación de ARP, DNS o identidades de red. Se utilizan para redirigir el tráfico, engañar a los sistemas o interceptar comunicaciones.
        
    - **Uso típico**: Ataques de hombre en el medio (MITM) o manipulación de la resolución de nombres.
        
18. **`sqli`**:
    
    - **Descripción**: Módulos especializados en la detección y explotación de vulnerabilidades de inyección SQL (SQL Injection) en aplicaciones web. Pueden automatizar la inyección de código SQL para extraer información de bases de datos o manipularlas.
        
    - **Uso típico**: Auditoría de seguridad de bases de datos a través de aplicaciones web.
        
19. **`voip`**:
    
    - **Descripción**: Módulos enfocados en la evaluación de la seguridad de sistemas de Voz sobre IP (VoIP), como SIP o H.323. Pueden incluir funciones para el escaneo de servicios VoIP, enumeración de extensiones o ataques específicos a estos protocolos.
        
    - **Uso típico**: Auditoría de sistemas de comunicación de voz.
        
20. **`vsploit`**:
    
    - **Descripción**: (Menos común o más específico) Esta categoría podría contener módulos relacionados con vulnerabilidades específicas o herramientas desarrolladas para un propósito muy particular que no encaja fácilmente en otras categorías, o quizás módulos para interactuar con entornos de laboratorio o "vulnerable by design" como el `vulnerable.jar` utilizado en algunos contextos.
        
    - **Uso típico**: Puede variar, pero generalmente para interacciones o pruebas muy específicas que requieren un contexto particular.
        

