# Laboratorio: Taller de Ataques de Contraseñas

En este laboratorio, pondremos en práctica los conceptos teóricos para descifrar contraseñas utilizando algunas de las herramientas más potentes disponibles en Kali Linux.

**Objetivos:**
*   Familiarizarse con el arsenal de herramientas de Kali para ataques de contraseñas.
*   Aprender a identificar y descifrar diferentes tipos de hashes con Hashcat.
*   Realizar ataques de diccionario y de fuerza bruta con John the Ripper.
*   Comprender el funcionamiento de las tablas arcoíris con RainbowCrack.

---

### Parte 1: Investigación de Herramientas en Kali Linux

Kali Linux viene pre-cargado con una vasta colección de herramientas para ataques de contraseñas. Tu primera tarea es explorar este arsenal.

**Instrucciones:**

1.  Abre una terminal en tu máquina Kali.
2.  Navega por el menú de aplicaciones o utiliza la línea de comandos para encontrar herramientas relacionadas con "Password Attacks".
    ```bash
    # El comando 'apropos' busca en las descripciones de los manuales
    apropos password
    ```
3.  Investiga y haz una breve descripción de para qué sirven al menos 5 de las siguientes herramientas (además de las que usaremos en el laboratorio):
    *   `fcrackzip`
    *   `cewl`
    *   `crunch`
    *   `medusa`
    *   `hydra`
    *   `ncrack`
    *   `ophcrack`

**Entregable:**
Crea un pequeño informe en tu carpeta de `mis-practicas/6-ataques-contrasenas/` describiendo la función principal de cada una de las 5 herramientas que elegiste.

---

### Parte 2: Descifrando Hashes con Hashcat

Hashcat es extremadamente potente, especialmente cuando se usa con una GPU. En esta práctica, nos centraremos en su uso básico para identificar y romper hashes.

**Escenario:**
Durante una auditoría, has obtenido los siguientes hashes de una base de datos. Tu misión es descubrir las contraseñas originales.

```
# hashes.txt
8743b52063cd84097a65d1633f5c74f5
7c6753c546a350c71065c52c938c439f4115132d
$2y$12$D8/0Vv4W8G9b/Rn2Yw4hI.42S2InB5vMWv4s3N9t9s5.31sJmN9yG
```

**Instrucciones:**

1.  **Guardar los Hashes**: Crea un archivo llamado `hashes.txt` y pega los hashes anteriores en él.

