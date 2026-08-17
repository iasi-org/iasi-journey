# ChatGPT — Skill `define` de IASI

## Usuario

bien, me vas preparando el skill?

## ChatGPT

Sí. Lo voy a montar como **skill real de IASI**, no como una plantilla tonta de “convierte cada input en una definition”. La pieza clave será el análisis transversal: leer el conjunto vigente de `inputs/`, detectar conceptos, decisiones, restricciones, contradicciones y huecos, y construir `definitions/` canónicas sin inventar nada.

Sí. Y ya tenemos bastante cerrada la frontera: **`/define` es la puerta del workflow y `define` es la inteligencia que hace el trabajo semántico**. Go se queda con la mecánica: discovery, fingerprints, gates, archivado y escrituras seguras. El agente interpreta.

Esta sería mi **primera versión de `iasi/skills/define/skill.md`**:

```markdown
---
name: define
description: Build and maintain IASI definitions from the current project inputs.
---

# Define

## Purpose

Transform the current set of IASI `inputs/` into a structured, canonical and maintainable representation under `definitions/`.

Definitions represent what IASI has understood from the available inputs.

The transformation is semantic, not mechanical.

There is no required 1:1 relationship between inputs and definitions.

A definition may:

- consolidate information from multiple inputs;
- separate several concepts found in a single input;
- refine an existing definition;
- reveal contradictions between inputs;
- expose missing information;
- remain unchanged when new inputs add no relevant knowledge.

## Source model

Read the current valid inputs from:

- `inputs/externals/`
- `inputs/internals/`
- `inputs/obtained/`

Treat inputs as evidence.

Do not rewrite, normalize or modify them.

Inputs are not definitions.

## Core rules

### Never invent

Do not create facts, requirements, decisions, constraints or assumptions that are not supported by the available inputs.

When relevant information is missing, expose the gap.

When a decision is required but has not been made, represent it as unresolved.

### Understand before writing

Do not process inputs independently.

First understand the complete current input set and the relationships between its contents.

Look for:

- concepts;
- goals;
- requirements;
- constraints;
- decisions;
- rules;
- actors;
- entities;
- relationships;
- dependencies;
- contradictions;
- uncertainties;
- missing information.

Only then determine the appropriate definition structure.

### Organize semantically

Definitions are organized by meaning, not by source file.

Do not reproduce the directory structure or filenames of `inputs/` unless that organization is itself semantically relevant.

Prefer cohesive definitions with a clear responsibility.

Split a definition when it contains independent concepts that should evolve separately.

Merge knowledge when several inputs describe the same concept.

### Maintain a canonical representation

`definitions/` is the editable canonical representation understood by IASI.

Definitions may be edited by humans.

Human edits are therefore part of the current canonical state and must not be silently destroyed during regeneration.

New input processing must reconcile existing definitions with new evidence.

### Preserve intent

Regeneration is non-destructive.

When an existing definition contains information that cannot be derived from the current inputs, do not automatically remove it.

Determine whether it is:

- a valid human refinement;
- knowledge originating from an older input;
- obsolete information;
- contradictory information;
- an unsupported statement.

Preserve it unless there is sufficient evidence and workflow authority to replace or remove it.

### Maintain traceability

Every material statement in a definition must be traceable to its supporting inputs whenever practical.

Each definition must contain a `Sources` section.

Sources identify the input files that support the definition.

Source references establish provenance, not ownership.

A definition may reference many inputs and an input may support many definitions.

## Workflow

### 1. Discover

Obtain the set of current valid inputs and existing definitions from the IASI runtime.

Do not bypass runtime discovery rules.

### 2. Read

Read all inputs relevant to the current definition operation.

Read existing definitions before proposing changes.

Do not assume that an input can be understood correctly in isolation.

### 3. Analyse

Build a semantic model of the available information.

Identify:

- existing concepts;
- new concepts;
- refinements;
- overlaps;
- contradictions;
- unresolved questions;
- dependencies between definitions.

### 4. Reconcile

Compare the semantic model with the current contents of `definitions/`.

For each concept determine whether to:

- create a definition;
- update a definition;
- split a definition;
- consolidate definitions;
- leave the current definition unchanged;
- flag a contradiction;
- report missing information.

Do not perform structural churn without semantic benefit.

### 5. Draft

Produce the proposed canonical definitions in structured Markdown.

Write for future use by both humans and intelligent systems.

Definitions should be explicit, concise and unambiguous.

Avoid narrative repetition of the original inputs.

### 6. Validate

Before writing, verify that:

- every definition has a clear purpose;
- no unsupported facts have been introduced;
- material claims are traceable;
- contradictory inputs have not been silently resolved;
- unresolved information remains explicitly unresolved;
- human refinements have not been silently discarded;
- the resulting definition set is internally coherent.

If the available information is insufficient for the next workflow step, report the gaps and stop.

"Sufficient" means sufficient to continue, not complete in an absolute sense.

### 7. Write

Return the proposed definition operations to the IASI runtime.

The runtime is responsible for safe filesystem changes.

The skill must not implement its own file mutation mechanism when runtime operations are available.

## Definition format

Definitions use structured Markdown.

A definition should normally contain:

```markdown
# <Definition name>

