# Conversación — IASI Graphics, Codex y Fase 1

**Fecha:** 13 de agosto de 2026

---

## Javier

bien, ya hemos puesto codex en vscode y tenemos go

## ChatGPT

Perfecto. Entonces ya tenemos las dos piezas que faltaban: **Codex dentro de VS Code** y **Go instalado**. 🧩

Para `iasi-graphics`, el siguiente movimiento es comprobar primero que Go responde:

```powershell
go version
```

Y ya dentro del repo:

```powershell
go test ./...
go build ./cmd/iasi-graphics
```

Si eso pasa, dejamos de preparar herramientas y **ponemos a Codex a trabajar de verdad**: que lea `README.md`, `13-codex-instructions.md` y las specs, haga Phase 0/1 y ataque el primer vertical slice:

```text
examples/flow.ig
        ↓
iasi-graphics render
        ↓
flow.svg
```

Ahí empieza el experimento interesante: nosotros especificamos, Codex implementa y los tests deciden quién tiene razón. 😈

---

## Javier

que coño va a construir en un directorio vacio atontá?

## ChatGPT

🤣 Tienes razón. Me he saltado alegremente **el pequeño detalle de que el directorio está vacío**.

Ahora mismo Codex no tiene nada que leer, compilar ni testear. **Estamos en el minuto cero.**

Si ya estás situado en el directorio que vamos a usar, empezamos por:

```powershell
git init
go mod init github.com/iasi/iasi-graphics
```

Eso nos deja únicamente:

```text
iasi-graphics/
└── go.mod
```

Y **ahí sí** entra Codex. Pero no para «continuar» nada, sino para **crear el proyecto desde cero con nosotros**.

Yo haría ahora exactamente esto: primero definimos qué demonios es `iasi-graphics`, su objetivo y el primer comportamiento observable. Después se lo damos a Codex para que genere el esqueleto inicial. Nada de inventarnos `README`, `13-codex-instructions.md`, tests y unicornios que todavía no existen. 😂

---

## Javier

el directorio ahora solo tiene inputs, tus documentos

## ChatGPT

Exacto. Entonces el punto de partida real es este:

```text
iasi-graphics/
└── inputs/
    └── ...mis documentos
```

Eso **sí cambia completamente la jugada**. No hay código, pero Codex **sí tiene contexto de entrada**. Esos documentos son precisamente la materia prima para que entienda qué queremos construir.

Yo no crearía todavía ni `go.mod`, ni estructura, ni nombres de paquetes a mano. Primero pondría a Codex en **modo arquitecto**, no en modo picapedrero:

> Lee íntegramente todo lo que hay en `inputs/`.  
> No escribas ni modifiques ningún archivo todavía.  
> A partir exclusivamente de esos documentos, dime:
> 1. qué producto entiendes que hay que construir;
> 2. cuáles son sus capacidades principales;
> 3. cuál propones como arquitectura inicial en Go;
> 4. cuál sería el mínimo vertical slice ejecutable;
> 5. qué estructura inicial de directorios y paquetes propones;
> 6. qué decisiones quedan ambiguas o no están especificadas.
>
> No implementes nada hasta haber presentado este análisis.

Y vemos **qué ha entendido Codex de nuestros documentos sin soplarle nosotros la respuesta**.

Eso además es parte del experimento IASI: si nuestra especificación vale algo, otro agente debería ser capaz de reconstruir la intención leyendo `inputs/`. Si Codex sale diciendo que vamos a fabricar una tostadora blockchain, sabemos dónde está el agujero. 😂

---

## Javier

se lo paso

## ChatGPT

Sí. **Pásaselo tal cual.** 👍

Y no le ayudes más de momento. Queremos ver qué reconstruye **solo a partir de `inputs/`**.

Cuando responda Codex, tráeme su análisis y lo destripamos: qué ha entendido bien, qué ha supuesto y dónde nuestros documentos hacen agua. 😈

---

## Javier

pero mejor desde la app

## ChatGPT

Sí. **Para este primer paso, mejor desde la app de Codex.**

La app está pensada justo para trabajar sobre una **carpeta local completa**, con archivos, terminal y herramientas del proyecto, y mantener el trabajo organizado por proyecto/hilo.

Yo haría esto:

1. Abre **Codex**.
2. Selecciona como proyecto la carpeta `iasi-graphics`.
3. Codex verá `inputs/`.
4. Le pegas el prompt que acabamos de preparar.
5. **No le damos permiso todavía para construir nada**, queremos primero su lectura del problema.

VS Code lo dejamos abierto para después, cuando empecemos a inspeccionar y tocar el código. Para esta fase de “toma estos documentos y dime qué coño entiendes”, la app es más limpia.

Y tiene gracia: estamos probando literalmente nuestra tesis. **Carpeta vacía de implementación + conocimiento persistido en `inputs/` → agente nuevo intenta reconstruir el sistema.** 🦉

---

## Javier

la respuesta

## Codex — análisis inicial

# Análisis inicial de `iasi-graphics`

Este análisis se basa exclusivamente en los documentos incluidos en `inputs/` y se realizó antes de iniciar la implementación.

## 1. Producto que hay que construir

`iasi-graphics` es un compilador de gráficos conceptuales declarativos.

El usuario escribe una descripción semántica breve en un archivo `.ig` o, posteriormente, en un bloque Quarto `{iasi-graphics}`. El programa interpreta esa descripción, compone automáticamente la figura y genera un SVG con calidad editorial o de presentación.

La promesa esencial es:

> El autor describe qué significa la figura; el motor decide cómo representarla visualmente.

No es un editor vectorial, una biblioteca de dibujo, un lenguaje SVG, una herramienta UML, un sistema de gráficos estadísticos ni un sustituto de PlantUML.

El producto debe ser:

- Un ejecutable nativo escrito en Go.
- Reproducible y determinista.
- Utilizable completamente offline.
- Independiente de JavaScript, Node.js, navegador, PowerPoint y servicios web.
- Capaz de producir gráficos presentacionales, no simples diagramas técnicos con cajas.

El MVP completo v0.1 incluye tres tipos de composición: `flow`, `compare` y `ecosystem`, además de una integración Quarto posterior al compilador standalone.

