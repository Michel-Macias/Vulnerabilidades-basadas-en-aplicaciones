# Teoría: Gestión de Sesiones Web

## 1. ¿Qué es una Sesión Web?

Dado que HTTP es un protocolo "sin estado", no puede recordar quién eres entre una petición y otra. Una **sesión web** es un mecanismo para solucionar esto. Es una secuencia de interacciones entre un cliente y un servidor que permite a la aplicación:

*   **Rastrear a los usuarios:** Tanto a usuarios anónimos (para recordar un carrito de la compra o el idioma) como a usuarios autenticados.
*   **Mantener el estado:** Conservar información como los derechos de acceso o las preferencias del usuario a lo largo de su navegación.

La sesión comienza con la primera petición del usuario y finaliza cuando se cierra la sesión (logout) o expira el tiempo de inactividad.

## 2. El ID de Sesión (Token)

Para que el servidor pueda identificar a cada usuario, le entrega un "ticket" único al iniciar la sesión. Este ticket es el **ID de Sesión** o **token de sesión**.

*   Es un par `nombre=valor` (ej. `PHPSESSID=a1b2c3d4e5f6`).
*   Se intercambia en cada petición y respuesta para que el servidor sepa quién está haciendo la solicitud.
*   Una vez que un usuario se autentica, **este token es temporalmente equivalente a su usuario y contraseña**. ¡Por eso es tan crítico protegerlo!

### Características de un ID de Sesión Seguro:

*   **Nombre Genérico:** El nombre del token no debe delatar la tecnología del servidor (ej. `PHPSESSID` revela que se usa PHP). Es mejor usar nombres genéricos como `id` o `session_token`.
*   **Longitud:** Debe ser lo suficientemente largo para evitar ataques de fuerza bruta. El estándar mínimo recomendado es de **128 bits** (16 bytes).
*   **Aleatoriedad (Entropía):** Debe ser impredecible. Se deben usar generadores de números pseudoaleatorios criptográficamente seguros para crearlos. Un atacante no debería poder adivinar el siguiente ID de sesión válido.

## 3. Mecanismos de Intercambio de Tokens

Existen varias formas de intercambiar el token de sesión entre el cliente y el servidor.

### 3.1. Cookies (Método Preferido)

Las cookies son pequeños datos que el servidor envía al cliente para que los almacene y los envíe de vuelta en futuras peticiones. Son el mecanismo más común y seguro si se configuran bien.

*   **Cookie de Sesión (No persistente):** No tiene fecha de expiración (`Expires` o `Max-Age`). El navegador la borra cuando se cierra. Es la ideal para IDs de sesión.
*   **Cookie Persistente:** Tiene una fecha de expiración. El navegador la guarda en disco hasta que caduque. Útil para recordar preferencias a largo plazo ("Recordarme en este equipo"), pero no recomendada para tokens de sesión críticos.

### 3.2. Reescritura de URL (URL Rewriting)

El ID de sesión se añade directamente a la URL como un parámetro.

`http://example.com/page?session_id=a1b2c3d4e5f6`

**¡Esta práctica es muy insegura y debe evitarse!**
*   **Riesgos:**
    *   El ID queda expuesto en el historial del navegador, logs de proxies y firewalls.
    *   Si un usuario comparte el enlace, comparte su sesión.
    *   Facilita enormemente los ataques de **fijación de sesión (Session Fixation)**.

### 3.3. Campos Ocultos de Formulario

El token se incluye en un campo oculto (`<input type="hidden">`) dentro de un formulario HTML. Solo es válido para las peticiones que se envían a través de ese formulario, por lo que no es un mecanismo general para mantener la sesión en toda la aplicación.

## 4. Riesgos y Ataques a la Sesión

*   **Robo de Cookies (Sniffing):** Si la comunicación no va cifrada (HTTP), un atacante en la misma red puede capturar el tráfico y robar la cookie de sesión. **Solución: Usar HTTPS siempre.**
*   **Fijación de Sesión (Session Fixation):** El atacante "fija" un ID de sesión en el navegador de la víctima (por ejemplo, enviándole un enlace con el ID en la URL). Cuando la víctima se loguea, el atacante, que conoce el ID, puede usar esa misma sesión para acceder a la cuenta de la víctima.
*   **Cross-Site Scripting (XSS):** Un atacante puede inyectar un script en la página que robe la cookie de la víctima y la envíe a un servidor controlado por el atacante.
*   **Adivinación de ID de Sesión:** Si los tokens no son suficientemente aleatorios, un atacante podría intentar adivinar o predecir un ID de sesión válido.

## 5. Buenas Prácticas y Ampliación

### Usar los Frameworks Nativos

Casi todos los frameworks de desarrollo web (PHP, ASP.NET, Ruby on Rails, Django, etc.) proporcionan un sistema de gestión de sesiones robusto y probado. **Siempre es mejor usar el sistema del framework que intentar crear uno propio desde cero.**

### JSON Web Tokens (JWT)

En aplicaciones modernas, especialmente en APIs REST y Single Page Applications (SPAs), es muy común usar **JWT**.

*   **¿Qué es?** Un JWT es un token que contiene información (llamada *claims*) en formato JSON. Este token está firmado digitalmente por el servidor.
*   **¿Cómo funciona?** En lugar de que el servidor almacene la información de la sesión, la propia información viaja dentro del token. Cuando el cliente envía el JWT, el servidor verifica la firma para asegurarse de que no ha sido manipulado.
*   **Ventaja:** Es un mecanismo "sin estado" del lado del servidor, lo que lo hace muy escalable. El servidor no necesita mantener una tabla de sesiones activas.