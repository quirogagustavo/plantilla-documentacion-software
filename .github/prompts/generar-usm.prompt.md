\# Prompt: Generador de User Story Map (USM) desde SRS ISO 29148

A partir de la Especificación de Requerimientos (SRS) en norma ISO 29148 proporcionada, genera un **User Story Map (USM)** en formato Markdown organizando las historias de usuario bajo el siguiente esquema:

1\. **Eje Horizontal (Jornada del Usuario / Back-bone):**

   - **Actividades principales del Usuario / Rol**

   - **Pasos / Tareas específicas**

2\. **Eje Vertical (Priorización de Slices / Releases):**

   - **Release 1 (MVP - Producto Mínimo Viable):** Funcionalidades indispensables para salir a producción.

   - **Release 2 (Evolutivo / Mejoras):** Automatizaciones y características complementarias.

3\. **Sintaxis de las Historias de Usuario:**

   Cada historia dentro del mapa debe mantener el formato:

   `Como [Rol], quiero [Acción] para [Beneficio].`

   Y hacer referencia al ID del Requerimiento Funcional correspondiente (ej. `[Origen: RF-CAP-001]`).