## 2. Capacidades principales

### Lenguaje declarativo

El DSL `.ig` permite expresar:

- Un único gráfico por documento.
- Títulos.
- Textos breves.
- Iconos opcionales de un registro incorporado.
- Mensajes destacados mediante `highlight`.
- Identificadores locales únicos.
- Comentarios con `#`.
- Strings UTF-8 con los escapes `\"`, `\\` y `\n`.

No permite coordenadas, dimensiones, colores, CSS ni primitivas gráficas.

### Tipos de gráfico

#### `flow`

- Secuencia ordenada de pasos.
- Grupos paralelos mediante `split`.
- Reconvergencia visual después de un split.
- Mensaje destacado.
- Pasos con etiqueta, textos e icono.

#### `compare`

- Exactamente dos lados.
- Peso visual equilibrado.
- Textos e iconos por lado.
- Cero o un `highlight`.

#### `ecosystem`

- Exactamente un centro.
- Entre tres y ocho elementos periféricos.
- Distribución equilibrada alrededor del centro.
- Apariencia de ecosistema o constelación, no de grafo genérico.

### Compilación y salida

Pipeline previsto:

```text
source
  → lexer
  → parser
  → AST
  → validación y normalización semántica
  → modelo semántico
  → layout específico
  → escena con geometría
  → renderer SVG
```

El resultado debe proporcionar:

- SVG válido y escalable mediante `viewBox`.
- Diseño interno inicial de `1600 × 900`.
- Texto real y seleccionable.
- `<title>` y, cuando sea viable, `<desc>`.
- Ausencia de scripts y recursos de red.
- IDs internos estables.
- Resultado determinista.
- Iconos vectoriales incorporados localmente.
- Una única tematización inicial: `iasi`.

### Interfaz de línea de comandos

Interfaz principal:

```bash
iasi-graphics render input.ig
iasi-graphics render input.ig -o output.svg
```

También debe aceptar stdin:

```bash
iasi-graphics render - -o output.svg
```

Los errores deben ir a stderr, incluir diagnóstico útil con archivo, línea y columna y producir un código de salida distinto de cero.

`version` y `validate` son deseables, pero secundarios frente a `render`.

### Quarto

Después de estabilizar el compilador standalone:

- Un filtro Lua detectará bloques `{iasi-graphics}`.
- Invocará el mismo ejecutable.
- Generará un SVG en una ubicación de build determinista.
- Sustituirá el bloque por una figura Pandoc/Quarto.
- Dará prioridad a `fig-cap`, `fig-alt` y `label`.

El adaptador no duplicará el parser, la validación, el layout ni el renderer.

## 3. Arquitectura inicial propuesta en Go

Mantendría las etapas especificadas, pero evitaría una fragmentación excesiva durante el primer slice:

```text
CLI
  → compiler
      → source
      → lexer/parser → AST
      → semantic → Graphic
      → layout/flow → Scene
      → svg → []byte
```

Propondría un pequeño paquete orquestador interno:

```go
func Compile(name string, source []byte, options Options) ([]byte, error)
```

No sería necesariamente una API pública en v0.1. Su función sería conectar etapas sin introducir decisiones semánticas propias.

Responsabilidades:

- `source`: nombre, contenido normalizado, posiciones y líneas para diagnósticos.
- `lexer`: tokens con rangos de origen.
- `parser`: AST fiel a la sintaxis.
- `ast`: estructuras sintácticas y posiciones.
- `semantic`: validación, resolución de iconos y normalización.
- `model`: modelo semántico sin geometría.
- `layout`: selección de estrategia por tipo de gráfico.
- `layout/flow`, `layout/compare`, `layout/ecosystem`: algoritmos independientes.
- `textmeasure`: medición heurística y wrapping deterministas mediante una interfaz aislada.
- `scene`: primitivas renderer-neutral con geometría.
- `theme`: tokens visuales resueltos del tema `iasi`.
- `icons`: registro pequeño de iconos y sus datos vectoriales locales.
- `render/svg`: serialización determinista de la escena.
- `diagnostic`: errores comunes con origen, línea, columna y mensaje.
- `compiler`: coordinación pura del pipeline.
- `cmd/iasi-graphics`: lectura de argumentos, archivos/stdin, escritura y códigos de salida.

Evitaría deliberadamente:

- Un motor universal de grafos.
- Interfaces abstractas para cada etapa antes de necesitarlas.
- Exponer paquetes públicos prematuramente.
- Hacer que el renderer conozca el AST.
- Hacer que el layout vuelva a validar estructuras inválidas.
- Incorporar una dependencia SVG si `encoding/xml` y escritura controlada son suficientes.

## 4. Mínimo vertical slice ejecutable

El primer slice debería ser un `flow` completo de referencia, no solamente un parser que “ya compila”.

Entrada:

```ig
flow "Del diálogo al artefacto" {
  step conversation "Conversación" {
    text "Exploración y discusión"
  }

  split {
    step journey "Journey" {
      text "Cómo llegamos"
    }

    step edr "EDR" {
      text "Qué decidimos"
    }
  }

  step knowledge "Conocimiento"
  step artifacts "Artefactos"

  highlight "La experiencia se convierte en conocimiento reutilizable"
}
```

Ejecución:

```bash
go run ./cmd/iasi-graphics render examples/flow.ig -o flow.svg
```

Debe atravesar realmente:

```text
archivo → lexer → parser → AST → validación → modelo semántico
        → FlowLayout → Scene → SVG → archivo
```

Este slice mínimo debe incluir:

- Lectura desde archivo.
- Tokens, strings, identificadores, comentarios y bloques.
- Parseo de `flow`, `step`, `split`, `text`, `icon` y `highlight`.
- Posiciones de origen.
- IDs duplicados y split con menos de dos pasos.
- Validación de iconos si aparecen.
- Layout vertical de pasos.
- Fila equilibrada para el split y reconvergencia.
- Título, tarjetas, conectores, texto y highlight.
- Canvas `1600 × 900` mediante `viewBox`.
- Theme `iasi` inicial.
- SVG determinista, accesible y sin recursos externos.
- Al menos pruebas de parser, validación, invariantes geométricas y validez XML.
- Inspección visual manual del SVG resultante.

