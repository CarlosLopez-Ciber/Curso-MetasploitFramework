# 🚀 Inicialización Correcta de Metasploit

Antes de lanzar el **Metasploit Framework**, es vital preparar los servicios de _backend_. Una correcta inicialización garantiza que no pierdas tus progresos y que puedas gestionar grandes volúmenes de datos de forma profesional.

### 🛠️ Servicios Esenciales

Para un funcionamiento óptimo, Metasploit se apoya en dos pilares:

- 🐘 **PostgreSQL**: El motor de base de datos relacional. Aquí se guarda toda la "inteligencia" de tus auditorías: hosts descubiertos, servicios activos, sesiones abiertas y credenciales obtenidas.
    
- 💾 **MSF Database (msfdb)**: Es el puente entre el Framework y la base de datos. Permite la persistencia de datos y habilita funciones avanzadas como la integración con **Nmap**.

## 🏁 Paso 1: Iniciar el servicio de PostgreSQL

PostgreSQL debe estar corriendo en segundo plano antes de intentar conectar Metasploit.

**Comandos de gestión:**

```bash
# Iniciar el servicio
sudo systemctl start postgresql

# Verificar que esté activo
sudo systemctl status postgresql
```

> [!IMPORTANT]
> 
> **Salida esperada:** Deberías ver un mensaje que diga `Active: active (running)`. Si aparece como `inactive`, muchas funciones de búsqueda y guardado en Metasploit no funcionarán.

---

## 🏗️ Paso 2: Inicializar la base de datos de Metasploit

El componente `msfdb` configura los esquemas y las tablas necesarias en PostgreSQL para que Metasploit pueda escribir datos.

**Inicializar por primera vez:**

Si es la primera vez que instalas Metasploit o si nunca has configurado la base de datos, ejecuta:

```bash
sudo msfdb init
```

**Verificar conexión:**

Una vez inicializado, comprueba que todo esté en orden:

```bash
msfdb status
```

> [!TIP]
> 
> ¿Por qué usar la base de datos? 📊
> 
> Al tenerla conectada, puedes usar comandos como `hosts` para ver todas las IPs atacadas o `services` para listar puertos abiertos sin tener que repetir escaneos de Nmap.

---

## ⚡ Paso 3: Lanzar msfconsole

Con los cimientos listos, es hora de entrar al entorno interactivo.

**Comando principal:**

```bash
msfconsole
```

**¿Qué sucede al cargar?** ⚙️

1. **Conexión a la DB:** El sistema verifica el enlace con PostgreSQL.
    
2. **Carga de Arsenal:** Se importan miles de exploits, payloads y auxiliares.
    
3. **Banner:** Aparece un arte ASCII aleatorio para darte la bienvenida.
    

Comando de verificación interna:

Una vez dentro de la consola (msf6 >), verifica la base de datos con:

```bash
db_status
```

_Salida esperada:_ `[*] postgresql connected to msf`

---

## 🎨 Personalización del Banner

Si quieres cambiar el arte visual de bienvenida sin salir de la consola, simplemente escribe:

```bash
msf6 > banner
```

![](../Adjuntos/Pasted%20image%2020250716002756.png)

