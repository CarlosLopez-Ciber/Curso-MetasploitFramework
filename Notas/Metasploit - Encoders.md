# Metasploit - Encoders

Durante una prueba de penetración real, **el código malicioso que se envía al objetivo (exploit + payload)** puede ser fácilmente detectado y bloqueado por soluciones de seguridad como antivirus, IDS/IPS o EDR.

Su propósito principal es doble:

1. **Evasión de Antivirus/IDS/IPS**: La forma "cruda" de un payload a menudo contiene secuencias de bytes o firmas que son fácilmente detectadas por el software de seguridad (antivirus, sistemas de detección/prevención de intrusiones). Los encoders cambian la "apariencia" del payload, ofuscándolo para intentar eludir estas detecciones basadas en firmas.
    
2. **Eliminación de "Bad Characters" (Caracteres Malos)**: En ciertos contextos de explotación, algunos caracteres específicos (ej. bytes nulos `\x00`, retornos de carro `\x0d`, saltos de línea `\x0a`, etc.) pueden romper la ejecución del payload si aparecen dentro de él. Estos son conocidos como "bad characters". Los encoders reescriben el payload de manera que estos caracteres problemáticos no estén presentes, asegurando que la carga útil se ejecute correctamente en el sistema objetivo.

> Un encoder no encripta ni cifra. Solo altera la forma en que se representa el shellcode para hacerlo menos reconocible por firmas estáticas.

### Submódulos de los Encoders

Los directorios que ha listado dentro de `/usr/share/metasploit-framework/modules/encoders/` categorizan los encoders según la arquitectura de la CPU a la que apunta el payload, o en algunos casos, el tipo de lenguaje de scripting.

Aquí una descripción de cada uno:

1. **`cmd`**:
    
    - **Descripción**: Contiene encoders diseñados para payloads que se ejecutan como **comandos de línea de comandos**. Estos payloads son a menudo cadenas de texto que se envían a un intérprete de comandos (como `cmd.exe` en Windows o `bash` en Linux). Los encoders aquí se enfocan en hacer que estas cadenas sean seguras y evadan la detección.
        
    - **Uso típico**: Codificar comandos simples para su ejecución remota.
        
2. **`generic`**:
    
    - **Descripción**: Incluye encoders que no están atados a una arquitectura o plataforma específica y pueden aplicarse a payloads de naturaleza más general. Suelen ser métodos de encoding más amplios o prototipos.
        
    - **Uso típico**: Pruebas generales o cuando no se necesita una optimización específica de arquitectura.
        
3. **`mipsbe`**:
    
    - **Descripción**: Encoders específicos para payloads destinados a sistemas con arquitectura **MIPS (Microprocessor without Interlocked Pipeline Stages) en formato Big-Endian**. Los sistemas MIPS son comunes en routers, dispositivos embebidos y algunas consolas de videojuegos.
        
    - **Uso típico**: Explotación de dispositivos IoT o de red basados en MIPS.
        
4. **`mipsle`**:
    
    - **Descripción**: Encoders específicos para payloads destinados a sistemas con arquitectura **MIPS en formato Little-Endian**.
        
    - **Uso típico**: Similar a `mipsbe`, pero para sistemas que utilizan la convención Little-Endian.
        
5. **`php`**:
    
    - **Descripción**: Encoders para payloads escritos en lenguaje **PHP**. Estos payloads suelen ser scripts PHP que se ejecutan en servidores web comprometidos. Los encoders los ofuscan o los hacen seguros para la inyección.
        
    - **Uso típico**: Explotación de aplicaciones web vulnerables desarrolladas en PHP.
        
6. **`ppc`**:
    
    - **Descripción**: Encoders para payloads destinados a sistemas con arquitectura **PowerPC (PPC)**. Esta arquitectura fue utilizada en sistemas Apple antiguos, algunas consolas (como Xbox 360, Wii) y dispositivos de red de alta gama.
        
    - **Uso típico**: Explotación de sistemas específicos basados en PowerPC.
        
7. **`ruby`**:
    
    - **Descripción**: Encoders para payloads escritos en lenguaje **Ruby**. Estos payloads son menos comunes para la ejecución directa en un sistema comprometido, pero pueden ser útiles en contextos específicos donde un intérprete Ruby está disponible.
        
    - **Uso típico**: Contextos de desarrollo o entornos donde Ruby es la plataforma principal.
        
8. **`sparc`**:
    
    - **Descripción**: Encoders para payloads destinados a sistemas con arquitectura **SPARC (Scalable Processor ARChitecture)**. Esta arquitectura es conocida por su uso en servidores de Oracle/Sun Microsystems.
        
    - **Uso típico**: Explotación de servidores antiguos o sistemas Unix/Solaris específicos.
        
9. **`x64`**:
    
    - **Descripción**: Encoders optimizados para payloads destinados a sistemas de 64 bits con arquitectura **x86-64** (también conocida como AMD64 o Intel 64). Esta es la arquitectura predominante en los sistemas operativos modernos de escritorio y servidor (Windows, Linux, macOS).
        
    - **Uso típico**: La mayoría de las explotaciones modernas en sistemas de 64 bits.
        
10. **`x86`**:
    
    - **Descripción**: Encoders optimizados para payloads destinados a sistemas de 32 bits con arquitectura **x86** (también conocida como IA-32). Aunque menos prevalente que x64 en nuevos sistemas, todavía se encuentra en sistemas legados y aplicaciones de 32 bits que se ejecutan en sistemas de 64 bits.
        
    - **Uso típico**: Explotación de sistemas operativos antiguos o de aplicaciones de 32 bits.

