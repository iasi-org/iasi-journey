# ChatGPT — iasi: agentics, instructions e instalación/CLI

**Fecha:** 2026-08-15  
**Hora aproximada de cierre:** 13:58 (Europe/Madrid)

---

## Usuario

vamos a continuar dodne lo hemos dejado antes

## ChatGPT

Sí. Nos habíamos quedado justo aquí:

**`IASI` es el repositorio de la metodología**, y ha llegado el momento de empezar a materializar una de sus piezas centrales:

```text
IASI/
└── agentics/
```

La idea importante era que **`agentics` no pertenece a Codex, Copilot, Claude ni a ningún modelo concreto**. Describe cómo queremos que trabajen los agentes según IASI. Luego vendrán los adaptadores que traduzcan esa definición al mecanismo que soporte cada herramienta.

Habíamos separado mentalmente varias cosas:

```text
IASI
├── commands/       # qué acciones puede pedir el humano
├── skills/         # capacidades reutilizables
├── agentics/       # cómo debe pensar/actuar cada agente
└── mcp/            # herramientas externas disponibles
```

Y precisamente cuando preguntaste:

> «¿Y cuando dices al agente cómo quieres que se comporte?»

ahí apareció **`agentics`**.

Un *agentic* sería, conceptualmente, algo parecido a:

```text
Agentic: reviewer

Responsabilidad
  Revisar una solución antes de considerarla válida.

Debe
  - buscar inconsistencias
  - contrastar con inputs
  - comprobar acceptance tests
  - señalar incertidumbre
  - justificar el resultado

No debe
  - modificar la solución
  - inventar requisitos
  - aprobar si falta información crítica

Salida
  PASS | FAIL

Si FAIL
  explicar qué impide avanzar.
```

Eso es **metodología**, no configuración de Codex.

Después podríamos tener:

```text
agentics/
├── reviewer/
├── implementer/
├── architect/
├── validator/
└── ...
```

Y más adelante:

```text
adapters/
├── codex/
├── copilot/
├── claude/
└── ...
```

que conviertan nuestra definición IASI a `AGENTS.md`, skills, prompts, instrucciones o lo que utilice cada plataforma.

Y aquí estaba el punto interesante: **no empezar creando veinte agentes**. Crear el modelo mínimo de lo que significa un `agentic` en IASI y uno o dos agentes reales para comprobar que la abstracción aguanta.

Yo empezaría ahora por definir **la anatomía formal de un agentic IASI**. Esa es la piedra que falta antes de crear directorios a lo loco.

---

## Usuario

IASI es iasi, error del movil

## ChatGPT

Sí, entendido: **`iasi`**, en minúsculas. Fue solo el autocorrector del móvil.

Así que seguimos con:

```text
iasi/
└── agentics/
```

y toda la nomenclatura del repo/metodología en minúsculas cuando corresponda.

---

## Usuario

instructions! Como debe comportarse el agente

## ChatGPT

Exacto. **`instructions`**. Esa era la palabra que buscábamos.

Si lo ordenamos bien:

```text
iasi/
├── instructions/   # cómo debe comportarse el agente
├── commands/       # qué acciones puede ejecutar
├── skills/         # capacidades reutilizables
└── mcp/            # herramientas/servicios externos
```

`instructions` define cosas como:

- rol y responsabilidad del agente;
- cómo debe razonar y validar;
- qué puede decidir por sí mismo;
- cuándo debe detenerse;
- cómo tratar incertidumbre;
- qué no debe hacer;
- formato esperado de sus resultados;
- relación con otros agentes y con el humano.

Por ejemplo, un agente de validación podría tener unas instrucciones conceptualmente así:

```text
Role: Validator

You validate whether the current work satisfies the available inputs,
requirements and acceptance criteria.

You must:
- verify before approving
- identify missing information
- distinguish facts from assumptions
- explain every blocking issue

You must not:
- invent requirements
- modify the implementation
- approve incomplete work

Result:
PASS | FAIL
```

