# 📌 Pre-requisitos

Para aprovechar al máximo este curso y comprender qué sucede "detrás de escena" en cada **exploit**, es recomendable contar con la siguiente base técnica:

#### **1. 🌐 Fundamentos de Redes (Networking)**

**Metasploit** interactúa directamente con protocolos y capas de red para lograr la explotación. Es fundamental que comprendas:

- **Modelo OSI y TCP/IP:** Cómo se encapsulan los datos y el flujo de comunicación entre dispositivos.
    
- **Protocolos Esenciales:** Funcionamiento de servicios como **HTTP/HTTPS**, **SMB** (muy común en exploits de Windows), **SSH**, **FTP** y **DNS**.
    
- **Direccionamiento y Segmentación:** Diferencia entre IP pública/privada, máscaras de subred y el funcionamiento de protocolos como **DHCP**.
    

> [!TIP]
> 
> ¿Eres nuevo en redes? 🎓 Si vienes de cero o quieres refrescar tus conocimientos antes de empezar, te recomiendo completar el curso gratuito de Cisco Networking Academy:
> 
> 🔗 **[Conceptos básicos de redes - NetAcad](https://www.netacad.com/es/courses/networking-basics?courseLang=es-XL)**
> 
> **¿Qué aprenderás?**
> 
> - 📡 Direccionamiento **IPv4**.
>     
> - 🗺️ Tipos de redes y estándares de protocolos.
>     
> - 🏗️ Servicios de la capa de aplicación.
>     
> - 📶 Medios de red y acceso inalámbrico.
>     
> 
> Tener estas bases te permitirá entender **por qué** y **cómo** funcionan los ataques que realizaremos en los laboratorios.

---

#### **2. 🐧 Sistemas Operativos y Consola**

- 💻 **Línea de comandos (CLI):** Manejo fluido de la terminal **Linux**.
    
- ⚙️ **Administración básica:** Conceptos de cómo **Windows** y **Linux** gestionan usuarios, servicios y registros del sistema.
    

> [!IMPORTANT]
> 
> ¿Aún no dominas la terminal de Linux? 🐧 Si nunca has usado Linux o quieres profundizar antes de entrar a Metasploit, este curso gratuito de Cisco en colaboración con el NDG es el mejor punto de partida:
> 
> 🔗 **[Fundamentos de Linux - NetAcad](https://www.netacad.com/es/courses/fundamentos-de-linux?courseLang=es-XL)**
> 
> Aprender Linux no solo te servirá para Metasploit, sino que es la base fundamental para cualquier carrera en **Ciberseguridad**.

---

#### **3. 🖥️ Entorno de Laboratorio (Hardware y Software)**

- 📦 **Hipervisor:** Tener instalado **[VirtualBox](https://www.virtualbox.org/)** o **[VMware Player/Workstation](https://www.vmware.com/)**.
    
- ⚔️ **Máquina Atacante:** Recomiendo **[Kali Linux](https://www.kali.org/)** o **[Parrot Security OS](https://parrotsec.org/)**, ya que vienen con **Metasploit** preinstalado y configurado.
    
- 🧠 **Recursos de Hardware:** Un mínimo de **4GB de RAM** (8GB recomendado) para correr al menos dos máquinas virtuales simultáneamente (Atacante + Víctima).
    

---

#### **4. ⚖️ Ética y Responsabilidad (¡Importante!)**

El uso de **Metasploit** debe realizarse exclusivamente en entornos controlados, laboratorios personales o bajo autorización explícita mediante un contrato de **Pentesting**.

> [!WARNING]
> 
> Advertencia: El uso de estas herramientas contra sistemas sin autorización es ilegal y acarrea consecuencias penales.

- 💎 **Lectura de código (Opcional):** No necesitas ser programador, pero tener nociones básicas de **Ruby** (lenguaje en el que está escrito Metasploit) te ayudará a entender cómo funcionan los archivos de configuración y los scripts de automatización.