No incluiría todavía `compare`, `ecosystem`, stdin, Quarto, empaquetado multiplataforma ni un sistema extensible de temas. Los iconos pueden limitarse a los necesarios para el ejemplo, aunque el registro debe quedar preparado para rechazar nombres desconocidos.

## 5. Estructura inicial propuesta

```text
iasi-graphics/
├── go.mod
├── cmd/
│   └── iasi-graphics/
│       └── main.go
├── internal/
│   ├── compiler/
│   ├── source/
│   ├── diagnostic/
│   ├── lexer/
│   ├── parser/
│   ├── ast/
│   ├── semantic/
│   ├── model/
│   ├── textmeasure/
│   ├── layout/
│   │   ├── layout.go
│   │   ├── flow/
│   │   ├── compare/
│   │   └── ecosystem/
│   ├── scene/
│   ├── theme/
│   ├── icons/
│   └── render/
│       └── svg/
├── examples/
│   ├── flow.ig
│   ├── compare.ig
│   └── ecosystem.ig
├── testdata/
│   ├── flow-basic.ig
│   ├── compare-basic.ig
│   ├── ecosystem-basic.ig
│   └── invalid/
├── docs/
│   └── implementation-notes/
└── quarto/
    └── _extensions/
        └── iasi-graphics/
```

Al principio solo crearía las carpetas y paquetes necesarios para el slice de `flow`. `compare`, `ecosystem` y `quarto` pueden añadirse cuando se alcancen sus fases; no necesitan paquetes vacíos.

## 6. Decisiones ambiguas o no especificadas

### DSL y validación

- No se establece si un `flow` debe contener un mínimo o máximo de pasos.
- No se fija cuántos `highlight` admite un `flow`; la gramática permite varios.
- No se define si `highlight` debe aparecer obligatoriamente al final.
- No se especifica si `center`, los dos `side` y los `item` deben respetar estrictamente el orden de la EBNF o si el parser acepta cualquier orden y la fase semántica valida cardinalidades.
- No se determina si un contenedor puede contener varios `icon`; el bloque común permite repetirlos conceptualmente, pero el modelo sugiere un solo icono.
- No se fijan límites concretos de longitud para título, etiqueta, texto o highlight.
- “Densidad excesiva” puede producir rechazo o advertencia, pero no se definen umbrales ni un mecanismo formal de warnings.
- No se aclara qué posiciones deben asociarse a errores semánticos compuestos.
- No se define si deben rechazarse tokens o contenido después del único gráfico raíz, aunque “exactamente uno” sugiere que sí.

### Modelo semántico y escena

- El esquema exacto del modelo semántico no es normativo.
- No se decide si usar un modelo normalizado común o tipos separados para cada layout.
- No se define formalmente la representación de relaciones, grupos y splits.
- Las primitivas y el orden de pintado de la escena no están cerrados.
- No se fija el contrato exacto entre theme, layout y renderer.
- No se especifica cómo representar clipping, sombras, bordes o conectores curvos.

### Diseño visual

- No están definidos los colores, tipografía, tamaños, espaciados, radios ni demás tokens concretos del tema `iasi`.
- La familia tipográfica final queda abierta.
- No existe una definición objetiva de “presentation-quality”; requiere revisión visual humana.
- No se concretan dimensiones de tarjetas, márgenes, áreas de título o highlight.
- No se define la heurística de medición y wrapping.
- No se decide cuándo el contenido debe ajustarse, envolverse o rechazarse.
- No se especifican reglas exactas para distribuir de tres a ocho elementos en `ecosystem`.
- No se define la geometría exacta de ramificación y reconvergencia de `split`.
- El conjunto inicial y el diseño vectorial de iconos no son definitivos; tampoco se especifican su procedencia o licencia.

### SVG

- No se fija si el root debe llevar `width` y `height` además de `viewBox`.
- No se define el contenido exacto de `<desc>`.
- No se establece una política concreta de escaping, precisión decimal o normalización del XML.
- “Byte-for-byte after normalization” no define qué normalización se aplicará.
- No se concreta el esquema para generar IDs internos estables.
- Los filtros visuales son opcionales, pero no hay criterio para decidir si usarlos.

### CLI

- No se especifica el path del módulo de Go.
- No se define el comportamiento si el archivo de salida ya existe.
- No se establece si `-o -` debe escribir el SVG en stdout.
- No se precisa cómo derivar el nombre de salida para stdin cuando falta `-o`.
- No se define si las extensiones distintas de `.ig` se rechazan.
- No se fija el formato estable de errores ni códigos de salida diferenciados.
- No se concreta la semántica de `validate` ni el formato de `version`.
- No queda cerrado si `--theme iasi` se incluye o se pospone.

### Quarto

- No está definida la estructura exacta de la extensión.
- No se decide la ubicación concreta de los SVG generados.
- No se fija el algoritmo de nombres o hashes.
- No se define cómo localizar el ejecutable.
- No se concreta cómo extraer las opciones `#|`.
- No se especifica el tratamiento de caché, limpieza o ejecuciones concurrentes.
- No se decide la relación final con `iasi-quarto` o `iasi-lua`.
- Tampoco se define el comportamiento exacto en HTML frente a formatos PDF cuando Quarto necesite convertir el SVG.

### Alcance y evolución

- El nombre del producto es provisionalmente fijo, pero no el nombre comercial del DSL.
- Temas personalizados, formatos alternativos, nuevos layouts e IA están explícitamente aplazados.
- No se define todavía una API Go pública para uso embebido.
- No se fija la estrategia de releases ni plataformas obligatorias.
- El documento de pruebas dice “cuatro niveles”, pero enumera cinco; la intención evidente es incluir el adaptador Quarto como quinto nivel posterior.

## Recomendación

Durante el primer slice deberían resolverse únicamente las ambigüedades locales necesarias, usando la opción más pequeña y reversible, y documentarlas en `docs/implementation-notes/`.

Las cuestiones que cambien el DSL público, el contrato del renderer, la integración Quarto o los criterios de aceptación deberían volver antes a la capa de especificación.

---

## ChatGPT

Sí. **Esto es exactamente lo que queríamos comprobar.** Y el resultado es bastante revelador.

