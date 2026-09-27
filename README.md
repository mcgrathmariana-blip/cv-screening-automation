# cv-screening-automation
Automatización del screening inicial de CVs mediante IA
# Automatización del Screening Inicial de CVs mediante IA

## 1. Descripción del proyecto

Este proyecto implementa una automatización para el análisis y clasificación inicial de CVs recibidos durante procesos de selección.

El objetivo es reducir el tiempo dedicado al screening manual, estructurar la información relevante de cada candidato y comparar su perfil con los requisitos de una posición.

La automatización no reemplaza la decisión final del equipo de selección. Su función es realizar un análisis preliminar y facilitar la priorización para revisión humana.

---

## 2. Problema identificado

La revisión manual de CVs presenta varios desafíos:

- Alto volumen de CVs recibidos.
- Lectura manual de cada documento.
- Extracción manual de información.
- Comparación manual contra los requisitos.
- Diferencias de criterio entre evaluadores.
- Tiempo elevado de procesamiento.
- Dificultad para priorizar candidatos.

---

## 3. Objetivo

Automatizar el análisis inicial de los CVs para:

1. Recibir el documento.
2. Extraer información relevante.
3. Normalizar los datos.
4. Comparar el perfil con los requisitos de la posición.
5. Generar una clasificación preliminar.
6. Identificar habilidades coincidentes y faltantes.
7. Determinar si se requiere revisión humana.
8. Almacenar los resultados.

---

## 4. Tecnologías utilizadas

| Herramienta | Función |
|---|---|
| Make | Orquestación del flujo |
| Webhook | Recepción del CV |
| Google Gemini | Extracción y análisis semántico |
| Google Sheets | Almacenamiento de resultados |

---

## 5. Arquitectura conceptual