## Definition

<Canonical description of the concept.>

## Details

<Relevant structured knowledge, when needed.>

## Constraints

<Constraints that apply to the definition, when present.>

## Relationships

<Relationships or dependencies with other definitions, when relevant.>

## Open questions

<Information that remains unresolved, when present.>

## Sources

- <input reference>
```

Sections that carry no information may be omitted except `Sources`.

The exact structure may evolve when a definition type requires a more appropriate representation.

Do not force information into irrelevant sections merely to satisfy a template.

## Contradictions

When inputs contradict each other:

1. do not choose a winner without evidence;
2. identify the conflicting statements;
3. identify their sources;
4. determine whether precedence can be established from explicit project rules;
5. otherwise mark the issue as unresolved.

A contradiction is information.

Do not hide it.

## Missing information

When the inputs are insufficient:

- identify exactly what is missing;
- explain why it matters for the next workflow step;
- avoid requesting information that is not currently necessary;
- do not fabricate defaults to make the workflow continue.

The result of `define` may therefore be a request for additional `inputs/internals/`.

## Responsibility boundary

The `define` skill owns semantic interpretation.

It is responsible for:

- understanding inputs;
- relating information;
- consolidating concepts;
- separating concepts;
- detecting contradictions;
- detecting missing information;
- maintaining semantic coherence;
- proposing canonical definitions.

The IASI runtime owns deterministic mechanics.

It is responsible for:

- discovering files;
- calculating fingerprints;
- determining changed inputs;
- enforcing workflow gates;
- managing archived inputs;
- applying filesystem operations;
- protecting writes;
- recording execution state.

Do not move semantic reasoning into the runtime merely because it can be implemented deterministically.

Do not move deterministic infrastructure into the skill merely because an agent can perform it.

## Principle

`inputs/` contains what enters IASI.

`definitions/` contains what IASI understands.
```

Hay una cosa de esta versión que me parece especialmente importante: **`definitions/` no es una salida generada que se pueda machacar**. Es estado canónico editable. Por tanto, `define` no es un “generador”, es un **reconciliador semántico** entre los inputs, lo ya entendido y las modificaciones humanas.

Y con esto, el siguiente artefacto natural ya sale casi solo: `iasi/commands/define.md`, que será muchísimo más pequeño porque se limitará a orquestar el skill dentro del workflow.

---

## Usuario

define/skill.md  
¿No le damos plantillas?

## ChatGPT

Sí. **Se las daría**, y además creo que son parte importante del diseño.

Y correcto: `define/skill.md`, no `define/SKILL.md`.

La gracia es que las plantillas no conviertan `define` en un formulario rígido. Deben darle al agente **formas canónicas disponibles** para estructurar lo que encuentre.

