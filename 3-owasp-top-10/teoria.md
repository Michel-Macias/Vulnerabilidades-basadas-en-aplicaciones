# Teoría: OWASP Top 10 (Versión 2021)

## 1. ¿Qué es el OWASP Top 10?

El **Proyecto Abierto de Seguridad de Aplicaciones Web (OWASP)** es una comunidad online y una organización sin ánimo de lucro dedicada a mejorar la seguridad del software.

El **OWASP Top 10** es su documento más conocido. Es una lista que se actualiza cada pocos años para crear conciencia sobre los 10 riesgos de seguridad más críticos en las aplicaciones web. No es un estándar exhaustivo, sino una guía de las vulnerabilidades más comunes y peligrosas que los desarrolladores y profesionales de la seguridad deben conocer.

**Nota:** Esta lista se basa en la versión de 2021. La versión de 2025 se espera para noviembre de 2025.

---

## La Lista OWASP Top 10 de 2021

### A01:2021 – Broken Access Control (Pérdida de Control de Acceso)
El control de acceso implementa políticas para que los usuarios no puedan actuar fuera de los permisos previstos. Las fallas suelen conducir a la divulgación de información no autorizada, a la modificación o destrucción de todos los datos o a la realización de una función empresarial fuera de los límites del usuario.

*   **Explicación Sencilla:** Ocurre cuando un usuario puede acceder a partes de la aplicación a las que no debería tener permiso.
*   **Ejemplo:** Un usuario normal que, cambiando un número en la URL (de `.../user/123` a `.../user/124`), puede ver o editar la información de otro usuario. O un usuario que accede a un panel de administración simplemente conociendo la URL (`/admin`).

### A02:2021 – Cryptographic Failures (Fallos Criptográficos)
Anteriormente conocido como *Exposición de datos sensibles*, se centra en las fallas relacionadas con la criptografía (o la falta de ella). Los fallos más comunes son el uso de algoritmos débiles, la mala gestión de claves o no cifrar datos sensibles en absoluto.

*   **Explicación Sencilla:** Proteger mal los datos importantes, como contraseñas, números de tarjetas de crédito o información personal.
*   **Ejemplo:** Guardar las contraseñas de los usuarios en la base de datos como texto plano en lugar de usar un hash seguro (como Argon2 o bcrypt).

### A03:2021 – Injection (Inyección)
Las fallas de inyección, como la inyección de SQL, NoSQL, comandos del sistema operativo y LDAP, ocurren cuando datos no confiables se envían a un intérprete como parte de un comando o consulta. Los datos hostiles del atacante pueden engañar al intérprete para que ejecute comandos no intencionados o acceda a los datos sin la debida autorización.

*   **Explicación Sencilla:** Un atacante envía "código malicioso" camuflado como datos normales, y la aplicación lo ejecuta por error.
*   **Ejemplo (SQL Injection):** En un campo de login, en lugar de una contraseña, un atacante escribe ` ' OR '1'='1' -- `. Si la aplicación es vulnerable, esta entrada puede engañar a la base de datos para que la consulta de login siempre sea verdadera, permitiendo el acceso sin una contraseña válida.

### A04:2021 – Insecure Design (Diseño Inseguro)
Esta es una nueva categoría para 2021 que se centra en los riesgos relacionados con las fallas de diseño y arquitectura. Un diseño inseguro no puede solucionarse implementando un control perfecto; requiere un rediseño.

*   **Explicación Sencilla:** La aplicación fue construida desde el principio con una lógica de negocio que tiene fallos de seguridad inherentes.
*   **Ejemplo:** Un sistema de venta de entradas para un cine que permite a un usuario seleccionar un asiento, pero no lo bloquea mientras realiza el pago. Otro usuario podría comprar la misma entrada mientras el primero introduce sus datos de pago.

### A05:2021 – Security Misconfiguration (Configuración de Seguridad Incorrecta)
Esto puede incluir la falta de un hardening de seguridad adecuado en cualquier parte de la pila de aplicaciones, configuraciones incorrectas, mensajes de error demasiado detallados, o tener funcionalidades de depuración o cuentas por defecto habilitadas en producción.

