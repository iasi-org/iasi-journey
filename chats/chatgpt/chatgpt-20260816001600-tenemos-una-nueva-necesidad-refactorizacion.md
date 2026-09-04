Exacto. Y ahí creo que está **el núcleo de lo que querías decir**.

Tú ahora llegas y dices:

> **Tenemos una nueva necesidad.**

Y la respuesta tradicional de la ingeniería suele ser:

“Vale. ¿Dónde la metemos?”

Y empieza el problema.

Porque el sistema existente ya funciona. Hay clases, interfaces, dependencias, decisiones antiguas, clientes que dependen de ellas… y **nadie quiere tocar demasiado**.

Entonces aparece el patrón:

**Nueva necesidad → pequeño añadido → excepción → parche → otro caso especial → más complejidad.**

No porque los ingenieros sean malos. Al contrario: muchas veces es una decisión perfectamente racional. **El coste y el riesgo de refactorizar lo existente son demasiado altos.**

Y aquí los Sistemas Inteligentes pueden cambiar radicalmente la situación.

La pregunta deja de ser:

> “¿Cómo añado esta funcionalidad al sistema que tenemos?”

y puede empezar a ser:

> **“Tenemos esta nueva necesidad. Con todo el conocimiento que tenemos del sistema, ¿cuál sería ahora la mejor forma de resolverla?”**

Eso es muchísimo más interesante.

Porque el Sistema Inteligente puede estudiar el código existente, documentación, tests, decisiones, dependencias y comportamiento, y plantear algo que un humano muchas veces no puede permitirse hacer:

**“No añadamos otro parche. Rediseñemos esta parte.”**

Y entonces aparece una consecuencia brutal:

**Si el coste de entender y reconstruir el software baja lo suficiente, deja de ser obligatorio conservar todas las decisiones históricas.**

Eso puede cambiar la ingeniería de software mucho más que simplemente generar código más rápido.

Y el ejemplo de Quarto encaja perfectamente:

> “Tenemos una nueva necesidad: quiero meter un bloque de Quarto dentro de una clase.”

La respuesta tradicional puede ser:

> “Añadamos `::` y ya está.”

La respuesta que empieza a ser posible con Sistemas Inteligentes podría ser:

> “Espera. ¿Por qué necesitamos una excepción sintáctica para esto? Analicemos el modelo. Quizá la abstracción que tenemos ya no es la adecuada.”

**Ahí está el salto.**

No es *“la IA programa más rápido”*.

Es:

**“La IA puede hacer que refactorizar vuelva a ser una opción.”**

Rehaz y refactoriza

Sí. Yo **refactorizaría el artículo entero alrededor de esa idea**, porque es bastante más potente que el enfoque anterior.

El eje sería:

> **Tenemos una nueva necesidad. ¿Añadimos un parche o volvemos a pensar el sistema?**

Y desde ahí conectar con IASI y con el papel de los Sistemas Inteligentes.

### Cuando AI cambió las reglas del negocio

𝗧𝗘𝗡𝗘𝗠𝗢𝗦 𝗨𝗡𝗔 𝗡𝗨𝗘𝗩𝗔 𝗡𝗘𝗖𝗘𝗦𝗜𝗗𝗔𝗗.

Esta frase parece inocente.

Pero detrás de ella empieza muchas veces una historia que todos los que hacemos software conocemos demasiado bien.

Tenemos un sistema que funciona.

Tiene sus clases, sus interfaces, sus dependencias, sus reglas y sus años de historia.

Y ahora aparece una nueva necesidad.

La primera pregunta suele ser:

**¿Cómo podemos meter esto dentro de lo que ya tenemos sin romper nada?**

Y normalmente encontramos una respuesta.

Un pequeño cambio.

Una extensión.

Una excepción.

Un parámetro más.

Un caso especial.

Unos dobles puntos.

Funciona.

Y seguimos.

Hasta que llega la siguiente necesidad.

Y hacemos lo mismo otra vez.

No porque no sepamos hacerlo mejor.

Sino porque **nadie quiere tocar lo que ya funciona**.

El problema es que el software empieza a acumular no solamente funcionalidad, sino también las decisiones que hemos ido tomando para evitar modificarlo.

Y esas decisiones se convierten en arquitectura.

