# Laboratorio 2: Interactuando con un Entorno Docker

## 1. Entorno de Trabajo

*   **Red:** Docker, con la subred `172.18.0.0/16`.
*   **Máquina Atacante:** Kali Linux.
*   **Objetivo Principal:** Aplicación web **OWASP Juice Shop** corriendo en `http://172.18.0.4:3000`.

## 2. Objetivos

*   Aplicar los conocimientos de HTTP contra una aplicación web real y vulnerable.
*   Aprender a descubrir y analizar las peticiones a la API de una aplicación moderna (Single Page Application).
*   Realizar peticiones `GET` y `POST` a puntos específicos de la aplicación.
*   Visualizar e interpretar el tráfico HTTP con una herramienta gráfica como Wireshark.

## 3. Prácticas

### Práctica 1: Reconocimiento Básico del Juice Shop

Vamos a empezar explorando la aplicación desde la línea de comandos.

1.  **Abre una terminal en tu Kali Linux.**

2.  **Explora la página principal:** Usa `curl` para ver las cabeceras que devuelve el servidor de Juice Shop.

    ```bash
    curl -v http://172.18.0.4:3000
    ```

    *   **Analiza la salida:** Fíjate en las cabeceras `Server`, `X-Powered-By`, `Set-Cookie`, etc. ¿Qué información puedes deducir sobre la tecnología que usa la aplicación (Node.js, Express)?

### Práctica 2: Descubriendo la API

Juice Shop, como muchas aplicaciones modernas, carga una página principal y luego realiza múltiples peticiones a una API REST para obtener los datos (productos, comentarios, etc.).

1.  **Abre el navegador en tu Kali y visita `http://172.18.0.4:3000`.**

2.  **Abre las Herramientas de Desarrollador:** Presiona `F12` y ve a la pestaña **"Network"** (o "Red").

3.  **Navega por la página:** Haz clic en productos, usa el buscador, etc. Verás cómo aparecen nuevas peticiones en la pestaña "Network".

4.  **Filtra por `Fetch/XHR`:** Esto te mostrará solo las peticiones a la API.

5.  **Busca una petición interesante:** Por ejemplo, la que se hace al buscar un producto. Verás algo como `http://172.18.0.4:3000/rest/products/search?q=...`

6.  **Replica la petición con `curl`:** Ahora que conoces el *endpoint* de la API, puedes consultarlo directamente.

    ```bash
    # Busca productos que contengan la palabra "apple"
    curl http://172.18.0.4:3000/rest/products/search?q=apple
    ```

    La respuesta será un JSON con la lista de productos. ¡Acabas de interactuar directamente con la API de la aplicación!

### Práctica 3: Realizando un POST a la API (Simulando un Login)

Ahora vamos a simular el envío de un formulario, como el de inicio de sesión.

1.  **En las Herramientas de Desarrollador del navegador**, busca la petición que se realiza al intentar iniciar sesión (aunque falles). Será una petición `POST` a un *endpoint* como `/rest/user/login`.

2.  **Analiza la petición:** Haz clic en ella y mira la pestaña **"Headers"** para ver la URL y el método, y la pestaña **"Request"** o **"Payload"** para ver el cuerpo (body) que se envía. Verás que es un JSON con el email y la contraseña.

3.  **Replica la petición de login con `curl`:**

    ```bash
    curl -X POST http://172.18.0.4:3000/rest/user/login \
    -H "Content-Type: application/json" \
    -d '{"email": "test@example.com", "password": "password123"}'
    ```

    *   `-X POST`: Especifica que el método es POST.
    *   `-H "Content-Type: application/json"`: Indica al servidor que le estamos enviando datos en formato JSON.
    *   `-d '{...}'`: Define el cuerpo (body) de la petición.

4.  **Analiza la respuesta:** El servidor te responderá probablemente con un error de "Invalid email or password", pero lo importante es que has conseguido enviar datos a la aplicación y recibir una respuesta. En un ataque real, aquí es donde empezarías a probar vulnerabilidades como SQL Injection o a intentar fuerza bruta.

### Práctica 4: Visualizando el Tráfico con Wireshark

Mientras que `tcpdump` es genial para la línea de comandos, Wireshark nos da una visión gráfica y mucho más detallada del tráfico.

1.  **Inicia Wireshark en tu Kali:** Puedes encontrarlo en el menú de aplicaciones o ejecutar `wireshark` en una terminal (requiere `sudo`).

2.  **Selecciona la interfaz de red:** En la pantalla de inicio de Wireshark, haz doble clic en tu interfaz de red (probablemente `eth0`) para empezar a capturar.

3.  **Aplica un filtro de visualización:** Para no ahogarte en paquetes, vamos a filtrar solo el tráfico HTTP hacia y desde el Juice Shop. En la barra de "Apply a display filter", escribe lo siguiente y presiona Enter:
    ```
    ip.addr == 172.18.0.4 && http
    ```

4.  **Genera tráfico:** Con Wireshark capturando, realiza las acciones de las prácticas anteriores:
    *   Visita `http://172.18.0.4:3000` en tu navegador.
    *   Usa el buscador de la página.
    *   Intenta hacer login.

5.  **Analiza los resultados en Wireshark:**
    *   Verás una lista de todas las peticiones y respuestas HTTP.
    *   Haz clic en una petición `GET` o `POST`. En el panel inferior, podrás expandir la sección "Hypertext Transfer Protocol" para ver las cabeceras y el cuerpo de forma ordenada.
    *   **Función clave:** Haz clic derecho en una petición HTTP y selecciona **Follow > TCP Stream**. Esto abrirá una nueva ventana mostrándote la conversación completa y sin procesar entre tu máquina y el servidor, permitiéndote ver exactamente lo que se envió y recibió.
