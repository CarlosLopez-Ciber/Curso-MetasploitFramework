# Gestión de Entornos de Trabajo (Workspaces)

La gestión de los espacios de trabajo se realiza a través del comando `workspace`, utilizando diferentes modificadores para realizar acciones específicas.

## Listado de Workspaces Disponibles

Para visualizar todos los espacios de trabajo existentes, se ejecuta el comando `workspace` sin argumentos. El workspace activo se resalta con un asterisco (`*`).

```sh
    msf6 > workspace
      default
    * pentest_cliente_a
      pentest_cliente_b
```

En este caso, `pentest_cliente_a` es el contexto de trabajo actual.

## Creación de un Nuevo Workspace

Para crear un nuevo entorno lógico, se utiliza el modificador `-a` (add) seguido del nombre deseado. Inmediatamente después de su creación, Metasploit cambia el contexto a este nuevo workspace.

```sh
msf6 > workspace -a red_corporativa
[*] Added workspace 'red_corporativa'
[*] Workspace: red_corporativa
```

A partir de este momento, todos los datos, como la importación de un escaneo de Nmap (`db_import scan.xml`), se almacenarán exclusivamente en `red_corporativa`.

## Cambio del Contexto de Trabajo

Para cambiar entre workspaces existentes, simplemente se ejecuta el comando `workspace` seguido del nombre del espacio de trabajo al que se desea cambiar.

```sh
    msf6 > workspace default
    [*] Workspace: default
```

Esta acción redirige todas las operaciones subsecuentes al workspace `default`, aislando el trabajo del entorno `red_corporativa`.

## Eliminación de un Workspace

El modificador `-d` (delete) se utiliza para eliminar permanentemente un espacio de trabajo y toda la información que contiene.

> ⚠️ **Advertencia:** Esta operación es irreversible. Eliminar un workspace resultará en la pérdida de todos los hosts, servicios, credenciales y demás datos asociados a él. Se recomienda realizar una copia de seguridad (`db_export`) antes de proceder.

```sh
    msf6 > workspace -d red_corporativa
    [*] Deleted workspace 'red_corporativa'
    [*] Switched back to workspace 'default'
```