Codex no se ha limitado a resumir los documentos. Ha reconstruido **el producto, sus fronteras, la arquitectura, el orden de construcción y, sobre todo, lo que falta por decidir**.

Lo más importante para mí es esto:

- Ha entendido la esencia sin que se la expliquemos: **“compilador de gráficos conceptuales declarativos”** y la separación entre significado y representación visual.
- Ha reconstruido correctamente el pipeline completo: `source → lexer → parser → AST → semantic → layout → scene → SVG`.
- No propone empezar haciendo piezas sueltas. Propone **un vertical slice real y ejecutable**, desde `.ig` hasta SVG.
- Y ha entendido una cosa arquitectónica muy buena: **no crear todavía los paquetes vacíos de compare/ecosystem/quarto**, sino únicamente lo que necesita el primer slice.

Pero para mí **la parte más valiosa empieza en la línea 284**.

Ha encontrado una cantidad considerable de agujeros reales de especificación sin inventarse respuestas: cardinalidad de `highlight`, orden de elementos, modelo semántico, contrato theme/layout/renderer, wrapping, geometría de splits, IDs SVG, política de overwrite, stdout, Quarto, etc.

Y termina diciendo:

> las cuestiones que cambien el DSL público, el contrato del renderer, Quarto o los acceptance criteria deben volver a especificación.

**Bingo.**

Esto conecta directamente con lo que hablábamos esta tarde sobre los acceptance tests.

No necesitamos que nuestra especificación determine el radio de cada esquina antes de escribir una línea de Go. Pero sí necesitamos distinguir:

**1. Ambigüedad local de implementación**  
Puede resolverla Codex con la opción más pequeña y reversible.

**2. Decisión arquitectónica**  
Debe quedar persistida.

**3. Contrato observable del producto**  
Debe convertirse en especificación + acceptance test.

Ahí está apareciendo, casi sin buscarlo, el workflow.

Y además ha ocurrido algo todavía más interesante: **el propio agente ha descubierto dónde termina su autonomía.**

No ha dicho «me falta información, dime qué hago» ante 70 detalles. Ha dicho básicamente:

> puedo construir hasta aquí; estas decisiones las puedo tomar provisionalmente; estas otras afectan al contrato y deben escalarse.

Eso es muchísimo más importante que si sabe escribir un parser en Go.

### Yo ahora NO le diría que implemente todavía

Haría una pequeña prueba más antes.

Le diría algo como:

```text
Muy bien.

Ahora clasifica todas las ambigüedades que has encontrado en tres categorías:

A. Implementation detail
   Puedes decidirla durante la implementación utilizando la opción
   más simple, determinista y reversible.

B. Architecture decision
   Debe decidirse y persistirse antes o durante la implementación,
   pero no modifica el contrato público del producto.

C. Specification / acceptance decision
   No debes decidirla unilateralmente porque cambia comportamiento
   observable, DSL, compatibilidad o criterios de aceptación.

Para cada una indica:
- categoría;
- por qué;
- si bloquea o no el primer vertical slice.

No modifiques ningún archivo todavía.
```

Porque entonces estamos probando algo bastante más poderoso que «Codex entiende documentación».

Estamos comprobando si puede **separar decisión de implementación, decisión de arquitectura y decisión de producto**.

Y si hace eso bien, entonces sí: le soltamos la correa y le decimos **construye Phase 1**. 🦉

---

## Javier

[Adjunta la clasificación de ambigüedades realizada por Codex]

## Codex — clasificación de ambigüedades

# Clasificación de ambigüedades de `iasi-graphics`

Este documento clasifica las ambigüedades encontradas en la especificación de `inputs/` antes de iniciar la implementación.

## Categorías

- **A. Implementation detail:** puede decidirse durante la implementación utilizando la opción más simple, determinista y reversible.
- **B. Architecture decision:** debe decidirse y persistirse antes o durante la implementación, pero no modifica el contrato público del producto.
- **C. Specification / acceptance decision:** no debe decidirse unilateralmente porque cambia comportamiento observable, DSL, compatibilidad o criterios de aceptación.

“Bloquea el primer vertical slice” significa que impide completar el recorrido ejecutable `flow.ig → SVG`. Una decisión puede ser importante para el MVP completo sin bloquear ese primer flujo positivo.

## DSL y validación

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Mínimo y máximo de elementos de un `flow` | C | Determina qué fuentes `.ig` son válidas y cambia el contrato observable del DSL. | No. El ejemplo puede ser válido sin decidir los límites generales. |
| Número máximo de `highlight` en `flow` | C | Aceptar varios, rechazar el segundo o aplicar una precedencia cambia la validez y el significado del DSL. | No. El slice puede usar exactamente uno y aplazar los casos múltiples. |
| Posición obligatoria de `highlight` dentro de `flow` | C | Obligar a colocarlo al final afecta compatibilidad sintáctica y comportamiento observable. | No. El ejemplo puede colocarlo al final. |
| Orden estricto o flexible de `center`, `side`, `item` y `highlight` | C | Determina qué documentos acepta el lenguaje. La EBNF sugiere un orden, pero el texto habla también de cardinalidades semánticas. | No para `flow`; tampoco hace falta resolverlo hasta `compare` y `ecosystem`. |
| Permitir varios `icon` dentro de un bloque | C | Cambia la cardinalidad y semántica pública de una construcción del DSL. | No. El slice puede utilizar cero o un icono sin definir el caso múltiple. |
| Límites de longitud para título, etiquetas, textos y highlight | C | Un límite convierte entradas antes válidas en errores o warnings y afecta aceptación. | No para una entrada breve; sí deberá resolverse antes de prometer validación de densidad. |
| Umbrales de “densidad excesiva” y si generan error o warning | C | Afecta códigos de salida, diagnósticos y aceptación de documentos. | No. El primer ejemplo puede mantenerse claramente dentro de una densidad razonable. |
| Posición que debe señalar un error semántico compuesto | A | Es una elección diagnóstica interna mientras siempre se proporcione una ubicación útil y determinista. | No. Se puede escoger el nodo causante o la apertura del contenedor. |
| Tratamiento de tokens después del gráfico raíz | C | Aceptarlos o rechazarlos modifica la gramática observable. “Exactamente un visual” sugiere rechazo, pero conviene fijarlo explícitamente. | No para el camino válido; sí para cerrar las pruebas de errores del parser. |

