# Recopilación de información y enumeración

## Protocolo de Transferencia de Archivos (FTP)

El **Protocolo de Transferencia de Archivos** (FTP) se utiliza comúnmente para compartir archivos entre el cliente y el servidor. FTP utiliza el puerto TCP 21 para la comunicación.

- `auxiliary/scanner/ftp/ftp_login` 
	- Este módulo nos ayuda a realizar un **ataque de fuerza bruta** contra el servidor FTP objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.
	- **USERPASS_FILE**: Ruta al archivo que contiene la lista de nombres de usuario/contraseñas.
	>Puedes crear tu propia lista personalizada para ser usada en el ataque de fuerza bruta, o utilizar una de las muchas listas de palabras (_wordlists_) ya disponibles en Kali Linux, ubicadas en `/usr/share/wordlists`.


- `auxiliary/scanner/ftp/ftp_version`
	- Este módulo utiliza la técnica de **_banner grabbing_** para detectar la versión del servidor FTP objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.


- `auxiliary/scanner/ftp/anonymous`
	- Algunos servidores FTP están mal configurados de tal manera que permiten el acceso anónimo a usuarios remotos. Este módulo auxiliar sondea el servidor FTP objetivo para verificar si permite el acceso anónimo.

## Server Message Block (SMB)

**Server Message Block** (SMB) es un protocolo de la capa de aplicación utilizado principalmente para compartir archivos, impresoras, etc. SMB utiliza el puerto TCP 445 para la comunicación.


- `auxiliary/scanner/smb/smb_version`
	- Este módulo auxiliar sondea el objetivo para verificar qué versión de SMB está ejecutando.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.


- `auxiliary/scanner/smb/smb_enumusers`
	- Este módulo auxiliar se conecta al sistema objetivo a través del servicio SMB RPC y enumera los usuarios del sistema.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/smb/smb_enumshares`
	- Este módulo auxiliar enumera los recursos compartidos (_shares_) de SMB que están disponibles en el sistema objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

## Protocolo de Transferencia de Hipertexto (HTTP)

El **Protocolo de Transferencia de Hipertexto** (HTTP) es un protocolo de la capa de aplicación sin estado (_stateless_) utilizado para el intercambio de información en la World Wide Web. HTTP utiliza el puerto TCP 80 para la comunicación.

- `auxiliary/scanner/http/http_version`
	- Este módulo auxiliar sondea y recupera la versión del servidor web que se ejecuta en el sistema objetivo. También puede proporcionar información sobre el sistema operativo y el _framework_ web que utiliza el objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/http/backup_file`
	- A veces, los desarrolladores y los administradores de aplicaciones olvidan eliminar los archivos de copia de seguridad del servidor web. Este módulo auxiliar sondea el servidor web objetivo en busca de la presencia de dichos archivos. Estos ficheros pueden revelar detalles adicionales sobre el sistema objetivo y ayudar en un compromiso posterior.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/http/backup_file`
	- Con bastante frecuencia, el servidor web está mal configurado para mostrar la lista de archivos que contiene el directorio raíz. El directorio puede contener archivos que normalmente no están expuestos a través de enlaces en el sitio web y filtrar información sensible. Este módulo auxiliar comprueba si el servidor web objetivo es vulnerable al **listado de directorios**.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.
	- **PATH**: Ruta posible para verificar el listado de directorios.

- `auxiliary/scanner/http/ssl`
	- Aunque los certificados SSL se utilizan muy comúnmente para cifrar datos en tránsito, a menudo se encuentran mal configurados o utilizan algoritmos de criptografía débiles. Este módulo auxiliar verifica posibles debilidades en el certificado SSL instalado en el sistema objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/http/http_header`
	- La mayoría de los servidores web no están reforzados (_hardened_) en materia de seguridad. Esto resulta en que las **cabeceras HTTP** filtren detalles de la versión del servidor y del sistema operativo. Este módulo auxiliar comprueba si el servidor web objetivo está revelando información de versión a través de las cabeceras HTTP.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/http/robots_txt`
	- La mayoría de los motores de búsqueda funcionan con la ayuda de bots que rastrean (_spider/crawl_) los sitios e indexan las páginas. Sin embargo, es posible que un administrador no quiera que una determinada sección de su sitio web sea rastreada. En este caso, utiliza el archivo `robots.txt` para indicar a los bots de búsqueda que excluyan ciertas secciones del sitio durante el rastreo. Este módulo auxiliar sondea el objetivo para comprobar la presencia del archivo `robots.txt`. Este archivo a menudo puede revelar una lista de archivos y carpetas sensibles presentes en el sistema objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/http/cert` 
	- Está diseñado para determinar la validez y el estado de los certificados SSL/TLS de una aplicación web. La información sobre la caducidad o configuraciones incorrectas de certificados es vital para evaluar la seguridad de la capa de transporte.
