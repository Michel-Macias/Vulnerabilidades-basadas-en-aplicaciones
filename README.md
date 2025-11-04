# 📚 Vulnerabilidades Basadas en Aplicaciones (Cisco Ethical Hacker)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OWASP Top 10](https://img.shields.io/badge/OWASP%20Top%2010-2021-blue)](https://owasp.org/www-project-top-ten/)
[![Language](https://img.shields.io/badge/Language-Spanish-red.svg)](https://github.com/Michel-Macias/Vulnerabilidades-basadas-en-aplicaciones)

## 🎯 ¿Cuál es el objetivo de este repositorio?

Este repositorio es una guía de estudio y un recurso práctico para el módulo de **"Explotación de Vulnerabilidades Basadas en Aplicaciones"** del curso de Ethical Hacker de Cisco. Su propósito es desglosar, explicar y ofrecer laboratorios prácticos sobre las vulnerabilidades más críticas que afectan a las aplicaciones web, siguiendo las mejores prácticas y referencias de la industria como el OWASP Top 10.

Aquí encontrarás:
*   **Apuntes teóricos** claros y concisos.
*   **Laboratorios prácticos** diseñados para replicar y entender los ataques en un entorno controlado (Kali Linux, Docker, VMs).
*   **Un espacio para tus propias prácticas** y resultados.

## 📖 Temario y Contenido

A continuación se muestra la estructura de temas y laboratorios disponibles en esta guía.

| # | Tema | Apuntes Teóricos | Laboratorios |
|:--|:---|:---|:---|
| 0 | Guía de Pentesting Profesional | [Ver Guía](./0-guia-pentesting-profesional.md) | |
| 1 | El Protocolo HTTP | [Ver Teoría](./1-protocolo-http/teoria.md) | [Básico](./1-protocolo-http/laboratorio.md) / [Avanzado (Docker)](./1-protocolo-http/laboratorio-docker.md) |
| 2 | Sesiones Web | [Ver Teoría](./2-sesiones-web/teoria.md) | [Burp Suite](./2-sesiones-web/laboratorio.md) |
| 3 | OWASP Top 10 | [Ver Teoría](./3-owasp-top-10/teoria.md) | (Ver temas específicos) |
| 4 | Escaneo Automatizado | [Ver Teoría](./4-escaneo-automatizado/teoria.md) | [Nikto](./4-escaneo-automatizado/laboratorio-nikto.md) / [GVM](./4-escaneo-automatizado/laboratorio-gvm.md) |
| 5 | Autenticación | [Ver Teoría](./5-autenticacion/teoria.md) | [Laboratorio](./5-autenticacion/laboratorio.md) |

## 🚀 ¿Cómo usar este repositorio?

1.  **Clona el repositorio:**
    ```bash
    git clone https://github.com/Michel-Macias/Vulnerabilidades-basadas-en-aplicaciones.git
    ```
2.  **Lee la teoría:** Cada archivo de teoría está diseñado para ser un resumen conciso y directo del concepto, sus riesgos y sus mitigaciones.
3.  **¡Haz los laboratorios!** La verdadera forma de aprender es practicando. Sigue las guías de los laboratorios en tu propio entorno (Kali Linux, Docker, VMs).
4.  **Guarda tus resultados:** Utiliza la carpeta `mis-practicas/` para almacenar tus notas, capturas de pantalla y resultados de los laboratorios.

## 🤝 ¿Quieres contribuir?

¡Toda ayuda es bienvenida! Si encuentras un error, quieres mejorar un apunte o laboratorio, o quieres añadir un tema nuevo, por favor:

1.  Haz un Fork del repositorio.
2.  Crea una nueva rama para tus cambios (ej. `git checkout -b mejora-lab-nikto`).
3.  Realiza tus cambios y haz commit (ej. `git commit -m 'Añade más detalles al laboratorio de Nikto'`).
4.  Haz un Push a tu rama (ej. `git push origin mejora-lab-nikto`).
5.  Abre un Pull Request.

## 📜 Licencia

Este proyecto está bajo la Licencia MIT. Eres libre de usar, modificar y distribuir el contenido.