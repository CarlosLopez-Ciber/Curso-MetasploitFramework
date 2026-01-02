# 🌐 Configuración de Red: Entorno Aislado y Seguro

Para practicar con **Metasploit Framework**, es vital crear un **Sandbox** (entorno de pruebas aislado). Esto evita que los exploits que lances salgan de tus máquinas virtuales hacia tu red local o internet, y protege tu máquina anfitriona (**Host**).

### 1. El peligro del modo "Adaptador Puente" (Bridged)

Por defecto, muchas personas usan el modo "Puente", que hace que la VM se comporte como un dispositivo más en tu casa. **No uses este modo** para laboratorios de explotación, ya que:

- Expones una máquina vulnerable (**Metasploitable**) a toda tu red.
    
- Otros dispositivos en tu casa podrían ser alcanzados por escaneos de red accidentales.
    

### 2. Configuración Recomendada: Red NAT (NAT Network)

Este es el modo ideal. Permite que tus máquinas virtuales se comuniquen entre sí y tengan salida a internet (para descargar actualizaciones en Kali), pero las mantiene ocultas de los dispositivos físicos de tu hogar.

#### **En VirtualBox:**

1. Ve a **Archivo** > **Herramientas** > **Red**.
    ![](../Adjuntos/Pasted%20image%2020260102162327.png)
2. Selecciona "Redes NAT" y luego dale a "Crear". Esto creará una nueva red (por defecto se llamará `NatNetwork`).
    ![](../Adjuntos/Pasted%20image%2020260102162448.png)
3. En **Configuración** de cada VM (**Kali** y **Metasploitable**):
    
    - Ve a **Red** > **Adaptador 1**.
        
    - Conectado a: **Red NAT**.
        
    - Nombre: Selecciona `NatNetwork`.
        ![](../Adjuntos/Pasted%20image%2020260102162925.png)

### 3. Prueba de Conectividad (Ping)

Una vez configuradas, ambas máquinas deben estar en el mismo rango de red (ej. `10.0.2.0/24`). Para verificar que tu entorno está listo, abre la terminal en tu máquina atacante (Kali Linux) y lanza un ping hacia la víctima (Metasploitable 2):

```bash
# Reemplaza con la IP que tenga tu Metasploitable
ping -c 4 10.0.2.5 
```

Si recibes respuesta, ¡tu laboratorio es seguro y está listo para la acción!