- `auxiliary/scanner/http/dir_scanner`
	- Busca la existencia de directorios comunes y potencialmente sensibles en el servidor web. La exposición de ciertos directorios puede revelar información valiosa, como archivos de configuración, copias de seguridad de bases de datos o paneles de administración.
- `auxiliary/scanner/http/files_dir`
	- Complementa el escaneo de directorios buscando archivos específicos que no deberían estar accesibles en un entorno de producción, como código fuente, archivos de copia de seguridad (`.bak`), archivos de configuración (`.conf`, `.cfg`), o archivos de contraseñas.

- `auxiliary/scanner/http/options`
	- Verifica qué métodos HTTP (como `OPTIONS`, `TRACE`, `GET`, `POST`, `HEAD`) están habilitados en el servidor web de destino. Métodos como `TRACE` pueden ser utilizados en ataques de Cross-Site Tracing (XST) para la fuga de cookies o credenciales.

- `auxiliary/scanner/http/enum_wayback`
	- Interactúa con `archive.org` (Wayback Machine) para recuperar versiones históricas y datos de un sitio web. Esto puede revelar URLs antiguas, archivos olvidados o cambios en la infraestructura que podrían contener vulnerabilidades o información sensible.
	- **`DOMAIN`**: El nombre de dominio del objetivo a consultar en Wayback Machine.
	- **`OUTFILE`**: Ruta opcional para guardar la lista de URLs recuperadas.

- `auxiliary/scanner/http/http_login`
	-  intenta realizar ataques de fuerza bruta contra la autenticación basada en HTTP (Basic o Digest) si está habilitada en el sistema objetivo. Utiliza diccionarios de usuarios y contraseñas predeterminados de Metasploit.
	- **`RHOSTS`**: Define la dirección IP o rango de IP del objetivo.
	- **`USERPASS_FILE`**: Ruta a un archivo personalizado con pares de usuario:contraseña.
	- **`USER_FILE`**: Ruta a un archivo personalizado con solo nombres de usuario.
	- **`PASS_FILE`**: Ruta a un archivo personalizado con solo contraseñas.
	- **`BRUTEFORCE_SPEED`**: Controla la velocidad del ataque (0-5).

## Protocolo Simple de Transferencia de Correo (SMTP)

- `auxiliary/scanner/smtp/smtp_enum`
	- SMTP se utiliza para enviar y recibir correos electrónicos. SMTP utiliza el puerto TCP 25 para la comunicación. Este módulo auxiliar sondea el servidor SMTP en el sistema objetivo para obtener su versión y enumera los usuarios configurados para utilizar el servicio SMTP.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.
	- **USER_FILE**: Ruta al archivo que contiene una lista de nombres de usuario.

## Secure Shell (SSH)

SSH se utiliza comúnmente para la administración remota a través de un canal cifrado. SSH utiliza el puerto TCP 22 para la comunicación.

- `auxiliary/scanner/ssh/ssh_enumusers`
	- Este módulo auxiliar sondea el servidor SSH en el sistema objetivo para obtener una lista de usuarios (configurados para funcionar con el servicio SSH) en el sistema remoto.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.
	- **USER_FILE**: Ruta al archivo que contiene una lista de nombres de usuario.

- `auxiliary/scanner/ssh/ssh_login`
	- Este módulo auxiliar realiza un ataque de fuerza bruta en el servidor SSH objetivo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.
	- **USERPASS_FILE**: Ruta al archivo que contiene una lista de nombres de usuario y contraseñas.

- `auxiliary/scanner/ssh/ssh_version`
	- Este módulo auxiliar sondea el servidor SSH objetivo para detectar su versión junto con la versión del sistema operativo subyacente.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

- `auxiliary/scanner/ssh/detect_kippo`
	- Kippo es un _honeypot_ basado en SSH que está especialmente diseñado para atraer y atrapar a atacantes potenciales. Este módulo auxiliar sondea el servidor SSH objetivo para detectar si es un servidor SSH real o simplemente un _honeypot_ Kippo. Si se detecta que el objetivo está ejecutando un _honeypot_ Kippo, no tiene sentido perder tiempo y esfuerzo en comprometerlo más a fondo.
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

## Sistema de Nombres de Dominio (DNS)

