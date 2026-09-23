# Conversación: documentación IASI, navegación y publicación

Exportada el 14 de agosto de 2026 desde la sesión local de Codex.

## Usuario

en docs-template, para pdf
en la portada, arriba pondriamos la imagen ants-ciegas.png de resources/images

## Codex

Añadida `ants-ciegas.png` en la parte superior de la portada PDF de ambas guías:

- [Guía de usuario](/C:/iasi-org/iasi-docs-template/01-user-guide/_quarto.yml)
- [Guía técnica](/C:/iasi-org/iasi-docs-template/02-technical-guide/_quarto.yml)

La imagen ocupa el 38 % del ancho del texto y solo afecta al PDF. He usado `\titlehead` de `scrreprt`, siguiendo el flujo de maquetación PDF. No pude renderizar para revisión visual porque Quarto no está instalado en este entorno.

## Usuario

no la imagen debe ocupar todo el ancho, sin margenes

## Codex

Hecho. La imagen ahora ocupa todo el ancho del papel (`\paperwidth`), pegada a los bordes superior, izquierdo y derecho, sin márgenes.

Aplicado tanto a la guía de usuario como a la técnica. No he podido renderizar el PDF porque Quarto no está disponible en este entorno.

## Usuario

los links del indice no van coloreados

## Codex

Hecho. Los enlaces internos del índice PDF siguen siendo clicables, pero ahora aparecen en negro, sin colorear.

Aplicado a las guías de usuario y técnica de `iasi-docs-template`.

## Usuario

se muestren SIN COLOR

## Codex

Corregido: `colorlinks: false`.

Los enlaces del índice continúan funcionando, pero se muestran como texto normal, sin ningún color ni decoración de enlace. Aplicado a ambas guías PDF.

## Usuario

vale, eso  ya tiene buena pinta

## Codex

Perfecto. La portada queda a ancho completo y el índice mantiene los enlaces clicables sin color.

## Usuario

ahora podemos aplicarlo a iasi-quarto-docs

## Codex

Aplicado en `iasi-quarto-docs`, tanto a la guía de usuario como a la técnica:

- Imagen `ants-ciegas.png` a todo el ancho y sin márgenes en la portada PDF.
- Enlaces del índice clicables, pero sin color (`colorlinks: false`).
- Recurso gráfico copiado en ambas guías.

Configuraciones: [guía de usuario](/C:/iasi-org/iasi-quarto-docs/01-user-guide/_quarto.yml) y [guía técnica](/C:/iasi-org/iasi-quarto-docs/02-technical-guide/_quarto.yml).

No he renderizado los PDF porque Quarto no está disponible en este entorno.

## Usuario

en el htmo no sale el banner superior

## Codex

Corregido. El banner ahora también está configurado para HTML en ambas guías:

- Se muestra antes del contenido.
- Ocupa todo el ancho de la ventana.
- No tiene márgenes laterales.
- Mantiene la proporción original de la imagen.
- Aparece en todas las páginas HTML.

No he regenerado el sitio porque Quarto no está disponible en este entorno.

## Usuario

ni la navbar

## Codex

Corregido. Añadida la navbar HTML en ambas guías con:

- Introducción
- Fundamentos
- Instalación
- PlantUML
- Selector de idioma
- Enlace de contacto

Las rutas de todos los apartados están verificadas. El banner aparecerá encima de esta navbar al renderizar con el perfil HTML.

## Usuario

estaba pensando, tal vez, en lugar de descarga del pdf, podriamos ponerlo como icono el navbar

## Codex

Hecho. He sustituido la descarga automática por un icono PDF en la parte derecha de la navbar:

- Cada guía enlaza con su PDF correspondiente.
- El icono aparece antes de idioma y contacto.
- Incluye la etiqueta accesible «Descargar guía en PDF».
- Eliminado `downloads: [pdf]` de ambas configuraciones principales.

## Usuario

asume que todo esta en publish, aunque falle si solo hacemos build

## Codex

De acuerdo. Tomamos `publish/` como artefacto final integrado.

