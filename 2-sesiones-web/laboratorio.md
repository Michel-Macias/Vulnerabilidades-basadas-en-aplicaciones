# Laboratorio: Análisis de Sesiones con Burp Suite

## 1. Entorno y Herramientas

*   **Máquina Atacante:** Kali Linux.
*   **Objetivo:** OWASP Juice Shop en `http://172.18.0.4:3000`.
*   **Herramienta Principal:** Burp Suite Community Edition.

## 2. Objetivos

*   Configurar un navegador para usar Burp Suite como proxy.
*   Interceptar y analizar el tráfico HTTP para identificar cookies de sesión.
*   Entender cómo una aplicación gestiona las sesiones antes y después de la autenticación.
*   Simular un ataque de robo de sesión (Session Hijacking) usando Burp Repeater.

## 3. Prácticas

### Práctica 1: Configuración de Burp Suite y el Navegador

Este es el paso más importante. Debemos redirigir el tráfico de nuestro navegador a través de Burp para poder analizarlo.

1.  **Inicia Burp Suite** en tu Kali Linux.

2.  **Configura el Proxy en Burp:**
    *   Ve a la pestaña **Proxy** -> **Options**.
    *   Asegúrate de que hay un proxy listener activo en la interfaz `127.0.0.1` y el puerto `8080`. Debería estar ahí por defecto.

3.  **Configura el Navegador (Firefox en Kali):**
    *   Abre Firefox y ve a **Settings** (o Preferencias).
    *   Busca "proxy" y selecciona **"Manual proxy configuration"**.
    *   En **HTTP Proxy**, introduce `127.0.0.1` y en **Port** `8080`.
    *   Marca la casilla **"Use this proxy for all protocols"**.
    *   Guarda la configuración.

4.  **Verifica la configuración:**
    *   En Burp, ve a la pestaña **Proxy** -> **Intercept** y asegúrate de que el botón dice **"Intercept is on"**.
    *   En Firefox, intenta visitar cualquier página, por ejemplo `http://example.com`. La página no cargará y Burp capturará la petición, mostrándotela en la pestaña "Intercept".
    *   Puedes hacer clic en **"Forward"** para enviar la petición o en **"Drop"** para descartarla. Por ahora, haz clic en el botón **"Intercept is on"** para que cambie a **"Intercept is off"**. Esto nos permitirá navegar libremente mientras Burp registra todo en segundo plano.

### Práctica 2: Identificando la Cookie de Sesión

Ahora que el tráfico pasa por Burp, vamos a ver cómo Juice Shop nos asigna una sesión.

1.  **Limpia el historial en Burp:** Ve a la pestaña **Proxy** -> **HTTP history**. Si hay tráfico anterior, puedes limpiarlo para empezar de cero (clic derecho -> "Clear history").

2.  **Visita el Juice Shop:** En tu navegador configurado, ve a `http://172.18.0.4:3000`.

3.  **Analiza el historial HTTP en Burp:**
    *   Vuelve a Burp y mira la pestaña **"HTTP history"**.
    *   Busca la primera petición `GET /` al host `172.18.0.4`.
    *   Selecciónala y mira la **Respuesta (Response)** en el panel inferior.
    *   Busca la cabecera `Set-Cookie`. Verás algo como `Set-Cookie: io=...` o `Set-Cookie: connect.sid=...`. ¡Esa es tu cookie de sesión de usuario anónimo!

4.  **Observa las peticiones siguientes:** Haz clic en cualquier otra petición que el navegador haya hecho después de la primera. Ahora, en el panel de la **Petición (Request)**, verás que el navegador incluye automáticamente la cabecera `Cookie: io=...` que el servidor le dio. Así es como el servidor te "recuerda".

### Práctica 3: Manipulación de Sesión con Repeater

Vamos a demostrar que la cookie es la única prueba de identidad para el servidor.

1.  **Inicia sesión en Juice Shop:** En el navegador, crea una cuenta o usa una existente para iniciar sesión.

2.  **Busca la cookie de sesión autenticada:** En el historial de Burp, busca la petición `POST /rest/user/login`. En la respuesta a esa petición, el servidor puede que te asigne una nueva cookie o que simplemente la asocie a tu usuario. En las peticiones posteriores a estar logueado, verás la cookie que te identifica como usuario autenticado.

3.  **Prepara el ataque:**
    *   Cierra la sesión en el navegador.
    *   En el historial de Burp, busca una petición que hiciste **antes de iniciar sesión** (por ejemplo, una petición a la API como `/rest/products/search?q=...`).
    *   Haz clic derecho sobre esa petición y selecciona **"Send to Repeater"**.

4.  **Ejecuta el ataque en Repeater:**
    *   Ve a la pestaña **Repeater**.
    *   A la izquierda tienes la petición original (sin estar logueado). Si la envías (botón "Send"), la respuesta de la derecha no mostrará datos de usuario.
    *   Ahora, vuelve a la pestaña **Proxy -> HTTP history**, busca una petición que hiciste **estando logueado** y copia el valor de su cabecera `Cookie` (ej: `io=AbCdEfGh...`).
    *   Vuelve a **Repeater**. Pega o reemplaza la cabecera `Cookie` de la petición con la que acabas de copiar (la de la sesión autenticada).
    *   Haz clic en **"Send"** de nuevo.

5.  **Analiza el resultado:** ¡Magia! La respuesta ahora debería ser la de un usuario autenticado. Has "secuestrado" la sesión simplemente usando el token de sesión correcto. Esto demuestra que si un atacante consigue robar esa cookie, puede suplantar tu identidad completamente.