Yo lo montaría así:

```text
define/
├── skill.md
└── templates/
    ├── definition.md
    ├── requirement.md
    ├── constraint.md
    ├── decision.md
    └── question.md
```

No significa que al principio necesitemos cinco. Podemos empezar incluso con dos o tres. Pero conceptualmente veo distintos tipos de conocimiento.

Por ejemplo, `definition.md` podría ser la plantilla general:

```markdown
---
type: definition
status: current
---

# <name>

## Definition

...

## Details

...

## Relationships

...

## Sources

- ...
```

Mientras que una decisión tiene otra naturaleza:

```markdown
---
type: decision
status: accepted
---

# <decision>

## Decision

...

## Context

...

## Rationale

...

## Consequences

...

## Sources

- ...
```

Y una cuestión abierta:

```markdown
---
type: question
status: open
---

# <question>

## Question

...

## Why it matters

...

## Known information

...

## Sources

- ...
```

Aquí aparece algo importante: **el skill no decide primero qué fichero producir y después rellena una plantilla**.

Hace al revés:

```text
inputs
   ↓
comprensión semántica
   ↓
¿qué tipo de conocimiento es esto?
   ↓
selección de plantilla
   ↓
definition
```

Eso mantiene nuestra filosofía.

Además, las plantillas nos permiten hacer otra cosa potente después: **validar `definitions/` mecánicamente desde Go**.

Por ejemplo:

```yaml
type: decision
status: accepted
```

permite que el runtime sepa qué campos son admisibles, qué estados existen, si falta `Sources`, etc., **sin intentar entender el contenido**.

Ahí vuelve a quedar clara la frontera:

**Skill**
> entiende qué es.

**Template**
> define cómo se representa.

**Runtime**
> comprueba que esa representación es válida.

Y hay una decisión que yo tomaría desde ya: **las plantillas forman parte del propio skill**, no del proyecto.

```text
define/templates/
```

porque representan **cómo IASI define conocimiento**, no cómo un proyecto concreto quiere documentarlo.

Luego, si algún día queremos extensibilidad, podríamos permitir templates adicionales en el proyecto. Pero el vocabulario base debería venir con IASI.

De hecho, esto cambia ligeramente cómo escribiría `define/skill.md`: no le explicaría en veinte líneas el formato de una definition. Le diría algo mucho más fuerte:

> **Use the available definition templates. Select the template according to the semantic nature of the knowledge being represented. Do not force knowledge into a template that does not match its meaning.**

---

## Usuario

dame las plantillas en un zip

## ChatGPT

Se entregó `define-templates.zip` con las cinco plantillas iniciales.

---

## Usuario

y que hay que tocar en skill.md? o me lo das completo?

## ChatGPT

Se propuso una versión completa de `define/skill.md` integrada con las plantillas.

El cambio conceptual fundamental quedó resumido así:

```text
skill      → entiende y clasifica
templates  → dan forma
runtime    → valida y ejecuta
```

Se añadió además `Classify` como paso explícito entre `Analyse` y `Reconcile`.

---

## Usuario

hablame de classify

## ChatGPT

`Classify` es el momento en el que **IASI ya ha entendido una pieza de conocimiento y decide qué naturaleza tiene**.

No clasifica el input. Clasifica **lo que ha extraído de él**.

Ejemplo:

```text
"El sistema debe generar HTML y PDF"
        ↓
requirement

"Para websites solo se genera HTML"
        ↓
constraint

"Hemos decidido usar Quarto"
        ↓
decision

"No sabemos cómo resolver EPUB"
        ↓
question
```

Por eso no debe ocurrir al principio:

```text
input → classify
```

sino:

```text
inputs
   ↓
Read
   ↓
Analyse
   ↓
modelo semántico
   ↓
Classify
   ↓
templates
   ↓
definitions
```

Los tipos iniciales son:

```text
definition
requirement
constraint
decision
question
```

