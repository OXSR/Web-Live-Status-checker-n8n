# Web-Live-Status-checker-n8n

Este flujo de trabajo de n8n monitoriza de forma periódica cada 30min un conjunto de URLs y envía un correo de alerta cuando alguna deja de responder o devuelve error.  
Está pensado para administradores web, equipos DevOps o cualquier persona que necesite saber al instante si un sitio está caído.

Estructura del flujo
--------------------

1. **Schedule Trigger**  
   - Ejecuta el workflow cada 30 minutos (intervalo configurable).

2. **Edit Fields – “URL Web”**  
   - Input manual de una o varias URLs a escanear.  
   - Permite añadir/quitar URLs sin tocar el resto del flujo.

3. **HTTP Request (Petición 1)**  
   - Lanza una petición GET a cada URL.  
   - Opción *Continue on Fail* activada para capturar también los fallos de red.

4. **HTTP Request (Petición 2 – redundante)**  
   - Segunda llamada opcional a un endpoint distinto (o al mismo con headers diferentes) para comparar resultados o hacer un fallback.

5. **Filtro**  
   - Deja pasar solo las respuestas con código de estado no deseado (por ejemplo ≥ 400) o errores de red (`ENOTFOUND`, `ECONNREFUSED`, etc.).

6. **Code – “Filtro URL”**  
   - Devuelve un objeto limpio con la URL problemática y el tipo de fallo.

7. **Gmail – “Email”**  
   - Envía un correo de alerta con la URL caída, el motivo y la marca temporal.  
   - Se puede sustituir por Slack, Telegram, Discord, etc., cambiando el nodo.

