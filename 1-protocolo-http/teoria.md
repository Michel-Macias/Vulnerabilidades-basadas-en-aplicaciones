# El Protocolo HTTP

## 1. ¿Qué es HTTP?

HTTP (Hypertext Transfer Protocol) es el protocolo de comunicación que permite la transferencia de información en la World Wide Web. Es un protocolo **sin estado** basado en un modelo de **petición-respuesta** entre un cliente y un servidor.

*   **Cliente:** Generalmente un navegador web (Chrome, Firefox), pero también puede ser una herramienta como `curl` o un script.
*   **Servidor:** Un servidor web (Apache, Nginx) que aloja el contenido.

Al ser **sin estado**, cada petición es independiente y el servidor no guarda información sobre peticiones anteriores. Para mantener una sesión (por ejemplo, un login de usuario), se utilizan mecanismos como las **cookies**.

## 2. Mensajes HTTP: Peticiones y Respuestas

La comunicación HTTP se basa en el intercambio de mensajes. Hay dos tipos:

### 2.1. Petición HTTP (Request)

Es el mensaje que envía el cliente al servidor. Sus partes son:

*   **Línea de inicio (Start Line):**
    *   **Método (Method):** La acción que se desea realizar (ej. `GET`, `POST`).
    *   **URI (Uniform Resource Identifier):** El recurso que se solicita (ej. `/index.html`).
    *   **Versión de HTTP:** (ej. `HTTP/1.1`).

*   **Cabeceras (Headers):** Pares `clave: valor` que proporcionan información adicional sobre la petición. Algunas comunes son:
    *   `Host`: El dominio del servidor.
    *   `User-Agent`: Información del cliente (navegador, SO).
    *   `Accept`: Tipos de contenido que el cliente puede entender.
    *   `Cookie`: Datos de sesión.

*   **Cuerpo (Body):** (Opcional) Contiene los datos que se envían al servidor, por ejemplo, en un formulario con el método `POST`.

**Ejemplo de petición GET:**
```http
GET /omar.html HTTP/1.1
Host: web.h4cker.org
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
```

### 2.2. Respuesta HTTP (Response)

Es el mensaje que envía el servidor al cliente. Sus partes son:

*   **Línea de estado (Status Line):**
    *   **Versión de HTTP:** (ej. `HTTP/1.1`).
    *   **Código de estado:** Un número de 3 dígitos que indica el resultado de la petición (ej. `200`).
    *   **Mensaje de estado:** Un texto breve que describe el código (ej. `OK`).

*   **Cabeceras (Headers):** Pares `clave: valor` con información sobre la respuesta.
    *   `Server`: Información del software del servidor.
    *   `Content-Type`: El tipo de contenido del cuerpo (ej. `text/html`).
    *   `Content-Length`: El tamaño del cuerpo.
    *   `Set-Cookie`: Para establecer una cookie en el cliente.

*   **Cuerpo (Body):** El contenido del recurso solicitado (ej. el código HTML de una página).

**Ejemplo de respuesta:**
```http
HTTP/1.1 200 OK
Server: nginx/1.18.0
Content-Type: text/html; charset=UTF-8
Content-Length: 1234

<!DOCTYPE html>
<html>
...
</html>
```

## 3. Métodos HTTP

Indican la acción que se desea realizar sobre un recurso. Los más comunes son:

| Método | Descripción |
| :--- | :--- |
| `GET` | Recupera información del servidor. Es el método más común. |
| `POST` | Envía datos al servidor para crear un nuevo recurso (ej. un post en un blog, un nuevo usuario). |
| `PUT` | Actualiza un recurso existente en el servidor. |
| `DELETE` | Elimina un recurso específico. |
| `HEAD` | Similar a `GET`, pero solo devuelve las cabeceras, sin el cuerpo. Útil para comprobar si un recurso existe sin descargarlo. |
| `OPTIONS` | Devuelve los métodos HTTP que el servidor admite para una URL específica. |
| `CONNECT` | Convierte la conexión de la petición en un túnel TCP/IP transparente, usado para `HTTPS`. |
| `TRACE` | Realiza una prueba de bucle de retorno de mensajes que revela la ruta al recurso de destino. Puede ser peligroso si está mal configurado. |

## 4. Estructura de una URL

Una URL (Uniform Resource Locator) es la dirección de un recurso en la web.

`https://theartofhacking.org:8123/dir/test;id=89?name=omar&x=true`

*   **`https://`**: **Esquema** o protocolo.
*   **`theartofhacking.org`**: **Host** o dominio.
*   **`:8123`**: **Puerto** (opcional, `80` para HTTP y `443` para HTTPS son los predeterminados).
*   **`/dir/test`**: **Ruta** (Path) al recurso.
*   **`;id=89`**: **Parámetros de segmento de ruta** (Path-segment-parameters), menos comunes.
*   **`?name=omar&x=true`**: **Query String**, pares `clave=valor` para enviar datos al servidor.

## 5. Códigos de Estado HTTP

Indican si una petición se ha completado con éxito o si ha ocurrido un error. Se agrupan en 5 clases:

*   **`1xx` (Respuestas informativas):** La petición se ha recibido y el proceso continúa.
*   **`2xx` (Respuestas correctas):** La petición se ha recibido, entendido y aceptado.
    *   `200 OK`: Petición correcta.
    *   `201 Created`: Se ha creado un nuevo recurso.
*   **`3xx` (Redirecciones):** Se necesita realizar una acción adicional para completar la petición.
    *   `301 Moved Permanently`: El recurso se ha movido permanentemente a una nueva URL.
    *   `302 Found`: El recurso se ha movido temporalmente.
*   **`4xx` (Errores del cliente):** La petición contiene una sintaxis incorrecta o no puede procesarse.
    *   `400 Bad Request`: La petición es incorrecta.
    *   `401 Unauthorized`: Se requiere autenticación.
    *   `403 Forbidden`: El servidor se niega a responder, incluso con autenticación.
    *   `404 Not Found`: El recurso solicitado no se ha encontrado.
*   **`5xx` (Errores del servidor):** El servidor no ha podido completar una petición aparentemente válida.
    *   `500 Internal Server Error`: Un error genérico del servidor.
    *   `503 Service Unavailable`: El servidor no está disponible (sobrecargado o en mantenimiento).

## 6. Ampliación de Conceptos

### HTTPS: La versión segura

HTTPS (HTTP Secure) es la versión segura de HTTP. La comunicación se cifra utilizando **TLS (Transport Layer Security)**, lo que proporciona:

*   **Confidencialidad:** Impide que terceros lean la comunicación.
*   **Integridad:** Asegura que los datos no han sido modificados.
*   **Autenticación:** Verifica la identidad del servidor al que te conectas.

### HTTP/1.1 vs HTTP/2.0

El documento menciona ambas versiones. La principal mejora de **HTTP/2.0** es el rendimiento:

*   **Multiplexación:** Permite realizar múltiples peticiones y recibir múltiples respuestas de forma simultánea a través de una única conexión TCP, eliminando el "head-of-line blocking" de HTTP/1.1.
*   **Compresión de cabeceras:** Reduce el tamaño de las cabeceras para disminuir la latencia.

### Proxies de Interceptación

Herramientas como **Burp Suite** y **OWASP ZAP** actúan como un "hombre en el medio" (Man-in-the-Middle) entre tu navegador y el servidor. Permiten:

*   **Interceptar y ver** todas las peticiones y respuestas.
*   **Modificar** los datos al vuelo antes de que lleguen a su destino.
*   **Automatizar** ataques para buscar vulnerabilidades.

Son herramientas esenciales para cualquier hacker ético.