Y esto conecta perfectamente con lo que hablábamos de Codex: **`iasi` define las instrucciones de forma neutral**, y luego cada adaptador las lleva al mecanismo concreto de Codex, Copilot, Claude, etc.

Así que corregiría lo anterior sin contemplaciones:

```text
agentics/   ❌
instructions/ ✅
```

El comportamiento del agente vive en **instructions**. `Agentic` describe más bien el enfoque o la naturaleza del sistema, no el artefacto que estamos definiendo.

---

## Usuario

asi como normas de escritura: estilo y cosas asi cuando se trate de documentos

## ChatGPT

Exacto. **`instructions` no son solo “cómo razona el agente”**. También son las **normas de comportamiento y producción** que debe respetar según el tipo de trabajo.

Por ejemplo, para documentación:

```text
iasi/
└── instructions/
    ├── general/
    ├── code/
    ├── documentation/
    └── diagrams/
```

Y dentro de `documentation/` podríamos definir cosas como:

- tono y estilo;
- longitud de párrafos;
- estructura de capítulos;
- uso de títulos y subtítulos;
- terminología preferida;
- reglas para ejemplos;
- cómo citar;
- qué evitar;
- tratamiento de notas, advertencias y referencias;
- convenciones para Quarto/Markdown;
- cuándo usar tablas, listas o diagramas;
- incluso reglas editoriales como «no repetir la conclusión tres veces».

Por ejemplo:

```text
instructions/documentation/style.md
```

podría decir conceptualmente:

```text
# Writing style

- Write in clear technical Spanish.
- Prefer short, cohesive paragraphs.
- Avoid unnecessary lists.
- Introduce a concept before using its terminology.
- Use examples only when they clarify the concept.
- Do not use promotional language.
- Keep terminology consistent across documents.
- Prefer explanation over enumeration.
```

Y habría otras instrucciones más funcionales:

```text
instructions/
├── general/
│   ├── behavior.md
│   ├── uncertainty.md
│   └── validation.md
│
├── documentation/
│   ├── style.md
│   ├── structure.md
│   ├── terminology.md
│   └── citations.md
│
├── code/
│   ├── style.md
│   ├── testing.md
│   └── comments.md
│
└── diagrams/
    ├── style.md
    └── conventions.md
```

Y aquí aparece algo importante para **iasi**: las instrucciones pueden ser **componibles**.

Un agente que escribe un capítulo podría recibir:

```text
general/behavior
+ general/validation
+ documentation/style
+ documentation/structure
+ proyecto_actual/instructions
```

Mientras que uno que implementa código recibiría otra combinación.

Así que ya tenemos una definición bastante limpia:

> **Instructions = reglas persistentes que determinan cómo debe comportarse el agente y cómo debe producir su trabajo.**

Y eso incluye perfectamente **normas editoriales, estilo de escritura y convenciones documentales**.

---

## Usuario

pero debemos definir una estructura antes no?

## ChatGPT

Sí. Antes de escribir instrucciones concretas, conviene definir **la estructura de una instruction en iasi**. Si no, acabaremos con una colección de `.md` útiles pero heterogéneos, y luego será difícil componerlos, validarlos o adaptarlos a Codex/Copilot/Claude.

Yo separaría dos niveles:

```text
iasi/
└── instructions/
    ├── README.md
    ├── schema/
    │   └── instruction.md
    └── catalog/
```

`schema/instruction.md` define **qué es una instruction y qué partes puede/debe tener**. `catalog/` contiene las instrucciones reales.

Y para cada instruction empezaría con una anatomía mínima:

```text
# <Instruction name>

## Purpose

Qué pretende conseguir esta instrucción.

## Applies to

En qué contexto aplica:
- general
- documentation
- code
- diagrams
- testing
- etc.

## Rules

Las reglas que debe cumplir el agente.

## Constraints

Lo que no debe hacer.

## Validation

Cómo comprobar que la instrucción se ha cumplido.
```

