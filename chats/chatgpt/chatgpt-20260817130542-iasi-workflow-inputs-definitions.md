# ChatGPT — IASI: workflow, inputs, definitions y skills

**Fecha:** 2026-08-17  
**Formato:** transcripción consolidada de la conversación.  
**Nota:** se omiten únicamente mensajes operativos de herramientas y progreso; se conservan las decisiones, razonamientos y artefactos relevantes.

---

## OpenSpec e inputs

**Javier:**  
me hasurgido una pregunta

**ChatGPT:**  
Dispara. ¿Qué pregunta te ha surgido?

**Javier:**  
tenemos nuestros inputs  
Para que necesitamos open specs, al menos por ahora?

**ChatGPT:**  
Ahora mismo, probablemente no lo necesitamos.

Cuando metimos OpenSpec en el dibujo tenía una función concreta: nuestros `inputs/` eran la fuente de conocimiento, y OpenSpec debía actuar después como una especie de compilador:

```text
conocimiento → inputs → OpenSpec → agentes → implementación
```

Pero IASI ha evolucionado. La pregunta correcta es:

> ¿Qué problema nos resuelve OpenSpec que nuestros inputs + agentes no resuelvan ya?

Por ahora, ninguno imprescindible.

Nuestros inputs ya son Markdown, independientes de herramienta, legibles por humanos y agentes, versionables en Git y parte natural del repositorio.

Meter OpenSpec ahora añade:

```text
inputs → OpenSpec → agente
```

cuando podemos tener:

```text
inputs → agente
```

OpenSpec puede recuperar sentido más adelante si aparece un problema real: cientos de inputs, inconsistencias, necesidad fuerte de trazabilidad, cobertura, gestión formal de cambios, etc.

Por ahora sería arquitectura anticipada.

---

## Primer comando: `/validate`

**Javier:**  
vale, eso nos lleva al primer comando /validate (o lo que propongas)  
Mira lo que hay en inputs y dice si hay incoherencias, errores o algo que impida ejecutar la siguiente fase

**ChatGPT:**  
Sí. `/validate` aparece como el primer comando natural del workflow.

Su responsabilidad:

> Leer todo lo que exista en `inputs/` y decidir si el conjunto está en condiciones de pasar a la siguiente fase.

No debe transformar ni arreglar inputs. Solo evaluarlos.

Debe detectar, entre otras cosas:

- errores estructurales;
- contradicciones;
- incoherencias;
- ambigüedades relevantes;
- información ausente que impida continuar;
- suposiciones ocultas;
- restricciones incompatibles;
- referencias inexistentes;
- duplicidades conflictivas;
- decisiones abiertas que deberían estar cerradas para la siguiente fase.

La pregunta no es si los inputs son perfectos, sino:

> ¿Existe información suficiente, coherente y no contradictoria para que la siguiente fase trabaje sin inventar decisiones importantes?

Warnings pueden permitir avanzar. Blockers no.

Regla:

> El agente que valida no completa huecos. Los detecta.

---

## Instructions y gate persistente

**Javier:**  
tiene que usar las instructions  
y si falla, dejar alguna marca para que no se avance

**ChatGPT:**  
Entonces `/validate` valida:

```text
instructions + inputs
```

y debe dejar estado persistente, por ejemplo:

```text
.iasi/validation.json
```

con estado `passed` o `failed`.

Además, la validación debe quedar obsoleta si cambian los inputs o las instructions. Para ello se guardan fingerprints deterministas.

Regla metodológica:

> Ninguna fase puede ejecutarse sobre inputs que no hayan sido validados contra las instructions actuales.

---

## Arquitectura de comandos IASI

Se decidió que la definición canónica de los comandos pertenece a IASI, no a Copilot ni Codex.

```text
iasi/
└── commands/
    └── validate.md
```

Los adapters de plataforma solo proyectan o invocan el comando canónico.

Dirección de dependencia:

```text
platform adapter
      ↓
IASI command
      ↓
IASI workflow
```

Todo lo que define comportamiento IASI vive dentro de `iasi/`:

```text
iasi/
├── commands/
├── skills/
├── instructions/
├── adapters/
└── ...
```

Fuera solo quedan puntos de integración exigidos por la plataforma, por ejemplo `.github/prompts/`.

---

## Herencia compuesta y limpieza arquitectónica

Tras la revisión de Copilot se decidió:

1. La herencia `.iasi` es compuesta padre→hijo.
2. La arquitectura nueva gana al código viejo.
3. `iasi/` es raíz canónica:

```text
iasi/
├── instructions/
├── commands/
├── skills/
├── mcp/
└── adapters/
```

