# ChatGPT — Labs, inputs y aceptación humana

**Fecha:** 2026-08-19  
**Proyecto:** IASI  
**Destino:** journey

---

## Conversación

**Usuario:** bien, hablemos ahora de como montar los labs

**ChatGPT:** Sí. Y creo que aquí hay una decisión importante: los labs no deberían ser una “zona de ejemplos”, sino el sitio donde IASI se pone a prueba de verdad.

El lab no es simplemente construir un resultado final. Es observar cómo IASI avanza paso a paso conforme recibe información, qué puede hacer, qué no puede hacer todavía y cómo evoluciona el proyecto real.

---

**Usuario:** si, algunos labs deben ser proyectos reales. Pensaba por ejemplo: iasi-graphics. En el lab podemos empezar por un input external: Queremos hacer un filtro y un mcp que genere graficos. Tal vez algo mas de informacion. Si la metodologia analiza los inputs, te debe decir que con eso no puede hacer nada ¿correcto?

**ChatGPT:** Exacto. Ese sería un buen lab precisamente porque IASI debería fallar ahí de forma correcta.

Con un input como:

> Queremos hacer un filtro y un MCP que genere gráficos.

IASI puede reconocer la intención, pero debería detectar que todavía no existe información suficiente para diseñar o implementar el sistema.

La metodología no debe completar mediante imaginación aquello que el input no define.

Puede distinguir entre:

- lo observado;
- lo inferido;
- lo desconocido.

El primer éxito del lab puede ser precisamente que IASI se niegue a construir aquello para lo que aún no dispone de información suficiente.

---

**Usuario:** sin que te explayes, lo que has dicho es exacto, y es parte del lab. Ahora le decimos, por ejemplo en inputs/internals: Por ahora unicamente creame el proyecto. Ahora si deberia poder hacer algo

**ChatGPT:** Sí.

Ese nuevo input interno limita explícitamente el alcance de la acción. IASI sigue sin saber lo suficiente para diseñar o implementar `iasi-graphics`, pero sí dispone de información suficiente para crear únicamente el proyecto.

La carencia de información anterior no desaparece. Simplemente deja de bloquear esta acción concreta.

---

**Usuario:** son varios pasos del lab ¿lo vas viendo?

**ChatGPT:** Sí.

El lab no es “construir `iasi-graphics`”. El lab es observar cómo IASI avanza conforme recibe información.

Cada paso prueba:

- qué sabe IASI;
- qué no sabe;
- qué puede hacer con lo que sabe;
- y, especialmente, que no se adelante al siguiente paso.

---

**Usuario:** entonces como montamos el lab, y tal vez el repo, para que controle cada uno de esos pasos?

**ChatGPT:** Separaría el experimento del proyecto sometido al experimento.

El lab controla la secuencia de pasos, los inputs, las expectativas y las evidencias.

El repositorio real contiene el estado real producido por IASI.

> El repo es el sujeto experimental. El lab es el protocolo experimental.

Cada paso significativo puede asociarse a un tag Git que marque un checkpoint reproducible del repositorio.

---

**Usuario:** Creamos el lab-005 (graphics). Creamos el tag lab-005-step-01. Creamos el tag lab-005-step-02. ¿Hasta ahi bien?

**ChatGPT:** Sí, pero aparece un problema si posteriormente `lab-005` cambia de posición editorial.

El número del lab no debería formar parte de su identidad estable.

Mejor:

```text
lab-graphics-step-01
lab-graphics-step-02
```

mientras la carpeta del libro puede cambiar de:

```text
005-graphics/
```

a:

```text
010-graphics/
```

sin modificar ningún tag.

> El número ordena. El nombre identifica.

---

**Usuario:** exacto, el usuario, si se pierde, puede volver a cambiar su repo local al estado correcto

**ChatGPT:** Exacto.

Los tags oficiales funcionan como checkpoints reproducibles.

El usuario puede experimentar, desviarse, crear sus propios commits y volver conscientemente a cualquier estado oficial del lab.

Los commits son del usuario.

Los tags son del lab.

Los tags no describen la historia Git del usuario, sino los estados oficiales del experimento.

---

**Usuario:** esto es otra decision maja, documentala

**ChatGPT:** Se documenta como parte del modelo de ejecución de labs sobre proyectos reales:

- el lab define la secuencia experimental;
- el repositorio contiene el proyecto real;
- cada paso significativo dispone de un tag oficial;
- los tags usan la identidad estable del lab, no su orden editorial;
- el usuario mantiene su propia historia Git;
- los tags oficiales son puntos de restauración reproducibles.

Principios:

> El lab controla el experimento; el repositorio contiene el proyecto.

> Los commits son del usuario; los tags son del lab.

> El número ordena. El nombre identifica.

---

