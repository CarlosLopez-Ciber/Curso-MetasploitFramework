# 🏗️ Estructura Interna del Framework

Comprender cómo se organiza **Metasploit** es fundamental para cualquier _pentester_. Saber dónde se ubica cada pieza te permitirá no solo usar la herramienta, sino también auditar su código o crear tus propios módulos en el futuro.

## 📍 ¿Dónde encontrar el Directorio de Metasploit?

La ubicación depende de tu sistema operativo y el método de instalación. 

**Para Kali Linux / Parrot OS:**

```bash
$ /usr/share/metasploit-framework/
```

### 🔍 Comandos rápidos para localizarlo

1. **Vía binario (El más rápido):**
    
    ```bash
    whereis msfconsole
    ```
    
1. **Búsqueda profunda (Si el anterior falla):**
    
    ```bash
    find / -type d -name "metasploit-framework" 2>/dev/null
    ```
    

---

## 📂 Anatomía del Sistema de Archivos

Metasploit está estructurado de manera lógica para separar el motor principal de los recursos de ataque.

### 1. El Directorio `/modules` (El Corazón)

Aquí reside todo el arsenal. Los módulos están categorizados por su función técnica:

- **`/exploits`**: Códigos diseñados para aprovechar una vulnerabilidad y ejecutar código en el objetivo. Se organizan por: `SO / Servicio / Nombre`.
    
- **`/payloads`**: Lo que se ejecuta tras el éxito del exploit (ej. Meterpreter, shells reversas).
    
- **`/auxiliary`**: Herramientas que no explotan directamente, como escáneres, sniffers y fuzzers.
    
- **`/post`**: Módulos para escalar privilegios y recolectar datos una vez dentro del sistema.
    
- **`/encoders`**: Utilizados para ofuscar el código y evadir firmas de Antivirus (AV) o IDS.
    
- **`/evasion`**: Módulos específicos para saltar protecciones de software de seguridad.
    
- **`/nops`**: Generadores de instrucciones que "no hacen nada" para rellenar memoria y estabilizar ataques.
    

### 2. Directorios de Sistema y Apoyo

Además de los módulos, estas carpetas son vitales para el funcionamiento del framework:

- **`/data`**: Almacena recursos necesarios para los módulos, como binarios de Meterpreter, imágenes y listas de palabras (_wordlists_).
    
- **`/db`**: Contiene la base de datos de metadatos de los módulos. Es lo que permite que el autocompletado en `msfconsole` sea tan rápido.
    
- **`/lib`**: El motor de Metasploit. Contiene todas las librerías en Ruby que gestionan las conexiones de red y protocolos.
    
- **`/scripts`**: Scripts adicionales para interfaces y sesiones activas (como scripts de Meterpreter).
    
- **`/tools`**: Utilidades de línea de comandos útiles para el desarrollo de exploits y depuración.
    
- **`/plugins`**: Extensiones que añaden nuevos comandos o integran Metasploit con herramientas como Nexpose o OpenVAS.
    

---

## 💡 Pro-Tip

Si decides crear tus propios módulos o modificar uno existente, **no lo hagas dentro de la carpeta del sistema** (`/usr/share/...`). GitHub o las actualizaciones del sistema podrían borrar tus cambios.

Usa siempre tu directorio local de usuario para tus creaciones personalizadas:

```
~/.msf4/modules/
```

---

> [!NOTE] Nota
> 
> Profundiza tus conocimientos: Para entender cómo navegar por el código fuente como un desarrollador, consulta la documentación oficial de Metasploit: [Navigating the codebase](https://docs.metasploit.com/docs/development/get-started/navigating-and-understanding-metasploits-codebase.html)