*   **Explicación Sencilla:** Dejar la "puerta de servicio" abierta o no configurar correctamente la seguridad del servidor o la aplicación.
*   **Ejemplo:** Dejar habilitada la cuenta de administrador por defecto (`admin/admin`) en un framework, o un servidor que muestra mensajes de error detallados que revelan información sobre la tecnología interna o la estructura de la base de datos.

### A06:2021 – Vulnerable and Outdated Components (Componentes Vulnerables y Obsoletos)
Ocurre cuando se utilizan librerías, frameworks u otros componentes de software con vulnerabilidades conocidas. Si tu aplicación utiliza un componente de un tercero que es vulnerable, tu aplicación también lo es.

*   **Explicación Sencilla:** Usar "piezas" de software de terceros que son viejas y tienen agujeros de seguridad conocidos.
*   **Ejemplo:** Construir una aplicación usando una versión de 2015 de una librería popular (como jQuery o Log4j) que tiene vulnerabilidades de seguridad críticas que ya han sido descubiertas y parcheadas en versiones más nuevas.

### A07:2021 – Identification and Authentication Failures (Fallos de Identificación y Autenticación)
Las fallas en las funciones de autenticación y gestión de sesiones pueden permitir a los atacantes comprometer contraseñas, claves o tokens de sesión, o explotar otras fallas de implementación para asumir las identidades de otros usuarios temporal o permanentemente.

*   **Explicación Sencilla:** Fallos en el sistema de "login" o en cómo la aplicación recuerda quién eres.
*   **Ejemplo:** Permitir contraseñas débiles como "123456", no tener un mecanismo para bloquear a un usuario después de muchos intentos de login fallidos (permitiendo ataques de fuerza bruta), o no invalidar el token de sesión después de un logout.

### A08:2021 – Software and Data Integrity Failures (Fallos de Integridad de Software y Datos)
Se refiere a fallas relacionadas con la integridad del código y la infraestructura que no protegen contra alteraciones. Un ejemplo es la "deserialización insegura", donde la aplicación toma datos serializados (un objeto convertido en texto) y los deserializa sin verificar su integridad, lo que puede llevar a la ejecución de código remoto.

*   **Explicación Sencilla:** Confiar en datos o actualizaciones sin verificar que son legítimos y no han sido manipulados.
*   **Ejemplo:** Una aplicación que descarga e instala actualizaciones automáticamente desde una URL sin verificar la firma digital del paquete. Un atacante podría interceptar esa conexión y enviar una actualización maliciosa.

### A09:2021 – Security Logging and Monitoring Failures (Fallos de Registro y Monitoreo de Seguridad)
La falta de un registro y monitoreo adecuados permite a los atacantes mantener su actividad maliciosa sin ser detectados, pivotar a otros sistemas y extraer o destruir datos.

*   **Explicación Sencilla:** No tener un sistema de "cámaras de seguridad" o no revisarlas. Si un atacante entra, nadie se da cuenta.
*   **Ejemplo:** Un atacante intenta durante horas un ataque de fuerza bruta contra múltiples cuentas de usuario. Como la aplicación no registra los intentos de login fallidos ni genera alertas por actividad sospechosa, el ataque pasa completamente desapercibido.

### A10:2021 – Server-Side Request Forgery (SSRF)
Las fallas de SSRF ocurren cuando una aplicación web obtiene un recurso remoto sin validar la URL proporcionada por el usuario. Permite que un atacante coaccione a la aplicación para que envíe una solicitud manipulada a un destino inesperado, incluso cuando está protegido por un firewall o VPN.

*   **Explicación Sencilla:** Engañar al servidor para que haga peticiones en tu nombre a otros sistemas que él puede ver pero tú no.
*   **Ejemplo:** Una función para "importar una imagen desde una URL" permite a un atacante introducir la URL de un servicio interno de la empresa, como `http://192.168.1.10/admin`. El servidor, que está en la red interna, accederá a esa URL y podría devolver información sensible al atacante.

---
Estos diez puntos nos servirán de guía para los próximos temas. Cada uno de ellos es un mundo en sí mismo que exploraremos con su propia teoría y laboratorio.