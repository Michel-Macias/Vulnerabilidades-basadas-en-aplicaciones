# Laboratorio: Gestión de Vulnerabilidades con GVM (OpenVAS)

## 1. Entorno y Herramientas

*   **Máquina Atacante:** Kali Linux.
*   **Objetivo:** Servidor Apache en `http://172.18.0.6` (o cualquier otro host de la red Docker).
*   **Herramienta Principal:** Greenbone Vulnerability Management (GVM), anteriormente OpenVAS.

## 2. Objetivos

*   Familiarizarse con la interfaz de GVM para configurar y lanzar escaneos de red.
*   Realizar un escaneo de vulnerabilidades completo contra un objetivo.
*   Analizar el informe de resultados para identificar las vulnerabilidades más críticas.
*   Entender el proceso de verificar y explotar una vulnerabilidad encontrada por un escáner.

**NOTA IMPORTANTE:** GVM es una herramienta potente pero pesada. Su primera configuración (`gvm-setup`) y los escaneos pueden tardar mucho tiempo (desde 30 minutos a varias horas). Se asume que tienes GVM instalado y funcionando en tu Kali. Puedes iniciarlo con `sudo gvm-start`.

## 3. Prácticas

### Parte 1: Analizar un Host en busca de Vulnerabilidades

En esta parte, configuraremos y lanzaremos un escaneo contra nuestro servidor Apache.

1.  **Acceder a la Interfaz Web (Greenbone Security Assistant):**
    *   Una vez iniciado GVM, abre tu navegador en Kali y dirígete a la dirección que te indica la herramienta, normalmente `https://127.0.0.1:9392`.
    *   Inicia sesión con el usuario y la contraseña que creaste durante la configuración (el usuario por defecto es `admin`).

2.  **Crear el Objetivo (Target):**
    *   Navega en el menú a **Configuration > Targets**.
    *   Haz clic en el icono de la estrella azul ("New Target") para crear un nuevo objetivo.
    *   **Name:** `Servidor Apache Docker`
    *   **Hosts:** Introduce la IP del servidor: `172.18.0.6`
    *   Deja el resto de opciones por defecto y haz clic en **Save**.

3.  **Crear y Lanzar la Tarea de Escaneo (Scan Task):**
    *   Navega a **Scans > Tasks**.
    *   Haz clic en el icono de la estrella azul ("New Task").
    *   **Name:** `Escaneo Apache`
    *   **Scan Target:** Selecciona el objetivo que acabas de crear (`Servidor Apache Docker`).
    *   **Scanner:** Debería estar seleccionado `OpenVAS Default`.
    *   **Scan Config:** Selecciona `Full and fast`. Es una de las configuraciones de escaneo más completas.
    *   Haz clic en **Save**.
    *   Volverás a la lista de tareas. Para iniciar el escaneo, haz clic en el botón de "Play" (▶️) que aparece a la derecha de tu nueva tarea.

4.  **Analizar los Resultados (¡Después de un buen rato!):**
    *   Una vez que el estado del escaneo sea "Done", haz clic en el nombre del escaneo y luego en la pestaña **Results**.
    *   Verás una lista de todas las vulnerabilidades encontradas, ordenadas por severidad (Severity).
    *   Haz clic en una de las vulnerabilidades de severidad "High" o "Medium". GVM te dará una información muy detallada:
        *   **Summary:** Un resumen de qué es la vulnerabilidad.
        *   **Vulnerability Detection Result:** Cómo el escáner ha determinado que el sistema es vulnerable.
        *   **Solution:** Qué debes hacer para arreglarlo.

### Parte 2: Explotar una Vulnerabilidad Detectada por GVM (Ejemplo Hipotético)

El verdadero trabajo de un pentester comienza donde termina el del escáner. GVM nos da el "qué", nosotros tenemos que averiguar el "cómo".

**Escenario Hipotético:** Imaginemos que en los resultados de tu escaneo a `172.18.0.6`, GVM reporta la siguiente vulnerabilidad:

*   **Vulnerabilidad:** `Apache HTTP Server 2.4.25 < 2.4.26 Directory Traversal`
*   **Severidad:** `High (7.5)`
*   **Resumen:** "La versión de Apache en el servidor es vulnerable a un ataque de Path Traversal. Un atacante podría leer archivos arbitrarios del sistema."
*   **Referencia CVE:** `CVE-2017-9798`

**Proceso de Verificación y Explotación:**

1.  **Investigar la Vulnerabilidad:**
    *   Lo primero es buscar el código **`CVE-2017-9798`** en Google. Encontrarás explicaciones detalladas y, con suerte, Pruebas de Concepto (PoCs) o exploits públicos.
    *   La investigación revela que esta vulnerabilidad se explota a través de la cabecera `Authorization` en una petición `OPTIONS`.

2.  **Verificar/Explotar Manualmente:**
    *   Ahora intentamos replicar el ataque para confirmar que la vulnerabilidad es real. Usaremos `curl` para construir la petición maliciosa que hemos aprendido en nuestra investigación.
    *   El objetivo es intentar leer el archivo `/etc/passwd` del servidor.

    ```bash
    # La vulnerabilidad requiere que el método sea OPTIONS
    # y que se envíe una cabecera Authorization con un payload de Path Traversal
    curl -X OPTIONS --header "Authorization: Basic YW5vbjpAAA../../../../../../../../etc/passwd" http://172.18.0.6/ -i
    ```

3.  **Analizar el Resultado:**
    *   Si el servidor es vulnerable, en el cuerpo de la respuesta HTTP verás el contenido del archivo `/etc/passwd` del contenedor Docker del servidor.
    *   ¡Has confirmado la vulnerabilidad! Has pasado de un informe automático a un compromiso real del sistema.

Este flujo de trabajo (**Escanear -> Investigar -> Verificar/Explotar**) es el núcleo de las pruebas de penetración de red y aplicaciones.