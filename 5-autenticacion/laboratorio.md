# 5.2. Autenticación: Laboratorio

[Este laboratorio se centrará en la identificación y explotación de vulnerabilidades relacionadas con la autenticación, como ataques de fuerza bruta, fijación de sesión o bypass de autenticación. Se utilizarán herramientas como Burp Suite para interceptar y manipular solicitudes de autenticación.]

## Objetivos del Laboratorio:

*   Identificar mecanismos de autenticación en aplicaciones web.
*   Detectar y explotar vulnerabilidades de fuerza bruta en formularios de login.
*   Comprender y mitigar ataques de fijación de sesión.
*   Analizar la gestión de sesiones y sus posibles debilidades.

## Herramientas a Utilizar:

*   Burp Suite (Community Edition)
*   OWASP Juice Shop (como aplicación objetivo)
*   Navegador web con herramientas de desarrollador

## Pasos del Laboratorio:

[Aquí se detallarán los pasos específicos para cada ejercicio. Por ejemplo:]

### Ejercicio 1: Ataque de Fuerza Bruta a un Formulario de Login

1.  **Configurar Burp Suite:** Asegúrate de que tu navegador esté configurado para usar Burp Suite como proxy.
2.  **Acceder al Login:** Navega a la página de login de OWASP Juice Shop.
3.  **Capturar Solicitud:** Introduce credenciales inválidas (ej. `admin`/`password`) y captura la solicitud POST de login en Burp Proxy.
4.  **Enviar a Intruder:** Envía la solicitud capturada a Burp Intruder.
5.  **Configurar Posiciones:** Define las posiciones para el ataque de fuerza bruta en el campo de la contraseña.
6.  **Cargar Payload:** Carga un diccionario de contraseñas comunes (ej. `rockyou.txt` o uno más pequeño para pruebas).
7.  **Iniciar Ataque:** Inicia el ataque y analiza las respuestas para identificar la contraseña correcta (buscando diferencias en la longitud de la respuesta o mensajes de éxito).

### Ejercicio 2: Análisis de Fijación de Sesión

1.  **Obtener ID de Sesión Pre-Login:** Antes de iniciar sesión, intercepta una solicitud a la aplicación y anota el ID de sesión que te asigna el servidor.
2.  **Compartir ID (Simulado):** Simula que un atacante comparte este ID de sesión contigo (ej. a través de un enlace malicioso).
3.  **Iniciar Sesión:** Inicia sesión con tus credenciales legítimas.
4.  **Verificar ID de Sesión:** Comprueba si el ID de sesión ha cambiado después de la autenticación. Si no ha cambiado, la aplicación es vulnerable a fijación de sesión.

### Ejercicio 3: Bypass de Autenticación (si aplica)

[Se diseñará un ejercicio específico si se identifica una vulnerabilidad de bypass en la aplicación objetivo, por ejemplo, manipulando cookies o parámetros de URL.]

## Notas y Observaciones:

[Espacio para que el usuario anote sus hallazgos, capturas de pantalla, comandos ejecutados y cualquier otra observación relevante durante el laboratorio.]