Los enlaces relativos actuales son correctos:

- `publish/user-guide/IASI-Quarto-User-Guide.pdf`
- `publish/technical-guide/IASI-Quarto-Technical-Guide.pdf`

Aceptamos que el icono pueda fallar en un build HTML aislado; funcionará al publicar el conjunto completo.

## Usuario

voy a verlo

## Codex

Perfecto. Revísalo en `publish/`; ahí deberían funcionar juntos el banner, la navbar y el icono del PDF.

## Usuario

en iasi-quarto deberiamos soportar dir1/dir2 para procesar dir2

## Usuario

perdon, iasi-dev

## Codex

Implementado en `iasi-dev`.

Ahora:

```bash
iasi-dev build dir1/dir2
iasi-dev publish dir1/dir2
```

mantienen `dir2` como ámbito de procesamiento. Se descubren únicamente las publicaciones contenidas en ese directorio, sin ascender al repositorio Git `dir1`.

El cambio está en [publish.sh](/C:/iasi-org/iasi-tools-dev/lib/commands/publish.sh) y también queda documentado en el README. Validé la sintaxis Bash y un caso real de ruta anidada: procesó solo `dir2` y su subproyecto.

## Usuario

user-guide sigue sin sacar navbar ¿igual no he cogido la ultima version?

## Codex

Sí: estás viendo una publicación anterior.

La configuración con navbar se modificó a las **15:22**, pero tanto `_outputs/html/index.html` como `publish/user-guide/index.html` se generaron a las **15:15**. El HTML publicado todavía contiene incluso el antiguo botón automático de descarga PDF.

Necesitas volver a ejecutar:

```bash
iasi-dev build iasi-quarto-docs/01-user-guide
iasi-dev publish iasi-quarto-docs/01-user-guide
```

Después deberían aparecer la navbar nueva, el banner y el icono PDF. No parece un problema de versión de `iasi-dev`, sino que `publish/` aún no se ha regenerado con la configuración actual.

## Usuario

volvemos a la parte home
de acuerdo con los proyectos y sub proyectos hay que considerar los links correctos a los manuales

## Codex

Actualizados los enlaces de manuales en `iasi-home` según los subproyectos realmente publicados:

- `iasi.quarto`: User Guide y Technical Guide.
- `iasi-lua`: User Guide y Technical Guide.
- `iasi-tools-dev`: añadidos ambos manuales al catálogo y a su ficha.
- `iasi-rstudio`: sustituido el enlace roto por “Documentación prevista”, porque su repositorio de documentación aún no contiene manuales.

También añadí el Technical Guide que faltaba en la ficha de `iasi-lua`. Verifiqué que los seis destinos publicados existen en sus respectivos directorios `publish/`. No he regenerado todavía `iasi-home/publish`.

## Usuario

bueno, los iremos testeando y corrigiendo

## Codex

Perfecto. Los probamos desde `publish/` y vamos corrigiendo rutas, navegación y presentación según aparezcan diferencias reales.

## Usuario

¿me has corregido lo de barra de navegacion en iasi-quarto-docs?

## Codex

Sí. En `iasi-quarto-docs` añadí la navbar para ambas guías dentro de sus perfiles HTML:

- [User Guide](/C:/iasi-org/iasi-quarto-docs/01-user-guide/_quarto-html.yml)
- [Technical Guide](/C:/iasi-org/iasi-quarto-docs/02-technical-guide/_quarto-html.yml)

Incluye Introducción, Fundamentos, Instalación, PlantUML y los iconos de PDF, idioma y contacto.

Eso sí: todavía necesitas volver a ejecutar `build` y `publish`, porque el contenido actual de `publish/` se generó antes del cambio.

## Usuario

y el banner de cabecera

## Codex

Sí, también está añadido en ambas guías HTML:

- Usa `ants-ciegas.png`.
- Ocupa todo el ancho de la ventana.
- No tiene márgenes laterales.
- Aparece antes de la navbar y del contenido.

