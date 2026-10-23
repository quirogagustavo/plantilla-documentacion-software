---
name: generar-narrativa-requisitos
description: "Genera una narrativa de negocio y su flujo de proceso desde formularios, reuniones o documentos como insumo para una SRS ISO 29148. Usar para describir actores, recorrido y conceptos del dominio sin redactar requisitos RF/RNF ni contratos API."
---

# Generar narrativa de negocio y flujo de proceso

## Propósito

Actúa como analista de negocio. Transforma formularios, notas de reuniones,
transcripciones y documentos de trabajo en una narrativa descriptiva en español,
en Markdown y con diagramas Mermaid.

El nombre del archivo se conserva por compatibilidad: este prompt **no genera
requisitos**. La narrativa es el insumo de la etapa SRS, que utiliza
[generar-srs-iso29148.prompt.md](./generar-srs-iso29148.prompt.md).

## Revisión de fuentes

1. Lee las instrucciones del repositorio, el contexto del proyecto y los
   documentos proporcionados. Busca una narrativa equivalente antes de crear otra.
2. Identifica el proceso, alcance, actores, vocabulario y fuentes. Conserva los
   identificadores existentes y referencia documentos con enlaces relativos.
3. Distingue el proceso actual de los cambios propuestos. Explicita contradicciones,
   datos pendientes y limitaciones; solicita confirmación cuando impidan describir
   el recorrido sin inventar reglas.
4. No asumas un sector, normativa, autenticación, plataforma, motor de base de
   datos o sistema de coordenadas. Incluye estos aspectos solo si están documentados
   y son relevantes para explicar el negocio.

## Exclusión de requisitos y diseño técnico

- No incluyas listas ni tablas de requisitos, ni asignes códigos RF, RNF o REQ.
  Conserva las referencias de origen en la sección de fuentes cuando sean necesarias.
- No utilices frases prescriptivas como «El sistema deberá» o «El sistema debe».
  Describe hechos del proceso y objetivos propuestos sin presentarlos como
  comportamiento implementado.
- No conviertas la narrativa en una SRS, un contrato API ni un diseño de tablas.
- Un modelo conceptual expresa conceptos del negocio; no define columnas, tipos
  físicos, PK/FK, índices ni migraciones. El diseño lógico/físico corresponde a
  [generar-erd-mer.prompt.md](./generar-erd-mer.prompt.md), después de los contratos API.
- No inventes actores, estados, validaciones, cardinalidades, plazos, notificaciones
  ni decisiones para completar el flujo.

## Estructura de salida

### Narrativa de Negocio: [Nombre del proceso o módulo]

Incluye el ID existente, si corresponde, fecha y fuentes consultadas.

### 1. Propósito y alcance del negocio

Describe en prosa el objetivo, los límites y los actores con sus responsabilidades.
Incluye normativa únicamente cuando esté respaldada por las fuentes, sin atribuir
obligaciones no documentadas.

### 2. Recorrido del proceso de principio a fin

Redacta en prosa fluida el inicio, la información aportada, las interacciones,
las revisiones, los resultados y el cierre del proceso. Describe alternativas y
excepciones documentadas, distinguiendo lo actual de lo propuesto.

No presupongas que un actor externo está autenticado. No agregues pasos
administrativos, espaciales o de integración que no provengan del contexto.
Cuando falte información, señala el vacío sin inventar la continuación.

### 3. Flujo visual del proceso

Incluye un bloque cerrado `mermaid` con `flowchart TD` o `sequenceDiagram`.
Usa nombres de actores y actividades coherentes con la prosa. Identifica los
pasos propuestos como tales y señala las partes del recorrido pendientes de
confirmación. El diagrama no debe introducir decisiones nuevas.

### 4. Modelo conceptual del dominio

Describe los conceptos y relaciones respaldados por las fuentes. Incluye un
diagrama Mermaid `erDiagram` solo si se conocen las cardinalidades y la
opcionalidad necesarias; de lo contrario, utiliza `flowchart` para mostrar
asociaciones conceptuales sin atribuirles cardinalidades.

No representa todavía un esquema de persistencia. Si las fuentes no permiten
definir conceptos o relaciones, explicita esa limitación en lugar de inventarlos.

### 5. Fuentes, propuestas y asuntos pendientes

Registra la procedencia de las reglas y decisiones descritas, los cambios
propuestos y los datos o contradicciones pendientes de confirmar. Estas listas
son de trazabilidad y aclaraciones, no de requisitos de software.

## Ubicación y verificación final

Actualiza la narrativa equivalente si existe. En esta plantilla, usa
`narrativa/NAR_[ID]_[NombreModulo].md`; respeta las rutas e IDs de proyectos
existentes y no renumeres sus documentos.

Antes de entregar, comprueba que:

- No hay encabezados de requisitos, catálogos RF/RNF/REQ ni frases prescriptivas
  de implementación.
- El recorrido y los diagramas son consistentes y trazables a las fuentes.
- Las propuestas no se presentan como hechos y los vacíos siguen explícitos.
- No se introdujo un modelo físico ni un contrato API.
- Los bloques Markdown están cerrados. No declares validación de renderizado
  Mermaid si no la ejecutaste.
