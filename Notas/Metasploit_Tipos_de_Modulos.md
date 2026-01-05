# 🧩 Tipos de Módulos en Metasploit

En el ecosistema de **Metasploit Framework**, un **módulo** es una unidad de código autocontenida y especializada. La arquitectura modular es lo que hace que Metasploit sea tan potente: permite a los desarrolladores añadir nuevas capacidades sin alterar el motor principal.

| **Tipo de Módulo** |     | **Descripción Técnica**                                                                                                                                         |
| ------------------ | --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Exploit**        | 🧨  | Componentes que aprovechan una **vulnerabilidad específica** para forzar un comportamiento no deseado en el objetivo. Su fin es "abrir la puerta".              |
| **Payload**        | 📦  | El código que se ejecuta tras el éxito del exploit. Define la acción a realizar: desde una simple consola (**shell**) hasta el potente **Meterpreter**.         |
| **Auxiliary**      | 🔍  | Herramientas que no inyectan payloads. Se usan para **escaneo, enumeración, recolección de información** y ataques de denegación de servicio (DoS).             |
| **Post**           | 🚩  | Módulos para la fase de **post-explotación**. Permiten escalar privilegios, robar hashes de contraseñas y realizar movimientos laterales en la red.             |
| **Encoder**        | 🎭  | Transforman el código del payload para **evadir firmas de Antivirus (AV)** y eliminar "caracteres malos" que podrían romper el exploit.                         |
| **NOP**            | 🧩  | Instrucciones que "no hacen nada" (*No Operation*). Se usan para rellenar espacio en memoria y lograr que el flujo de ejecución caiga suavemente en el payload. |
| **Evasion**        | 👻  | (Introducidos en MSF 5) Módulos diseñados específicamente para generar archivos que **burlan soluciones EDR y antivirus** modernos de forma activa.             |

---

## 🛠️ Entendiendo la diferencia: Exploit vs. Payload

Es común confundirlos al principio. Una analogía sencilla:

* **El Exploit** es el **misil**: el vehículo que transporta la carga y atraviesa las defensas.
* **El Payload** es la **carga explosiva**: lo que realmente hace el trabajo una vez que el misil llega a su destino.

---

## 🔄 Proceso de Actualización del Framework

Como el panorama de las vulnerabilidades cambia a diario, mantener tu arsenal al día es obligatorio. Dependiendo de tu instalación, tienes dos rutas principales:

### 1. El comando clásico: `msfupdate`

Históricamente, este era el comando estándar para sincronizar con los repositorios de **Rapid7**.

```bash
sudo msfupdate
```

### 2. En distribuciones como Kali Linux

Hoy en día, en Kali se recomienda utilizar el gestor de paquetes del sistema para mantener la estabilidad de las dependencias:

```bash
sudo apt update && sudo apt install metasploit-framework
```

> [!TIP]
> Si notas que un exploit que viste en una noticia no aparece en tu `search`, lo primero que debes hacer es verificar tu conexión a internet y ejecutar una actualización de la base de datos.

