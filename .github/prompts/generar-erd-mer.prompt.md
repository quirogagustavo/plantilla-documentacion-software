---
name: generar-erd-mer
description: "Genera modelos entidad-relación (ERD/DER/MER) en Markdown y Mermaid desde narrativas, SRS, USM y contratos API. Usar para diseñar o documentar tablas, relaciones, restricciones y persistencia; incluir PostgreSQL/PostGIS solo cuando el proyecto lo requiera."
---

# Generación de modelos de datos (ERD / DER / MER)

## Propósito y alcance

Actúa como arquitecto de datos. Transforma los documentos de negocio y contratos
de API en un modelo lógico y, cuando el motor esté confirmado, un modelo físico
trazable. Redacta en español y no asumas un dominio, motor ni versión concretos.

El flujo de referencia es:

`Narrativa -> SRS (ISO 29148) -> USM -> Contratos de API -> Modelo de datos`

Los contratos describen el intercambio de datos, no necesariamente la estructura
de almacenamiento. No conviertas automáticamente cada DTO, endpoint o campo de
respuesta en una tabla o columna: distingue datos persistidos, calculados y externos.
El CSV de Redmine sigue siendo una salida de planificación independiente.

## Insumos y revisión previa

1. Lee las instrucciones del repositorio y su contexto actual, si existe.
2. Identifica la narrativa, SRS, USM y contratos API del módulo indicado.
3. Revisa los esquemas SQL, migraciones, diccionarios y modelos existentes antes
   de proponer entidades. Reutiliza lo existente y justifica cualquier nueva tabla.
4. Conserva los IDs de requisitos e historias y las convenciones del proyecto.
5. Confirma el motor, versión, esquemas y convenciones de claves. Si faltan,
   documenta el modelo lógico y registra el diseño físico como pendiente.
6. Si hay fuentes ausentes o contradictorias, explicita la limitación y solicita
   confirmación cuando impida definir relaciones o restricciones. No inventes
   cardinalidades, obligatoriedad, estados, reglas de borrado ni umbrales.

## Reglas de modelado

- Separa los hechos documentados, las propuestas de diseño y los pendientes.
- Distingue entidades existentes, nuevas y modificadas. No presentes propuestas
  como tablas implementadas ni generes migraciones salvo solicitud expresa.
- Documenta claves primarias, candidatas y foráneas, cardinalidad y opcionalidad
  según las fuentes. Las relaciones sin confirmar no deben aparecer como hechos
  en el diagrama; regístralas como pendientes.
- Explica las relaciones muchos-a-muchos y sus entidades asociativas cuando estén
  justificadas. Evita duplicación; justifica cualquier desnormalización.
- Define tipos físicos, nulabilidad, unicidad, valores predeterminados y reglas
  de integridad solo si están confirmados o identificados como propuestas.
- Justifica índices según consultas y operaciones documentadas; no inventes cargas,
  tiempos de respuesta ni políticas de retención.
- No agregues autenticación, auditoría, borrado lógico o datos personales por
  defecto. Respeta las políticas y los límites de persistencia del proyecto.
- Para PostgreSQL, utiliza tipos y restricciones compatibles con la versión
  confirmada. No impongas PostgreSQL 15+ ni otro mínimo sin una fuente.
- Usa PostGIS únicamente si hay datos espaciales y el proyecto lo requiere.
  Documenta `geometry`/`geography`, tipo geométrico, SRID y necesidades de índices
  espaciales; si no están definidos, márcalos como pendientes. No supongas
  coordenadas, proyecciones ni separación de bases o esquemas.

## Ubicación de salida

Actualiza el documento equivalente si ya existe. Respeta la ruta establecida por
el proyecto; en esta plantilla utiliza `modeloDatos/ERD_[ID]_[NombreModulo].md`.
En proyectos que ya utilicen `docs/erd/`, conserva esa convención.
Reutiliza el ID del módulo cuando exista y no renumeres documentos anteriores.

## Estructura del documento

### 1. Encabezado y alcance

- Título: `Modelo Entidad-Relación: ERD_[ID]_[NombreModulo]`.
- Módulo/proceso, fecha y alcance.
- Documentos insumo con enlaces relativos e IDs de origen.
- Motor y versión confirmados, o estado pendiente.
- Estado del modelo: existente, propuesto o mixto.

### 2. Fuentes, decisiones y pendientes

Incluye una tabla con `Tema`, `Definición`, `Estado` (documentado, propuesto o
pendiente) y `Fuente / justificación`. Explicita conflictos entre documentos.

### 3. Inventario de entidades

Incluye `Entidad / tabla`, `Propósito`, `Existente / nueva / modificada`,
`Base / esquema` y `Origen funcional`. Justifica altas y cambios.

### 4. Diagrama Mermaid

Incluye un bloque cerrado de código `mermaid` con `erDiagram`, entidades,
atributos y relaciones confirmadas. Usa las marcas `PK`, `FK` y `UK` cuando
correspondan y cardinalidades coherentes con el diccionario.
Usa identificadores y tipos compatibles con Mermaid; detalla los tipos físicos
complejos (por ejemplo, geometrías con SRID) en el diccionario.
Separa o etiqueta claramente cualquier diagrama propuesto respecto del existente.
Si una cardinalidad no está definida, omite esa relación del diagrama y registra
la omisión y su origen en pendientes.

### 5. Diccionario de datos

Por entidad, incluye `Campo`, `Tipo lógico / físico`, `Nulabilidad`,
`Clave / referencia`, `Restricciones / valor predeterminado`, `Descripción`,
`Estado` y `Fuente`. Indica explícitamente los valores no definidos.

### 6. Relaciones, integridad e índices

Documenta cardinalidad, opcionalidad, claves relacionadas y reglas de
actualización/borrado, además de restricciones e índices y su justificación.
Distingue lo implementado de lo propuesto y de lo pendiente.

### 7. Mapeo API y trazabilidad

Incluye `Requisito / historia`, `Método y ruta`, `Campo de solicitud / respuesta`,
`Entidad.campo o cálculo / fuente externa` y `Observaciones`.
Mantén las rutas del contrato y señala los datos que no se persisten.

### 8. Impacto y validación

Resume cambios respecto del esquema existente, compatibilidad, dependencias,
decisiones pendientes y necesidades de migración sin ejecutarlas.
Comprueba que entidades, atributos, claves y cardinalidades coincidan entre
diagrama y diccionario; que cada propuesta tenga origen; y que las restricciones
no contradigan narrativa, SRS, USM ni API. No declares validaciones de sintaxis o
renderizado que no hayas ejecutado.