## Modelo semántico y escena

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Esquema exacto del modelo semántico | B | Es una frontera arquitectónica entre sintaxis y layout, sin necesidad de exponerla públicamente. | Sí. Se necesita una representación concreta para conectar validación y layout. |
| Modelo normalizado común frente a tipos separados por layout | B | Afecta extensibilidad, acoplamiento y organización interna, pero no el DSL ni el SVG contractual. | Sí. Al menos debe adoptarse una forma inicial para `flow`. |
| Representación de relaciones, grupos y `split` | B | Define el contrato interno entre semantic model y layout. | Sí. El layout de `flow` necesita comprender orden y grupos paralelos. |
| Primitivas exactas de la escena | B | La escena es la frontera entre layout y renderer; su diseño afecta paquetes y evolución interna. | Sí. Hace falta un mínimo de texto, tarjeta, conector, grupo y highlight. |
| Orden de pintado de los elementos de la escena | A | Es una decisión renderer/layout local y reversible mientras produzca un resultado correcto y determinista. | Sí, pero puede resolverse durante la implementación. |
| Contrato exacto entre theme, layout y renderer | B | Determina qué capa resuelve colores, dimensiones, estilos y tokens; una mala frontera genera acoplamiento estructural. | Sí. Debe existir una división mínima antes de conectar layout y SVG. |
| Representación de clipping, sombras, bordes y conectores curvos | A | Son técnicas internas de representación, sustituibles sin cambiar el DSL. | No. El slice puede comenzar sin clipping ni sombras y con conectores simples. |

## Diseño visual

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Paleta, tamaños tipográficos, espaciado, radios y demás tokens del tema `iasi` | C | El tema incorporado forma parte directa del resultado observable y de la aceptación visual del producto. | Sí para declarar el slice “visualmente convincente”. No para producir un primer SVG técnico. |
| Familia y stack tipográfico inicial | C | Afecta apariencia, métricas, wrapping y consistencia entre plataformas. Además, la especificación deja pendiente el estándar tipográfico IASI. | Sí para aceptación visual; no para una prueba técnica provisional. |
| Definición objetiva de “presentation-quality” | C | Es un criterio explícito de aceptación, actualmente dependiente de revisión humana. Codificar un criterio propio cambiaría el umbral de éxito. | Sí para afirmar que la fase visual está completada. |
| Dimensiones de tarjetas, márgenes y áreas de título/highlight | A | Son parámetros internos ajustables y reversibles dentro de un canvas ya fijado. | Sí, pero pueden elegirse durante el layout inicial y refinarse. |
| Heurística de medición y wrapping | B | Debe aislarse detrás de una interfaz y afecta de forma transversal todos los layouts. El algoritmo concreto puede evolucionar. | Sí. Hace falta una primera estrategia determinista para colocar texto. |
| Cuándo envolver, ajustar o rechazar contenido | C | Rechazar o transformar texto afecta entradas aceptadas y resultado observable. | No para textos breves; sí antes de aceptar casos largos como comportamiento estable. |
| Distribución de 3 a 8 elementos de `ecosystem` | A | Es un algoritmo específico de layout y puede cambiar sin alterar el DSL. | No para el slice de `flow`. |
| Geometría de ramificación y reconvergencia de `split` | A | La semántica ya está fijada; la ruta y forma concreta de los conectores es interna. | Sí, pero se puede decidir y ajustar durante la implementación. |
| Conjunto definitivo de iconos | C | Los nombres publicados pasan a formar parte del DSL y afectan compatibilidad futura. | No. Se puede omitir el icono en el primer ejemplo o usar únicamente nombres ya sugeridos sin declarar definitivo el registro completo. |
| Diseño vectorial, procedencia y licencia de los iconos | B | Afecta activos incorporados, distribución y riesgos legales, aunque no la sintaxis si los nombres permanecen iguales. | No si el primer slice no usa iconos; sí antes de distribuir iconos incorporados. |

En la heurística de texto hay dos decisiones: la existencia y forma de la abstracción es B; los coeficientes concretos usados inicialmente para estimar anchura son A.

## Renderer SVG

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Incluir `width` y `height` en el `<svg>` además de `viewBox` | C | Cambia el comportamiento de incrustación y tamaño por defecto del SVG en consumidores. | No para generar SVG escalable; sí conviene decidirlo antes de congelar golden files. |
| Contenido exacto de `<desc>` | A | Puede derivarse de manera determinista del contenido sin introducir nueva semántica. | No. Puede usarse una descripción mínima o posponerse, pues se pide “cuando sea viable”. |
| Política de escaping XML | A | Debe ser correcta, pero es una decisión de serialización interna; lo natural es usar capacidades estándar de Go. | Sí, pero no requiere una decisión de producto. |
| Precisión decimal de coordenadas | A | Es interna, determinista y reversible mientras no cause defectos visibles. | Sí, pero puede fijarse localmente. |
| Formato y normalización del XML | B | Determina estabilidad de snapshots y reproducibilidad entre versiones del renderer. | No para el primer render; sí antes de establecer golden tests byte a byte. |
| Esquema para generar IDs internos estables | A | Los IDs son internos mientras permanezcan deterministas, válidos y no colisionen. | No si el SVG inicial no necesita definiciones con IDs. |
| Uso de filtros visuales | A | La especificación los hace opcionales. Puede comenzarse sin filtros. | No. La opción más simple es no utilizarlos inicialmente. |