```text
CV
 ↓
Recepción
 ↓
Extracción de información
 ↓
Normalización
 ↓
Análisis IA
 ↓
Validación
 ↓
Clasificación
 ↓
Almacenamiento
Webhook
   ↓
Google Gemini
   ↓
Datos estructurados
   ↓
Set Variable
   ↓
Router
   ├── CV válido → Google Sheets
   │                Status: valid
   │
   └── Revisión → Google Sheets
                   Status: needs_review
 Flujo de procesamiento
Paso 1 — Recepción
El candidato envía un CV en formato PDF.

El documento es recibido mediante un Webhook de Make.

Paso 2 — Procesamiento mediante IA
Google Gemini analiza el contenido del CV y extrae información estructurada.

Se identifican:

Nombre.

Email.

Experiencia.

Habilidades.

Formación.

Información relevante del perfil.

Paso 3 — Comparación con la posición
El modelo compara la información del candidato con los requisitos definidos para la posición.

Se consideran, entre otros:

Experiencia mínima.

Habilidades requeridas.

Habilidades presentes.

Habilidades faltantes.

Paso 4 — Resultado estructurado
La IA genera:

Años de experiencia.

Habilidades.

Habilidades coincidentes.

Habilidades faltantes.

Nivel de coincidencia.

Clasificación.

Resumen.

Recomendación.

Indicador de revisión humana.

Paso 5 — Validación
Un Router divide el resultado en dos rutas:

CV válido

El registro se almacena en Google Sheets con:

Status = valid

Revisión / Error

El registro se almacena con:

Status = needs_review

Esto permite identificar los casos que requieren intervención humana.

8. Datos de entrada
Información del candidato
Nombre.

Email.

Teléfono.

Ubicación.

Formación
Título.

Institución.

Nivel académico.

Experiencia
Empresa.

Puesto.

Período.

Descripción.

Habilidades
Habilidades técnicas.

Herramientas.

Idiomas.

Información de la posición
job_id.

job_title.

required_skills.

minimum_experience.

Resultado generado
{
  "candidate_name": "Ricardo Salinas",
  "job_id": "DEV-001",
  "job_title": "Backend Developer",
  "experience_years": 8,
  "matching_skills": [
    "Python",
    "SQL"
  ],
  "missing_skills": [
    "Docker"
  ],
  "match_score": 75,
  "classification": "medium_match",
  "recommendation": "review",
  "requires_human_review": true
}

JSON schema
El esquema utilizado para estructurar el resultado se encuentra en:

schemas/candidate-screening-schema.json

Define los tipos, campos obligatorios y restricciones del resultado generado.

Justificación del uso de LLM
Los CVs contienen información no estructurada y presentan variaciones importantes en lenguaje, formato y forma de describir experiencias y habilidades.

Una implementación basada exclusivamente en reglas if/else requeriría numerosas condiciones para contemplar las diferentes formas de expresar una misma experiencia o competencia.

El uso de un LLM permite interpretar el contenido semántico del CV, estructurar la información y compararla con los requisitos definidos para la posición.

Criterio	Nivel
Volumen de CVs	Alto
Tiempo de revisión	Alto
Repetitividad	Alto
Riesgo de inconsistencias	Medio / Alto
Potencial de automatización	Alto
Impacto esperado	Alto
Esfuerzo técnico	Medio
El principal impacto esperado es la reducción del tiempo destinado al screening inicial y una mayor estandarización del procesamiento.


La automatización está diseñada como una herramienta de apoyo al proceso de selección.

El resultado generado por IA constituye una clasificación preliminar y no una decisión definitiva sobre la candidatura.

Los casos marcados con:

requires_human_review = true

deben ser revisados por una persona antes de continuar con el proceso.

Limitaciones
La información depende de lo que esté presente en el CV.

Un CV puede utilizar términos diferentes para describir habilidades similares.

La extracción automática puede contener errores.

El resultado generado por IA requiere validación.

La clasificación no debe utilizarse como única base para decisiones de contratación.

El sistema debe evitar utilizar atributos personales no relacionados con los requisitos profesionales.

Próximas mejoras
Integración con Gmail/Outlook.

Notificaciones automáticas al equipo de selección.

Incorporación de más posiciones.

Gestión de múltiples candidatos.

Dashboard de resultados.

Registro de métricas de procesamiento.

Mejoras en validación y manejo de errores.

Incorporación de controles adicionales para revisión humana

El flujo principal fue implementado utilizando Make, Google Gemini y Google Sheets.

La prueba realizada permitió comprobar la recepción del CV, el análisis mediante IA, la extracción estructurada de información y la comparación con los requisitos de una posición.

La ejecución completa de producción requiere considerar los límites de cuota de la API utilizada y realizar pruebas adicionales antes de su uso operativo.

## Resultado de prueba

Se realizó una ejecución de prueba utilizando un CV de datos ficticios.

La ejecución produjo un resultado estructurado con:

- 8 años de experiencia.
- Python y SQL como habilidades coincidentes.
- Docker como habilidad faltante.
- Match score de 75.
- Clasificación `medium_match`.
- Recomendación `review`.
- Revisión humana requerida: `true`.

La ejecución posterior puede estar limitada por la cuota disponible de la API de Gemini.

## Normalización de datos

Antes de almacenar los resultados, el flujo normaliza los principales campos utilizados por el proceso:

- `candidate_id`: se trata como texto y se eliminan espacios innecesarios.
- `email`: se normaliza eliminando espacios y utilizando minúsculas.
- `experience_years`: se convierte a valor numérico.
- `match_score`: se trata como valor numérico entre 0 y 100.
- `skills`: se mantiene como una lista de valores de texto.
- Las fechas, cuando están disponibles, deben utilizar el formato `YYYY-MM-DD`.

La normalización busca mantener consistencia de tipos y facilitar el almacenamiento y procesamiento posterior.

## Manejo de errores

El flujo incorpora una validación antes de considerar válido el resultado del análisis.

Los campos críticos utilizados para la validación son:

- `candidate_id`
- `email`

Si ambos campos están presentes, el candidato continúa por la ruta de procesamiento normal.

Si alguno de estos campos está vacío, el registro se deriva a la ruta de revisión/error mediante el Router.

Los documentos vacíos, incompletos o que no puedan ser procesados correctamente deben quedar en revisión humana en lugar de continuar como candidatos válidos.

## Variables de configuración

El flujo utiliza las siguientes configuraciones:

| Variable | Descripción |
|---|---|
| `candidate_id` | Identificador del candidato |
| `job_id` | Identificador de la posición |
| `job_title` | Nombre de la posición |
| `required_skills` | Habilidades requeridas |
| `minimum_experience` | Experiencia mínima requerida |
| `email` | Email del candidato |
| `experience_years` | Años de experiencia normalizados |

Los valores sensibles, credenciales y API keys no forman parte del repositorio.

## Ejemplo de payload de entrada

Ejemplo simplificado de los datos que puede recibir el webhook:

```json
{
  "candidate_id": "CAND-001",
  "candidate_name": "Ricardo Salinas",
  "email": "hola@sitioincreible.com",
  "job_id": "DEV-001",
  "job_title": "Backend Developer",
  "minimum_experience": 3,
  "required_skills": [
    "Python",
    "SQL",
    "Docker"
  ]
}

## Historial de cambios

### Versión 1.1

Se incorporaron mejoras luego de la primera revisión:

- Exportación del flujo como blueprint.
- Organización de evidencias en `/assets`.
- Normalización de campos del candidato.
- Validación de `candidate_id` y `email`.
- Ruta de revisión mediante Router.
- Documentación de variables de configuración.
- Ejemplo de payload de entrada.



