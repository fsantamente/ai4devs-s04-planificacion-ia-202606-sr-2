## Índice

1. [Descripción general del producto](#1-descripción-general-del-producto)
2. [Historias de usuario](#2-Historias-de-usuario)
3. [AI as poke-holes](#3-AI-as-poke-holes)

---

## 1. Descripción general del producto

**Prompt 1:**
```
Eres un Product Owner con experiencia en proyectos de IA. Yo seré el cliente y el que tenga todo el conocimiento de negocio y técnico. Estoy trabajando en un producto llamado FlowSync que es una plataforma de orquestación que coordina el trabajo entre personas, agentes de IA y herramientas de desarrollo, manteniendo el contexto compartido y asegurando que cada tarea llegue al agente adecuado en el momento oportuno. Si lo definimos funcionalmente, sería una plataforma que:

a.-   Orquesta agentes de IA durante el ciclo de vida del desarrollo.
b.-   Sincroniza personas, agentes y herramientas (GitHub, Jira, CI/CD, IDE, etc.).
c.-   Mantiene el contexto compartido para que los agentes no trabajen de forma aislada.
d.-   Gestiona el flujo de trabajo, decidiendo qué agente actúa, cuándo y con qué información.
e.-   Coordina los handoffs entre humanos y agentes IA.
f.-   Monitoriza el estado de las tareas, revisiones, pruebas y despliegues.

Debes crear el producto(PRD) en versión MVP dónde sólo esta dentro del alcance el punto a.- indicado anteriormente, con  con toda la información detallada que ayude a aterrizar la idea de negocio, de momento no entres en nada tecnico, enfocate en el QUE y no en el COMO, no inventes features, no estimar tiempos, no proponer arquitectura. Debes enriquecer la informacion con diagramas utilizando codigo mermaid. utiliza buenas practicas para la redaccion del producto, documenta todo en formato markdown en un nuevo archivo entragables\sesion-04\PRD.md
```

## 2. Historias de Usuario

**Prompt 2:**
```
Analiza @PRD.md y genera todas las historias de usuario sólo para el MVP necesarias para abarcar las funcionalidades del proyecto agrupando por épica para que tenga más sentido el PRD FlowSync. guiate por la siguiente informacion y ejemplos: Estructura basica de una User Story Formato estándar: 'Como [tipo de usuario], quiero [realizar una acción] para [obtener un beneficio]'. Descripción: Una descripción concisa y en lenguaje natural de la funcionalidad que el usuario desea. Criterios de Aceptación: Condiciones específicas que deben cumplirse para considerar la User Story como 'terminada', éstos deberian de seguir un formato similar a "Dado que" [contexto inicial], 'cuando" [acción realizada], "entonces" [resultado esperado].La estructura simple y el lenguaje natural ayudan a que todos los miembros del equipo, incluyendo stakeholders no técnicos, puedan entender y colaborar en el desarrollo del producto. Ejemplo completo: Título de la Historia de Usuario: Como [rol del usuario], quiero [acción que desea realizar el usuario], para que [beneficio que espera obtener el usuario]. Criterios de Aceptación: [Detalle específico de funcionalidad] [Detalle específico de funcionalidad] [Detalle específico de funcionalidad] Notas Adicionales: [Cualquier consideración adicional] Historias de Usuario Relacionadas: [Relaciones con otras historias de usuario] cada user story debe tener un codigo de identificacion para facilitar el seguimiento formato HDU-XXX por ejemplo HDU-001 la parte numerica del codigo debe ser incremental y secuencial en la medida que se van creando las HDU agrupa las HDU dentro de epicas, las epicas deben tener un nombre representativo y una codificacion EP-XXX ejemplo EP-001, debe ser secuencial e incremental en la medida q se van creando tanto la epica como la hdu deben tener un titulo descriptivo claro y conciso sin ambiguedades. Además cada User Story tiene que tener un Definition of Done(DoD) dónde a título de ejemplo se adjunta la siguiente:
## DoD (feature nueva)

- [ ] Endpoint implementado siguiendo el patrón existente
- [ ] Validación con VineJS de inputs y outputs
- [ ] Tests funcionales cubriendo todos los AC del GWT
- [ ] Documentación en OpenAPI auto-generada
- [ ] OpenSpec change archivado tras merge

Documenta todo en @output.md
```
---
## 3-AI-as-poke-holes

**Prompt 3:**
```
Analiza @output.md tomando de referencia la User Story HDU-001 con sus criterios de aceptación y tu tarea es identificar edge cases, supuestos implícitos, escenarios faltantes y dependencias o riesgos no mencionados. No reescribas la story, solo lista lo que falta o lo que asumiste. Documenta todo en @pokes-holes.md

```
---