Distinción importante:

```text
requirement = qué debe ocurrir
constraint  = dentro de qué límites puede ocurrir
```

`definition` representa conocimiento descriptivo.

Una `question` puede evolucionar a `decision`, `requirement`, `definition`, etc., cuando nueva evidencia resuelve la cuestión.

`Classify` **no es otro comando** ni genera `classifications/`. Es una fase cognitiva interna de `define`.

Separación:

```text
Analyse    → ¿qué dice todo esto?
Classify   → ¿qué clase de conocimiento es?
Reconcile  → ¿cómo encaja con lo que ya sabemos?
Draft      → ¿cómo queda representado?
```

---

## Usuario

vale, me das la info para copilot?

## ChatGPT

Se preparó un prompt específico para Copilot explicando cómo incorporar `Classify` correctamente en `define/skill.md`.

---

## Usuario

pero todavia no le hemos dicho que haga define

## ChatGPT

Correcto. Se preparó entonces un prompt completo para Copilot con el encargo de crear:

```text
define/
├── skill.md
└── templates/
    ├── definition.md
    ├── requirement.md
    ├── constraint.md
    ├── decision.md
    └── question.md
```

El prompt establecía:

- `define` transforma `inputs/` en `definitions/`;
- no existe relación 1:1;
- `definitions/` es estado canónico editable;
- debe reconciliar inputs + definitions existentes + cambios humanos;
- workflow `Discover → Read → Analyse → Classify → Reconcile → Draft → Validate`;
- clasificación semántica;
- trazabilidad;
- contradicciones;
- missing information;
- reclassification;
- separación entre skill y runtime;
- todavía no implementar `/define` ni runtime Go.

---

## Usuario

Copilot respondió:

> Las templates existentes corresponden exactamente a los cinco tipos requeridos: definition, requirement, constraint, decision y question. El skill respeta las reglas clave: interpretación semántica, trazabilidad, reconciliación no destructiva de definitions/, preservación de refinamientos humanos y separación entre razonamiento del skill y mecánica del runtime.
>
> Límites que aún no define...
>
> No especifica un comando canónico /define.
> No define un gate o checkpoint...
> ...
> Ajuste menor recomendado...
> El nuevo input todavía no está registrado en inputs/externals/README.md...

## ChatGPT

Se concluyó que Copilot había entendido correctamente el alcance y no había inventado arquitectura.

Se decidió separar:

```text
skill_define/
    → contrato semántico y comportamiento del skill

command_define/
    → futuro contrato del comando /define
```

y usar `skill_define/` como nombre normativo.

---

## Usuario

bueno, el doc para darselo a copilot

## ChatGPT

Se preparó un documento para Copilot cuyo objetivo era registrar formalmente `define` como **skill normativo de IASI**:

```text
inputs/externals/skill_define/
```

y registrarlo en:

```text
inputs/externals/README.md
```

sin definir todavía `/define`, gates, fingerprints, runtime Go ni protocolo de operaciones.

---

## Usuario

vale, para copilot ya esta bien
¿que le digo que haga?

## ChatGPT

Se recomendó:

> Implementa lo descrito en este documento. Inspecciona primero el repositorio para respetar la estructura y convenciones existentes, realiza únicamente los cambios indicados y, al terminar, muéstrame los archivos creados/modificados y cualquier inconsistencia encontrada.

---

## Usuario

dice que lo ha hecho

## ChatGPT

Se propuso verificar mediante `git diff`, pero debido al límite de créditos de Copilot se decidió no usarlo para tareas mecánicas.

---

## Usuario

estamos casi al limite de creditos en copilot

## ChatGPT

Se decidió reservar Copilot para implementación real:

```text
Copilot → implementar cambios en el repo
ChatGPT → pensar, diseñar, revisar y preparar instrucciones
Git     → verificar qué se ha tocado
```

---

## Usuario

ahi tienes los cambios

## ChatGPT

Se revisó `skill-define-changes.diff`.

