# Laboratorio: El Protocolo HTTP en Acción

## 1. Entorno de Trabajo

*   **SO Anfitrión:** Ubuntu con Virt-Manager y QEMU.
*   **Máquinas Virtuales:**
    *   Kali Linux (nuestra máquina de atacante).
    *   Windows Server (nuestro objetivo, aunque para este lab no será necesario).
*   **Red:** Red virtual interna que conecta las VMs.

## 2. Objetivos

*   Familiarizarse con herramientas de línea de comandos para interactuar con servidores web.
*   Entender la estructura de una petición y respuesta HTTP de forma práctica.
*   Visualizar el tráfico HTTP.

## 3. Prácticas

### Práctica 1: Realizar peticiones con `curl`

`curl` es una herramienta fundamental para cualquier pentester. Nos permite realizar peticiones HTTP desde la línea de comandos.

1.  **Abre una terminal en tu Kali Linux.**

2.  **Realiza una petición GET simple:** Vamos a hacer una petición a un sitio web de ejemplo.

    ```bash
    curl http://example.com
    ```

    Verás el código HTML de la página como respuesta.

3.  **Ver las cabeceras de la respuesta:** Para entender mejor lo que nos devuelve el servidor, podemos usar la opción `-v` (verbose) o `-i` para incluir las cabeceras.

    ```bash
    curl -i http://example.com
    ```

    **Salida esperada (similar a esta):**
    ```http
    HTTP/1.1 200 OK
    Content-Encoding: gzip
    Accept-Ranges: bytes
    Age: 531993
    Cache-Control: max-age=604800
    Content-Type: text/html; charset=UTF-8
    Date: Mon, 03 Nov 2025 12:00:00 GMT
    Etag: "3147526947"
    Expires: Mon, 10 Nov 2025 12:00:00 GMT
    Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
    Server: ECS (sjc/4E76)
    Vary: Accept-Encoding
    X-Cache: HIT
    Content-Length: 648

    <!doctype html>
    <html>
    ...
    </html>
    ```
    *   **Analiza la salida:** Fíjate en la línea de estado (`HTTP/1.1 200 OK`), las cabeceras (`Content-Type`, `Server`, etc.) y el cuerpo HTML.

### Práctica 2: "Hablando" HTTP con `netcat`

`netcat` (o `nc`) es la "navaja suiza" de las redes. Nos permite leer y escribir datos a través de conexiones de red usando TCP o UDP. Vamos a usarla para escribir una petición HTTP a mano.

1.  **En tu terminal de Kali, conéctate al puerto 80 de `example.com`:**

    ```bash
    nc example.com 80
    ```

2.  **Escribe la petición HTTP:** Una vez conectado, escribe las siguientes líneas EXACTAMENTE como se muestran. **Después de la última cabecera, presiona Enter DOS VECES**.

    ```http
    GET / HTTP/1.1
    Host: example.com

    ```

3.  **Analiza la respuesta:** El servidor te devolverá la misma respuesta que obtuviste con `curl`. Este ejercicio te ayuda a interiorizar la estructura de una petición HTTP.

### Práctica 3: Espiando el tráfico con `tcpdump`

`tcpdump` es un sniffer de red en línea de comandos. Nos permitirá ver la conversación completa entre nuestro cliente y el servidor.

1.  **Abre una nueva terminal en Kali.** Necesitarás privilegios de superusuario.

2.  **Inicia la captura:** Vamos a escuchar el tráfico en nuestra interfaz de red (probablemente `eth0`) que vaya hacia `example.com`.

    ```bash
    sudo tcpdump -i eth0 host example.com -A
    ```
    *   `-i eth0`: Especifica la interfaz de red. Cámbiala si la tuya es diferente (puedes verla con `ip a`).
    *   `host example.com`: Filtra el tráfico para que solo nos muestre el que tiene como origen o destino `example.com`.
    *   `-A`: Muestra el contenido de los paquetes en ASCII, lo que nos permite leer las peticiones y respuestas HTTP.

3.  **En tu otra terminal, realiza una petición con `curl` o `netcat` como en las prácticas anteriores.**

4.  **Vuelve a la terminal de `tcpdump` y observa la salida.** Verás:
    *   El "three-way handshake" de TCP (SYN, SYN-ACK, ACK).
    *   La petición HTTP que has enviado.
    *   La respuesta HTTP del servidor.

    Para detener la captura, presiona `Ctrl+C`.

### Práctica 4 (Opcional): Tu propio servidor web

Para no depender de sitios externos, puedes levantar un servidor web muy simple en tu Kali para hacer pruebas.

1.  **Crea un archivo `index.html`:**
    ```bash
    echo "<h1>Hola, mundo!</h1>" > index.html
    ```

2.  **Levanta el servidor:** Python tiene un módulo HTTP muy útil.
    *   Para Python 3:
        ```bash
        python3 -m http.server 8000
        ```
    *   Para Python 2:
        ```bash
        python -m SimpleHTTPServer 8000
        ```

3.  **Ahora puedes usar `curl`, `netcat` o tu navegador para acceder a `http://localhost:8000`** y verás tu página. También puedes usar `tcpdump` para analizar el tráfico local.