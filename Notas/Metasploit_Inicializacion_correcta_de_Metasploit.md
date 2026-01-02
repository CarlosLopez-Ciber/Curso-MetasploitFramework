# Inicialización Correcta de Metasploit

Antes de utilizar el **Metasploit Framework**, es fundamental preparar correctamente los servicios de backend que permiten su funcionamiento óptimo. Entre estos, destacan:

- **PostgreSQL**: Motor de base de datos relacional utilizado por Metasploit para almacenar hosts, servicios, exploits, sesiones, credenciales, etc.
    

- **MSF Database (msfdb)**: Componente que actúa como intermediario entre Metasploit y PostgreSQL, proporcionando persistencia y funciones avanzadas como importación de resultados, integración con Nmap, entre otras.

## Paso 1: Iniciar el servicio de PostgreSQL
**Comando para iniciar:**

```SH
sudo systemctl start postgresql
```

**Verificar estado:**

```shell
sudo systemctl status postgresql
# Salida esperada:
Active: active (exited) […]
```

**Detener el servicio (opcional):**

```
sudo systemctl stop postgresql
```

> **Nota**: PostgreSQL debe estar activo antes de lanzar Metasploit. Si no lo está, la base de datos no podrá usarse y muchas funcionalidades del Framework quedarán inhabilitadas.

## Paso 2: Inicializar la base de datos de Metasploit

`msfdb` es un servicio que configura y vincula Metasploit con PostgreSQL, creando las tablas necesarias y habilitando funcionalidades como `db_nmap`, `db_import`, `hosts`, `services`, entre otros.

**Inicializar msfdb (por primera vez):**

```SH
sudo msfdb init
```

Esto configurará automáticamente el acceso, creará los esquemas necesarios y verificará que PostgreSQL esté corriendo.

**Verificar estado:**

```shell
msfdb status
# Salida esperada:
Database Status: Connected
```

**Detener msfdb (opcional):**

```
sudo msfdb stop
```

> Si es la primera vez que ejecutas `msfdb`, es probable que esté inactivo. El comando `init` lo activará y configurará de forma automática.

## Paso 3: Iniciar Metasploit Framework

Una vez configurados PostgreSQL y msfdb, puedes ejecutar el **entorno interactivo** del Metasploit Framework a través de su consola.

**Comando:**

```sh
msfconsole
```

**Proceso de carga inicial:**

- Verificación de la base de datos
- Carga de módulos auxiliares, exploits, payloads y post módulos
- Mostrar banner de bienvenida (cambia en cada ejecución)

**Salida esperada:**

![[Pasted image 20250716002756.png]]

> Puedes cambiar el banner cuantas veces quieras con el comando:

```sh
msf6 > banner
```
