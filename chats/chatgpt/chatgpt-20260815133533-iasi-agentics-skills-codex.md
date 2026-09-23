# IASI Agentics, Skills y Codex

**Fecha:** 2026-08-15  
**Tema:** Codex, skills, agentics e inicio de `IASI/agentics`

---

## Conversación

**Usuario:**  
Oye, codex soporta skills y esas cosas?

**ChatGPT:**  
Sí. 😄 Y esto nos encaja **demasiado bien** con lo que estábamos planteando para IASI.

Codex soporta **Skills**, además de **MCP**, herramientas y otras capacidades agentic. La documentación actual de OpenAI muestra explícitamente soporte para Skills, y OpenAI incluso publica ejemplos de uso de skills con Codex.

La distinción que estábamos buscando quedaría bastante limpia:

- **Comando** → una operación/intención que queremos ofrecer al agente: `validate`, `build`, `publish`, `diagram`...
- **Skill** → conocimiento procedural reutilizable: *cómo validar un proyecto IASI*, *cómo generar un ADR*, *cómo preparar una release*, etc.
- **Tool** → capacidad ejecutable concreta.
- **Servidor MCP** → expone herramientas/recursos mediante el protocolo MCP.
- **Agente/modelo** → Codex, Copilot, ChatGPT, Ollama... consume esas capacidades según lo que soporte.

Y aquí aparece una consecuencia bastante bonita para lo que hablábamos ayer:

**las Skills de IASI no tienen por qué ser "skills de Codex".**

Podemos definir conceptualmente una **IASI Skill** y luego tener adaptadores/representaciones para Codex, Copilot u otros agentes. Codex sería simplemente **uno de los runtimes que sabe consumirlas**.

Eso refuerza muchísimo la idea que estabas defendiendo: **la metodología no debe estar vinculada al modelo**.

Y, de hecho, ya tenemos una clasificación bastante seria para esa futura zona de `iasi-home`:

**Productos → Extensiones · Herramientas · Skills · Servidores MCP**

Creo que acabamos de encontrar dónde encajan las *skills* en el ecosistema. 🦉

---

**Usuario:**  
O sea, propones skills iasi y un adaptador a codex

**ChatGPT:**  
Sí. Esa es exactamente la idea que propondría. 😄

No definiría una “skill de Codex” como fuente de verdad. Definiría una **skill IASI** como concepto propio de la metodología, y después un adaptador que la traduzca al formato que Codex necesite.

Así tendríamos algo parecido a:

`IASI Skill → adapter Codex → Codex`

y mañana:

`IASI Skill → adapter Copilot → Copilot`

`IASI Skill → adapter Claude → Claude`

La gracia está en que **la intención, reglas, entradas, salidas y criterios de aceptación de la skill pertenecen a IASI**. El adaptador solo resuelve la fontanería específica del consumidor.

Por ejemplo, una skill conceptual:

`iasi.validate-project`

podría definir: qué valida, qué información necesita, cuándo debe detenerse, qué produce y qué condiciones significan “válido”.

Luego `adapters/codex` genera o coloca aquello que Codex espera para poder utilizarla.

Pero hay un matiz importante: **no haría el adaptador hasta demostrar que hace falta**. Si podemos diseñar el formato nativo de IASI suficientemente cercano o incluso compatible con el de Codex, mejor. Menos capas, menos duendes en la tubería.

La arquitectura que me gusta sería:

```text
skills/
  validate-project/
    skill.md
    tests/
    examples/

adapters/
  codex/
  copilot/
```

Y ahí aparece algo muy IASI: **la skill es producto; Codex es una plataforma de ejecución**.

Eso además evita que dentro de dos años nuestro repositorio parezca un museo arqueológico de herramientas de IA extintas. 🦉

---

**Usuario:**  
Antes me has descrito los agentic  
Vuelve a decirmelo

**ChatGPT:**  
Sí. Lo que estábamos separando era el **ecosistema agentic**, no meterlo todo en el saco de “MCP”.

La foto conceptual era esta:

- **Agente** → quien razona y decide qué hacer. Codex, Copilot, Claude Code, etc.
- **Comandos** → acciones explícitas que puede solicitar el humano, por ejemplo `/validate`, `/build`, `/review`.
- **Skills** → conocimiento procedural reutilizable. Describen **cómo hacer bien una tarea**.
- **Tools** → capacidades ejecutables que el agente puede invocar: ejecutar un script, consultar algo, generar un diagrama...
- **MCP** → el **protocolo** que permite conectar el agente con capacidades externas.
- **Servidor MCP** → el software concreto que implementa MCP y expone **tools, resources y prompts** al agente.
- **Resources** → información/contexto que un servidor MCP pone a disposición del agente.
- **Prompts** → plantillas/instrucciones reutilizables que también puede exponer MCP.
- **Hooks** → acciones automáticas asociadas a determinados momentos del ciclo agentic, cuando la plataforma los soporta.
- **Subagentes** → agentes especializados a los que el agente principal puede delegar una parte del trabajo.

Y hay una separación especialmente importante para IASI:

> **Skill = sabe cómo hacerlo.**  
> **Tool = puede hacerlo.**  
> **MCP = sabe cómo conectarse con quien puede hacerlo.**