## CLI y distribución

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Path del módulo Go | B | Es identidad técnica persistida en `go.mod` e imports internos; no altera el DSL ni la CLI. | Sí. Es necesario para inicializar y construir el módulo. |
| Sobrescritura de un archivo de salida existente | C | Es comportamiento observable y puede implicar pérdida de datos o exigir confirmación. | No si la prueba escribe a un destino nuevo. |
| Soporte de `-o -` para stdout | C | Amplía la interfaz pública de la CLI y afecta cómo los adaptadores consumen la salida. | No. La especificación solo exige stdin con salida a archivo. |
| Salida por defecto al usar stdin sin `-o` | C | No existe un nombre de entrada del que derivar el SVG; elegir error, stdout o nombre fijo cambia la CLI. | No. El slice inicial lee un archivo y la integración prevista puede proporcionar `-o`. |
| Rechazar entradas cuya extensión no sea `.ig` | C | Afecta qué invocaciones acepta la CLI. | No. El ejemplo puede usar `.ig`. |
| Formato estable de diagnósticos | C | El requisito mínimo está fijado, pero convertir el formato completo en contrato afecta herramientas y compatibilidad. | Parcialmente: sí hace falta `archivo:línea:columna: mensaje`; no hace falta fijar todavía más estructura. |
| Códigos de salida diferenciados por tipo de error | C | Es una interfaz observable para scripts. La especificación solo exige cero/no cero. | No. Basta inicialmente con `0` y un único valor no cero. |
| Semántica de `validate` | C | Define una orden pública todavía marcada como secundaria. | No. Debe posponerse. |
| Formato de `version` | C | Es salida pública consumible por usuarios y scripts. | No. Debe posponerse. |
| Inclusión de `--theme iasi` en v0.1 | C | Es una opción pública y la especificación dice que puede existir, no que deba hacerlo. | No. La opción mínima es no exponerla todavía. |

## Integración Quarto

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Estructura exacta de la extensión Quarto | B | Es organización interna del adaptador mientras preserve el bloque público `{iasi-graphics}`. | No. Quarto se implementa después del core. |
| Ubicación de los SVG generados | C | Es comportamiento observable del build y afecta limpieza, publicación y tooling. | No. |
| Algoritmo de nombres o hashes | B | Es una decisión persistente del adaptador para determinismo, caché y colisiones. | No. |
| Localización del ejecutable `iasi-graphics` | C | Afecta instalación, PATH, configuración y mensajes de error visibles. | No. |
| Extracción de opciones `#|` | B | Debe seguir las convenciones de Quarto; la técnica concreta pertenece al adaptador. | No. |
| Política de caché | C | Cambia cuándo se recompila y qué percibe el usuario. | No. |
| Limpieza de artefactos generados | C | Afecta archivos observables y posibles eliminaciones. | No. |
| Ejecuciones concurrentes | B | Requiere una estrategia interna segura para nombres y escritura atómica. | No. |
| Relación con `iasi-quarto` o `iasi-lua` | B | Define propiedad, empaquetado y dependencia entre repositorios sin cambiar necesariamente el bloque público. | No. |
| Comportamiento HTML frente a PDF | C | Afecta formatos soportados y criterios de aceptación de la integración. | No. |

## Alcance, evolución y pruebas

| Ambigüedad | Categoría | Por qué | ¿Bloquea el primer slice? |
|---|---|---|---|
| Nombre comercial del DSL | C | Es identidad pública y compatibilidad documental. | No. El producto y la extensión `.ig` ya bastan. |
| Temas personalizados | C | Introducen nueva sintaxis o configuración pública. Están explícitamente fuera de v0.1. | No. |
| Formatos PNG/PDF y otros renderers | C | Amplían el contrato de salida y están fuera de v0.1. | No. |
| Nuevos layouts | C | Amplían el lenguaje público. | No. |
| Generación mediante IA | C | Amplía alcance y workflow del producto; está fuera del renderer y del MVP. | No. |
| Existencia de una API Go pública | C | Una API exportada crea un contrato de compatibilidad externo. | No. El orquestador puede permanecer en `internal/`. |
| Estrategia de releases | B | Afecta distribución y mantenimiento, no el comportamiento del DSL. | No. |
| Plataformas obligatorias | C | Define compatibilidad y aceptación del producto distribuido. | No para el slice local. |
| “Cuatro niveles” de pruebas aunque se enumeran cinco | A | Es una inconsistencia editorial evidente: los cinco niveles enumerados pueden conservarse, con Quarto como fase posterior. | No. |

## Resumen de bloqueos reales

Para comenzar y terminar técnicamente el primer vertical slice hay que tomar las siguientes decisiones.

### B — Deben persistirse

- Path del módulo Go.
- Forma inicial del modelo semántico.
- Representación de `split` y orden del flujo.
- Primitivas mínimas de la escena.
- Contrato entre theme, layout y renderer.
- Abstracción de medición de texto.

### A — Se pueden resolver durante la implementación

- Orden de pintado.
- Geometría de tarjetas y conectores.
- Márgenes y dimensiones iniciales.
- Precisión numérica.
- Escaping XML.
- Heurística concreta de medición.
- Representación SVG simple, sin filtros.

### C — Impiden declarar el resultado visual como aceptado

- Tokens concretos del tema `iasi`.
- Stack tipográfico.
- Umbral de “presentation-quality”.

Las demás decisiones C no bloquean el slice positivo si se evita convertir los casos ambiguos en comportamiento estable. Se pueden posponer manteniendo pruebas únicamente para lo expresamente definido.

## Criterio operativo propuesto

Implementar el camino válido especificado, no añadir aceptación ni rechazo explícito para casos ambiguos salvo lo indispensable, y no congelar golden SVGs hasta resolver las decisiones visuales de categoría C.

---

## ChatGPT

Sí. **Ahora ya tenemos algo serio.** Codex ha hecho bien la separación y, además, nos ha reducido el problema a un núcleo muy pequeño.

Para arrancar el primer slice dice que solo hay **seis decisiones arquitectónicas que deben persistirse**: módulo Go, modelo semántico, representación de `split`, primitivas de escena, contrato `theme/layout/renderer` y abstracción de medición de texto.

Y separa correctamente las decisiones que puede tomar él mientras implementa, geometría concreta, orden de pintado, precisión, escaping, heurística inicial, etc.

Además ha detectado que **no podemos declarar el gráfico visualmente aceptado** sin cerrar tres cosas nuestras: tokens del tema IASI, tipografía y qué significa realmente `presentation-quality`.

Eso me parece exactamente la frontera que buscábamos.

### Pero hay una frase suya que yo corregiría

Esta:

> “no añadir aceptación ni rechazo explícito para casos ambiguos salvo lo indispensable”

