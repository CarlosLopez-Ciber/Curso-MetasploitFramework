# 📂 Gestión de Entornos de Trabajo (Workspaces)

En proyectos de ciberseguridad reales, es común manejar múltiples objetivos o clientes simultáneamente. Metasploit utiliza los **Workspaces** (espacios de trabajo) para segmentar y aislar la información en la base de datos.

Imagina los workspaces como "proyectos" independientes: lo que descubras en el _Cliente A_ (hosts, servicios, credenciales) no se mezclará con los datos del _Cliente B_. Esto evita confusiones y garantiza la integridad de tus reportes.

---

## 1. 📋 Listado de Workspaces Disponibles

Para ver qué entornos tienes creados, simplemente escribe `workspace`. El que tenga un asterisco (`*`) es el que estás utilizando actualmente.

```Bash
msf6 > workspace
  default
* pentest_banco_lima
  auditoria_interna
```

- **`default`**: Es el espacio de trabajo por defecto que crea Metasploit. No se puede eliminar.
    
- **Contexto:** En el ejemplo anterior, cualquier host que añadas se guardará en `pentest_banco_lima`.
    

---

## 2. ➕ Creación de un Nuevo Workspace

Para crear un entorno nuevo, usamos el modificador `-a` (_add_). Al crearlo, Metasploit te moverá automáticamente a ese nuevo espacio.

```Bash
msf6 > workspace -a red_corporativa
[*] Added workspace 'red_corporativa'
[*] Workspace: red_corporativa
```

> [!TIP]
> 
> Orden en el Laboratorio: 🧪
> 
> Si estás practicando en VulnHub o HackMyVM, te recomiendo crear un workspace con el nombre de la máquina (ej: workspace -a dc1). Así, si luego pasas a otra máquina, tus datos de Nmap no se mezclarán.

---

## 3. 🔄 Cambio de Contexto

Para saltar de un proyecto a otro, solo escribe `workspace` seguido del nombre del destino.

```Bash
msf6 > workspace default
[*] Workspace: default
```

---

## 4. 🗑️ Eliminación de un Workspace

Si has terminado una auditoría y quieres limpiar la base de datos, usa el modificador `-d` (_delete_).

```Bash
msf6 > workspace -d red_corporativa
[*] Deleted workspace 'red_corporativa'
[*] Switched back to workspace 'default'
```

> [!CAUTION]
> 
> Esta acción es permanente. Se borrarán todos los hosts, servicios y credenciales capturadas en ese entorno. Si necesitas guardar los datos antes de borrar, utiliza primero el comando db_export.

---
### 💡 Nota de Integración

Recuerda que para que los workspaces funcionen, debes tener **PostgreSQL** activo y conectado. Si la base de datos está desconectada, Metasploit no podrá crear ni gestionar estos entornos lógicos.
