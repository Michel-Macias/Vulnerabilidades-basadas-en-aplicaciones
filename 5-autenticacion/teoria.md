# 5.1. Autenticación: Teoría

La autenticación es el proceso de verificar la identidad de un usuario, sistema o entidad. Es un pilar fundamental de la seguridad en cualquier aplicación, ya que asegura que solo los usuarios legítimos puedan acceder a los recursos y funcionalidades para los que tienen permiso.

## 5.1.1. Principios Clave de una Autenticación Segura

Basado en la [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html), estos son algunos principios fundamentales:

*   **Credenciales Fuertes:** Implementar políticas de contraseñas robustas (longitud, complejidad, caducidad) y considerar métodos de autenticación multifactor (MFA).
*   **Almacenamiento Seguro de Contraseñas:** Nunca almacenar contraseñas en texto plano. Utilizar funciones hash criptográficas fuertes y con "salt" (ej. Argon2, bcrypt, scrypt).
*   **Protección contra Ataques de Fuerza Bruta:** Implementar mecanismos de bloqueo de cuentas, retrasos exponenciales y CAPTCHAs para prevenir ataques de adivinación de contraseñas.
*   **Gestión de Sesiones Segura:** Generar IDs de sesión aleatorios y de alta entropía, usar cookies seguras (flags `HttpOnly`, `Secure`, `SameSite`), y invalidar sesiones correctamente al cerrar sesión o por inactividad.
*   **Manejo de Errores:** Evitar mensajes de error genéricos que puedan dar pistas a un atacante sobre la existencia de usuarios o la validez de las credenciales.
*   **Autenticación Multifactor (MFA):** Siempre que sea posible, implementar MFA para añadir una capa extra de seguridad, requiriendo dos o más factores de autenticación (algo que sabes, algo que tienes, algo que eres).
*   **Recuperación de Contraseñas Segura:** Implementar procesos de recuperación de contraseñas que sean robustos y no permitan la enumeración de usuarios o el restablecimiento no autorizado.
*   **Protección contra Enumeración de Usuarios:** Asegurarse de que los mensajes de error y los flujos de autenticación no permitan a un atacante determinar si un nombre de usuario existe o no.

## 5.1.2. Tipos Comunes de Autenticación

*   **Basada en Contraseña:** El método más común, donde el usuario proporciona un nombre de usuario y una contraseña.
*   **Autenticación Multifactor (MFA):** Combina dos o más factores (ej. contraseña + código de un token, huella dactilar).
*   **Basada en Certificados:** Utiliza certificados digitales para verificar la identidad.
*   **OAuth/OpenID Connect:** Protocolos de autorización y autenticación que permiten a los usuarios iniciar sesión en una aplicación utilizando sus credenciales de otro servicio (ej. Google, Facebook).
*   **Tokens (JWT):** JSON Web Tokens son una forma compacta y segura de transmitir información entre partes como un objeto JSON. Se utilizan comúnmente para la autenticación y autorización en APIs RESTful.

## 5.1.3. Vulnerabilidades Comunes en la Autenticación

Las vulnerabilidades en la autenticación son una de las principales causas de brechas de seguridad. Algunas de las más comunes incluyen:

*   **Credenciales Débiles o por Defecto:** Uso de contraseñas fáciles de adivinar o credenciales predeterminadas que no se cambian.
*   **Ataques de Fuerza Bruta y Relleno de Credenciales (Credential Stuffing):** Intentos repetidos de adivinar contraseñas o usar credenciales robadas de otras brechas.
*   **Fijación de Sesión (Session Fixation):** Un atacante fija el ID de sesión de una víctima antes de que esta inicie sesión, permitiendo al atacante secuestrar la sesión una vez autenticada.
*   **Secuestro de Sesión (Session Hijacking):** Un atacante roba un ID de sesión válido y lo utiliza para suplantar al usuario legítimo.
*   **Mala Gestión de Sesiones:** Sesiones que no caducan, IDs de sesión predecibles o transmitidos de forma insegura.
*   **Omisión de Autenticación:** Fallos lógicos que permiten a un atacante eludir el proceso de autenticación por completo.
*   **Enumeración de Usuarios:** Mensajes de error que revelan si un nombre de usuario existe o no, facilitando ataques de fuerza bruta.

## 5.1.4. Recursos Adicionales

*   **OWASP Authentication Cheat Sheet:** [https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
*   **OWASP Top 10 - A07:2021-Identification and Authentication Failures:** [https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)
