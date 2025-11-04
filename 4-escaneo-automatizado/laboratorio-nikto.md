# Laboratorio: Escaneo de Servidores Web con Nikto

## 1. Entorno y Herramientas

*   **Máquina Atacante:** Kali Linux.
*   **Objetivos:**
    *   Servidor Apache en `http://172.18.0.6`.
    *   OWASP Juice Shop en `http://172.18.0.4:3000`.
*   **Herramienta Principal:** Nikto.

## 2. Objetivos

*   Aprender a usar Nikto para realizar escaneos de vulnerabilidades web.
*   Escanear diferentes tipos de servidores web en distintos puertos.
*   Interpretar los resultados de un escaneo de Nikto.
*   Guardar los resultados en un informe para su posterior análisis.

## 3. Prácticas

### Parte 1: Iniciar Nikto y Realizar un Escaneo Básico

Vamos a lanzar nuestro primer ataque (autorizado) contra el servidor Apache de nuestro entorno Docker.

1.  **Abre una terminal en tu Kali Linux.**

2.  **Lanza un escaneo básico:** La sintaxis más simple de Nikto es `nikto -h <URL_objetivo>`.

    ```bash
    nikto -h http://172.18.0.6
    ```

3.  **Analiza la salida:** Nikto te mostrará información en tiempo real. Fíjate en los primeros datos que te proporciona:
    *   La versión del servidor (`Apache/2.4.25`).
    *   Las cabeceras HTTP que devuelve el servidor (`X-Frame-Options`, etc.).
    *   Posibles vulnerabilidades o malas configuraciones que va encontrando.

### Parte 2: Usar Nikto para Escanear Varios Servidores Web

Nikto no solo puede escanear el puerto 80. Podemos especificar un puerto diferente y también escanear múltiples objetivos.

1.  **Escanear un puerto no estándar:** Ahora apuntaremos al Juice Shop, que corre en el puerto 3000.

    ```bash
    nikto -h http://172.18.0.4 -p 3000
    ```
    Nikto detectará automáticamente que el puerto 3000 está abierto y comenzará el escaneo contra la aplicación Node.js.

2.  **(Opcional) Escanear desde un archivo:** En un pentest real, es común tener una lista de objetivos. Podemos guardar nuestras IPs en un archivo y pasárselo a Nikto.

    *   Crea un archivo `targets.txt`:
        ```bash
        echo "http://172.18.0.6" > targets.txt
        echo "http://172.18.0.4:3000" >> targets.txt
        ```
    *   Lanza el escaneo usando el archivo como objetivo:
        ```bash
        nikto -h targets.txt
        ```

### Parte 3: Investigar las Vulnerabilidades de los Sitios Web

Nikto no solo te dice que hay un problema, a menudo te da un código para que investigues más.

1.  **Revisa los resultados del escaneo anterior.** Es muy probable que encuentres una línea como esta:
    `+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different format to the MIME type.`

2.  **Interpreta el resultado:** Esto es un ejemplo de una cabecera de seguridad HTTP que falta. Nikto nos está diciendo que la aplicación no le está diciendo explícitamente al navegador cómo debe interpretar el contenido, lo que en algunos casos podría llevar a ataques de "MIME sniffing".

3.  **Busca los códigos OSVDB/CVE:** En muchos hallazgos, Nikto incluye un código de referencia (aunque OSVDB ya no está activo, el concepto es el mismo para los códigos CVE que puedas encontrar). Si vieras `OSVDB-12345`, tu siguiente paso como pentester sería buscar ese código en Google para entender la vulnerabilidad en profundidad, su impacto y cómo se explota.

### Parte 4: Exportar Resultados de Nikto a un Archivo

Un pentest sin informes no es un pentest. Es crucial guardar tus hallazgos.

1.  **Guardar en formato HTML:** El formato HTML es muy visual y fácil de leer.

    ```bash
    nikto -h http://172.18.0.6 -o reporte_nikto.html -F html
    ```
    *   `-o`: Especifica el archivo de salida.
    *   `-F`: Especifica el formato.

    Ahora puedes abrir el archivo `reporte_nikto.html` en tu navegador para ver un informe completo y ordenado.

2.  **Guardar en formato CSV:** El formato CSV (Comma-Separated Values) es ideal si quieres importar los datos a una hoja de cálculo u otra herramienta para procesarlos.

    ```bash
    nikto -h http://172.18.0.6 -o reporte_nikto.csv -F csv
    ```

Este laboratorio te ha dado las habilidades básicas para usar Nikto. Es una herramienta que usarás constantemente en las primeras fases de reconocimiento de una aplicación web.