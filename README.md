\# Guía de Documentación y Skills de IA para el Proyecto

Este repositorio utiliza una metodología estandarizada de ingeniería de requerimientos e integración impulsada por **GitHub Copilot en VS Code**. Su objetivo es transformar visiones de producto en requerimientos formales, mapas de historias, contratos de API desacoplados y tareas listas para importar en Redmine.

\---

\## 🛠️ Requisitos Previos

1\. Utilizar **Visual Studio Code**.

2\. Tener instalada y activa la extensión **GitHub Copilot** y **GitHub Copilot Chat**.

3\. Tener clonado este repositorio localmente en tu equipo.

\---

\## 🔄 Flujo de Trabajo Completo (End-to-End Workflow)

El proceso de documentación sigue un pipeline secuencial y trazable donde cada etapa sirve como insumo de entrada para la siguiente:

\`\`\`mermaid

graph TD

    A\[1. Épica / Narrativa\] --&gt;|[01-generar-srs-iso29148.prompt.md](http://01-generar-srs-iso29148.prompt.md)| B\[2. SRS - ISO 29148\]

    B --&gt;|[02-generar-usm.prompt.md](http://02-generar-usm.prompt.md)| C\[3. User Story Map - USM\]

    C --&gt;|[03-generar-contrato-api.prompt.md](http://03-generar-contrato-api.prompt.md)| D\[4. Contratos de API REST\]

    C --&gt;|[04-generar-csv-redmine.prompt.md](http://04-generar-csv-redmine.prompt.md)| E\[5. Archivo CSV Tareas Redmine\]

    D --&gt;|Desarrollo Paralelo| F\[Equipo Frontend\]

    D --&gt;|Desarrollo Paralelo| G\[Equipo Backend\]

    E --&gt;|Importación Masiva| H\[Gestión de Tareas en Redmine\]

📂 Estructura del Repositorio

├── .github/

│   ├── [copilot-instructions.md](http://copilot-instructions.md)          # Reglas globales de IA (Norma ISO 29148)

│   └── prompts/                         # Skills y Prompts ejecutables de Copilot

│       ├── [01-generar-srs-iso29148.prompt.md](http://01-generar-srs-iso29148.prompt.md)

│       ├── [02-generar-usm.prompt.md](http://02-generar-usm.prompt.md)

│       ├── [03-generar-contrato-api.prompt.md](http://03-generar-contrato-api.prompt.md)

│       └── [04-generar-csv-redmine.prompt.md](http://04-generar-csv-redmine.prompt.md)

├── narrativa/                            # Visión de negocio y épicas

├── historias/                            # User Story Map (USM) e Historias de Usuario

├── contrato_api/                         # Contratos de API REST (Frontend y Backend)

├── modeloDatos/                          # Diseños de base de datos y esquemas

├── redmine/                              # Archivos CSV para importación masiva en Redmine

├── SRS/                                  # Especificaciones de requerimientos

├── USM/                                  # Mapas de historias de usuario

└── [README.md](http://README.md)

## 🚀 Guía Paso a Paso para la Ejecución de Skills

Para ejecutar cada etapa con **GitHub Copilot Chat** (`Ctrl + Alt + I` / `Cmd + Alt + I`), utiliza los siguientes comandos:

### Paso 1: Generar los Requerimientos SRS (ISO 29148)

A partir del documento de épica en `narrativa/`, genera los Requerimientos Funcionales (`RF-XXX`) y No Funcionales (`RNF-XXX`).

- **Comando en Copilot Chat:**

  @workspace Genera la especificación SRS aplicando el prompt .github/prompts/[01-generar-srs-iso29148.prompt.md](http://01-generar-srs-iso29148.prompt.md) basándote en el archivo narrativa/tu\_[archivo.md](http://archivo.md)

### Paso 2: Generar el User Story Map (USM)

A partir de la SRS generada en el paso anterior, organiza las historias de usuario y define la línea de corte del MVP.

- **Comando en Copilot Chat:**

  @workspace Genera el User Story Map utilizando el prompt .github/prompts/[02-generar-usm.prompt.md](http://02-generar-usm.prompt.md) a partir de los requerimientos de SRS/tu_archivo\_[srs.md](http://srs.md)

### Paso 3: Generar los Contratos de API (Frontend / Backend)

A partir de la definición del USM, genera las especificaciones de la API RESTful para desacoplar el desarrollo de Front y Back.

- **Comando en Copilot Chat:**

  @workspace Diseña el contrato de API ejecutando el prompt .github/prompts/[03-generar-contrato-api.prompt.md](http://03-generar-contrato-api.prompt.md) tomando como entrada historias/tu\_[USM.md](http://USM.md)

### Paso 4: Generar el CSV de Tareas para Redmine

A partir del User Story Map y los Contratos de API, desglosa las historias en tareas para importar de forma masiva a Redmine.

- **Comando en Copilot Chat:**

  @workspace Genera el archivo CSV para Redmine ejecutando el prompt .github/prompts/[04-generar-csv-redmine.prompt.md](http://04-generar-csv-redmine.prompt.md) tomando como entrada historias/tu\_[USM.md](http://USM.md) y guarda el resultado en redmine/tareas_redmine_tu_modulo.csv

## 📌 Reglas de Calidad y Colaboración

- **Formato ISO 29148 Obligatorio:** Todos los requerimientos del sistema deben mantener de forma estricta la sintaxis `[Sujeto] + deberá + [Acción/Condición] + [Resultado]`.

- **Desacoplamiento de Desarrollo:**

  - **Frontend:** Puede utilizar las estructuras JSON definidas en `contrato_api/` para maquetar e implementar datos simulados (*mock data*) sin depender del avance del servidor.

  - **Backend:** Debe implementar la lógica de negocio, validaciones (DTOs) y controladores siguiendo exactamente los esquemas y códigos de respuesta del contrato.