Ahí hay una pequeña trampa.

Un compilador **siempre acaba aceptando o rechazando**. No puede dejar verdaderamente indefinido qué ocurre con:

```text
highlight
highlight
```

o con basura después del gráfico raíz.

Aunque no escriba un test, el parser tendrá un comportamiento.

Así que nuestro principio debería ser algo ligeramente distinto:

> **No convertir una decisión provisional en contrato.**

Puede implementar el comportamiento mínimo necesario, pero debe quedar identificado como provisional y **no respaldado por acceptance tests** hasta que nosotros decidamos la semántica.

Eso encaja mucho mejor con lo que estamos construyendo.

---

Y creo que **ya no debemos seguir interrogándolo**.

Ahora toca hacer ingeniería.

Yo resolvería nosotros las seis B, muy rápido. De hecho, casi todas las podemos decidir ya:

```text
Go module
    ↓
Semantic model
    ↓
Scene model
    ↓
theme / layout / renderer contract
    ↓
text measurement abstraction
```

Después se las persistimos y le soltamos el primer vertical slice.

Y aquí aparece una cosa preciosa del experimento: **Codex nos acaba de fabricar la lista exacta de decisiones que nosotros, como arquitectos, tenemos que tomar antes de que él programe.**

No hemos tenido que inventarnos un proceso. **El proceso está emergiendo de trabajar así.** 🦉

---

## Javier

entonces, a lo que tu corregirias habria que añadirlo a los inputs

## ChatGPT

Sí. **Exactamente.**

Porque si esa corrección solo vive en nuestra conversación, Codex volverá a caer en la misma interpretación cuando entre otro agente, otra sesión o dentro de tres meses.

Esto:

> **No convertir una decisión provisional en contrato.**

debería pasar a `inputs/` como **regla del proceso de implementación**, no como detalle de `iasi-graphics`.

La formulación completa que yo persistiría sería algo así:

> Cuando una parte de la especificación sea ambigua pero no bloquee la implementación, el agente puede adoptar una solución provisional, mínima, determinista y reversible.  
>
> Esa solución **no se considera parte del contrato del producto** mientras no haya sido explícitamente decidida y persistida en la especificación.  
>
> En particular, una decisión provisional no debe congelarse mediante acceptance tests, golden files, documentación pública o APIs estables.  
>
> Si la decisión afecta al DSL, comportamiento observable, compatibilidad o criterios de aceptación, debe volver a la capa de especificación antes de convertirse en comportamiento contractual.

Esto complementa perfectamente la clasificación A/B/C que Codex acaba de producir.

Y corrige precisamente el pequeño problema de su criterio final, donde propone «no añadir aceptación ni rechazo explícito» para casos ambiguos. El comportamiento puede existir provisionalmente. **Lo que no puede ocurrir es que accidentalmente lo convirtamos en ley.**

Y esto es importante: yo **no lo pondría en una spec concreta de `flow`**. Lo pondría en uno de los documentos generales de `inputs/`, donde estemos definiendo cómo debe trabajar el agente con la especificación.

Porque ya no estamos mejorando `iasi-graphics`.

Estamos empezando a especificar **cómo se construyen sistemas a partir de conocimiento persistido**. 😏

---

## Javier

lo estas haciendo tu, dame el doc

## ChatGPT

He integrado la clasificación A/B/C que ha propuesto Codex y he corregido el punto delicado de su criterio operativo: una decisión provisional **puede existir en la implementación**, pero no debe convertirse accidentalmente en contrato.

Yo lo metería en `inputs/` como **`implementation-decisions.md`**.

[Se genera el documento.]

---

## Javier

en castellano huevona

## ChatGPT

🤣 Sí, se me ha ido completamente la pinza. **En castellano**, que además todo IASI lo estamos construyendo así.

Yo lo llamaría `decisiones-implementacion.md`.

# Decisiones de implementación

## Propósito

Este documento define cómo deben gestionarse las decisiones de implementación al construir `iasi-graphics` a partir de sus especificaciones.

Su objetivo es mantener una frontera clara entre:

- detalles de implementación;
- decisiones de arquitectura;
- decisiones de especificación;
- criterios de aceptación.

La implementación no debe convertir silenciosamente una cuestión aún no resuelta en un contrato permanente del producto.

---

## Categorías de decisión

Toda ambigüedad relevante descubierta durante la implementación debe clasificarse en una de las siguientes categorías.

### A. Detalle de implementación

Una decisión que puede tomarse localmente durante la implementación.

Debe utilizarse la solución más sencilla que sea:

- determinista;
- reversible;
- fácil de sustituir;
- compatible con la especificación existente.

Ejemplos:

- geometría interna;
- orden de pintado;
- precisión numérica;
- estrategia de escape XML;
- heurísticas iniciales de layout;
- representación interna del SVG.

Estas decisiones no necesitan volver previamente a especificación mientras no se conviertan en comportamiento contractual o sean difíciles de revertir.

---

### B. Decisión de arquitectura

Una decisión que establece una frontera estructural interna del sistema y que, por tanto, debe tomarse deliberadamente y persistirse.

Ejemplos:

- estructura del modelo semántico;
- representación de `flow` y `split`;
- modelo de escena;
- fronteras entre tema, layout y renderer;
- abstracción para la medición de texto;
- estructura de módulos y paquetes.

Las decisiones arquitectónicas pueden evolucionar, pero no deben permanecer como conocimiento accidental contenido únicamente en el código.

Las decisiones arquitectónicas relevantes deben persistirse en la base de conocimiento del proyecto.

---

### C. Decisión de especificación o aceptación

Una decisión que afecta al comportamiento observable desde el exterior.

Incluye decisiones que modifican:

- el DSL;
- qué entradas son válidas o inválidas;
- el comportamiento de la línea de comandos;
- la compatibilidad;
- la salida generada considerada contractual;
- APIs públicas;
- criterios de aceptación visual;
- plataformas o formatos soportados.

Un agente de implementación no debe tomar unilateralmente estas decisiones.

Deben volver a la capa de especificación antes de convertirse en comportamiento contractual.

---

## Decisiones provisionales

Una ambigüedad no tiene por qué bloquear necesariamente la implementación.