4. Copilot debe evolucionar para soportar commands.
5. `workspace/profile` deja de existir como concepto operativo.
6. Los inputs antiguos no deben quedar como historia normativa activa.
7. `inputs/externals/` se reemplaza entero cuando cambia el contrato normativo.

---

## `/archive`

Se creó `/archive` y se refinó su semántica.

No archiva iteraciones completas. Recibe exactamente un documento:

```text
/archive <documento>
```

Los inputs se organizan en tres ramas:

```text
inputs/
├── externals/
├── internals/
└── obtained/
```

Cada una mantiene su propio histórico:

```text
inputs/
├── externals/
│   └── archived/
├── internals/
│   └── archived/
└── obtained/
    └── archived/
```

Ejemplo:

```text
inputs/internals/labs/graphics/scope.md
```

pasa a:

```text
inputs/internals/archived/scope-YYYYMMDDhhmmss.md
```

El timestamp usa exactamente:

```text
YYYYMMDDhhmmss
```

`/validate` debe ignorar exactamente estos tres subárboles históricos y no cualquier carpeta llamada `archived` en otra profundidad.

---

## `/plan`

El siguiente comando acordado fue `/plan`.

Su función es convertir inputs válidos en trabajo concreto sin implementar todavía.

El resultado del plan pertenece a:

```text
inputs/obtained/
```

porque es conocimiento derivado por IASI.

El plan puede estar formado por uno o varios `.md`. No hay límites rígidos de palabras, páginas o tokens. La separación es semántica.

Regla:

> El plan debe ser el mínimo conjunto coherente de documentos necesario para hacer ejecutable la iteración actual.

Cada iteración puede tener un plan distinto.

Antes de generar el nuevo plan, `/plan` archiva todo el plan anterior como una unidad:

```text
inputs/obtained/archived/
└── plan-YYYYMMDDhhmmss/
    ├── architecture.md
    ├── implementation.md
    └── acceptance.md
```

Se conserva el nombre y estructura interna de los documentos del plan anterior.

---

## Workflow gates

Se estableció que los comandos no pueden ejecutarse libremente.

Máquina de estados:

```text
INPUTS
  ↓
INPUTS_VALIDATED
  ↓
PLANNED
  ↓
PLAN_VALIDATED
  ↓
EXECUTED
  ↓
VERIFIED
```

Flujo:

```text
/validate
    ↓
/plan
    ↓
/validate
    ↓
/execute
    ↓
/verify
```

Si una fase falla, todas las posteriores quedan bloqueadas.

El estado persistente común vive en:

```text
.iasi/workflow.json
```

`/archive` queda fuera de la progresión lineal: modifica contexto e invalida checkpoints dependientes, pero no habilita ninguna fase posterior.

---

## Validación pre-plan y post-plan

Se detectó una trampa importante: el plan anterior en `inputs/obtained/` no debe contaminar la validación previa de una nueva iteración.

Por ello hay dos modos conceptuales de `/validate`:

### Pre-plan

Valida:

```text
inputs/externals
+
inputs/internals
+
instructions
```

Produce:

```text
INPUTS_VALIDATED
```

### Post-plan

Valida:

```text
inputs/externals
+
inputs/internals
+
inputs/obtained
+
instructions
```

Produce:

```text
PLAN_VALIDATED
```

Después:

```text
/execute
  ↓
EXECUTED
  ↓
/verify
  ↓
VERIFIED
```

---

## Runtime compartido y `iasi __runtime`

Copilot señaló una decisión de interfaz pendiente: los prompts agentic necesitan invocar operaciones reales del runtime Go para gates, fingerprints, estado y filesystem, pero los comandos agentic no son subcomandos públicos del CLI.

Se decidió reservar una interfaz interna:

```text
iasi __runtime ...
```

La CLI pública permanece:

```text
iasi install
iasi reinstall
iasi status
iasi version
iasi adapt copilot
```

Flujo:

```text
Copilot /plan
      ↓
.github/prompts/plan.prompt.md
      ↓
iasi/commands/plan.md
      ↓
iasi __runtime ...
      ↓
runtime Go compartido
```

La interfaz `__runtime` no es CLI pública. Es un puente máquina-a-máquina para adapters y comandos IASI.

El documento de decisión se ubicó en:

```text
inputs/internals/runtime-interface.md
```

---

## IASI se construye con IASI

Se fijó como principio de trabajo:

> **IASI se construye con IASI.**

A partir de ahora, el propio desarrollo de IASI debe usar sus inputs, comandos, workflow, validaciones, planificación, ejecución, verificación y archivado.

