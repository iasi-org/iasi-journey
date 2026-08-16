# ChatGPT — Arquitectura, refactorización y el lugar natural del cambio

**Fecha:** 2026-08-16  
**Contexto:** continuación de la conversación sobre IASI, arquitectura, refactorización y cómo los Sistemas Inteligentes pueden cambiar el coste de comprender y rehacer sistemas existentes.

---

## Conversación

**Usuario:**

ahi tenemos mucho material  
como ingeniero/arquitecto lo he visto muchas veces  
¿Por que han hecho esto?  
Por que no saben rehacer el sistema, ni sus implicaciones, añado otra capa mas y punto, otra escepcion: ejemplo quarto y sus bloques y muchas otras situaciones que he visto  
Me acuerdo:  
los markdown y los lenguajes con lineas de #no se que  
R ya lo invento de manera natural: usando llaves

**ChatGPT:**

Sí. Ahí hay **muchísimo material de arquitectura**, porque no es un problema nuevo de la IA. Es uno de los problemas clásicos de los sistemas que envejecen.

La escena la hemos visto mil veces:

> «¿Por qué demonios está hecho así?»

Y muchas veces la respuesta real no es «porque ésa era la mejor arquitectura», sino:

> **Porque cambiar la arquitectura existente era demasiado difícil, demasiado arriesgado o nadie comprendía ya suficientemente bien sus implicaciones.**

Entonces aparece otra capa. Otra excepción. Otro parámetro. Otro modo especial.

Y cada decisión aislada puede ser perfectamente racional. El problema aparece **por acumulación**.

Tu ejemplo de Quarto es muy bueno porque es pequeño y visible: aparece una necesidad nueva y la reacción inmediata es encontrar alguna sintaxis que permita *encajarla* en el modelo actual. `:::` por aquí, una excepción por allá, un comportamiento especial. Funciona. Pero la pregunta arquitectónica debería ser anterior:

**¿Esta nueva necesidad pertenece realmente al modelo que tenemos o nos está diciendo que el modelo ya no es correcto?**

Y lo de los lenguajes apunta a algo parecido. R, obviamente, no inventó históricamente las llaves, pero sí ilustra perfectamente la diferencia entre **una construcción integrada en la gramática** y una directiva añadida externamente.

Ese patrón de las líneas especiales, prefijos, anotaciones, directivas, escapes, etc. aparece muchísimo cuando un lenguaje o sistema tiene que incorporar algo que **su modelo original no sabía representar bien**.

Y aquí hay una idea de arquitectura muy potente para el Journey:

**Las excepciones sintácticas suelen ser síntomas arquitectónicos.**

No significa que toda excepción sea mala. A veces es exactamente la solución adecuada. Pero cuando empiezan a acumularse, quizá no estamos ampliando un buen modelo. **Estamos intentando evitar rehacerlo.**

Y eso nos devuelve directamente al cambio que permiten los Sistemas Inteligentes.

Antes podías detectar perfectamente que había una abstracción mejor y aun así decidir:

> «Ni la toques. Funciona.»

Porque entender todas las consecuencias podía costar semanas.

Si ahora podemos recuperar el conocimiento, analizar dependencias, recorrer tests, estudiar llamadas, reconstruir decisiones históricas y evaluar el impacto de alternativas, cambia la ecuación.

Entonces ante la quinta excepción ya podemos preguntar:

**¿Añadimos la sexta o ha llegado el momento de cambiar la abstracción?**

Eso me parece material puro de IASI. Y además tiene una tesis bastante incómoda para arquitectos:

**parte de la complejidad que llamamos “legacy” no representa la complejidad del problema; representa el coste histórico de cambiar soluciones anteriores.**

---

**Usuario:**

no! en lo de R me referia a:  
el markdown moderno: `lenguaje  
R, knitr: `{ lenguage opciones ....}  
Los de R venian de C y de saber con que jugaban  
Los del md moderno no, les falta conocimiento  
por que las llaves son naturales para cualquier tecnico

**ChatGPT:**

Exacto. Ahora sí te sigo. No hablabas de las llaves de R como lenguaje, sino de **cómo knitr/R Markdown diseñó la cabecera de los bloques ejecutables**.

La diferencia que estás señalando es esta:

```markdown
```python
...
```
```

frente a algo del estilo knitr/R Markdown:

```markdown
```{r, echo=FALSE, warning=FALSE}
...
```
```

Y conceptualmente hay una diferencia de diseño bastante importante.

En el primer caso, la sintaxis resuelve **el caso inmediato**: “esto es código y el lenguaje es X”.

En el segundo, las llaves crean desde el principio **un pequeño espacio estructurado de configuración**:

```text
{
    lenguaje,
    opciones,
    parámetros,
    ...
}
```

Eso es muy natural para alguien procedente de C, R y lenguajes con gramáticas estructuradas: **si esto va a contener más cosas, dale un contenedor**.

Y cuando posteriormente aparecen nuevas necesidades, tienes dónde ponerlas sin inventarte otra convención sintáctica.