𝗘𝗟 𝗦𝗢𝗙𝗧𝗪𝗔𝗥𝗘 𝗔𝗖𝗔𝗕𝗔 𝗦𝗜𝗘𝗡𝗗𝗢 𝗨𝗡 𝗥𝗘𝗙𝗟𝗘𝗝𝗢 𝗗𝗘𝗟 𝗖𝗢𝗡𝗢𝗖𝗜𝗠𝗜𝗘𝗡𝗧𝗢 𝗗𝗘 𝗟𝗔𝗦 𝗣𝗘𝗥𝗦𝗢𝗡𝗔𝗦 𝗤𝗨𝗘 𝗟𝗢 𝗛𝗔𝗡 𝗖𝗢𝗡𝗦𝗧𝗥𝗨𝗜𝗗𝗢.

Las personas saben lo que saben.

Conocen una parte del sistema.

Conocen las razones de algunas decisiones.

Otras se han perdido.

Algunas decisiones fueron buenas en su momento.

Otras fueron simplemente las posibles dadas las circunstancias.

Y otras fueron parches que nunca llegaron a desaparecer.

Después de años, nadie tiene realmente en la cabeza todo el sistema.

Pero seguimos construyendo sobre él.

Porque reconstruirlo requiere algo que históricamente ha sido muy caro:

**entenderlo.**

Entender qué hace.

Por qué lo hace.

Qué depende de qué.

Qué se puede cambiar.

Qué se romperá si lo cambiamos.

Qué decisiones siguen teniendo sentido.

Y qué partes del diseño ya no responden a las necesidades actuales.

Por eso, cuando aparece una nueva necesidad, muchas veces no rediseñamos.

𝗣𝗔𝗥𝗖𝗛𝗔𝗠𝗢𝗦.

---

### Y aquí empieza a cambiar la historia

Imaginemos que ahora podemos decirle a un Sistema Inteligente:

**Tenemos una nueva necesidad.**

Y no le pedimos simplemente que escriba el código necesario para implementarla.

Le pedimos algo diferente:

**Analiza el sistema que tenemos y dime cuál sería la mejor forma de resolver esta necesidad.**

No solamente generar código.

Entender.

Analizar.

Relacionar.

Cuestionar.

Proponer.

Y, si es necesario:

**refactorizar.**

Porque quizá la mejor solución no sea añadir otra clase.

Quizá sea cambiar una abstracción.

Quizá sea eliminar una capa.

Quizá sea reconstruir una parte completa del sistema.

Quizá aquello que llevamos años extendiendo ya no tenga sentido.

Y aquí aparece una diferencia fundamental.

Hasta ahora, muchas decisiones de arquitectura estaban condicionadas por el coste de comprender y modificar el sistema existente.

**Si entenderlo cuesta demasiado, parcheamos.**

**Si refactorizar da demasiado miedo, extendemos.**

**Si reconstruir es demasiado caro, añadimos otra excepción.**

Pero ¿qué ocurre cuando el coste de entender el sistema disminuye radicalmente?

¿Qué ocurre cuando podemos disponer de un agente que conoce el código, la documentación, las pruebas, las dependencias, las decisiones y la evolución del sistema?

¿Qué ocurre cuando ese agente puede analizar alternativas y ayudarnos a reconstruir una parte del software en lugar de limitarse a añadir otra pieza?

𝗟𝗔 𝗥𝗘𝗙𝗔𝗖𝗧𝗢𝗥𝗜𝗭𝗔𝗖𝗜Ó𝗡 𝗣𝗨𝗘𝗗𝗘 𝗩𝗢𝗟𝗩𝗘𝗥 𝗔 𝗦𝗘𝗥 𝗨𝗡𝗔 𝗢𝗣𝗖𝗜Ó𝗡.

Y eso cambia mucho más que la velocidad de programación.

---

### Un ejemplo sencillo

Supongamos que tenemos una clase.

Llega una nueva necesidad:

**“Necesitamos incorporar un bloque de Quarto dentro de esta clase.”**

Podemos hacer lo habitual.

Añadimos una excepción.

Unos dobles puntos.

Un comportamiento especial.

Un pequeño parche.

Y seguimos.

Funciona.

Pero también podemos hacer otra cosa.

Preguntarnos:

**¿Por qué necesitamos meter un bloque de Quarto dentro de una clase?**

**¿Es realmente esa la abstracción que necesitamos?**

**¿Por qué la clase tiene que conocer ese concepto?**

**¿Hay una forma mejor de modelar lo que estamos intentando hacer?**

Y quizá descubramos que el problema no está en cómo añadir Quarto.

El problema está en que **la abstracción original ya no representa correctamente el problema que tenemos delante**.

Ese es el momento en el que refactorizar deja de ser una actividad de mantenimiento y vuelve a convertirse en una actividad de diseño.

---

### No se trata de tirar todo y empezar de cero

Tampoco se trata de eso.