Por ejemplo, `documentation-style`:

```text
# Documentation style

## Purpose

Establecer el estilo editorial de los documentos iasi.

## Applies to

Documentation.

## Rules

- Escribir en español técnico claro.
- Mantener consistencia terminológica.
- Preferir párrafos cohesionados frente a enumeraciones innecesarias.
- Introducir los conceptos antes de utilizar terminología especializada.

## Constraints

- No utilizar lenguaje comercial.
- No repetir innecesariamente las conclusiones.
- No introducir términos nuevos sin explicación.

## Validation

El documento debe poder revisarse contra estas reglas antes de considerarse terminado.
```

Pero creo que hay una decisión todavía **anterior** que merece fijarse: qué dimensiones forman una instruction.

Yo veo, como mínimo:

```text
instruction
├── identity        # qué es
├── scope           # cuándo aplica
├── rules           # qué debe hacer
├── constraints     # qué no debe hacer
└── validation      # cómo sabemos que se cumplió
```

Y quizá después aparezcan `priority`, `dependencies`, `inputs`, etc., pero **no las metería todavía**.

Primero haría el modelo mínimo y lo probaría con tres casos radicalmente distintos:

```text
general-behavior
documentation-style
code-testing
```

Si los tres caben cómodamente en el mismo esquema, tenemos una abstracción buena.

---

## Usuario

me creas el primer bloque de instructions?
Todas las que consideres, ademas de shcema/instructions.md

## ChatGPT

Se creó un primer bloque de `instructions` con esta estructura:

```text
instructions/
├── README.md
├── schema/
│   └── instructions.md
├── general/
│   ├── behavior.md
│   ├── human-control.md
│   ├── precedence.md
│   ├── tool-use.md
│   ├── uncertainty.md
│   └── validation.md
├── documentation/
│   ├── sources.md
│   ├── structure.md
│   └── style.md
├── code/
│   ├── style.md
│   └── testing.md
└── diagrams/
    └── style.md
```

Se generaron 14 archivos contando `README.md` y el esquema.

La pieza central es `schema/instructions.md`, que fija una estructura común mediante metadata YAML:

```yaml
---
id: documentation.style
version: 0.1.0
status: active
scope: documentation
applies_to:
  - documentation
---
```

y las secciones:

```text
Purpose
Rules
Constraints
Validation
```

También se añadió `general/precedence.md` para definir cómo se componen instrucciones generales, de dominio y específicas de proyecto, y `general/human-control.md` para fijar la frontera entre autonomía del agente y decisiones humanas.

---

## Usuario

como era la carpeta de la que colgaba todo ¿agentics?

## ChatGPT

Sí: **`agentics/`** era la carpeta paraguas. Ahí estaba la confusión de antes: `instructions` **no sustituye** a `agentics`, sino que cuelga de ella.

La estructura conceptual era:

```text
iasi/
└── agentics/
    ├── instructions/   # cómo debe comportarse el agente
    ├── commands/       # acciones/órdenes definidas por la metodología
    ├── skills/         # capacidades reutilizables
    └── mcp/            # integración con servidores/herramientas MCP
```

Así que lo que acabamos de crear realmente debería vivir en:

```text
iasi/agentics/instructions/
```

**`agentics` es el sistema; `instructions` es una de sus piezas.**

---

## Usuario

ahora vamos con el README del repo completo: iasi
Todo README de repo tendra un README y un README_en equivalente
toma como ejemplo el README raiz

## ChatGPT

Se tomó como referencia el README raíz del ecosistema IASI y se definió para el repositorio `iasi` la convención:

```text
README.md       # Castellano
README_en.md    # English
```

El README de `iasi` se centró en el repositorio metodológico, no en volver a describir todo el ecosistema.

Se estructuró alrededor de:

- propósito del repositorio;
- aproximación de IASI;
- `agentics`;
- `instructions`;
- independencia de plataforma;
- relación humano-Sistema Inteligente;
- validación;
- principios;
- estado del repositorio;
- relación con el ecosistema IASI.

