# 🕹️ Interfaces de Metasploit Framework

En el mundo de **Metasploit**, una **interfaz** es el medio por el cual interactuamos con el potente motor del framework. Aunque todas las interfaces utilizan las mismas librerías de Ruby, cada una ofrece una experiencia distinta dependiendo de tus necesidades.

Imagina que Metasploit es el motor de un vehículo; las interfaces son el volante, los pedales y el tablero. Todas hacen que el coche avance, pero de formas diferentes.

### 💻 1. MSFconsole

La **MSFconsole** es la interfaz más popular, potente y estable del framework. Es una consola centralizada "todo en uno" que permite un acceso eficiente a casi todas las opciones disponibles en MSF.

- Es la única interfaz que garantiza compatibilidad con el 100% de los nuevos módulos y funciones que lanza **Rapid7**.
    

### 🛠️ 2. MSFvenom: Creación de Payloads

Es la interfaz de línea de comandos especializada en la **generación y codificación de payloads**. Combina las antiguas herramientas `msfpayload` y `msfencode`. La usaremos constantemente para crear archivos que nos permitan obtener sesiones remotas en nuestros laboratorios.

### 🔌 3. MSFrpcd (Remote Procedure Call)

Es una interfaz que permite que otros programas se conecten a Metasploit a través de una red. Es la que hace posible que herramientas externas "manejen" a Metasploit de forma automática.

### 📊 4. Versiones Comerciales: Metasploit Pro

Es importante saber que, aunque nosotros usaremos la versión **Framework (Open Source)**, existe una versión de paga desarrollada por **Rapid7**:

- **Metasploit Pro:** Es la versión empresarial. Incluye una interfaz web avanzada, automatización de tareas de recolección de pruebas, auditorías de aplicaciones web y funciones avanzadas para equipos de seguridad profesionales (_Red Teams_).
    
- **Diferencia clave:** Mientras que en la **MSFconsole** hacemos todo manualmente (ideal para aprender), la versión **Pro** automatiza gran parte del flujo de trabajo para ahorrar tiempo en auditorías reales.
    

---

### 💡 Nota

Aunque existen interfaces gráficas como **Armitage**, en este proyecto nos enfocaremos exclusivamente en **MSFconsole**. Aprender a configurar módulos y payloads manualmente te dará una comprensión profunda de la ciberseguridad que ninguna herramienta automática puede ofrecer.

> [!TIP]
> 
> Si usas Kali Linux o Parrot OS, ya tienes la MSFconsole lista para ser ejecutada.
