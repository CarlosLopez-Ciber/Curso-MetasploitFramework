# Comando help
`msf > help`

### Comandos del Núcleo

|Comando|Descripción|
|---|---|
|`?`|Menú de ayuda.|
|`banner`|Muestra un increíble banner de Metasploit.|
|`cd`|Cambia el directorio de trabajo actual.|
|`color`|Activa o desactiva el color en la salida.|
|`connect`|Se comunica con un host.|
|`debug`|Muestra información útil para la depuración.|
|`exit`|Sale de la consola.|
|`features`|Muestra la lista de características aún no lanzadas a las que se puede optar.|
|`get`|Obtiene el valor de una variable específica del contexto.|
|`getg`|Obtiene el valor de una variable global.|
|`grep`|Filtra la salida de otro comando (similar a `grep`).|
|`help`|Menú de ayuda.|
|`history`|Muestra el historial de comandos.|
|`load`|Carga un plugin del framework.|
|`quit`|Sale de la consola.|
|`repeat`|Repite una lista de comandos.|
|`route`|Enruta el tráfico a través de una sesión.|
|`save`|Guarda los _datastores_ activos.|
|`sessions`|Muestra listados de sesiones e información sobre ellas.|
|`set`|Establece un valor para una variable específica del contexto.|
|`setg`|Establece un valor para una variable global.|
|`sleep`|No hace nada durante el número de segundos especificado.|
|`spool`|Escribe la salida de la consola en un archivo además de en la pantalla.|
|`threads`|Visualiza y manipula los hilos (_threads_) en segundo plano.|
|`tips`|Muestra una lista de consejos de productividad útiles.|
|`unload`|Descarga un plugin del framework.|
|`unset`|Elimina el valor de una o más variables específicas del contexto.|
|`unsetg`|Elimina el valor de una o más variables globales.|
|`version`|Muestra los números de versión del framework y de la librería de la consola.|

### Comandos de Módulo

| Comando      | Descripción                                                           |
| ------------ | --------------------------------------------------------------------- |
| `advanced`   | Muestra opciones avanzadas para uno o más módulos.                    |
| `back`       | Retrocede desde el contexto actual.                                   |
| `clearm`     | Limpia la pila de módulos.                                            |
| `favorite`   | Añade uno o más módulos a la lista de módulos favoritos.              |
| `favorites`  | Imprime la lista de módulos favoritos (alias de `show favorites`).    |
| `info`       | Muestra información sobre uno o más módulos.                          |
| `listm`      | Lista la pila de módulos.                                             |
| `loadpath`   | Busca y carga módulos desde una ruta específica.                      |
| `options`    | Muestra las opciones globales o para uno o más módulos.               |
| `popm`       | Saca el último módulo de la pila y lo activa.                         |
| `previous`   | Establece el módulo cargado previamente como el módulo actual.        |
| `pushm`      | Empuja el módulo activo o una lista de módulos a la pila de módulos.  |
| `reload_all` | Recarga todos los módulos desde todas las rutas de módulos definidas. |
| `search`     | Busca en los nombres y descripciones de los módulos.                  |
| `show`       | Muestra módulos de un tipo determinado, o todos los módulos.          |
| `use`        | Interactúa con un módulo por nombre o término/índice de búsqueda.     |

### Comandos de Trabajos (Jobs)

|Comando|Descripción|
|---|---|
|`handler`|Inicia un manejador de _payloads_ (_handler_) como un trabajo (_job_).|
|`jobs`|Muestra y gestiona los trabajos.|
|`kill`|Mata (termina) un trabajo.|
|`rename_job`|Renombra un trabajo.|

### Comandos de Scripts de Recursos

|Comando|Descripción|
|---|---|
|`makerc`|Guarda en un archivo los comandos introducidos desde el inicio.|
|`resource`|Ejecuta los comandos almacenados en un archivo.|

### Comandos del Backend de la Base de Datos