2.  **Identificar el Tipo de Hash**: Antes de atacar, necesitas saber qué algoritmo se usó.
    *   Puedes usar herramientas online como [hashes.com](https://hashes.com/en/tools/hash_identifier) o una herramienta local como `hash-identifier`.
    *   **Pista**: Los hashes corresponden a `MD5`, `SHA1` y `Bcrypt`.

3.  **Preparar el Ataque de Diccionario**:
    *   Kali incluye múltiples listas de palabras. La más famosa es `rockyou.txt`. Vamos a localizarla y descomprimirla.
        ```bash
        sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
        ```
    *   Crea una lista más pequeña para la prueba inicial, ya que `rockyou.txt` es muy grande.
        ```bash
        head -n 1000 /usr/share/wordlists/rockyou.txt > mi_diccionario.txt
        ```

4.  **Ejecutar Hashcat**:
    *   La sintaxis básica de Hashcat es: `hashcat -m <modo> <archivo_hashes> <diccionario>`
    *   El "modo" (`-m`) es un número que representa el algoritmo de hash. Consulta la ayuda de hashcat (`hashcat --help`) para encontrar los códigos. (MD5=`0`, SHA1=`100`, Bcrypt=`3200`).

    *   **Ataque al hash MD5:**
        ```bash
        hashcat -m 0 hashes.txt mi_diccionario.txt --force
        # Usamos --force para que se ejecute en la VM sin GPU dedicada
        ```
    *   **Ataque al hash SHA1:**
        ```bash
        hashcat -m 100 hashes.txt mi_diccionario.txt --force
        ```
    *   **Ataque al hash Bcrypt:**
        ```bash
        hashcat -m 3200 hashes.txt mi_diccionario.txt --force
        ```

5.  **Ver los Resultados**:
    *   Hashcat guarda las contraseñas encontradas en un archivo `potfile`.
        ```bash
        hashcat --show hashes.txt
        ```

**Entregable:**
Apunta las contraseñas que lograste descifrar. ¿Por qué el hash de Bcrypt es significativamente más lento de atacar que MD5 o SHA1?

---

### Parte 3: Ataques con John the Ripper (JtR)

John the Ripper es otra herramienta clásica. Su fortaleza radica en su flexibilidad y sus múltiples modos de ataque.

**Escenario:**
Has conseguido el archivo `/etc/shadow` de un sistema Linux antiguo. Contiene los nombres de usuario y los hashes de sus contraseñas.

```
# shadow.txt
root:$6$a/b/c...$long_hash_string:18635:0:99999:7:::
user1:$1$3z/x/y...$another_hash:18635:0:99999:7:::
guest:U*U*U*U*U*U*U*U:18635:0:99999:7:::
```
*(Nota: los hashes están acortados por brevedad)*

**Instrucciones:**

1.  **Preparar los archivos**: JtR puede tomar el archivo `shadow` directamente. Sin embargo, es una buena práctica separar los hashes que quieres romper usando la utilidad `unshadow`. Para este ejercicio, crearemos un archivo simple. Guarda el siguiente contenido en `users.txt`:
    ```
    user1:$1$DSQiKS6G$kugD9J50LQ6QYfM3J81P3/
    user2:$1$A9z8GfI1$szl5b1s5A1fS/pZ.a2E7g.
    ```
    *Pista: Estos son hashes MD5, comunes en sistemas antiguos.*

2.  **Ataque de Diccionario con JtR**:
    *   Usa la misma lista `mi_diccionario.txt` que creaste antes.
        ```bash
        john --wordlist=mi_diccionario.txt users.txt
        ```

3.  **Ataque de Fuerza Bruta (Modo Incremental)**:
    *   Si el ataque de diccionario falla, puedes pasar a fuerza bruta. JtR tiene un modo incremental inteligente.
    *   Primero, intenta ver los resultados del paso anterior:
        ```bash
        john --show users.txt
        ```
    *   Ahora, inicia un ataque de fuerza bruta en los hashes que queden sin romper.
        ```bash
        john --incremental users.txt
        ```
    *   Este proceso puede tardar. Puedes detenerlo con `Ctrl+C`. JtR guardará la sesión para reanudarla más tarde.

4.  **Resultados**:
    *   Usa el comando `--show` de nuevo para ver todas las contraseñas que John ha conseguido descifrar.

**Entregable:**
Anota las contraseñas descifradas para `user1` y `user2`. Describe la diferencia que observaste en la velocidad y el enfoque entre el modo "wordlist" y el modo "incremental".

---

### Parte 4: Usando Tablas Arcoíris con RainbowCrack

Esta parte es más conceptual, ya que generar y usar tablas arcoíris requiere mucho tiempo y espacio. El objetivo es entender el proceso.

**Escenario:**
Un atacante ha obtenido un hash MD5 `202cb962ac59075b964b07152d234b70` y sospecha que la contraseña es una clave numérica simple. En lugar de calcular hashes, usará una tabla pre-calculada.

**Instrucciones (Conceptuales):**

1.  **Generar las Tablas**:
    *   El primer paso (y el más largo) es generar las tablas. Con `RainbowCrack`, se usa el comando `rtgen`.
    *   Necesitarías definir el algoritmo de hash, el conjunto de caracteres (ej. solo numérico), la longitud mínima y máxima de la contraseña, y la configuración de la tabla.
        ```bash
        # Ejemplo de generación de tabla para contraseñas numéricas de 3 dígitos
        rtgen md5 numeric 3 3 0 1000 0
        ```
    *   Este proceso crea archivos `.rt` que pueden ocupar gigabytes.

2.  **Ordenar las Tablas**:
    *   Antes de poder usarlas, las tablas deben ser ordenadas con `rtsort`.
        ```bash
        rtsort .
        ```

3.  **Descifrar el Hash**:
    *   Con las tablas listas, el descifrado es casi instantáneo. Se usa el comando `rcrack`.
        ```bash
        rcrack . -h 202cb962ac59075b964b07152d234b70
        ```
    *   Si el hash se encuentra en la tabla, el programa devolverá el texto plano correspondiente (en este caso, `123`).

**Entregable (Investigación):**
1.  Busca en internet "md5 rainbow table online".
2.  Usa uno de esos servicios para "descifrar" el hash `202cb962ac59075b964b07152d234b70`.
3.  Explica en tus notas por qué este método falla completamente si el hash hubiera sido creado usando un *salt*.