Está configurado en los perfiles `_quarto-html.yml`, con sus archivos `resources/html/top-banner.html` y `resources/css/top-banner.css`.

Para verlo hay que regenerar `build` y `publish`; el HTML publicado actual todavía es anterior a esos cambios.

## Usuario

el banner no es ants-ciegas, es ants y deber ir encima de la navbar, como en la plantilla

## Codex

Corregido para replicar la plantilla:

- El banner HTML ahora usa `ants.png`.
- Está implementado como anuncio de Quarto con `position: above-navbar`.
- Queda situado encima de la navbar.
- Se copiaron los mismos estilos y ajuste de rutas utilizados por `iasi-docs-template`.
- Eliminado el banner HTML personalizado que usaba `ants-ciegas.png`.

`ants-ciegas.png` se mantiene únicamente para la portada PDF. Debes volver a ejecutar `build` y `publish` para verlo.

## Usuario

pues ahora pega una revision al estilo y los docs de iasi-quarto-docs/01-user-guide
incoherencias, ambiguedades, ausencias, ....

## Codex

La revisión confirma que la guía necesita una reorganización importante antes de pulir detalles menores. No he modificado archivos.

## Hallazgos críticos

1. La API pública documentada está incompleta.

La guía afirma que solo se exponen `validate()` y `build()` en [06-public-api.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/02-fundamentals/06-public-api.qmd:5). Sin embargo, el paquete exporta también `publish()` y `deploy()` en [NAMESPACE](/C:/iasi-org/iasi-quarto/NAMESPACE).

Esto deja fuera justamente el recorrido operativo completo:

```text
validate → build → publish → deploy
```

Deberían añadirse capítulos orientados al usuario para `publish()` y `deploy()`, incluyendo la relación entre `_outputs/` y `publish/`.

2. User Guide y Technical Guide son actualmente el mismo documento.

Los 26 archivos `.qmd` de ambas guías son idénticos. Esto contradice la separación editorial entre una guía de uso y otra técnica.

Además, la User Guide dice que no exige conocer la arquitectura interna en [index.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/index.qmd:7), pero dedica unas 740 líneas a `.discover()`, `.check()`, `.prepare()` y `.render()` en [05-lifecycle.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/02-fundamentals/05-lifecycle.qmd:11).

Mi recomendación:

- User Guide: instalación, creación, configuración, build, publish, deploy y resolución de problemas.
- Technical Guide: modelo interno, objetos, pipeline privado, invariantes y estrategias de implementación.

3. Falta el tutorial prometido para crear el primer proyecto.

La introducción promete “crear un nuevo proyecto” en [index.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/index.qmd:25), pero no existe un recorrido completo y copiable que parta de un directorio vacío.

Falta un quick start con:

```text
crear directorios
crear _quarto.yml
crear _iasi.yml
crear perfiles HTML/PDF
crear index.qmd y un capítulo
validate()
build()
publish()
```

Actualmente el lector recibe mucha explicación conceptual antes de poder obtener un resultado.

4. La configuración documentada de PlantUML no coincide con el proyecto real.

La guía indica:

```yaml
filters:
  - extensions/plantuml/plantuml.lua
```

en [04-configuration.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/04-plantuml/04-configuration.qmd:7), pero el proyecto usa:

```yaml
filters:
  - iasi-plantuml
```

y la extensión está instalada bajo `_extensions/iasi-plantuml`.

La estructura `extensions/core/` descrita desde la [línea 44](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/04-plantuml/04-configuration.qmd:44) tampoco representa la distribución actual.

5. Hay una contradicción de puertos PlantUML.

La guía establece `localhost:1025` como instalación de referencia en [03-server.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/04-plantuml/03-server.qmd:92), mientras que el propio proyecto está configurado con `localhost:10025` en [_quarto.yml](/C:/iasi-org/iasi-quarto-docs/01-user-guide/_quarto.yml).

Debe elegirse una sola convención y aplicarla a:

- Docker;
- Docker Compose;
- `_quarto.yml`;
- ejemplos;
- troubleshooting;
- configuración predeterminada de la extensión.