Hallazgos:

1. `skill_define/` estaba registrado en `inputs/externals/README.md`.
2. Había que cambiar:

```text
The eight groups form one normative contract:
```

por:

```text
The nine groups form one normative contract:
```

3. `Classify` estaba correctamente introducido.
4. El principal ajuste de `skill.md` era eliminar la sección genérica `## Definition format` y sustituirla por una sección `## Templates`.
5. `Discover` debía incluir también las templates disponibles.

La arquitectura correcta quedó:

```text
skill      → entiende y clasifica
templates  → definen la representación
runtime    → mecánica y validación
```

---

## Usuario

¿donde toco esto?
Yo quitaría completamente `## Definition format`...

## ChatGPT

En:

```text
iasi/skills/define/skill.md
```

Se indicó reemplazar todo `## Definition format` por `## Templates`, con el mapping:

```text
definition  → definition.md
requirement → requirement.md
constraint  → constraint.md
decision    → decision.md
question    → question.md
```

y el flujo:

```text
Analyse
  ↓
Classify
  ↓
Select template
  ↓
Reconcile
  ↓
Draft
```

También se cambió `Discover` para obtener:

```text
current valid inputs
existing definitions
available definition templates
```

---

## Usuario

¿y esto?
The eight groups form one normative contract:

## ChatGPT

En `inputs/externals/README.md`, cambiarlo por:

```text
The nine groups form one normative contract:
```

---

## Usuario

¿que le digo a copilot?

## ChatGPT

Se propuso una instrucción mínima, pero el usuario indicó que ya había hecho manualmente los cambios 1 y 3.

---

## Usuario

1 y 3 ya lo he hecho yo

## ChatGPT

Se redujo el cambio pendiente únicamente a `Discover`.

---

## Usuario

eso tambien lo puedo hacer yo, ¿donde?

## ChatGPT

En:

```text
iasi/skills/define/skill.md
```

dentro de:

```markdown
### 1. Discover
```

cambiar:

```text
Obtain the set of current valid inputs and existing definitions from the IASI runtime.
```

por:

```text
Obtain the set of current valid inputs, existing definitions and available definition templates from the IASI runtime.
```

---

## Usuario

entonces ya estaria

## ChatGPT

Sí. Se dio por bueno el `skill.md` para el alcance actual, quedando únicamente comprobar que existiera:

```text
inputs/externals/skill_define/
```

---

## Usuario

si tengo skill_Define

## ChatGPT

Se cerró `skill_define` por ahora.

Estado:

- `inputs/externals/skill_define/` como contrato normativo.
- `iasi/skills/define/skill.md`.
- Cinco templates.
- `Classify` explícito.
- Reconciliación no destructiva.
- Separación skill/templates/runtime.

El siguiente paso sería `command_define`.

---

## Usuario

igual no nos da con los creditos que nos quedan

## ChatGPT

Se decidió diseñar todo `command_define` aquí y usar Copilot una sola vez para implementación:

```text
Aquí:
1. diseñamos /define
2. definimos workflow gate
3. definimos qué entrega el runtime al skill
4. definimos qué devuelve el skill
5. definimos operaciones create/update/split/consolidate/etc.
6. dejamos escrito command_define completo

Copilot:
7. implementa todo en una sola pasada
```

---

## Usuario

de acuerdo

## ChatGPT

Se propuso el diseño inicial de `command_define`.

### 1. Qué hace `/define`

`/define` **orquesta** el skill `define`. No interpreta conocimiento.

```text
/define
   ↓
runtime: valida + descubre estado
   ↓
define skill: entiende + clasifica + reconcilia
   ↓
runtime: valida operaciones + aplica cambios
   ↓
checkpoint define
```

Separación:

```text
command_define → cuándo y cómo se ejecuta
skill_define   → cómo se entiende el conocimiento
```

### 2. Qué procesa

El runtime puede detectar qué ha cambiado, pero eso no limita automáticamente lo que puede leer el skill.

