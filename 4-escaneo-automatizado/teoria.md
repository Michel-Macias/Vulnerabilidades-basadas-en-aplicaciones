# Teoría: Escaneo Automatizado de Vulnerabilidades

## 1. ¿Qué es un Escáner de Vulnerabilidades?

Un escáner de vulnerabilidades es una herramienta de software diseñada para analizar sistemas, redes o aplicaciones en busca de debilidades de seguridad conocidas. En el contexto de la seguridad web, estos escáneres automatizan el proceso de "tocar a la puerta" de un servidor para ver si alguna de las "cerraduras" es defectuosa.

Son una pieza fundamental en las pruebas de penetración porque permiten:

*   **Identificar rápidamente vulnerabilidades de "fruta madura" (low-hanging fruit):** Fallos de configuración comunes, software desactualizado, archivos por defecto peligrosos, etc.
*   **Cubrir un gran volumen de comprobaciones:** Un escáner puede realizar miles de pruebas en minutos, una tarea que sería tediosa y propensa a errores si se hiciera manualmente.
*   **Obtener información de referencia:** A menudo, los resultados del escáner proporcionan enlaces a bases de datos de vulnerabilidades (como CVE) que explican el riesgo y cómo mitigarlo.

## 2. Tipos de Escaneo: Ruidoso vs. Sigiloso

No todos los escáneres se comportan igual. Una distinción clave es cuánto "ruido" hacen en la red.

*   **Escáneres Ruidosos (Activos):** Herramientas como **Nikto** son consideradas "ruidosas". Envían una gran cantidad de peticiones en muy poco tiempo para comprobar cientos o miles de posibles fallos. Este comportamiento es muy fácil de detectar por sistemas de seguridad como Firewalls de Aplicaciones Web (WAF) o Sistemas de Detección de Intrusos (IDS/IPS).
    *   **¿Cuándo se usan?** Son perfectos para las primeras fases de una prueba de penetración autorizada, donde el cliente sabe que se va a realizar el escaneo y puede haber añadido la IP del atacante a una "lista blanca" para permitirlo.

*   **Técnicas Sigilosas (Pasivas/Furtivas):** En fases más avanzadas de un pentest, o cuando se quiere simular a un atacante real que intenta no ser detectado, se usan técnicas más sutiles. Esto puede implicar realizar escaneos mucho más lentos, usar proxies para rotar la IP de origen o centrarse en el análisis pasivo (por ejemplo, analizando el tráfico sin enviar peticiones maliciosas).

## 3. Herramientas de Escaneo

### 3.1. Nikto

**Nikto** es un escáner de vulnerabilidades web de código abierto, muy popular y presente en distribuciones como Kali Linux. Es una herramienta de línea de comandos, rápida y fácil de usar.

*   **¿Qué busca principalmente?**
    *   **Versiones de software desactualizadas:** Identifica la versión del servidor web (Apache, Nginx, etc.) y alerta si es una versión con vulnerabilidades conocidas.
    *   **Archivos y carpetas peligrosas:** Busca más de 6700 archivos potencialmente peligrosos, como paneles de administración expuestos, archivos de configuración, etc.
    *   **Configuraciones incorrectas del servidor:** Por ejemplo, si los listados de directorios están habilitados.
    *   **Vulnerabilidades específicas** de ciertos servidores web.

Nikto es una excelente primera herramienta para obtener una visión general rápida de la seguridad de un servidor web.

### 3.2. GVM (Greenbone Vulnerability Management) / OpenVAS

**GVM** es mucho más que un simple escáner; es una **plataforma completa de gestión de vulnerabilidades**. Es más potente y complejo que Nikto.

*   **La relación con OpenVAS:** A menudo se usan como sinónimos, pero **OpenVAS** es en realidad el componente de escaneo principal dentro de la arquitectura de GVM.

*   **Características clave:**
    *   **Base de datos de pruebas (NVTs):** GVM utiliza una enorme base de datos de Pruebas de Vulnerabilidad de Red (Network Vulnerability Tests) que se actualiza diariamente. Esto le permite detectar una gama mucho más amplia de vulnerabilidades que Nikto.
    *   **Escaneo completo de red:** A diferencia de Nikto, que se centra en web, GVM puede escanear cualquier servicio en cualquier puerto (FTP, SMTP, SMB, etc.) en busca de fallos.
    *   **Gestión de vulnerabilidades:** Permite guardar escaneos, comparar resultados a lo largo del tiempo, generar informes detallados y gestionar el ciclo de vida de una vulnerabilidad (desde que se descubre hasta que se soluciona).
    *   **Interfaz web:** Se gestiona a través de una interfaz web (Greenbone Security Assistant), lo que facilita la configuración de escaneos complejos y el análisis de resultados.

En resumen, mientras que Nikto es un bisturí rápido para servidores web, GVM es un completo equipo de diagnóstico por imagen para toda la red.