## Ausencias importantes

6. No se documenta `publication.html.landing-page`.

La implementación actual soporta publicaciones `book` cuyo HTML efectivo se convierte en `website` mediante:

```yaml
publication:
  html:
    landing-page: true
```

La User Guide no menciona esta capacidad. Es especialmente relevante porque cambia la naturaleza del HTML sin cambiar el PDF.

7. No hay una explicación clara del resultado publicado.

Se explica `build()`, pero no queda claro para un usuario:

- dónde aparece cada salida;
- qué contiene `_outputs/html`;
- dónde queda el PDF;
- cómo se ensambla `publish/`;
- cómo se estructura un multiproyecto publicado;
- qué enlaces funcionan solamente después de `publish()`.

Este vacío está relacionado con la ausencia de `publish()` y ha provocado precisamente varias dudas durante nuestras pruebas recientes.

8. La versión mostrada está desactualizada.

La guía utiliza `0.5.0` como resultado esperado en [04-verify.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/03-installation/04-verify.qmd:30), mientras que el paquete actual declara `0.5.1`.

Conviene evitar resultados rígidos o formularlos así:

```r
packageVersion("iasi.quarto")
# Debe ser igual o posterior a la versión mínima documentada.
```

## Ambigüedades y estilo

9. Algunas capacidades reales se presentan como posibilidades inciertas.

Por ejemplo:

- “La extensión puede admitir `caption`...” en [08-parameters.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/04-plantuml/08-parameters.qmd:364).
- “Puede admitirse `label`...” en la [línea 382](/C:/iasi-org/iasi-quarto-docs/01-user-guide/chapters/04-plantuml/08-parameters.qmd:382).

La implementación sí procesa ambos atributos. Debe decirse de manera inequívoca:

- `caption` es un alias admitido de `fig-cap`;
- `label` está admitido, aunque se recomienda `#fig-id`.

10. La introducción repite demasiado el mismo mensaje.

[index.qmd](/C:/iasi-org/iasi-quarto-docs/01-user-guide/index.qmd:11), `00-index.qmd` y `01-what-is.qmd` explican varias veces que Quarto renderiza e `iasi.quarto` organiza.

Podrían condensarse en:

- una introducción breve;
- un esquema de responsabilidades;
- un quick start inmediato.

11. La densidad no corresponde a una User Guide.

Solo “Fundamentos” suma más de 3.300 líneas y PlantUML supera las 4.200. Hay explicaciones excelentes, pero muchas son material de referencia o de guía técnica.

Para mejorar la navegación:

- mover detalles internos a Technical Guide;
- crear tablas de referencia separadas;
- dejar en User Guide flujos orientados a tareas;
- añadir “Qué conseguirás” y “Siguiente paso” por capítulo;
- reducir ejemplos repetidos de `build()` y configuración YAML.

12. El alcance de PlantUML resulta ambiguo.

PlantUML ocupa casi la mitad de la guía de `iasi.quarto`, aunque la extensión pertenece conceptualmente a `iasi-lua`. Debe decidirse explícitamente si:

- aquí solo se documenta cómo consumir la extensión, enlazando al manual completo de `iasi-lua`; o
- `iasi-quarto` asume oficialmente su instalación y soporte.

Ahora mismo parece simultáneamente una capacidad de `iasi.quarto` y una extensión externa.

## Prioridad recomendada

Yo lo abordaría en este orden:

1. Corregir errores factuales de API, filtro y puerto.
2. Añadir `publish()` y `deploy()`.
3. Crear un quick start completo.
4. Separar User Guide y Technical Guide.
5. Decidir el alcance de PlantUML.
6. Añadir landing pages y multipublicación.
7. Reducir repeticiones y actualizar versión.
8. Hacer después una revisión lingüística y visual fina.

La base conceptual es sólida y los ejemplos son abundantes. El problema principal no es falta de contenido, sino que la User Guide mezcla uso, referencia exhaustiva e implementación interna sin una frontera editorial clara.

## Usuario

descargame este chat