Se fijó como idea central:

> **`iasi` define la metodología.**

---

## Usuario

al igual que con openspecs, habria que "instalarlo" en el proyecto ¿no?

## ChatGPT

Sí. **Exactamente igual que OpenSpec en ese sentido**: `iasi` debería existir como metodología central, pero cada proyecto tendría que **instalar una instancia de IASI** para poder trabajar bajo ella.

Separación propuesta:

```text
iasi                         # repositorio de la metodología
│
├── agentics/
│   ├── instructions/
│   ├── commands/
│   ├── skills/
│   └── mcp/
│
└── ...
```

y en un proyecto:

```text
iasi-quarto/
├── .iasi/
│   ├── instructions/
│   ├── commands/
│   ├── skills/
│   └── ...
├── src/
├── tests/
└── ...
```

La instalación debería construir una **proyección de la metodología que necesita ese proyecto**.

Ejemplo:

```text
iasi install
```

podría detectar que `iasi-quarto` es un proyecto de código + documentación y generar una instalación `.iasi/`.

Después podrían existir adaptadores:

```text
iasi adapt codex
iasi adapt copilot
```

que generasen representaciones específicas como:

```text
AGENTS.md
.github/copilot-instructions.md
```

Se fijó la relación conceptual:

```text
                  iasi
             metodología fuente
                    │
                    ▼
              iasi install
                    │
                    ▼
                 .iasi/
          metodología del proyecto
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
     adapt codex         adapt copilot
```

Y la idea:

> **`.iasi/` sería para la metodología lo que `openspec/` es para la especificación.**

OpenSpec persiste **qué queremos construir**.

IASI persistiría **cómo queremos trabajar para construirlo**.

---

## Usuario

me parece bien la idea, pero tambien lo puedo instalar, como en nuestro caso, en un workspace generico, ahi deberia instalar todo ¿no crees?

## ChatGPT

Sí.

En un **workspace genérico**, `iasi install` debería instalar **el conjunto completo de la metodología**, porque todavía no sabemos qué tipos de trabajo van a aparecer ahí.

Se propusieron dos modos:

```text
iasi install --workspace
iasi install --project
```

Workspace:

```text
workspace/
├── .iasi/
│   ├── manifest.yml
│   ├── instructions/
│   │   ├── general/
│   │   ├── documentation/
│   │   ├── code/
│   │   └── diagrams/
│   ├── commands/
│   ├── skills/
│   └── mcp/
│
├── project-a/
├── project-b/
└── project-c/
```

En workspace, `.iasi/` contendría todo lo disponible y activo en la versión instalada de IASI.

Se distinguieron tres conceptos:

```text
available   → existe en la instalación
active      → se aplica en este contexto
adapted     → se ha traducido para Codex/Copilot/etc.
```

También una jerarquía conceptual:

```text
iasi global
    ↓
workspace .iasi
    ↓
project .iasi
    ↓
task-specific instructions
```

Principios fijados:

> **Una instalación de workspace incorpora el catálogo completo de IASI.**

> **Una instalación de proyecto selecciona o activa únicamente la parte de IASI necesaria para ese proyecto.**

> **Los proyectos pueden heredar la instalación IASI de su workspace y especializarla sin duplicarla.**

---

## Usuario

me gusta
Y como vamos a hacer esas herramientas?

## ChatGPT

Se propuso empezar por **una CLI pequeña en Go**:

```text
iasi
```

La CLI debería funcionar sin depender de R, Python, Node o Java y compilar a un único binario:

```text
iasi
iasi.exe
```

Arquitectura inicial:

```text
iasi repo
│
├── agentics/
│   ├── instructions/
│   ├── commands/
│   ├── skills/
│   └── mcp/
│
├── schema/
│
└── cmd/
    └── iasi/
```

Operaciones previstas:

```text
iasi install
iasi status
iasi resolve
iasi validate
iasi adapt
```