**Usuario:** Trabajar con una IA (o mas) en el equipo supone un esfuerzo intelectual bastante alto ¡tu no te cansas!

**ChatGPT:** La integración de uno o varios sistemas inteligentes puede elevar mucho la intensidad cognitiva del trabajo humano.

Los agentes eliminan esperas, resuelven tareas rápidamente y permiten encadenar decisiones de alta exigencia casi sin pausas naturales.

La velocidad sostenible del sistema no puede ser la velocidad máxima de los agentes.

> La velocidad sostenible de un sistema humano-inteligente está limitada por la capacidad humana de comprender, decidir, validar y asimilar.

---

**Usuario:** pero tiene mucha importancia, mas me canso mentalmente, mas cosas tengo en la cabeza, mas errores puedo cometer

**ChatGPT:** Exacto.

La cadena puede expresarse así:

```text
más velocidad de los agentes
        ↓
más decisiones por unidad de tiempo
        ↓
más contexto simultáneo
        ↓
más carga cognitiva
        ↓
menos capacidad de atención y validación
        ↓
más probabilidad de error
```

La carga cognitiva debe tratarse como una variable metodológica, operativa y de calidad.

> Aumentar la capacidad de ejecución no aumenta necesariamente la capacidad de decisión.

---

**Usuario:** ahora estaba pensando. Para el lab de montar la infraestructura, habria que dar un pdf con la guia SOLO de eso, porque no olvidemos que los labs son el libro iasi-book-ii y es un pdf unico. Y ademas un script completo, o varios, si hay intervenciones manuales intermedias, como por ejemplo crear las claves de github. Yo no usaria powershell, usario scripts bash con mingw (git bash), que por definicion estará disponible

**ChatGPT:** El lab de infraestructura debe disponer de una guía operativa independiente, además de formar parte de Book II.

La automatización debe dividirse en scripts Bash secuenciales cuando existan intervenciones humanas inevitables entre fases.

Principio:

> Automatizar todo lo automatizable; aislar y documentar explícitamente las intervenciones humanas inevitables.

Se prefiere Bash sobre Git Bash/MinGW en Windows, evitando PowerShell.

---

**Usuario:** el pdf lo podemos hacer fuera del libro, manualmente y luego pegar el resultado al lab. Por seguir nuestro metodo, un repo, pero del que solo nos interesa el pdf. Y que manualmente copiamos book ii

**ChatGPT:** Sí.

El repositorio del lab produce el PDF y Book II consume una copia validada del artefacto.

No se acoplan los builds.

> El repo del lab produce el artefacto; Book II lo integra.

---

**Usuario:** voy a crear iasi-labs-infra

**ChatGPT:** `iasi-labs-infra` será la fuente de verdad del lab operativo de infraestructura: contenido fuente, scripts Bash y generación del PDF independiente.

El PDF validado se copiará manualmente al lab correspondiente de `iasi-book-II`.

---

**Usuario:** ahora me gustaria escribir una primera entrada para book III. Ya tenemos clara la carpeta inputs, o como el usuario quiera llamarla, y sus tres subcarpetas. Y su comportamiento

**ChatGPT:** `inputs/` representa la frontera por la que la información entra en IASI.

Todo proyecto o proceso comienza con información. Nada nace de la nada: puede existir un pliego, una presentación, una conversación, una idea, código existente, una hoja de cálculo, una normativa o cualquier combinación de ellos.

La información puede llegar en formatos heterogéneos, con distintos grados de detalle, calidad, coherencia y completitud.

IASI conserva esa realidad en:

```text
inputs/
├── externals/
├── internals/
└── obtained/
```

Posteriormente esa información se analiza, contrasta, reconcilia y normaliza para producir `definitions/`.

> `inputs/` conserva la información tal como entra, se aporta o se obtiene.

> `definitions/` representa esa información una vez comprendida y estructurada por IASI.

---

**Usuario:** importante que inputs/external es siempre de SOLO LECTURA

**ChatGPT:** Regla fuerte:

> `inputs/externals/` es siempre de solo lectura para IASI.

IASI puede leer, analizar, relacionar, contrastar e interpretar un input externo, pero nunca modificarlo.

No debe corregirlo, reescribirlo, completarlo, normalizarlo ni sustituir silenciosamente una versión por otra.

> Lo externo representa lo que recibimos, no lo que hubiéramos querido recibir.

Cualquier transformación o normalización produce nuevos artefactos fuera de `inputs/externals/`.

---

**Usuario:** algo que me chirria en la metodologia

**Usuario:** los acceptance test

**ChatGPT:** Los acceptance tests son el punto donde se comprueba si el sistema satisface lo que se esperaba.

La cuestión abierta no es su importancia, sino quién los define, cuándo se generan y cuánto juicio humano contienen.

---

