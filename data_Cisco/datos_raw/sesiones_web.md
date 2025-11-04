 Sesiones Web
Una sesión web es una secuencia de transacciones de solicitud y respuesta HTTP entre un cliente web y un servidor. Estas transacciones incluyen tareas previas de autenticación, el proceso de autenticación, administración de sesiones, control de acceso y finalización de sesiones. Numerosas aplicaciones web realizan un seguimiento de la información sobre cada usuario durante una transacción web. Varias aplicaciones web tienen la capacidad de establecer variables como derechos de acceso y configuraciones de localización. Estas variables se aplican a todas y cada una de las interacciones que tiene un usuario con la aplicación web durante la sesión.

Las aplicaciones web pueden crear sesiones para realizar un seguimiento de los usuarios anónimos después de la primera solicitud de usuario. Por ejemplo, una aplicación puede recordar la preferencia de idioma del usuario cada vez que visita el sitio o la interfaz de la aplicación. Además, una aplicación web utiliza una sesión después de que el usuario se ha autenticado. Esto permite que la aplicación identifique al usuario en cualquier solicitud posterior y pueda aplicar controles de acceso de seguridad y aumentar la facilidad de uso de la aplicación. En resumen, las aplicaciones web pueden proporcionar capacidades de sesión antes y después de la autenticación.

Después de establecer una sesión autenticada, el ID de la sesión (o token) equivale temporalmente al método de autenticación más seguro utilizado por la aplicación, como los nombres de usuario y las contraseñas, las contraseñas de un solo uso y los certificados digitales basados en el cliente.

NOTA Un buen recurso que proporciona mucha información sobre la autenticación de aplicaciones es la Hoja de Referencia para la Autenticación de OWASP, disponible en https://www.owasp.org/index.php/Authentication_Cheat_Sheet.

Para mantener el estado de autenticación y realizar un seguimiento del progreso del usuario, las aplicaciones proporcionan a los usuarios ID de sesión o tokens. Se asigna un token en el momento de la creación de la sesión, y el usuario y la aplicación web lo comparten e intercambian durante la sesión. El ID de la sesión es un par de nombre/valor.

Los nombres de ID de sesión utilizados por los marcos de desarrollo de aplicaciones web más comunes pueden identificarse fácilmente. Por ejemplo, puede usar huellas digitales de PHPSESSID (PHP), JSESSIONID (J2EE), CFID y CFTOKEN (ColdFusion), ASP.NET_SessionId (ASP.NET) y muchos otros. Además, el nombre del ID de la sesión puede indicar qué marco y qué lenguajes de programación utiliza la aplicación web.

Se recomienda cambiar el nombre de ID de sesión predeterminado del marco de desarrollo web a un nombre genérico, como id.

La ID de la sesión debe ser lo suficientemente larga para evitar ataques de fuerza bruta. A veces, los desarrolladores lo establecen en unos pocos bits, aunque debe ser de al menos 128 bits (16 bytes).

NOTA Es muy importante que la ID de una sesión sea única e impredecible. Debe usar un buen generador de bits aleatorios determinista (DRBG) para crear un valor de ID de sesión que proporcione al menos 256 bits de entropía.

Hay varios mecanismos disponibles en HTTP para mantener el estado de la sesión dentro de las aplicaciones web, incluidas las cookies (en el encabezado HTTP estándar), los parámetros de URL y la reescritura definidos en RFC 3986 y los argumentos de URL en las solicitudes GET. Además, los desarrolladores utilizan argumentos del cuerpo en las solicitudes POST, como campos de formulario ocultos (formularios HTML) o encabezados HTTP exclusivos. Sin embargo, uno de los mecanismos de intercambio de ID de sesión más utilizados son las cookies, que ofrecen capacidades avanzadas que no están disponibles en otros métodos.

Incluir el ID de la sesión en la URL puede provocar la manipulación del ID o ataques de fijación de la sesión. Por lo tanto, es importante mantener el ID de la sesión fuera de la URL.

CONSEJO Los marcos de desarrollo web, como ASP.NET, PHP y Ruby on Rails, proporcionan sus propias funciones de administración de sesiones e implementaciones asociadas. Se recomienda utilizar estos marcos integrados en lugar de crear los suyos propios desde cero, ya que han sido probados por muchas personas. Cuando realiza pruebas de penetración, es probable que encuentre personas que intentan crear sus propios marcos. Además, JSON Web Token (JWT) se puede utilizar para la autenticación en aplicaciones modernas.

AlexV4_pose04.png
Esto puede parecer bastante obvio, pero debe recordar cifrar una sesión web completa, no solo para el proceso de autenticación donde se intercambian las credenciales de usuario, sino también para garantizar que la ID de la sesión se intercambie solo a través de un canal cifrado. El uso de un canal de comunicación cifrado también protege la sesión contra algunos ataques de fijación de sesión, en los que el atacante puede interceptar y manipular el tráfico web para inyectar (o fijar) la ID de sesión en el navegador web de la víctima.

Los mecanismos de administración de sesiones basados en cookies pueden hacer uso de dos tipos de cookies: cookies no persistentes (o de sesión) y cookies persistentes. Si una cookie tiene un atributo Max-Age o Expires, se considera una cookie persistente y el navegador web la almacena en un disco hasta la fecha de vencimiento. Las aplicaciones web y los clientes comunes priorizan el atributo Max-Age sobre el atributo Expires.

Las aplicaciones modernas suelen realizar un seguimiento de los usuarios después de la autenticación mediante cookies no persistentes. Esto fuerza la eliminación de la información de la sesión del cliente si se cierra la instancia del navegador web actual. Por eso es importante utilizar cookies no persistentes: para que el ID de sesión no permanezca en la caché del cliente web durante largos períodos de tiempo.

Una aplicación debe validar y verificar cuidadosamente los ID de sesión. Según el mecanismo de administración de sesiones utilizado, el ID de la sesión se recibirá en un parámetro GET o POST, en la URL o en un encabezado HTTP mediante cookies.

Si las aplicaciones web no validan y filtran los valores de ID de sesión no válidos, pueden utilizarse para explotar otras vulnerabilidades web, como la inyección de SQL si los ID de sesión se almacenan en una base de datos relacional o secuencias de comandos entre sitios (XSS) persistentes, si las ID de sesión se almacenan y reflejan posteriormente en la aplicación web.

NOTA Aprenderá sobre la inyección SQL y XSS más adelante en este módulo.

6.1.5 Práctica - Sesiones Web
Está realizando una prueba inicial del sitio web de un cliente y observa que se configuró una cookie de ID de sesión de PHP de 128 bits en su computadora de prueba. ¿Qué recomendación práctica le haría a su cliente para mejorar la seguridad de la aplicación?


done
Se debe cambiar el nombre de la cookie de ID de sesión a algo genérico para que el marco y el lenguaje de programación en uso sean más difíciles de determinar.

La ID de sesión se puede acortar de forma segura a 8 bits para conservar los recursos de procesamiento de scripts de PHP.

Mejore la seguridad de las aplicaciones web mediante la adopción del uso de ID de sesión J2EE.

El sitio web debe usar un valor PHPSESSID en la URL en lugar de en la cookie.