El Sistema de Nombres de Dominio (DNS) se encarga de traducir los nombres de host a sus direcciones IP correspondientes. Normalmente, DNS funciona sobre el puerto UDP 53, pero también puede operar sobre TCP. 

- `auxiliary/gather/dns_info`
	- Este módulo auxiliar puede ser utilizado para extraer información de los servidores de nombres (_name server_) y registros de correo (_mail record_) del servidor DNS objetivo.
	- **DOMAIN**: Nombre de dominio del objetivo a escanear.

## Protocolo de Escritorio Remoto (RDP)

El Protocolo de Escritorio Remoto (RDP) se utiliza para conectarse de forma remota a un sistema Windows. RDP utiliza el puerto TCP 3389 para la comunicación. 

- `auxiliary/scanner/rdp/ms12_020`
	- Este módulo auxiliar comprueba si el sistema objetivo es vulnerable a **MS12-020**. MS12-020 es una vulnerabilidad en el Escritorio Remoto de Windows que permite a un atacante ejecutar código arbitrario de forma remota. Puede encontrar más información sobre la vulnerabilidad MS12-020 en [https://technet.microsoft.com/en-us/library/security/ms12-020.aspx](https://technet.microsoft.com/en-us/library/security/ms12-020.aspx).
	- **RHOSTS**: Dirección IP o rango de IPs del objetivo a escanear.

## Simple Network Management Protocol (SNMP)

Es fundamental para la administración y monitorización remota de dispositivos de red.

SNMP expuesto en redes corporativas puede permitir:

- Enumeración de interfaces de red y tablas de enrutamiento.
- Detección de usuarios locales y tiempo de actividad del dispositivo (_uptime_).
- Extracción de configuraciones de _routers_, _switches_ y otros dispositivos.
- Posible modificación de configuraciones en dispositivos con _community strings_ de escritura (`RW`) débiles.

> ❗ Es crucial destacar que **SNMPv1 y SNMPv2 son inherentemente inseguros** al utilizar _community strings_ (equivalentes a contraseñas) en texto plano. Solo **SNMPv3** incorpora cifrado y autenticación robusta.

- `auxiliary/scanner/snmp/snmp_login` 
	- permite realizar ataques de diccionario para descubrir _community strings_ SNMP válidos, tanto de solo lectura (`RO`) como de lectura/escritura (`RW`).

## Sniffing de Contraseñas (Password sniffing)

- `auxiliary/sniffer/psnuffle`
	- El _sniffing_ de contraseñas es un tipo especial de módulo auxiliar que se pone a la escucha en la interfaz de red y busca contraseñas enviadas a través de varios protocolos como FTP, IMAP, POP3 y SMB. También proporciona una opción para importar tráfico de red previamente volcado en formato `.pcap` y buscar credenciales dentro de él.

## Búsqueda avanzada con Shodan
Para integrar Shodan con el Metasploit Framework, primero necesita registrarse en [https://www.shodan.io](https://www.shodan.io). Una vez registrado, puede obtener la clave API (_API key_) desde la sección _Account Overview_ (Resumen de la Cuenta), como se muestra a continuación:

El nombre de su módulo auxiliar es `auxiliary/gather/shodan_search`, y este módulo auxiliar se conecta al motor de búsqueda Shodan para lanzar consultas desde `msfconsole` y obtener los resultados de la búsqueda.

Tendrá que configurar los siguientes parámetros:

- **SHODAN_APIKEY**: La clave API de Shodan disponible para los usuarios registrados de Shodan.
    
- **QUERY**: Palabra clave a buscar.

## Escáneres de Puertos
- `auxiliary/scanner/portscan/syn`: Realiza un **escaneo SYN** (medio abierto), rápido y sigiloso.
- `auxiliary/scanner/portscan/tcp`: Realiza un **escaneo TCP completo**, estableciendo la conexión completa para cada puerto.
- `auxiliary/scanner/portscan/xmas`: Envía paquetes con las banderas FIN, URG y PSH activadas, a menudo utilizado para evadir _firewalls_ simples.
- `auxiliary/scanner/portscan/ack`: Utilizado principalmente para **mapear reglas de _firewall_** y detectar filtrado de puertos.

## Detección de Servidores Microsoft SQL Vulnerables

- `auxiliary/scanner/mssql/mssql_ping`
	- Las instancias de MS SQL Server, especialmente aquellas con configuraciones predeterminadas o débiles, pueden exponer información sensible o incluso permitir acceso remoto no autenticado.
	- Este módulo permite detectar rápidamente:
		- Nombre del servidor SQL.
		- Nombre de la instancia de SQL Server.
		- Versión exacta de SQL Server.
		- Puerto TCP en uso por la instancia.