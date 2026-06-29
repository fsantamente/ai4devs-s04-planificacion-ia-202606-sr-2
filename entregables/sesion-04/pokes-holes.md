# FlowSync MVP — Análisis crítico de HDU-001
**Técnica:** Poke-the-holes
**Historia analizada:** HDU-001 — Registro de un nuevo agente de IA en la plataforma
**Referencia:** [output.md](output.md) · [PRD.md](PRD.md)
**Fecha:** 2026-06-29

> Este documento no reescribe la historia. Solo lista lo que falta, lo que se asumió sin declararlo y los riesgos que la story ignora.

---

## 1. Edge cases no cubiertos por los criterios de aceptación

### 1.1 Nombre con solo espacios en blanco
El AC-2 dice "cuando no introduce el nombre". Un nombre formado únicamente por espacios (`"   "`) no está vacío técnicamente pero tampoco es un nombre válido. No hay criterio que cubra este caso; la validación podría aceptarlo y crear un agente sin nombre visible en el listado.

### 1.2 Nombre duplicado
No hay ningún criterio que defina qué ocurre si el Tech Lead intenta registrar un agente con el mismo nombre que uno ya existente. El listado de HDU-002 mostraría dos entradas idénticas, haciendo imposible distinguir cuál asignar en HDU-006.

### 1.3 Descripción funcional vacía
El AC-1 menciona "completa el nombre **y** la descripción funcional" como condición de éxito. Sin embargo, el AC-2 solo declara el nombre como campo obligatorio. No queda claro si la descripción es obligatoria u opcional. Un agente sin descripción registrado es inútil para que otro Tech Lead entienda su propósito.

### 1.4 Sin ninguna fase seleccionada
La historia dice que el registro incluye "las fases en las que puede participar", pero no especifica si seleccionar al menos una fase es obligatorio. Un agente registrado sin fases nunca podrá ser asignado a ningún flujo (bloquea HDU-004 y HDU-006), pero el formulario podría permitirlo sin avisar.

### 1.5 Longitud máxima de nombre y descripción
No hay límites definidos para ninguno de los dos campos. Un nombre de 2.000 caracteres rompería el diseño del listado; una descripción de 100.000 caracteres degradaría el rendimiento. Sin tope declarado, cada implementador decidirá un límite arbitrario.

### 1.6 Caracteres especiales y contenido potencialmente peligroso en el nombre
No se indica si el nombre admite caracteres especiales, HTML o scripts. Si el nombre se muestra sin sanitización en el listado o en el contexto de una tarea, existe un vector de XSS. La story tampoco aclara si el nombre puede contener emojis, saltos de línea o caracteres no-ASCII.

### 1.7 Fallo de servidor durante el guardado
El AC-3 asume que "el proceso finaliza correctamente". No hay criterio para el escenario en que la llamada al servidor falla después de que el usuario ha pulsado confirmar. ¿El formulario retiene los datos? ¿El agente queda en estado inconsistente (medio creado)?

### 1.8 Expiración de sesión a mitad del formulario
Si la sesión del Tech Lead caduca mientras rellena el formulario y pulsa "Guardar", ¿qué ocurre? No hay criterio. El usuario podría perder todos los datos introducidos sin aviso.

### 1.9 Registro simultáneo del mismo agente por dos Tech Leads
Dos Tech Leads distintos registran un agente con el mismo nombre al mismo tiempo. Sin control de concurrencia declarado, ambos registros podrían completarse, creando un duplicado silencioso.

---

## 2. Supuestos implícitos no declarados

### 2.1 El nombre del agente es único en la plataforma
La story no lo declara, pero la lógica de flujos lo requiere: si dos agentes comparten nombre, el listado de HDU-002 y el selector de HDU-006 se vuelven ambiguos. Se asumió unicidad sin escribirla como regla.

### 2.2 El estado inicial del agente al registrarse es "activo"
No se especifica. Si el agente nace en estado inactivo (como un borrador), no aparecerá disponible en HDU-006 sin que el Tech Lead lo active explícitamente en HDU-003. Si nace activo, podría ser asignado antes de estar correctamente configurado. El AC-3 dice "disponible para ser asignado a flujos" sin aclarar en qué estado queda.

### 2.3 Solo el Tech Lead puede registrar agentes
La story dice "Como Tech Lead" pero no define qué ocurre si un desarrollador o Engineering Manager accede a la sección de registro. ¿No ve el botón? ¿Recibe un error de permisos? El control de acceso se asumió pero no está en ningún criterio ni en el DoD.

### 2.4 El agente no se valida contra ningún sistema externo
La precondición del PRD (CU1) afirma "el agente existe y está disponible para ser invocado". Sin embargo, HDU-001 no incluye ningún paso de verificación de conectividad o disponibilidad del agente real. Se asume que el Tech Lead ya conoce que el agente funciona externamente.