```text
changed inputs
      ↓
sirven como trigger / pista

all relevant inputs
existing definitions
templates
      ↓
son el contexto semántico disponible
```

No:

```text
solo cambió foo.md
      ↓
define solo puede mirar foo.md
```

### 3. Cuándo no hace nada

Si desde el último `/define` no ha cambiado:

```text
inputs/
definitions/
define/skill.md
define/templates/
```

entonces:

```text
/define → no-op
```

Las modificaciones humanas en `definitions/` también invalidan el checkpoint.

### 4. Qué le entrega el runtime al skill

Conceptualmente:

```text
current valid inputs
changed inputs
existing definitions
available templates
previous define state/checkpoint
```

`changed inputs` es información auxiliar. No constituye el universo semántico.

### 5. Qué devuelve el skill

Contrato estructurado, no prosa libre:

```json
{
  "status": "complete",
  "operations": [],
  "issues": []
}
```

Estados:

```text
complete
    define pudo construir un estado coherente.

needs_input
    pudo actualizar lo conocido, pero existe
    información necesaria todavía pendiente.

no_changes
    no hay modificaciones semánticas.
```

Los errores técnicos pertenecen al runtime.

### 6. Operaciones

Para v1:

```text
create
update
rename
reclassify
split
consolidate
```

Sin `delete` genérico.

Una definition solo desaparece como consecuencia explícita de:

```text
rename
split
consolidate
```

Ejemplo `create`:

```json
{
  "action": "create",
  "path": "definitions/runtime.md",
  "template": "definition.md",
  "content": "..."
}
```

Ejemplo `reclassify`:

```json
{
  "action": "reclassify",
  "path": "definitions/runtime-language.md",
  "from": "question",
  "to": "decision",
  "template": "decision.md",
  "content": "..."
}
```

Ejemplo `split`:

```json
{
  "action": "split",
  "source": "definitions/runtime.md",
  "outputs": [
    {
      "path": "definitions/runtime-language.md",
      "template": "decision.md",
      "content": "..."
    },
    {
      "path": "definitions/runtime-distribution.md",
      "template": "constraint.md",
      "content": "..."
    }
  ]
}
```

El skill decide semánticamente la transformación. El runtime toca el filesystem.

### 7. Issues

Separados de las operaciones:

```json
{
  "kind": "missing_information",
  "blocking": true,
  "message": "...",
  "sources": [...]
}
```

Tipos iniciales:

```text
missing_information
contradiction
template_mismatch
```

Una `question` no implica necesariamente bloqueo.

```text
question + blocking=false
```

puede permitir continuar.

Si falta algo imprescindible:

```text
question + blocking=true
```

el estado será:

```text
needs_input
```

### 8. Gate

Primera conexión real con el workflow:

```text
inputs
  ↓
/define
  ↓
definitions
  ↓
/plan
```

`/plan` solo puede ejecutarse sobre un estado con `/define` vigente.

Vigente significa que desde ese checkpoint no han cambiado:

```text
inputs
definitions
define skill
templates
```

y que el último `/define` no tenga issues bloqueantes.

```text
define checkpoint current
+
no blocking issues
        ↓
/plan permitido
```

Si cambia un input:

```text
input changed
    ↓
define checkpoint stale
    ↓
/plan bloqueado
```

### 9. Checkpoint

Tras ejecución correcta:

```text
define checkpoint
├── input fingerprints
├── definition fingerprints
├── skill fingerprint
├── template fingerprints
├── status
└── blocking issues
```

Todavía no se decide dónde vive físicamente ese checkpoint.

### 10. Regla central

**`/define` no genera definitions desde cero. Reconciles the current IASI knowledge state.**

```text
inputs nuevos
+
inputs anteriores
+
definitions existentes
+
ediciones humanas
+
templates
        ↓
       define
        ↓
nuevo estado canónico coherente
```

Con esto queda prácticamente cerrado el contrato conceptual inicial de `command_define`.
