Proyecto 1 - API Testing con Postman (QA)

Plan de pruebas de API con 27 casos de prueba diseñados, ejecutados y documentados con Postman, más reporte de 4 defectos en Jira.

1. Información general
Nombre del proyecto: Proyecto 1 - API Testing (JSONPlaceholder)
Especialidad: QA
Fuente del proyecto: API de práctica (simulada) para probar; los nombres de producto usados en los bodies de prueba provienen de un dataset de cosméticos / e-commerce de Kaggle.
Link a la fuente original:
API a testear: https://jsonplaceholder.typicode.com/posts
Dataset de Kaggle (datos de ejemplo para los bodies): <PEGAR LINK DEL DATASET DE KAGGLE>
Link al proyecto publicado: <PEGAR LINK DE ESTE REPOSITORIO EN GITHUB>
2. Objetivo

Diseñar y ejecutar un plan de pruebas de API REST que valide el comportamiento de los endpoints de /posts (crear, consultar, actualizar y eliminar) y detecte defectos de validación de datos.

El riesgo que se busca prevenir es que una API acepte datos inválidos (body vacío, campos obligatorios ausentes, tipos de dato incorrectos) y cree registros corruptos sin ningún control. Está pensado para un equipo de desarrollo que necesita saber, antes de salir a producción, qué casos la API no está manejando bien.

3. Plan de trabajo
Análisis de requisitos: revisión de los endpoints de /posts y de los campos title, body y userId, y definición del comportamiento esperado en cada caso (códigos de estado, formato de respuesta).
Diseño de casos de prueba: 27 casos en una matriz (método, endpoint, body, script, resultado esperado, resultado actual y estado), usando nombres de productos del dataset de Kaggle como datos de prueba.
Ejecución: pruebas automatizadas con scripts de Postman, ejecutadas con el Collection Runner.
Evaluación y reporte de defectos: análisis de resultados, clasificación por severidad y registro de los bugs en Jira (espacio "Proyecto 1 API").
Conclusiones y próximos pasos.
4. Preguntas clave
¿Qué pasaría si estos bugs llegaran a producción? Se crearían registros vacíos, incompletos o con tipos de dato incorrectos sin ningún control, lo que podría corromper datos y generar errores en otros sistemas que los consuman.
¿Los resultados son confiables? Parcialmente. JSONPlaceholder es una API simulada: responde como si guardara, pero no persiste nada. Por eso hay pruebas (por ejemplo, de sanitización) que no se pueden verificar de verdad.
¿Qué cobertura falta? Autenticación, rate limiting, paginación y pruebas de contrato, que no aplican en esta API pero sí en una API real.
5. Qué se hizo y cómo
Herramientas: Postman (colección y scripts de prueba), Collection Runner, Jira (reporte de bugs) y Google Sheets (matriz de casos).
Tipos de pruebas aplicadas:
Tipo	Casos
Positivas (crear, consultar, actualizar, eliminar)	TC-01, 02, 03, 05, 06, 07, 08, 09, 10, 11, 22
Rendimiento (tiempo de respuesta < 1000 ms)	TC-04
Negativas	TC-12, 13, 15, 16, 17, 18, 19
Valores límite y filtros sin resultado	TC-20, 21
Validación de esquema y estructura	TC-14, 23, 24, 27
Seguridad básica y caracteres especiales	TC-25, 26
Criterios de aceptación: cada caso define un resultado esperado (código de estado y, cuando aplica, contenido del body). Si la API responde distinto, el caso se marca como FAIL.
Decisión importante: en los casos negativos (TC-15 a TC-18) los scripts verifican el comportamiento ideal de una API real (404 o 400/422), no lo que la API hace hoy. Así los tests fallan cuando hay un defecto y el fallo queda como evidencia.
6. Resultados
27 casos de prueba ejecutados (35 aserciones en total): 31 pasaron y 4 fallaron, sin errores de ejecución.
Bugs encontrados:
ID	Caso	Descripción	Severidad	Resultado actual	Resultado esperado	Ticket
BUG-01	TC-15	DELETE /posts/9999 (recurso inexistente) devuelve 200	Baja	200 OK	404 Not Found	<LINK JIRA>
BUG-02	TC-16	POST /posts acepta un body vacío {}	Media	201 Created	400/422	<LINK JIRA>
BUG-03	TC-17	POST /posts no valida el campo obligatorio title	Media	201 Created	400/422	<LINK JIRA>
BUG-04	TC-18	POST /posts acepta userId con tipo incorrecto ("abc")	Media	201 Created	400/422	<LINK JIRA>
Causa raíz común: la API no valida los datos de entrada.
Limitación de la prueba (no es un bug): en el TC-25 (inyección SQL) la API devuelve el texto tal cual y sin error 500, pero como no guarda datos, no se puede confirmar si habría sanitización real.
Tiempos de respuesta: promedio de ~100 ms en la corrida final.
Evidencia
Resumen del Collection Runner: evidencia/runner-resumen.png
Casos fallidos (TC-15 a TC-18): evidencia/casos-fallidos.png
Tablero de Jira con los 4 bugs: evidencia/jira-bugs.png
7. Conclusiones
Qué aprendí: a diseñar casos de prueba de API más allá del camino feliz, a escribir tests que validen el comportamiento esperado (no solo el actual) y a documentar defectos con pasos, severidad, resultado actual/esperado e impacto.
Qué mejoraría con más tiempo:
Ejecutar la colección desde la terminal con Newman y generar un reporte.
Agregar un flujo encadenado (crear, leer, actualizar, eliminar) usando variables de Postman.
Validar el contenido del body en todos los casos positivos, no solo el código de estado.
Probar una API real con autenticación y paginación.
Qué mencionaría en una entrevista: el ciclo completo de probar, encontrar, reportar y priorizar defectos, y la decisión de escribir los tests contra el comportamiento ideal para que los defectos queden visibles.
8. Checklist antes de publicar
 README explica el proyecto sin necesidad de revisar todo el detalle
 Archivos organizados, sin pruebas sueltas o versiones viejas
 Sin credenciales ni datos sensibles en el repositorio
 Link a la fuente original incluido y funcionando
 Proyecto publicado y accesible (repo, sitio en vivo, o prototipo interactivo según corresponda)
 Link compartido con mi coach
Estructura del repositorio
proyecto-1-api-testing/
├── README.md
├── collection/             (colección de Postman exportada, .json)
├── matriz-de-pruebas.xlsx  (27 casos con resultado esperado y actual)
└── evidencia/              (capturas del Runner, casos fallidos y Jira)
