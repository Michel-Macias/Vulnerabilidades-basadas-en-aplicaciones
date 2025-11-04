# Informe de Prueba de Penetración - [NOMBRE_DEL_CLIENTE] - [FECHA_DEL_INFORME]

## 1. Información del Documento

*   **Versión del Informe:** 1.0
*   **Fecha de Emisión:** [AAAA-MM-DD]
*   **Autor(es):** [Tu Nombre/Equipo]
*   **Revisor(es):** [Nombre del Revisor (si aplica)]

## 2. Resumen Ejecutivo

[Este es un resumen no técnico, dirigido a la dirección y a los responsables de negocio. Debe ser conciso y destacar los hallazgos más críticos, el impacto general en la organización y las recomendaciones de alto nivel. Evita la jerga técnica excesiva.]

La prueba de penetración realizada en [Nombre del Cliente/Sistema] entre [Fecha Inicio] y [Fecha Fin] ha revelado [Número] vulnerabilidades, de las cuales [Número] son de severidad Crítica/Alta. Estos hallazgos representan un riesgo significativo para [Activos/Operaciones del Cliente], pudiendo llevar a [Ej. compromiso de datos sensibles, interrupción del servicio, acceso no autorizado]. Se recomienda encarecidamente abordar las vulnerabilidades de alta criticidad con carácter inmediato, siguiendo las recomendaciones detalladas en este informe.

## 3. Alcance y Metodología

### 3.1. Alcance de la Prueba

*   **Activos Incluidos:** [Listado de IPs, rangos de red, dominios, URLs de aplicaciones web, sistemas, etc. que fueron parte de la prueba]
*   **Activos Excluidos:** [Listado de elementos que explícitamente NO fueron parte de la prueba]
*   **Tipo de Prueba:** [Caja Negra (sin conocimiento previo), Caja Blanca (conocimiento total), Caja Gris (conocimiento parcial)]
*   **Fechas de la Prueba:** Desde [Fecha Inicio] hasta [Fecha Fin]

### 3.2. Metodología Empleada

[Describir la metodología utilizada. Ej. PTES (Penetration Testing Execution Standard), OWASP Testing Guide, NIST SP 800-115, etc. Detallar las fases aplicadas: Reconocimiento, Escaneo, Enumeración, Análisis de Vulnerabilidades, Explotación, Post-Explotación, etc.]

### 3.3. Limitaciones y Exclusiones

[Cualquier limitación impuesta por el cliente, restricciones de tiempo, o áreas que no pudieron ser probadas por motivos técnicos o de alcance.]

## 4. Hallazgos

### 4.1. Resumen de Vulnerabilidades por Severidad

[Tabla o gráfico que muestre la distribución de vulnerabilidades por nivel de severidad. Ej.]

| Severidad | Cantidad | Porcentaje |
|:----------|:---------|:-----------|
| Crítica   | [X]      | [Y]%       |
| Alta      | [X]      | [Y]%       |
| Media     | [X]      | [Y]%       |
| Baja      | [X]      | [Y]%       |
| Informativa | [X]      | [Y]%       |
| **Total** | [X]      | **100%**   |

### 4.2. Hallazgos Detallados

[Para cada vulnerabilidad encontrada, se debe incluir la siguiente información. Puedes referenciar aquí los informes individuales generados con la `plantilla-informe-escaneo.md` o incrustar su contenido directamente.]

--- 
### 4.2.1. Vulnerabilidad: [Nombre de la Vulnerabilidad 1]

*   **Referencia:** [CVE-AAAA-BBBB, OSVDB-CCCCC, CWE-DDDDD, o cualquier ID de referencia]
*   **Severidad:** [Crítica / Alta / Media / Baja / Informativa] (Puntuación CVSS: [X.X])
*   **Descripción:** [Breve explicación de la vulnerabilidad, qué es y por qué es un problema]
*   **Impacto:** [Consecuencias de la explotación de esta vulnerabilidad (ej. robo de datos, ejecución de código remoto, denegación de servicio)]
*   **Activo Afectado:** [IP, URL, Nombre del Sistema]
*   **Pasos de Reproducción:**

    ```bash
    # Aquí van los comandos, o pasos detallados para replicar la vulnerabilidad.
    # Incluir configuraciones, payloads, etc.
    1. [Paso 1]
    2. [Paso 2]
    #...
    ```

*   **Evidencia:**

    ```
    # Capturas de pantalla (referenciar imágenes o pegar enlaces), logs de herramientas, salida de comandos.
    # Ejemplo: ![Acceso Root a /etc/passwd](captura_root.png)
    ```

*   **Recomendaciones:** [Cómo mitigar o solucionar esta vulnerabilidad. Ser específico y práctico. Ej. "Actualizar Apache a la versión 2.4.26", "Implementar WAF con reglas contra XSS", "Sanitizar todas las entradas de usuario"]

---
[DUPLICAR LA SECCIÓN `4.2.X. Vulnerabilidad` PARA CADA HALLAZGO RELEVANTE]

## 5. Conclusiones

[Una visión general de la postura de seguridad del cliente basada en los hallazgos. Resaltar las áreas fuertes y las débiles. Por ejemplo, "La organización demuestra una buena base de seguridad en [Área X], sin embargo, se han identificado deficiencias significativas en [Área Y] que requieren atención urgente."]

## 6. Recomendaciones Generales

[Recomendaciones estratégicas o de alto nivel que no están ligadas a una vulnerabilidad específica, sino a la mejora continua de la seguridad. Ej. "Implementar un programa de formación en seguridad para desarrolladores", "Establecer un ciclo de vida de desarrollo seguro (SDLC)", "Realizar auditorías de seguridad periódicas".]

## 7. Apéndices

[Aquí se pueden incluir logs completos, listados de herramientas utilizadas, referencias a documentación externa, etc.]

## 8. Descargos de Responsabilidad

[Cláusulas legales que protegen al pentester y a la empresa. Ej. "Este informe refleja el estado de seguridad en el momento de la prueba y no garantiza la ausencia de otras vulnerabilidades no detectadas. La información contenida es confidencial y debe ser tratada como tal."]