Ejemplo con `iasi-graphics`: una **skill** sabe cómo debe construirse un diagrama IASI; una **tool** puede ejecutar PlantUML; un **servidor MCP PlantUML** expone esa capacidad; y **Codex** decide cuándo utilizarla.

Por eso ayer fue importante corregir “nuestros MCP” por **“nuestros servidores MCP”**. MCP es solo el protocolo. 🦉

Y ahora la idea de **IASI Skills + adapters** empieza a completar bastante bien ese mapa.

---

**Usuario:**  
Y cuando dices al agente como quieres que se comporte?

**ChatGPT:**  
Eso es otra pieza distinta: **las instrucciones del agente**.

Son las que definen **cómo quieres que el agente se comporte de forma general**, no cómo ejecutar una tarea concreta.

La separación sería:

- **Instructions** → *cómo debes comportarte.*
- **Skill** → *cómo se hace esta clase de tarea.*
- **Command** → *haz esta tarea ahora.*
- **Tool** → *capacidad que puedes utilizar para hacerla.*
- **MCP server** → *mecanismo para acceder a capacidades externas.*
- **Agent/Subagent** → *quién razona y ejecuta.*

Por ejemplo:

```text
INSTRUCTION
"No modifiques código sin ejecutar antes los tests.
Si falta información, no inventes.
Las decisiones arquitectónicas deben quedar documentadas."

SKILL
"Cómo crear un ADR siguiendo la metodología IASI."

COMMAND
/create-adr

TOOL
write_file()

MCP SERVER
iasi-adr
```

Y aquí veo una pieza importante para IASI: **las instrucciones también deberían ser independientes del agente**.

Es decir, conceptualmente:

```text
              IASI
                │
       ┌────────┼─────────┐
       │        │         │
 Instructions  Skills   Commands
       │        │         │
       └────────┼─────────┘
                ↓
             Adapter
                ↓
       Codex / Copilot / ...
```

Porque si escribimos directamente `AGENTS.md`, `.github/copilot-instructions.md`, etc., estamos otra vez acoplando **la metodología a cada producto**.

IASI debería decir **cómo queremos que trabaje un agente**. El adaptador debería encargarse de explicárselo a Codex en su dialecto particular.

Y eso, curiosamente, empieza a parecerse bastante al **compilador de metodología** que llevamos días descubriendo. 😏🦉

---

**Usuario:**  
Tengo un repo vacío: IASI  
Ese está pensado para los resultados de la metodología  
Creo que es el momento de crear IASI/agentics

**ChatGPT:**  
Sí. **Ahora sí tiene sentido.**

Y lo llamaría exactamente:

```text
IASI/
└── agentics/
```

Ese directorio sería la **fuente de verdad agentic de la metodología**, no una carpeta específica de Codex, Copilot o Claude.

Yo arrancaría mínimo así:

```text
IASI/
└── agentics/
    ├── instructions/
    ├── skills/
    ├── commands/
    ├── agents/
    └── adapters/
```

Y dejaría `tools/` y `mcp/` fuera por ahora, porque son capacidades de ejecución/productos, mientras que aquí estamos describiendo **cómo trabaja el sistema agentic**.

La semántica quedaría muy limpia:

```text
instructions/   cómo deben comportarse los agentes
skills/         cómo realizar tipos de tareas
commands/       acciones que IASI define
agents/         roles/agentes especializados
adapters/       traducción hacia Codex, Copilot, Claude...
```

Por ejemplo, más adelante:

```text
agentics/
├── instructions/
│   ├── engineering.md
│   ├── decisions.md
│   └── validation.md
│
├── skills/
│   ├── create-adr/
│   ├── validate-project/
│   └── reverse-engineer/
│
├── commands/
│   ├── validate.md
│   ├── implement.md
│   └── archive.md
│
├── agents/
│   ├── architect.md
│   ├── reviewer.md
│   └── researcher.md
│
└── adapters/
    ├── codex/
    ├── copilot/
    └── claude/
```

Y esto conecta directamente con lo que descubrimos con `inputs`, `obtained`, `internals`, validación y aceptación: **ahí estábamos diseñando el proceso; `agentics/` empieza a convertir ese proceso en algo ejecutable por agentes.**

Hay además una frontera que me gusta mucho:

> `IASI/agentics` **no contiene configuración de Codex. Contiene IASI.**

Después:

```text
IASI agentics
      ↓
   adapter
      ↓
 AGENTS.md
 Codex skills
 Copilot instructions
 Claude config
 ...
```

Eso convierte el repo `IASI` en lo que originalmente querías que fuese: **el resultado material de la metodología**, mientras los volúmenes explican por qué hemos llegado ahí y cómo. 🦉

Yo no crearía veinte archivos todavía. Crearía `agentics/` y empezaría por **`instructions/`**, porque ya tenemos un montón de principios definidos que pueden convertirse en instrucciones agentic reales.

---

**Usuario:**  
puedes descargar ese chat?

**ChatGPT:**  
Sí. ¿Quieres que descargue **esta conversación completa hasta este mensaje** en Markdown, usando nuestro patrón `chatgpt-YYYYMMDDhhmmss-titulo`?

---

**Usuario:**  
si
