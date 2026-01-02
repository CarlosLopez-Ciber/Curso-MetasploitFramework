# Meterpreter

`Meterpreter` es un `payload` avanzado y extensible que utiliza inyección de DLL en memoria (_in-memory_). Incrementa significativamente las capacidades de post-explotación del Metasploit Framework. Al comunicarse a través del socket del _stager_, proporciona una extensa API de Ruby del lado del cliente. Algunas de las características notables de `meterpreter` son las siguientes:

- **Sigiloso (_Stealthy_):** `Meterpreter` reside completamente en la memoria del sistema comprometido y no escribe nada en el disco. No genera ningún proceso nuevo; se inyecta a sí mismo en el proceso comprometido.4 Tiene la capacidad de migrar fácilmente a otros procesos en ejecución. Por defecto, `Meterpreter` se comunica a través de un canal cifrado. Esto deja un rastro limitado en el sistema comprometido desde una perspectiva forense.
    
- **Extensible:** Las funcionalidades se pueden añadir en tiempo de ejecución (_runtime_) y se cargan directamente a través de la red.5 Se pueden agregar nuevas funcionalidades a `Meterpreter` sin necesidad de reconstruirlo. El `payload` de `meterpreter` se ejecuta de manera fluida y muy rápida.

Antes de usar el exploit, necesitamos configurar el `payload` de `meterpreter` emitiendo el comando `use payload/windows/meterpreter/reverse_tcp` y luego estableciendo el valor de la variable `LHOST`.

## Volcado de hashes y cracking con JTR (John the Ripper)

- `post/windows/gather/hashdump`
	- Windows almacena las credenciales de usuario en un formato cifrado en su base de datos **SAM**. Una vez que hemos comprometido nuestro sistema objetivo, queremos obtener todas las credenciales de ese sistema. 

- `auxiliary/analyze/jtr_crack_fast`
	- Una vez que tenemos un volcado de las credenciales, el siguiente paso es crackearlas y recuperar las contraseñas en **texto plano**.

## Comando `shell`

Una vez que hemos explotado con éxito la vulnerabilidad y obtenido acceso a `meterpreter`, podemos usar el comando `shell` para obtener acceso a la línea de comandos del sistema comprometido (como se muestra en la siguiente captura de pantalla). El acceso a la línea de comandos te hará sentir como si estuvieras trabajando físicamente en el sistema objetivo.

## Escalada de privilegios

`Meterpreter` ofrece una funcionalidad para escalar privilegios. Primero, cargamos una extensión llamada `priv` y luego usamos el comando `getsystem` para escalar los privilegios.

Luego podemos verificar nuestro nivel de privilegio usando el comando `getuid`.

