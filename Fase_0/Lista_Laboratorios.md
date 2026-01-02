# **🧪 Laboratorios y Entornos de Práctica (CTFs)**

La mejor forma de dominar **Metasploit Framework** es mediante la experimentación en entornos controlados y seguros. En esta sección encontrarás los recursos necesarios para montar tu propio laboratorio de pruebas.

#### **1. El Escenario Principal: Metasploitable 2**

Para la mayoría de los módulos de este curso, utilizaremos **Metasploitable 2**. Es una máquina virtual intencionalmente vulnerable diseñada específicamente para practicar técnicas de seguridad y pruebas de penetración.

- **¿Por qué Metasploitable 2?** Contiene numerosas vulnerabilidades en servicios comunes (SMB, HTTP, FTP, etc.), lo que la hace ideal para aprender a utilizar diferentes módulos de Metasploit.
    
- **Instalación:** Si tienes dudas sobre cómo ponerla en marcha, aquí tienes tutoriales seleccionados:
    
    - 📺 [Instalación de Metasploitable 2 - Opción 1](https://www.youtube.com/watch?v=x0Pj0rIV_Mk)
        
    - 📺 [Guía paso a paso - Opción 2](https://www.youtube.com/watch?v=gdMRiCGB5U8)
        
    - 📺 [Configuración rápida - Opción 3](https://www.youtube.com/watch?v=0twIf49UIT0)
        

#### **2. Software de Virtualización**

Eres libre de elegir el hipervisor que prefieras. Ambos son excelentes opciones y no afectan el funcionamiento de las herramientas del curso:

- **VirtualBox:** Gratuito, de código abierto y muy popular en la comunidad.
    
- **VMware (Workstation Player/Pro):** Conocido por su gran rendimiento y estabilidad.
    

> [!NOTE]
> 
> Independientemente de tu elección, asegúrate de configurar las máquinas en una red aislada (Host-Only o NAT Network) para evitar exponer servicios vulnerables a tu red local o a Internet.

#### **3. Fuentes de Desafíos Adicionales (CTFs)**

Una vez que domines los conceptos básicos con Metasploitable, puedes expandir tus conocimientos con máquinas de estos sitios, que ofrecen retos tipo _Capture The Flag_ (CTF):

- 🌐 **[VulnHub](https://www.vulnhub.com/):** El catálogo más grande de máquinas virtuales vulnerables creadas por la comunidad.
    
- 🌐 **[Vulnyx](https://vulnyx.com/):** Excelente plataforma con laboratorios enfocados en sistemas Linux y Windows.
    
- 🌐 **[HackMyVM](https://hackmyvm.eu/):** Una comunidad creciente con máquinas de diversos niveles de dificultad.
    

---

### **¿Por qué estos cambios mejoran tu texto?**

1. **Contexto Técnico:** Añadimos por qué Metasploitable 2 es importante, citando que es un estándar para aprender el framework.
    
2. **Seguridad:** Incluimos una nota sobre la importancia de las **redes aisladas**, algo vital en ciberseguridad para evitar riesgos innecesarios en la red real del estudiante.
    
3. **Formato Visual:** El uso de iconos (📺, 🌐, 🧪) y bloques de notas (`[!NOTE]`) hace que el documento sea mucho más fácil de leer en GitHub.
    
4. **Llamada a la Acción:** Al presentar VulnHub y otras plataformas como "pasos siguientes", motivas al estudiante a terminar tu curso para poder enfrentarse a esos retos.
    

¿Te gustaría que añadiera una pequeña tabla con los **usuarios y contraseñas por defecto** de Metasploitable 2 para que los alumnos no se queden trabados al iniciar la máquina?