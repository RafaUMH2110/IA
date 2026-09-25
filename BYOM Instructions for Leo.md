### Parte 2: Configuración BYOM en Leo (Paso a Paso)

A continuación, le detallo el procedimiento para configurar **Bring Your Own Model (BYOM)** en Leo, de modo que pueda usar sus claves de Anthropic (u OpenAI/Gemini) directamente desde el asistente, manteniendo la privacidad del proxy de Brave.

#### 1. Acceso a la configuración de BYOM

1. Abra **Brave Leo** (normalmente en la barra lateral o con `Ctrl+Shift+L` / `Cmd+Shift+L`).
2. Haga clic en el icono de **Configuración** (rueda dentada) en la parte inferior del chat de Leo.
3. Busque la sección **"Modelos"** o **"BYOM"** (Bring Your Own Model).

#### 2. Inyección de la API Key

1. Seleccione la opción **"Bring Your Own Model"** o **"Custom API"**.
2. En el campo **Proveedor**, elija el que corresponda a su clave (ej. **Anthropic**, **OpenAI**, **Google Gemini**).
3. En el campo **API Key**, pegue su clave completa (ej. `sk-ant-...`).
  - *Nota:* Leo no almacena esta clave en sus servidores; se usa en la memoria del navegador para la sesión actual.
4. En el campo **Endpoint** (si es editable), asegúrese de que apunte al estándar (ej. `https://api.anthropic.com/v1/messages`).
5. (Opcional) En el campo **System Prompt** o **Model Configuration**, puede ingresar el prompt específico para su clase de patronaje, por ejemplo:
  
  > "Eres un experto en patronaje textil. Sigue estrictamente la norma UNE-EN 13402. Devuelve respuestas en formato JSON."
  

#### 3. Validación y Prueba

1. Guarde la configuración.
2. Leo debería mostrar un mensaje de confirmación o un icono de conexión exitosa.
3. Realice una prueba simple:
  
  > "Genera un patrón de corte para un vestido de seda, talla M, con cierre invisible y mangas tres cuartos."
  
4. **Resultado esperado:** La respuesta debería generar el patrón con el formato y estilo definidos en su *system prompt*, y el coste se imputará a su cuenta de Anthropic.

* * *

### Parte 3: Wrapper en Python (Simulación de Proxy Anónimo)

Dado su experiencia en Python y control de sistemas, le presento una implementación minimalista pero robusta que replica la arquitectura **BYOM** fuera del navegador. Este script actúa como un "proxy anónimo" local, despojando las cabeceras identificativas antes de enviar la petición al proveedor de IA.

#### Código Python: `proxy_anonymous.py`

1. **Aislamiento de Headers:** El script elimina explícitamente cualquier cabecera que pueda identificar al usuario (como `User-Agent` personalizado o `X-Forwarded-For`). Esto simula el comportamiento del proxy de Brave, donde la IP se despoja antes de llegar al modelo.
2. **Gestión de Errores:** Incluye un mecanismo de reintentos con *backoff exponencial* (1s, 2s, 4s), típico en sistemas de control robustos para manejar intermitencia de red.
3. **Inyección de System Prompt:** Permite inyectar el *system prompt* específico para la tarea (patronaje), replicando la funcionalidad de BYOM en Leo.

Esta implementación le permite probar la topología BYOM fuera del navegador, o integrarla en sus aplicaciones Python existentes para tareas de investigación.