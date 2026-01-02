# Escaneo de aplicaciones web


**Escaneo de vulnerabilidades en aplicaciones web** utilizando el **plugin WMAP** (Web Mapping) integrado en el **Metasploit Framework**, una herramienta fundamental para la evaluación de la seguridad.

### 1. Inicialización y Configuración de WMAP

La primera etapa en el proceso de escaneo con WMAP implica la **activación y configuración del plugin** dentro del entorno de la consola de Metasploit (`msfconsole`).

#### 1.1 Carga del Plugin WMAP
Para iniciar las funcionalidades de WMAP, es imperativo cargar el módulo correspondiente. Esto se logra mediante el comando `load wmap` dentro de la `msfconsole`.

```sh
msf > load wmap
[*] Successfully loaded plugin: wmap
```

_Esta acción inicializa el entorno de WMAP, integrándolo en la sesión actual de Metasploit._

#### 1.2 Definición del Espacio de Trabajo y Objetivo

Una vez que WMAP está activo, se requiere la **creación de un sitio (workspace)** y la **especificación del objetivo** de escaneo. Esto permite organizar los resultados y enfocar el análisis en una aplicación web particular.

- **Creación de un Nuevo Sitio:** Se utiliza el comando `wmap_sites -a <IP_del_sitio / Nombre_de_host>` para registrar un nuevo objetivo de escaneo. Este comando establece el contexto para futuras operaciones de WMAP.

	Para un sitio alojado en la dirección IP 192.168.44.133:
    
    ```sh
    msf > wmap_sites -a 192.168.44.133
    [*] Site created.
    ```
    
- **Especificación del Target URL:** La URL específica de la aplicación web a analizar se define mediante el comando `wmap_targets -t <URL_de_destino>`. Esto asegura que WMAP dirija sus pruebas a la ruta correcta dentro del sitio.
    
    Para un entorno de prueba como Mutillidae alojado en la IP anterior:
    
    ```sh
    msf > wmap_targets -t http://192.168.44.133/mutillidae/index.php
    ```
    
- **Verificación de Targets Definidos:** Para confirmar que los objetivos se han configurado correctamente, se puede emplear el comando `wmap_targets -l`. Este comando lista todos los targets actualmente definidos, mostrando su ID, Vhost, Host, Puerto, estado SSL y Path.
    
    ```sh
    msf > wmap_targets -l
    [*] Defined targets
    Id  Vhost         Host          Port  SSL    Path
    --  ----------    ----------    ----  ---    ----
    0   192.168.44.133  192.168.44.133  80    false  /mutillidae/index.php
    ```
    
    _Este resultado valida la configuración del target, confirmando que la aplicación web es accesible a través del puerto 80 y no utiliza SSL en este escenario particular._
    

### 2. Ejecución del Escaneo de Vulnerabilidades

Con el sitio y el target debidamente configurados, se procede a la ejecución de las pruebas de vulnerabilidad utilizando los módulos de escaneo de WMAP.

#### 2.1 Enumeración de Módulos Aplicables

Antes de iniciar un escaneo exhaustivo, es recomendable identificar qué módulos de WMAP son relevantes para el objetivo. Esto optimiza el tiempo de escaneo y evita la ejecución de pruebas innecesarias (por ejemplo, pruebas SSL en un sitio sin HTTPS). El comando `wmap_run -t` realiza una evaluación preliminar del target y enumera los módulos que WMAP considera aplicables.

```sh
msf > wmap_run -t
[*] Testing target:
    Site: 192.168.44.133 (192.168.44.133)
    Port: 80 SSL: false
[*] Testing started. 2017-05-15 22:44:33 -0400
[*] Loading wmap modules...
    40 wmap enabled modules
[=] SSL testing ]=
[*] Target is not SSL. SSL modules disabled.
[=] Web Server testing ]=
[*] Module auxiliary/scanner/http/http_version
[*] Module auxiliary/admin/http/tomcat_administration
[*] Module auxiliary/admin/http/tomcat_utf8_traversal
...
```

_Este output detalla los módulos que se ejecutarán, como la detección de versiones HTTP (`http_version`), intentos de acceso a paneles de administración (`tomcat_administration`, `drupal_views_user_enum`), y la exploración de archivos comunes (`robots_txt`, `sitemap`). También se observa la deshabilitación de pruebas SSL, lo cual es eficiente para un target HTTP._

#### 2.2 Ejecución de Pruebas de Vulnerabilidad

Una vez validados los módulos aplicables, se inicia el escaneo real utilizando el comando `wmap_run -e`. Este comando activa la ejecución de los módulos de prueba contra el objetivo definido.

```sh
msf > wmap_run -e
[*] Using ALL wmap enabled modules.
[*] NO WMAP NODES DEFINED. Executing local modules
[*] Testing target:
    Site: 192.168.44.133 (192.168.44.133)
    Port: 80 SSL: false
[*] Testing started. 2017-05-15 22:53:06 -0400
...
[*] Module auxiliary/scanner/http/http_version
    192.168.44.133:80 Apache/2.2.8 (Ubuntu) DAV/2 (Powered by PHP/5.2.4-2ubuntu5.10 )
...
[*] Module auxiliary/scanner/http/trace
    192.168.44.133:80 is vulnerable to Cross-Site Tracing
...
```

_Durante esta fase, WMAP ejecuta activamente las pruebas de los módulos cargados. El resultado muestra hallazgos específicos, como la versión del servidor web (Apache/2.2.8) y la identificación de vulnerabilidades, por ejemplo, la susceptibilidad a **Cross-Site Tracing (XST)**, una debilidad que permite la fuga de información sensible a través de métodos HTTP TRACE/TRACK._

### 3. Recuperación y Análisis de Resultados

Una vez completada la ejecución de las pruebas, WMAP almacena los hallazgos en la base de datos interna de Metasploit.

#### 3.1 Listado de Vulnerabilidades Identificadas

Para visualizar las vulnerabilidades descubiertas, se utiliza el comando `wmap_vulns -l`. Este comando extrae y presenta un resumen de todas las vulnerabilidades detectadas durante el escaneo.

```shell
msf > wmap_vulns -l
[+] wmap_vulns -l (192.168.44.133): scraper /
    scraper Scraper
[+] GET Metasploitable2 - Linux
[+] [192.168.44.133]: directory /dav/
    directory Directory Found.
    GET Res code: 200
[+] [192.168.44.133]: directory /cgi-bin/
    directory Directory Found.
    GET Res code: 200
...
[+] [192.168.44.133]: file /index.php
    file File Found.
    GET Res code: 200
[+] [192.168.44.133]: file /phpMyAdmin
    file File Found.
    GET Res code: 200
...
```

_Este listado consolida los hallazgos, incluyendo directorios y archivos accesibles (`/dav/`, `/cgi-bin/`, `/phpMyAdmin/`, `/index.php`), así como códigos de respuesta HTTP (e.g., `200 OK` para éxito, `403 Forbidden` para acceso denegado, `404 Not Found` para recursos inexistentes, `301 Moved Permanently`). La presencia de directorios como `/phpMyAdmin/` en un servidor expuesto es un hallazgo crítico que puede indicar la existencia de un panel de administración de base de datos vulnerable._