**Usuario:** no, si me parece bien, es el punto donde se dice si el sistema esta bien o no. Y deberia ser la etapa final. La pregunta es: ¿quien los hace?

**ChatGPT:** Una hipótesis es separar claramente quién implementa de quién verifica.

Pero hay que distinguir dos momentos:

1. definir los criterios o tests;
2. ejecutarlos al final.

Si los tests se diseñan solo después de ver la implementación, existe el riesgo de adaptar el examen al resultado.

Una posible secuencia sería:

```text
inputs
  ↓
definitions
  ↓
acceptance criteria / tests
  ↓
plan
  ↓
implementation
  ↓
verification
  ↓
acceptance results
```

---

**Usuario:** si estoy de acuerdo, pero no lo tenemos ni abierto ni cerrado

**ChatGPT:** Correcto.

No debe convertirse todavía en norma.

Tenemos claro:

- el papel de los acceptance tests;
- que deben estar relacionados con las `definitions`;
- que no deberían estar sesgados por la implementación;
- que probablemente exista una combinación de comprobación automática y juicio humano.

Pero siguen abiertas preguntas como:

- quién los genera;
- cuándo se generan exactamente;
- qué parte es ejecutable;
- qué parte requiere intervención humana;
- cómo se versionan;
- qué ocurre cuando cambian las `definitions`.

Es una cuestión metodológica abierta que debe probarse en labs reales.

---

**Usuario:** exacto, no tenemos quien los genera ni si hay parte humana. Normalmente habra parte humana. Y los acceptance test deberian ser lo primero. Y la decision de aceptar humana

**ChatGPT:** Ahí aparece una separación fundamental:

> Los acceptance tests no son la aceptación.

Son mecanismos para producir evidencia.

La decisión de aceptar pertenece al humano.

```text
definitions
    ↓
acceptance criteria / tests
    ↓
plan
    ↓
implementation
    ↓
verification
    ↓
evidence
    ↓
human review
    ↓
ACCEPT / REJECT
```

Probablemente exista una combinación de:

```text
automatic acceptance checks
human acceptance checks
```

Lo que todavía queda abierto es quién define los tests y cuánto participa el humano en su elaboración.

Pero la autoridad final queda clara:

> Los tests informan la aceptación. La aceptación la decide el humano.

---

**Usuario:** estoy contigo. tenemos que darle una vuelta

**ChatGPT:** Sí.

No conviene cerrar todavía el mecanismo concreto.

El esqueleto está bastante claro, pero las cuestiones pendientes deben probarse en un lab real.

`iasi-graphics` puede ser un buen caso para descubrir cómo definir de antemano qué significará que el sistema sea aceptable.

---

**Usuario:** el builder no decide que esta bien, lo decide un humano

**ChatGPT:** Exacto.

Esa debe ser una frontera metodológica fuerte:

> **El builder construye. El humano acepta.**

El builder puede:

- implementar;
- ejecutar pruebas;
- informar resultados;
- detectar desviaciones;
- recomendar.

Pero no tiene autoridad para declarar aceptado el sistema.

Un `PASS` técnico tampoco equivale necesariamente a aceptación.

Puede existir:

```text
tests: PASS
human: REJECT
```

o incluso:

```text
tests: partial FAIL
human: ACCEPT
```

si el humano responsable entiende y acepta conscientemente la desviación.

La autoridad final no pertenece al builder ni al conjunto de tests.

Pertenece al humano responsable.

---

## Estado de las decisiones

### Decisiones bastante firmes

- Los labs pueden ejecutarse sobre proyectos reales.
- El lab controla el experimento; el repositorio contiene el proyecto.
- Los estados oficiales de los pasos del lab se representan mediante tags Git.
- Los tags usan la identidad estable del lab, no su posición editorial.
- Los commits son del usuario; los tags son del lab.
- `inputs/` es la frontera de entrada de información.
- `inputs/externals/` es siempre de solo lectura.
- `definitions/` contiene la representación normalizada y estructurada de lo comprendido.
- La falta de información limita el alcance, pero no necesariamente detiene todo el proceso.
- La carga cognitiva humana debe tratarse como variable metodológica y de calidad.
- La velocidad sostenible de un sistema humano-inteligente está limitada por la capacidad humana de comprender, decidir, validar y asimilar.
- El builder construye.
- El humano acepta.
- Los tests informan la aceptación, pero no sustituyen la decisión humana.

### Cuestiones todavía abiertas

- Quién genera exactamente los acceptance tests.
- En qué momento preciso se generan.
- Qué parte debe ser automática.
- Qué parte debe requerir juicio humano.
- Cómo se relacionan y versionan respecto a las `definitions`.
- Cómo se actualizan cuando cambian las `definitions`.
- Qué artefacto o espacio metodológico los contiene.

Estas cuestiones deben resolverse mediante uso real y labs, no únicamente mediante diseño teórico.