Pero se acordó **no construir todo de golpe**.

Primer hito:

```text
iasi install --workspace
iasi status
```

Después:

```text
iasi validate
```

Después:

```text
iasi resolve
```

Y solo entonces:

```text
iasi adapt codex
```

Principio arquitectónico fijado:

> **La CLI `iasi` no contiene la metodología. Interpreta, instala, compone y valida la metodología contenida en el repositorio `iasi`.**

Separación:

```text
Markdown/YAML       Go
metodología         motor
     │                │
     └──────┬─────────┘
            ▼
          .iasi
```

---

## Usuario

me parece bien, se lo daremos a copilot por ahora

## ChatGPT

Se acordó que, por ahora, **Copilot implementa y nosotros diseñamos**.

Primer alcance:

```text
Milestone 1

Implementar la CLI `iasi` en Go.

Comandos iniciales:

iasi install --workspace
iasi status
```

`install --workspace` debe instalar todo el contenido activo de `agentics/` en `.iasi/` y generar `manifest.yml`.

`status` debe localizar la instalación aplicable y mostrar información básica.

No implementar todavía adapters, resolve ni otras sofisticaciones.

---

## Usuario

dame lo que tengo que pasarle a copilots

## ChatGPT

Se preparó una especificación completa para Copilot con:

- objetivo;
- contexto;
- principio arquitectónico;
- tecnología Go;
- comportamiento de `iasi install --workspace`;
- generación de `.iasi/manifest.yml`;
- no sobrescribir instalaciones existentes;
- comportamiento de `iasi status`;
- búsqueda ascendente de `.iasi`;
- conteo de instrucciones, commands, skills y MCP;
- localización aislada de la metodología fuente;
- posible uso de `go:embed`;
- estructura interna;
- CLI;
- compatibilidad Windows/Linux;
- tests;
- criterios de aceptación.

Criterio central:

```text
iasi
  → install
  → .iasi
  → status
```

antes de añadir más capacidades.

---

## Usuario

dentro de iasi, donde metemos el codigo

## ChatGPT

Inicialmente se propuso:

```text
iasi/
├── README.md
├── README_en.md
├── go.mod
│
├── agentics/
│   ├── instructions/
│   ├── commands/
│   ├── skills/
│   └── mcp/
│
├── cmd/
│   └── iasi/
│       └── main.go
│
└── internal/
    ├── install/
    ├── status/
    ├── manifest/
    └── source/
```

con la frontera:

```text
agentics/     → QUÉ define IASI
cmd/          → entrada de la herramienta
internal/     → CÓMO funciona la herramienta
```

---

## Usuario

bueno, pero podria crear src/go/xxxx
Donde xxxx es el nommbre para esta herramienta concreta

## ChatGPT

Sí. Si `iasi` va a contener **metodología + varias herramientas**, se consideró mejor agrupar el software bajo `src/`.

Nombre propuesto para la herramienta concreta:

```text
iasi-cli
```

Estructura:

```text
iasi/
├── README.md
├── README_en.md
│
├── agentics/
│   ├── instructions/
│   ├── commands/
│   ├── skills/
│   └── mcp/
│
└── src/
    └── go/
        └── iasi-cli/
            ├── go.mod
            ├── cmd/
            │   └── iasi/
            │       └── main.go
            └── internal/
                ├── install/
                ├── status/
                ├── manifest/
                └── source/
```

Distinción:

```text
agentics/        metodología
src/             software que soporta la metodología

src/go/iasi-cli  CLI de IASI
```

El repositorio se llama:

```text
iasi
```

La herramienta/producto:

```text
iasi-cli
```

Y el ejecutable para el usuario sigue siendo:

```text
iasi install --workspace
iasi status
```

Esto además deja espacio para futuras herramientas:

```text
src/
├── go/
│   ├── iasi-cli/
│   └── otra-herramienta/
├── python/
└── r/
```

---

## Usuario

bajame el chat
