# Proyecto 1 - API Testing con Postman (QA)

Plan de pruebas de API con 27 casos de prueba diseñados, ejecutados y documentados con Postman, más reporte de 4 defectos en Jira.

## 1. INFORMACIÓN GENERAL
- **Nombre del proyecto:** Automated API Testing & Bug Reporting Suite — JSONPlaceholder
- **Especialidad:** Quality Assurance (QA Engineering / API Automation)
- **Fuente del proyecto:** QA — Sistema o App a testear (JSONPlaceholder API)
- **Link a la fuente original:** [https://github.com/isidro-caicedo/proyecto-1-api-testing.git](https://github.com/isidro-caicedo/proyecto-1-api-testing.git)
- **Link al proyecto publicado:** [https://github.com/isidro-caicedo/proyecto-1-api-testing](https://github.com/isidro-caicedo/proyecto-1-api-testing)

---

## 2. OBJETIVO
Este proyecto diseña y ejecuta una suite automatizada de 27 casos de prueba para validar la funcionalidad, integridad de datos, manejo de errores y seguridad de una API REST. Su objetivo es prevenir fallas en producción identificando discrepancias entre el comportamiento real de la API y los estándares RESTful ideales, documentando formalmente las vulnerabilidades críticas en un sistema de gestión de incidencias (Jira).

---

## 3. PLAN DE TRABAJO
1. **Análisis de Requisitos y Arquitectura REST:** Revisión de endpoints, métodos HTTP (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) y códigos de estado esperados.
2. **Diseño de Casos de Prueba (Test Design):** Creación de la matriz con 27 casos de prueba cubriendo caminos felices (*Happy Path*), valores límite, pruebas negativas y de seguridad básica.
3. **Automatización e Inserción de Scripts:** Configuración de la colección en Postman implementando scripts en JavaScript (`pm.test`, `pm.expect`, librería Chai JS) para validar códigos HTTP, esquemas JSON y tiempos de respuesta.
4. **Ejecución y Gestión de Defectos:** Ejecución de la suite, análisis de respuestas del servidor y reporte formal en Jira para los casos con discrepancias de diseño (*Bugs*).
5. **Evaluación de Cobertura y Documentación:** Generación de métricas de pruebas, exportación del entorno/colección y redacción del reporte técnico para el portafolio.

---

## 4. PREGUNTAS CLAVE
- **¿Qué impacto tendría en el negocio si estos bugs (como el borrado falso `DELETE 200 OK`) llegan a producción?**  
  Generaría inconsistencia de datos, falsos positivos en el frontend y posible degradación de la confianza del usuario al confirmar acciones no procesadas en la base de datos.
- **¿Cómo asegurar que esta suite se mantenga funcional a medida que la API evoluciona?**  
  Integrando la colección de Postman en un pipeline de CI/CD mediante **Newman** para ejecutar pruebas de regresión automáticas en cada *deploy*.
- **¿Por qué es importante validar que las entradas maliciosas (ej. SQL Injection) no lancen errores `500 Internal Server Error`?**  
  Porque un error 500 expone trazas internas del servidor (*stack traces*), entregando información crítica a potenciales atacantes sobre la infraestructura subyacente.

---

## 5. QUÉ SE HIZO Y CÓMO
- **Estrategia de Pruebas:** Combinación de pruebas de aceptación, pruebas negativas y validación estricta de esquemas.
- **Herramientas Utilizadas:** **Postman** (diseño, scripts de automatización en JS y ejecución), **Chai JS** (librería de aserciones), **Jira** (gestión y reporte de incidencias).
- **Manejo de Scripts:** Se implementó lógica dinámica en JavaScript para iterar sobre colecciones de objetos (ej. validación de recursos anidados en `TC-27`), verificar tipos de datos primitivos y validar listas de códigos de respuesta aceptables (`pm.expect(code).to.be.oneOf([400, 422])`).
- **Criterio de Aceptación:** Validación estricta bajo los estándares RESTful ideales frente a comportamientos permisivos de entornos *mock*.

---

## 6. RESULTADOS
- **Cobertura de Pruebas:** 100% de la matriz planificada (27 de 27 casos de prueba ejecutados).
- **Tasa de Éxito Automático:** 23 Casos `PASS` / 4 Casos `FAIL` *(Fallos esperados por discrepancias de diseño en la API)*.
- **Bugs Encontrados y Reportados (Severidad Media/Baja):**
  - **BUG-01 (TC-15):** `DELETE /posts/9999` devuelve `200 OK` en lugar de `404 Not Found` al intentar eliminar un recurso inexistente.
  - **BUG-02 (TC-16):** `POST /posts` permite la creación de registros enviando un cuerpo (*body*) vacío, respondiendo `201 Created` en vez de `400/422`.
  - **BUG-03 (TC-17):** `POST /posts` acepta la creación de registros omitiendo el campo obligatorio `title`, respondiendo `201 Created`.
  - **BUG-04 (TC-18):** `POST /posts` acepta tipos de datos inválidos en `userId` (`string` en lugar de `integer`), respondiendo `201 Created`.

---

## 7. CONCLUSIONES
- **Aprendizajes Clave:** Comprendí que el rol de QA va más allá de buscar que todas las pruebas se pongan en verde (`PASS`); el valor real radica en evaluar la lógica de negocio esperada, identificar vulnerabilidades de validación y saber documentarlas técnicamente. Aprendí a estructurar scripts de prueba automatizados en JavaScript utilizando el objeto `pm` y a redactar reportes de incidencias profesionales en Jira.
- **Mejoras con más Recursos:** Implementaría ejecución en línea de comandos (*CLI*) mediante **Newman**, integraría reportes gráficos con **HTML Extra Reporter** y configuraría ejecuciones automatizadas programadas con **GitHub Actions**.
- **Punto a destacar en Entrevistas:** Cómo detecté y argumenté los 4 hallazgos de diseño en la API, demostrando criterio técnico para diferenciar entre un fallo de ejecución en la herramienta y una falla de validación real en la arquitectura del backend.