Ahí está tu crítica arquitectónica, que es bastante buena. No hace falta afirmar que quienes diseñaron Markdown “no sabían”, porque eso sería atribuir intenciones que no conocemos. Pero sí podemos decir algo más defendible y más interesante:

**Markdown resolvió brillantemente un problema pequeño. Cuando el problema creció, su sintaxis original no siempre tenía lugares naturales donde alojar las nuevas necesidades.**

Y entonces aparecen extensiones:

```text
atributos
directivas
líneas especiales
::: bloques
#| opciones
metadatos laterales
```

No porque sean necesariamente malas soluciones, sino porque **el modelo inicial no había reservado estructura para ese crecimiento**.

Mientras que el enfoque `{ ... }` dice de entrada:

> Aquí hay una unidad sintáctica. Dentro puede crecer información.

Y esto conecta PERFECTAMENTE con lo que hablábamos del legacy.

La cuestión no es:

**“¿Podemos añadir otra sintaxis?”**

La cuestión arquitectónica es:

**“¿La nueva necesidad encaja naturalmente en el modelo, o estamos inventando otra excepción porque no queremos rediseñarlo?”**

De hecho, de aquí sale una formulación bastante buena para IASI:

> **Una arquitectura envejece no solo cuando acumula código, sino cuando acumula excepciones necesarias para representar conceptos que su modelo original ya no sabe expresar.**

Y el ejemplo Quarto/R Markdown es estupendo porque es pequeñito, reconocible y casi didáctico.

---

**Usuario:**

mas o menos me has pillado, eso deberemos extenderlo, sin hacer sangre

**ChatGPT:**

Sí. **Sin hacer sangre**, porque el punto interesante no es “estos lo hicieron mal”, sino algo mucho más útil:

**cuando una nueva necesidad no encaja de forma natural en un modelo, la forma en que la incorporamos revela la calidad y los límites de la abstracción existente.**

El ejemplo R Markdown / knitr frente a algunas extensiones posteriores de Markdown nos sirve para estudiar eso: cuándo una sintaxis tiene desde el principio un lugar natural para crecer y cuándo el crecimiento acaba necesitando nuevas convenciones, directivas o excepciones.

Y lo extendería a arquitectura en general:

> Una excepción puede ser una solución razonable.  
> Una colección creciente de excepciones puede ser una señal de que el modelo necesita ser reconsiderado.

Ahí podemos meter Quarto, lenguajes, APIs, modelos de clases, capas de integración y varios casos que has visto profesionalmente. Sin buscar culpables, porque casi siempre hubo **restricciones históricas, costes, compatibilidad y conocimiento limitado** que explicaban perfectamente la decisión original.

Y entonces entra el giro IASI: si los Sistemas Inteligentes reducen radicalmente el coste de **entender las implicaciones de cambiar algo**, quizá también cambia el punto en el que dejamos de añadir excepciones y decidimos **rehacer la abstracción**.

---

**Usuario:**

jajajajajajajajaja  
has dicho lo que yo siempre he dicho de mis sistemas o de los demas  
Cuando aparece una nueva necesidad, debe ir a su sitio natural que ademas es intuitivo, si no es asi, tu sistema esta mal

**ChatGPT:**

Exacto. Y además **esa frase es mucho más profunda de lo que parece**:

> **Cuando aparece una nueva necesidad, debe tener un sitio natural, evidente e intuitivo dentro del sistema. Si para incorporarla hay que forzar el modelo, inventar una excepción o explicar demasiado dónde encaja, probablemente el problema no sea la nueva necesidad: sea la arquitectura.**

Eso explica muchísimas cosas.

Un buen sistema no solo funciona hoy. **Hace que el cambio de mañana tenga sentido.** Cuando llega algo nuevo, miras la arquitectura y casi dices: “claro, va aquí”.

Si en cambio la conversación empieza con:

> “Bueno… técnicamente esto debería ir aquí, pero entonces rompemos aquello, así que creamos otro tipo especial, le ponemos un flag, una capa intermedia y documentamos que en este caso funciona distinto…”

Ya está sonando la alarma.

Y hay un matiz importante para no convertirlo en dogma: una necesidad realmente nueva puede revelar que el modelo original era perfectamente bueno **para el problema que conocíamos entonces**, pero ya no representa correctamente el problema actual.

Por eso la señal no debería ser:

**“Hay una excepción, arquitectura mala.”**

Sino:

**“La nueva necesidad no encuentra un lugar natural. Antes de crear una excepción, revisemos si la abstracción sigue siendo válida.”**

Y esto conecta con lo que acabamos de guardar para Journey.

Hasta ahora, muchas veces el arquitecto veía perfectamente que **el sitio natural ya no existía**, pero rehacerlo era demasiado caro o arriesgado. Entonces añadía la excepción y seguía.

Con Sistemas Inteligentes podemos empezar a cambiar esa economía:

**Nueva necesidad → buscar su lugar natural → si no existe, cuestionar el modelo → evaluar refactorización.**

No:

**Nueva necesidad → buscar dónde meterla → parche.**

Y me quedaría con una frase para IASI:

> **Una arquitectura buena hace que lo nuevo encuentre su sitio. Una arquitectura agotada obliga a inventárselo.**

---

**Usuario:**

me ha gustado una cosa, igual el sistema estaba pensado bien, pero para lo que sabiamos entonces  
no me gusta mucho, pero si, justifica

**ChatGPT:**

Sí. Yo tampoco lo usaría como una **justificación defensiva** tipo “bueno, es que entonces no lo sabíamos”. Eso le quita fuerza.

Lo formularía como una propiedad normal de la arquitectura:

**Una arquitectura puede haber sido correcta y dejar de serlo.**

Porque la arquitectura representa **el problema que conocemos en un momento determinado**. Si el problema cambia suficientemente, puede ocurrir que el modelo deje de representarlo bien. Eso no convierte automáticamente en mala la decisión original.

Lo importante viene después.

Cuando aparece una nueva necesidad y **no encuentra un lugar natural**, eso es información. El sistema te está diciendo que quizá has llegado al límite de alguna abstracción.

Ahí tienes dos caminos:

**adaptar el modelo** o **empezar a acumular excepciones**.

Y esta formulación encaja bien con IASI:

> **Una arquitectura no es buena porque nunca tenga que cambiar. Es buena mientras representa de forma natural el problema que tiene que resolver.**

---

**Usuario:**

te pongo un ejemplo:

**ChatGPT:**

Dale, te sigo.

---

**Usuario:**

Diseñe un sistema de prestamos y creditos, con un monton de formas de ellos, que se llaman productos y son numericos (banca)  
¿Como incluias una nueva forma?  
Facil: Creabas unicamente un modulo con una nomeclatura asociada al producto para gestionar sus peculiaridades  
el sistema ya lo descubria solo

**ChatGPT:**

Exacto. Y este ejemplo **afina muchísimo** lo que estábamos diciendo.

No diseñaste el sistema intentando adivinar todos los préstamos y créditos que existirían en el futuro. Eso habría sido imposible.

Lo que hiciste fue identificar **qué era estable y qué era variable**.

El núcleo sabía gestionar el concepto general de producto. Las peculiaridades vivían fuera. Y existía una convención para encontrarlas:

> aparece un nuevo producto → creas su módulo con la nomenclatura correspondiente → el sistema lo descubre → funciona dentro del modelo existente.

Eso es muy distinto de:

> aparece un nuevo producto → `if producto == 317...` → excepción → parámetro nuevo → otra rama → cuidado con el producto 422 porque funciona distinto.

Y aquí sale una formulación bastante mejor de nuestra idea anterior:

**Una buena arquitectura no tiene que prever las necesidades futuras. Tiene que prever dónde vivirán las variaciones cuando aparezcan.**

O todavía más arquitectónica:

> **No necesitas conocer el cambio futuro. Necesitas haber identificado correctamente los ejes sobre los que el sistema puede cambiar.**

Eso explica por qué el sistema podía aceptar productos que **no existían cuando lo diseñaste** sin modificar el núcleo. No habías predicho esos productos. Habías modelado correctamente el dominio.

Y esto modifica también aquella frase de «una arquitectura puede haber sido correcta para lo que sabíamos entonces». Sí, pero ahora tenemos una vara de medir bastante mejor.

Una arquitectura buena puede encontrarse mañana con una necesidad que nadie había previsto y aun así decir:

**«Vale. Tú perteneces aquí.»**

Si, por el contrario, cada nueva variante obliga a tocar el núcleo, introducir condiciones o inventar excepciones, posiblemente **no hemos identificado bien la dimensión de variabilidad del problema**.

Tu ejemplo bancario es casi perfecto para explicarlo:

**No diseñaste los productos futuros. Diseñaste el lugar natural donde los productos futuros podrían existir.**

Esa frase resume exactamente la diferencia entre **predecir el futuro** y **diseñar para el cambio**.

---

## Ideas fijadas para Journey

- Una nueva necesidad debería encontrar un **sitio natural, evidente e intuitivo** dentro de la arquitectura.
- Si para incorporarla hace falta una excepción, un parche o una capa adicional, hay que preguntarse si la abstracción sigue siendo válida.
- Una excepción aislada puede ser razonable; una colección creciente de excepciones puede indicar agotamiento arquitectónico.
- Parte de la complejidad del legacy no representa la complejidad del dominio, sino el coste histórico de cambiar soluciones anteriores.
- Los Sistemas Inteligentes pueden reducir el coste de comprender dependencias, decisiones e implicaciones y volver a hacer viable la refactorización.
- Una buena arquitectura no necesita prever las necesidades futuras.
- **Una buena arquitectura necesita prever dónde vivirán las variaciones cuando aparezcan.**
- **No necesitas conocer el cambio futuro. Necesitas identificar correctamente los ejes sobre los que el sistema puede cambiar.**
- Ejemplo bancario: nuevos productos de préstamos/créditos se incorporaban creando únicamente un módulo con nomenclatura asociada al producto; el sistema lo descubría automáticamente.
- **No diseñaste los productos futuros. Diseñaste el lugar natural donde los productos futuros podrían existir.**