No necesitamos que una IA llegue a una organización y diga:

**“He revisado vuestro código. Hay que reescribirlo entero.”**

Eso sería simplemente sustituir una mala decisión por otra.

El objetivo es poder tomar decisiones mejor informadas.

Conservar lo que funciona.

Entender lo que ya existe.

Identificar dónde la arquitectura empieza a convertirse en una colección de excepciones.

Y decidir, con evidencia, cuándo merece la pena cambiarla.

𝗟𝗔 𝗖𝗨𝗘𝗦𝗧𝗜Ó𝗡 𝗬𝗔 𝗡𝗢 𝗘𝗦 𝗦𝗜 𝗣𝗢𝗗𝗘𝗠𝗢𝗦 𝗔Ñ𝗔𝗗𝗜𝗥 𝗟𝗔 𝗡𝗨𝗘𝗩𝗔 𝗙𝗨𝗡𝗖𝗜𝗢𝗡𝗔𝗟𝗜𝗗𝗔𝗗.

La cuestión es:

**¿Cuál es ahora la mejor forma de construir el sistema que necesitamos?**

Incluso aunque eso implique cambiar el sistema que ya tenemos.

---

### Ingeniería aumentada por Sistemas Inteligentes

Esta es precisamente una de las cuestiones que queremos explorar en IASI.

No estamos interesados únicamente en utilizar Sistemas Inteligentes para producir código más rápidamente.

Eso es solamente una parte del problema.

Nos interesa entender qué ocurre cuando estos sistemas pasan a formar parte del proceso de ingeniería.

Cuando pueden analizar sistemas existentes.

Cuando pueden recuperar conocimiento que estaba disperso.

Cuando pueden ayudarnos a entender por qué el software es como es.

Cuando pueden proponer alternativas.

Cuando pueden detectar que una nueva necesidad está forzando una abstracción hasta romperla.

Y cuando pueden ayudarnos a hacer algo que durante años hemos ido evitando:

**volver a diseñar.**

Porque quizá el verdadero cambio no sea que las máquinas escriban más código.

Quizá sea que **podamos volver a permitirnos pensar en el software como algo que se puede reconstruir.**

---

𝗧𝗘𝗡𝗘𝗠𝗢𝗦 𝗨𝗡𝗔 𝗡𝗨𝗘𝗩𝗔 𝗡𝗘𝗖𝗘𝗦𝗜𝗗𝗔𝗗.

Hasta ahora, muchas veces preguntábamos:

**¿Cómo la metemos en lo que ya tenemos?**

Quizá la pregunta que debemos empezar a hacer sea otra:

**¿Cómo construiríamos hoy el sistema que realmente necesitamos?**

Y si la respuesta es diferente...

**refactoricemos.**

Porque si la implementación cada vez cuesta menos,

**el conocimiento cada vez vale más.**

Javier G. Grandez

IASI rocks. 🤘

esto a los chats exportados y al correo

Sí. **Esto hay que llevarlo también a los chats exportados y al correo**, porque ahí es donde está buena parte del conocimiento que luego necesitamos para entender por qué el software es como es.

Lo plantearía como una extensión natural del argumento:

> **El código no contiene todo el conocimiento del sistema.**
>
> Parte está en los chats, en los correos, en las discusiones, en las decisiones que se tomaron y en las razones por las que se tomaron.

Y eso refuerza muchísimo la idea de IASI.

Cuando aparece una nueva necesidad, el Sistema Inteligente no debería analizar solamente el repositorio. Debería poder recuperar también:

-  decisiones tomadas en chats; 
-  discusiones de diseño; 
-  correos; 
-  requisitos que fueron cambiando; 
-  problemas que se encontraron; 
-  soluciones que se descartaron; 
-  razones por las que se eligió una determinada arquitectura; 
-  conocimiento que nunca llegó a convertirse en documentación formal. 

Así, ante:

> **Tenemos una nueva necesidad.**

la pregunta podría ser:

> **¿Qué sabemos realmente sobre este sistema y por qué es como es?**

Y después:

> **Con todo ese conocimiento, ¿seguimos extendiéndolo o ha llegado el momento de refactorizarlo?**

Eso conecta directamente con la frase que acabamos de construir:

**Si la implementación cada vez cuesta menos, el conocimiento cada vez vale más.**

Porque el conocimiento ya no está únicamente en el código. Está **disperso por toda la organización**.

Y ahí creo que tenemos una línea especialmente potente para IASI:

**El primer trabajo de un Sistema Inteligente no debería ser escribir código. Debería ser recuperar el conocimiento necesario para decidir qué código merece la pena escribir.**