|Comando|Descripción|
|---|---|
|`analyze`|Analiza información de la base de datos sobre una dirección o rango de direcciones específico.|
|`certs`|Lista los paquetes de certificados Pkcs12 en la base de datos.|
|`db_connect`|Se conecta a un servicio de datos existente.|
|`db_disconnect`|Se desconecta del servicio de datos actual.|
|`db_export`|Exporta un archivo con el contenido de la base de datos.|
|`db_import`|Importa un archivo de resultados de escaneo (el tipo de archivo se detectará automáticamente).|
|`db_nmap`|Ejecuta nmap y guarda la salida automáticamente.|
|`db_rebuild_cache`|Reconstruye la caché de módulos almacenada en la base de datos (obsoleto).|
|`db_remove`|Elimina la entrada guardada del servicio de datos.|
|`db_save`|Guarda la conexión actual del servicio de datos como la predeterminada para reconectar al inicio.|
|`db_stats`|Muestra estadísticas de la base de datos.|
|`db_status`|Muestra el estado actual del servicio de datos.|
|`hosts`|Lista todos los hosts en la base de datos.|
|`klist`|Lista los tickets Kerberos en la base de datos.|
|`loot`|Lista todo el _loot_ (botín) en la base de datos.|
|`notes`|Lista todas las notas en la base de datos.|
|`services`|Lista todos los servicios en la base de datos.|
|`vulns`|Lista todas las vulnerabilidades en la base de datos.|
|`workspace`|Cambia entre espacios de trabajo de la base de datos.|

### Comandos del Backend de Credenciales

|Comando|Descripción|
|---|---|
|`creds`|Lista todas las credenciales en la base de datos.|

### Comandos de Desarrollador

|Comando|Descripción|
|---|---|
|`edit`|Edita el módulo actual o un archivo con el editor preferido.|
|`irb`|Abre una consola interactiva de Ruby en el contexto actual.|
|`log`|Muestra el archivo `framework.log` paginado hasta el final si es posible.|
|`pry`|Abre el depurador Pry en el módulo actual o en el Framework.|
|`reload_lib`|Recarga archivos de la librería de Ruby desde rutas especificadas.|
|`time`|Mide cuánto tiempo tarda en ejecutarse un comando en particular.|

### Comandos de DNS

|Comando|Descripción|
|---|---|
|`dns`|Gestiona el comportamiento de resolución DNS de Metasploit.|

Para más información sobre un comando específico, usa `<comando> -h` o `help <comando>`.

### Construyendo rangos y listas

---

Muchos comandos y opciones que aceptan una lista de elementos pueden usar rangos para evitar tener que listar manualmente cada elemento deseado. Todos los rangos son inclusivos.

#### Rangos de IDs

Los comandos que aceptan una lista de IDs pueden usar rangos. Los IDs individuales deben estar separados por una `,` (no se permite espacio) y los rangos pueden expresarse con `-` o `..`.

#### Rangos de IPs

Hay varias formas de especificar rangos de direcciones IP que se pueden mezclar. La primera es una lista de IPs separadas solo por un (espacio ASCII), con una , opcional. La siguiente forma son dos direcciones IP completas en el formato DIRECCIÓN_INICIAL-DIRECCIÓN_FINAL como 127.0.1.44-127.0.2.33. También se pueden usar especificaciones CIDR, sin embargo, se debe proporcionar la dirección completa a Metasploit, como 127.0.0.0/8 y no 127/8, a diferencia del RFC.

Además, se puede usar una máscara de red junto con un nombre de dominio para resolver dinámicamente qué bloque atacar. Todos estos métodos funcionan tanto para direcciones IPv4 como IPv6. Las direcciones IPv4 también se pueden especificar con rangos de octetos especiales de la especificación de objetivos de NMAP.

### Ejemplos

Terminar las primeras sesiones:

```shell
sessions -k 1
```

Detener algunos trabajos extra en ejecución:

```shell
jobs -k 2-6,7,8,11..15
```

Verificar un conjunto de direcciones IP:

```shell
check 127.168.0.0/16, 127.0.0-2.1-4,15 127.0.0.255
```

Apuntar a un conjunto de hosts IPv6:

```shell
set RHOSTS fe80::3990:0000/110, ::1-::f0f0
```

Apuntar a un bloque desde un nombre de dominio resuelto:

```shell
set RHOSTS www.example.test/24
```