IASI sirve como caso de uso y prueba continua de sí mismo.

---

# De `inputs/` a `definitions/`

Se retomó una idea del día anterior: el input humano no tiene por qué venir perfectamente estructurado.

IASI debe transformar los inputs a una representación canónica antes de utilizarlos.

Conceptualmente:

```text
input humano
    ↓
interpretación / estructuración
    ↓
representación canónica
```

Se revisó la filosofía de OpenSpec, pero se descartó adoptarlo como dependencia.

Nos quedamos con la filosofía:

- Markdown legible;
- poca ceremonia;
- intención y alcance explícitos;
- requisitos y escenarios solo cuando aportan precisión;
- trazabilidad;
- separar comprensión, planificación y ejecución.

Se descartó `specs/` para no confundirlo con OpenSpec.

Nombre acordado por ahora:

```text
definitions/
```

Semántica:

```text
inputs/       → lo que entra en IASI
definitions/  → lo que IASI ha entendido y estructurado
```

---

## `/define`

Se propuso un nuevo comando:

```text
/define
```

Responsabilidad:

> Interpretar los inputs activos y producir su representación estructurada y canónica en Markdown, sin añadir decisiones que los inputs no soporten.

No es una transformación 1:1.

Varios inputs pueden consolidarse en una definition:

```text
inputs/a.md ─┐
inputs/b.md ─┼──→ definitions/authentication.md
inputs/c.md ─┘
```

Un solo input puede producir varias definitions:

```text
inputs/request.md
      ├──→ definitions/intent.md
      └──→ definitions/scope.md
```

Las definitions deben conservar trazabilidad hacia sus fuentes.

`/define` no resuelve decisiones ausentes. Puede reformular, ordenar, consolidar y hacer explícito lo inequívoco, pero no inventar.

Si encuentra una incógnita, la representa como pregunta abierta y `/validate` decide si es bloqueante.

---

## Quién genera las definitions

Decisión:

> Las definitions las genera el agente, no el runtime Go.

El agente realiza:

```text
1. DISCOVER
2. UNDERSTAND
3. RELATE
4. CONSOLIDATE
5. STRUCTURE
6. TRACE
7. WRITE
```

El runtime Go se ocupa de:

```text
discovery
fingerprints
gates
archivado
atomicidad
filesystem
```

Separación conceptual:

```text
                   /define
                      │
        ┌─────────────┴─────────────┐
        │                           │
     AGENTE                     GO RUNTIME
        │                           │
 comprensión semántica         mecánica segura
 consolidación                 discovery
 estructuración                fingerprints
 redacción                     archived
 relaciones                    atomic writes
        │                           │
        └─────────────┬─────────────┘
                      ↓
                definitions/
```

Regla:

> **El agente decide el contenido. El runtime controla el terreno.**

---

## Skill IASI para `define`

Se concluyó que `/define` encaja especialmente bien como **command + skill + runtime**.

Estructura propuesta:

```text
iasi/
├── commands/
│   └── define.md
└── skills/
    └── define/
        └── SKILL.md
```

Separación:

```text
COMMAND
qué ocurre dentro del workflow

SKILL
cómo realiza el agente la tarea cognitiva
```

El skill `define` describe cómo:

- comprender;
- relacionar;
- consolidar;
- separar semánticamente;
- explicitar alcance;
- detectar incógnitas;
- mantener trazabilidad;
- no inventar decisiones.

El runtime Go controla:

- discovery;
- fingerprints;
- gates;
- archivado;
- atomicidad;
- filesystem.

Modelo final de esta pieza:

```text
/define
   ↓
workflow command
   ↓
define skill        ← inteligencia
   +
IASI runtime        ← control
   ↓
definitions/
```

---

# Estado conceptual al cierre

Estructura de inputs:

```text
inputs/
├── externals/
│   └── archived/
├── internals/
│   └── archived/
└── obtained/
    └── archived/

definitions/
```

Comandos discutidos:

```text
/define
/validate
/plan
/execute
/verify
/archive
```

Workflow conceptual actualizado:

```text
inputs
  ↓
/define
  ↓
definitions
  ↓
/validate
  ↓
/plan
  ↓
inputs/obtained
  ↓
/define
  ↓
definitions
  ↓
/validate
  ↓
/execute
  ↓
/verify
```

Arquitectura de ejecución:

```text
IASI commands
      ↓
IASI skills        ← razonamiento especializado
      +
iasi __runtime     ← runtime Go compartido
      ↓
estado / filesystem / gates
```

Principio de desarrollo:

> **IASI se construye con IASI.**