Cuando una cuestión todavía no resuelta permita continuar trabajando, la implementación puede adoptar una decisión provisional.

Toda decisión provisional debe ser:

- mínima;
- determinista;
- reversible;
- explícitamente identificable como provisional.

El objetivo es permitir que la implementación avance sin ampliar prematuramente la especificación del producto.

---

## El comportamiento provisional no es contrato del producto

Toda implementación presenta necesariamente algún comportamiento, incluso en aquellas áreas que todavía no han sido completamente especificadas.

Por tanto, la regla no es:

> No implementar ningún comportamiento cuando exista una ambigüedad.

La regla es:

> **No convertir un comportamiento provisional en contrato del producto.**

Una decisión provisional de implementación no se convierte en contractual simplemente porque el código actual se comporte de esa manera.

Mientras no haya sido decidida explícitamente y persistida en la especificación, ese comportamiento no debe considerarse una garantía de compatibilidad.

---

## Congelación del contrato

Una decisión provisional no debe quedar congelada mediante mecanismos que la conviertan implícitamente en requisito del producto.

En particular, un comportamiento provisional no debe estabilizarse mediante:

- acceptance tests;
- golden files;
- documentación pública;
- APIs públicas;
- compromisos de compatibilidad;
- ejemplos normativos;
- acoplamiento arquitectónico difícilmente reversible.

Pueden existir tests unitarios que protejan la corrección interna de una implementación provisional cuando sean necesarios.

Pero esos tests no deben transformar una decisión de producto aún no resuelta en un criterio de aceptación.

---

## Acceptance Tests

Los acceptance tests representan el contrato del producto.

Responden a la pregunta:

> ¿Qué comportamiento observable debe seguir siendo cierto?

Por tanto, un acceptance test solo debe introducirse cuando el comportamiento correspondiente esté explícitamente especificado.

Los detalles internos de implementación pueden cambiar mientras los acceptance tests continúan siendo válidos.

Si modificar un detalle interno obliga a modificar un acceptance test, debe revisarse si ese test está verificando realmente comportamiento del producto o simplemente detalles de implementación.

---

## Regla de escalado

Siempre que una decisión de implementación afecte a:

- sintaxis o semántica del DSL;
- entradas aceptadas o rechazadas;
- comportamiento observable de la CLI;
- compatibilidad;
- contrato del renderer;
- comportamiento público de una integración;
- aceptación visual;
- criterios de aceptación;

la decisión debe volver a la capa de especificación antes de considerarse estable.

El agente de implementación debe identificar la ambigüedad, explicar sus consecuencias y continuar con trabajo reversible siempre que sea posible.

---

## Principio operativo

La implementación debe avanzar mediante el menor vertical slice completo permitido por la especificación existente.

Durante ese trabajo:

1. Las decisiones **A** pueden resolverse localmente utilizando la opción más sencilla, determinista y reversible.
2. Las decisiones **B** deben tomarse deliberadamente y persistirse como conocimiento arquitectónico.
3. Las decisiones **C** deben volver a especificación antes de convertirse en comportamiento contractual.
4. Las cuestiones aún no resueltas que no bloqueen el trabajo pueden utilizar implementaciones provisionales.
5. Las implementaciones provisionales nunca deben convertirse accidentalmente en contrato del producto.

**La implementación puede descubrir decisiones.**

**No puede definir silenciosamente el producto.**

---

## Javier

y de forma que pueda bajarlo

## ChatGPT

Aquí lo tienes listo para bajar y meter directamente en `inputs/`:

`decisiones-implementacion.md`

---

## Javier

mucho mejor, le decimos que empieze por la fase 1

## ChatGPT

Sí. **Ahora sí le damos permiso para construir.**

Le pasaría esto:

```text
Empieza la Fase 1 de implementación de `iasi-graphics`.

Trabaja a partir de todo el contenido de `inputs/`, incluido
`decisiones-implementacion.md`.

Objetivo de esta fase:
construir el primer vertical slice completo y ejecutable:

    flow.ig
      → lexer
      → parser
      → AST
      → validación semántica
      → modelo semántico
      → layout de flow
      → escena
      → renderer SVG
      → archivo SVG

Reglas:

1. Implementa únicamente lo necesario para este primer vertical slice.
2. No implementes todavía `compare`, `ecosystem`, Quarto, stdin,
   empaquetado multiplataforma ni extensibilidad de temas.
3. Las decisiones de categoría A puedes tomarlas utilizando la solución
   más simple, determinista y reversible.
4. Las decisiones de categoría B necesarias para esta fase deben tomarse
   deliberadamente y quedar persistidas como decisiones arquitectónicas.
5. No conviertas ninguna decisión de categoría C no resuelta en contrato
   del producto.
6. Si necesitas adoptar provisionalmente algún comportamiento relacionado
   con una decisión C, identifícalo claramente como provisional.
7. No congeles comportamientos provisionales mediante acceptance tests,
   golden files, documentación normativa o APIs públicas.
8. Implementa tests técnicos suficientes para garantizar el correcto
   funcionamiento interno del slice.
9. No amplíes el alcance para “preparar el futuro” salvo que sea
   estrictamente necesario para mantener una frontera arquitectónica clara.
10. Si descubres durante la implementación una nueva decisión relevante,
    clasifícala como A, B o C y actúa según
    `decisiones-implementacion.md`.

El resultado de la Fase 1 debe poder ejecutarse realmente desde CLI
sobre un ejemplo `flow.ig` y producir un SVG válido.

Al terminar, informa de:

- qué has implementado;
- qué tests existen y qué verifican;
- qué decisiones B has tomado y dónde las has persistido;
- qué decisiones provisionales has tenido que adoptar;
- qué decisiones C siguen pendientes;
- cómo ejecutar el vertical slice desde cero.

Puedes comenzar a modificar y crear archivos.
```

Y **lo soltamos**.

Especialmente importante: ya no le pedimos otro análisis previo. Tiene claro qué construir, ha identificado los bloqueos y ahora dispone de nuestra regla sobre decisiones provisionales.

Ahora viene la parte buena: ver si **la especificación produce software**. 😈🦉

---

## Javier

descargarme la charla
