# Teoría: Ataques de Contraseñas

Los ataques de contraseñas son uno de los vectores de ataque más comunes y persistentes en el mundo de la ciberseguridad. El objetivo final es simple: obtener acceso no autorizado a una cuenta, sistema o servicio adivinando o descifrando la credencial de un usuario.

## Conceptos Fundamentales

Para entender cómo funcionan estos ataques, es crucial diferenciar tres conceptos que a menudo se confunden:

| Concepto | Propósito | ¿Es Reversible? | Ejemplo de Uso |
| :--- | :--- | :--- | :--- |
| **Codificación (Encoding)** | Transformar datos para que puedan ser consumidos de forma segura por diferentes sistemas. No es un método de seguridad. | Sí, trivialmente. | `Base64`, `URL Encoding`. |
| **Cifrado (Encryption)** | Ocultar información para que solo las partes autorizadas (que poseen una clave) puedan leerla. | Sí, con la clave correcta. | `AES`, `RSA`, `TLS`. |
| **Hashing** | Crear una representación única de longitud fija de un conjunto de datos. Es un proceso de **una sola vía**. | **No**, está diseñado para ser irreversible. | `SHA-256`, `bcrypt`, `Argon2`. |

### ¿Por Qué Usamos Hashes para las Contraseñas?

Dado que el hashing es irreversible, es el método estándar para almacenar contraseñas de forma segura. Cuando un usuario crea una cuenta, el sistema no guarda la contraseña en texto plano, sino su hash.

1.  **Registro**: `Usuario` -> `Ingresa "P@ssw0rd123"` -> `Sistema calcula hash("P@ssw0rd123")` -> `Almacena "df4...b2a"`.
2.  **Login**: `Usuario` -> `Ingresa "P@ssw0rd123"` -> `Sistema calcula hash("P@ssw0rd123")` -> `Compara el nuevo hash con el almacenado`.

Si un atacante roba la base de datos, solo obtendrá los hashes, no las contraseñas originales. Aquí es donde empiezan los ataques de contraseñas: el objetivo del atacante es encontrar la cadena de texto original que produce un hash coincidente.

### Salt y Pepper: Fortaleciendo los Hashes

Para hacer el descifrado de hashes más difícil, se utilizan dos técnicas principales:

*   **Salt (Sal)**: Es una cadena de datos aleatoria y única que se añade a la contraseña *antes* de calcular el hash. El salt se almacena junto con el hash en la base de datos.
    *   **Propósito**: Asegura que dos usuarios con la misma contraseña tengan hashes diferentes. Esto invalida los ataques de pre-cálculo como las tablas arcoíris.
    *   `hash("P@ssw0rd123" + "random_salt_1")` -> `"a1b2..."`
    *   `hash("P@ssw0rd123" + "random_salt_2")` -> `"c3d4..."`

*   **Pepper (Pimienta)**: Es una cadena de datos secreta y fija que se añade a la contraseña *antes* del hashing, pero a diferencia del salt, el pepper **no se almacena en la base de datos**. Se guarda de forma segura en un lugar diferente, como un archivo de configuración o un gestor de secretos.
    *   **Propósito**: Añade una capa extra de seguridad. Si un atacante roba la base de datos (con hashes y salts), todavía le faltará el pepper para poder intentar descifrar las contraseñas.

## Tipos de Ataques de Contraseñas

### 1. Ataque de Diccionario

Este es el método más simple. El atacante utiliza una lista predefinida de palabras (un "diccionario") que contiene contraseñas comunes, frases, nombres y variaciones. El software de ataque hashea cada palabra de la lista y la compara con el hash objetivo.

*   **Ventaja**: Muy rápido si la contraseña es débil o común.
*   **Desventaja**: Inútil si la contraseña no está en el diccionario.
*   **Listas comunes**: `rockyou.txt`, `cain.txt`.

### 2. Ataque de Fuerza Bruta

Este ataque consiste en probar sistemáticamente todas las combinaciones posibles de caracteres hasta encontrar la contraseña correcta.

*   **Tipos**:
    *   **Fuerza Bruta Pura**: Prueba `a`, `b`, `c`... `aa`, `ab`... etc. Es extremadamente lento.
    *   **Ataque de Máscara**: Si se conoce parte de la estructura de la contraseña (ej. empieza con mayúscula, termina con 2 dígitos), se puede definir una "máscara" (`?U?l?l?l?l?d?d`) para acotar la búsqueda y acelerar el proceso.
*   **Ventaja**: Eventualmente, siempre encontrará la contraseña.
*   **Desventaja**: El tiempo requerido puede ser inviable para contraseñas largas y complejas.

### 3. Ataque de Tablas Arcoíris (Rainbow Tables)

Es un ataque de "tiempo-memoria". En lugar de calcular hashes en tiempo real, se utilizan tablas precalculadas gigantes que mapean millones de hashes a sus contraseñas en texto plano correspondientes.

*   **Proceso**:
    1.  El atacante busca el hash objetivo en la tabla.
    2.  Si lo encuentra, obtiene la contraseña original al instante.
*   **Ventaja**: Extremadamente rápido si el hash está en la tabla.
*   **Desventaja**:
    *   Requiere una enorme cantidad de almacenamiento (terabytes).
    *   Las tablas son específicas para un algoritmo de hash (una tabla para MD5 es inútil para SHA-256).
    *   **Son ineficaces contra contraseñas con *salt***, ya que el salt cambia el hash final, haciendo que la tabla precalculada no sirva.

## Herramientas Populares

*   **Hashcat**: Considerada la herramienta de recuperación de contraseñas más rápida del mundo. Soporta una gran cantidad de algoritmos de hash y puede aprovechar la potencia de las GPUs para acelerar masivamente los ataques.
*   **John the Ripper (JtR)**: Una de las herramientas más conocidas y versátiles. Es multiplataforma y combina varios modos de ataque: diccionario, fuerza bruta y modo "single crack" (que utiliza información del propio nombre de usuario para generar permutaciones).
*   **RainbowCrack**: Herramienta especializada en el uso de tablas arcoíris para descifrar hashes.