### 2.5 El agente registrado es visible para todos los usuarios de la plataforma
No se declara si los agentes son globales (compartidos por todos los flujos y Tech Leads) o privados del Tech Lead que los registró. Se asumió visibilidad global, pero eso no está escrito.

### 2.6 La descripción es texto libre sin formato
No se especifica si admite markdown, texto enriquecido o solo texto plano. Impacta en cómo se renderiza en el listado y en el contexto de la tarea.

### 2.7 El nombre del agente no puede modificarse después del registro
HDU-001 no menciona edición. Si el nombre puede cambiarse post-registro y los flujos ya tienen el agente asignado, habría que decidir si el flujo refleja el nombre nuevo automáticamente o conserva el nombre original. Este comportamiento no está definido en ninguna historia.

### 2.8 No es posible eliminar un agente, solo desactivarlo
HDU-001 no menciona eliminación y HDU-003 solo cubre activación/desactivación. Se asumió que la eliminación no existe en el MVP, pero no está explícitamente excluida.

---

## 3. Escenarios faltantes en los criterios de aceptación

| # | Escenario ausente | Impacto si no se define |
|---|-------------------|------------------------|
| 3.1 | Intento de registro con nombre duplicado | Duplicados silenciosos en el listado |
| 3.2 | Nombre formado solo por espacios | Pasa la validación de "campo no vacío" y crea un agente sin nombre visible |
| 3.3 | Descripción omitida (campo opcional vs. obligatorio) | Comportamiento inconsistente entre implementadores |
| 3.4 | Registro sin seleccionar ninguna fase | Agente creado pero inutilizable; bloquea HDU-004 y HDU-006 |
| 3.5 | Error de servidor al guardar | El usuario no sabe si el registro se completó o no |
| 3.6 | Cancelación del formulario a mitad | Ambigüedad sobre pérdida de datos y navegación |
| 3.7 | Navegación post-registro | No está definido a dónde va el usuario tras el éxito (listado, ficha del agente, mismo formulario) |
| 3.8 | Acceso al formulario por un rol sin permisos | No hay criterio de acceso; queda a interpretación del desarrollador |
| 3.9 | Campos que superan la longitud máxima | Sin límites, la implementación decide arbitrariamente |

---

## 4. Dependencias y riesgos no mencionados

### 4.1 Solapamiento de alcance entre HDU-001 y HDU-004
El DoD de HDU-001 incluye el campo "fases asociadas" en el formulario de registro. HDU-004 también describe la asociación de fases "al registrar o editar" un agente. No está claro qué historia implementa el selector de fases dentro del formulario de alta. Si dos equipos trabajan en paralelo en HDU-001 y HDU-004, pueden duplicar trabajo o generar conflictos de interfaz.

### 4.2 Sin trazabilidad de autoría del registro
No se registra qué Tech Lead creó el agente ni cuándo. El PRD exige auditabilidad (RNF-02): "Toda activación de agente debe quedar registrada". Por extensión, el registro de un agente también debería quedar auditado, pero la story no lo contempla.

### 4.3 El error se detecta demasiado tarde
FlowSync registra el agente basándose solo en lo que el Tech Lead escribe, sin validar que el agente subyacente realmente exista y sea invocable. El fallo real se descubrirá en HDU-011 (activación automática), cuando ya haya una tarea bloqueada en producción. El riesgo no está señalado en HDU-001.

### 4.4 HDU-001 es un bloqueante en cadena
HDU-001 es prerrequisito directo de HDU-004, HDU-006 y HDU-007, que a su vez bloquean todo EP-002 y EP-003. Un retraso o defecto en esta historia arrastra al menos a otras 8 historias de usuario. Su posición crítica en el grafo de dependencias no está señalada.

### 4.5 Riesgo de proliferación de agentes sin gobierno
Sin restricción de unicidad, sin validación de formato y sin rol de administrador global declarado, el catálogo de agentes puede crecer de forma no controlada (agentes duplicados, con nombres crípticos, sin descripción, sin fases). A escala de equipo mediano, el listado de HDU-002 se vuelve inmanejable. No hay criterio de higiene del catálogo en el MVP.

### 4.6 Visibilidad global no acotada
Si los agentes son accesibles por todos los flujos y todos los Tech Leads de la plataforma (supuesto implícito 2.5), un agente registrado para un equipo podría ser asignado inadvertidamente por otro equipo a sus flujos, sin que esto aparezca como riesgo en ninguna historia.

---

## 5. Resumen de hallazgos

| Categoría | Nº de ítems identificados |
|-----------|--------------------------|
| Edge cases | 9 |
| Supuestos implícitos | 8 |
| Escenarios faltantes en AC | 9 |
| Dependencias y riesgos | 6 |
| **Total** | **32** |

---

*Análisis realizado sobre HDU-001 del documento output.md v1.1*
*FlowSync MVP — AI4Devs Sesión 04*
