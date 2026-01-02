# Tipos de Modulos

En el contexto de **Metasploit Framework**, un **módulo** se define como una **unidad de código autocontenida** diseñada para ejecutar una función específica dentro del ciclo de vida del _pentesting_ o la evaluación de seguridad. Estos módulos abarcan desde la explotación de vulnerabilidades hasta la recolección de información y la gestión de _payloads_.

| **Tipo de Módulo**                                        | **Descripción Técnica**                                                                                                                                                                                                                                                                                                                    |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Metasploit - Exploit](./Metasploit%20-%20Exploit.md)     | Componentes diseñados para **aprovechar vulnerabilidades específicas** en _software_, sistemas operativos o servicios de red. Su objetivo es obtener acceso inicial o elevar privilegios mediante la manipulación del flujo de ejecución de un proceso vulnerable.                                                                         |
| [Metasploit - Payloads](./Metasploit%20-%20Payloads.md)   | Código binario o _script_ que se **ejecuta en el sistema objetivo** una vez que una vulnerabilidad ha sido explotada con éxito. Los _payloads_ determinan la acción posterior a la explotación, como el establecimiento de una _shell_ remota (_reverse_ o _bind shell_) o la inyección de funcionalidades avanzadas como **Meterpreter**. |
| [Metasploit - Auxiliary](./Metasploit%20-%20Auxiliary.md) | Módulos que realizan **funciones auxiliares** no directamente relacionadas con la explotación, pero cruciales para la fase de **reconocimiento, escaneo de red, enumeración de servicios, fuzzing o ataques de fuerza bruta**. No inyectan un _payload_ directamente, pero pueden ser preparatorios para una explotación.                  |
| [Metasploit - Post](./Metasploit%20-%20Post.md)           | Módulos ejecutados **después de una explotación exitosa** y el establecimiento de una sesión remota (ej., _shell_ o Meterpreter). Facilitan tareas de **post-explotación** como la escalada de privilegios, el movimiento lateral dentro de la red, la recolección de credenciales, la persistencia o la exfiltración de datos.            |
| [Metasploit - Encoders](./Metasploit%20-%20Encoders.md)   | Herramientas utilizadas para **transformar o ofuscar el código de un _payload_** con el fin de **evadir la detección por _software antivirus_ (AV) o sistemas de detección de intrusiones (IDS/IPS)**. Los _encoders_ aplican algoritmos que modifican la _signature_ del _payload_ sin alterar su funcionalidad.                          |
| [Metasploit - NOPs](./Metasploit%20-%20NOPs.md)           | Secuencias de **instrucciones de "no-operación"** (`NOP`) que se insertan antes de un _payload_ o _shellcode_. Su función principal es **estabilizar la ejecución del _payload_**, especialmente en _exploits_ que dependen de desbordamientos de búfer, proporcionando un margen de error para la dirección de salto.                     |
| [Metasploit - Evasion](./Metasploit%20-%20Evasion.md)     | Módulos o técnicas activas diseñadas para **burlar la detección por _software antivirus_ (AV)**, **Endpoint Detection and Response (EDR)** y otras soluciones de seguridad, permitiendo que las operaciones de Metasploit pasen desapercibidas.                                                                                            |

> **"En Metasploit Framework, la modularidad es un principio fundamental: cada módulo cumple un propósito específico y bien definido en el ciclo de vida de la prueba de penetración."**

---

## Proceso de Actualización de Metasploit Framework

- El proceso de actualización se gestiona eficientemente mediante la utilidad `msfupdate` en sistemas basados en Linux/Unix, que sincroniza la instalación local con el repositorio oficial.

