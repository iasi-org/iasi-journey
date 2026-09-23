# IASI Home, subproductos y despliegue

**Fecha:** 14 de agosto de 2026  
**Participantes:** Javier y Codex

## Contexto

Continuación del trabajo sobre `iasi-home`, `iasi-quarto-docs`, `iasi-lua`, `iasi-tools-dev` y sus manuales. La conversación se centró en integrar publicaciones externas dentro de la home, ordenar la navegación del ecosistema, revisar la nomenclatura y precisar el comportamiento de los comandos de despliegue.

## Documentación integrada en IASI Home

Se acordó que `iasi-home` actúa como portal del ecosistema. La documentación continúa viviendo y publicándose en sus repositorios originales, pero las URLs externas al propio sitio se consultan desde un visor integrado mediante `iframe`.

El visor incluye:

- contenido ocupando prácticamente todo el espacio disponible;
- enlace `↗` para abrir el sitio original en una pestaña nueva;
- validación de los repositorios externos permitidos;
- salida alternativa cuando el contenido no puede cargarse.

Se incorporaron al visor:

- IASI Quarto User Guide;
- IASI Quarto Technical Guide;
- IASI Lua User Guide;
- IASI Lua Technical Guide;
- IASI Tools Dev User Guide;
- IASI Tools Dev Technical Guide;
- IASI I.

Los libros II y III continúan apuntando a páginas pendientes porque todavía no tienen una publicación externa disponible.

## GitHub Pages y workflows

El despliegue inicial de `iasi-tools-dev-docs` fallaba porque GitHub Pages todavía no estaba habilitado en el repositorio. También se detectó que varios workflows conservaban versiones antiguas de las acciones de Pages.

Las versiones unificadas son:

```yaml
actions/configure-pages@v6
actions/upload-pages-artifact@v5
actions/deploy-pages@v5
```

La activación inicial de Pages debe realizarse desde la configuración del repositorio cuando el `GITHUB_TOKEN` del workflow no dispone de permiso administrativo para crear el sitio.

## Fecha de última publicación

La fecha ya se generaba en `publish/.publish`, pero las acciones actuales de GitHub Pages no incluyen archivos ocultos en el artefacto. Se añadió un paso que expone el valor como:

```text
publish/published-at.txt
```

El pie de `iasi-home` consulta ese archivo y muestra la fecha de última publicación.

## IASI Lua y sus extensiones

Se confirmó que IASI Lua contiene actualmente:

- `iasi-plantuml`, para diagramas PlantUML;
- `iasi-shiny`, para aplicaciones Shinylive de R ejecutadas en el navegador mediante webR;
- IASI Graphics, todavía pendiente.

La página de IASI Lua se actualizó para presentar PlantUML, Shiny y Graphics como sus tres extensiones.

## Productos, subproductos y artefactos

Se fijó la siguiente distinción conceptual:

- Los **productos** son los libros.
- Los **subproductos** son soluciones surgidas durante el desarrollo que adquieren identidad, reutilización y ciclo de vida propios.
- Los **artefactos** son elementos relacionados con IASI y con el funcionamiento interno del ecosistema.

La navbar utiliza ahora **Subproductos** en lugar de **Productos**. Su menú contiene:

- Todos;
- Herramientas;
- Extensiones;
- Servidores MCP.

Quarto no admite menús anidados dentro de un menú de navbar. Al intentarlo devuelve:

```text
"Herramientas" menu: this menu does not support sub-menus
```

Por ello, cada entrada se resolverá mediante una landing independiente con su propia subnavbar.

La landing **Todos** está en:

```text
pages/subproducts/index.qmd
```

El antiguo directorio `pages/products/` se renombró a `pages/subproducts/` para mantener una nomenclatura coherente.

La subnavbar actual de Todos contiene:

- IASI Quarto;
- IASI PlantUML;
- IASI Shiny;
- IASI Graphics.

El texto provisional fue sustituido por una explicación redactada por Javier sobre el origen de los subproductos. Al final de la página se añadió:

```text
pages/resources/images/iasi-banner.png
```

Se acordó además utilizar delimitadores diferenciados para los bloques Div anidados de Quarto, por ejemplo `:::`, `::::` y `:::::`.

## Modernización visual

Las tarjetas de `iasi-home` se modernizaron con un estilo más editorial:

- superficies suaves sin borde perimetral;
- acento de color superior;
- más espacio interior;
- jerarquía tipográfica más clara;
- sombras ligeras;
- movimiento discreto al pasar el cursor;
- adaptación a pantallas pequeñas.

La landing de IASI Quarto también se rediseñó con:

- portada visual;
- acceso directo a la User Guide y al repositorio;
- capacidades principales;
- workflow visible de cinco etapas;
- diferenciación clara entre construir y publicar;
- enlaces a User Guide, Technical Guide y código fuente.

## Navbar fija

Quarto activa por defecto `Headroom`, que oculta la navbar al desplazarse hacia abajo. Para mantenerla siempre visible en el portal se configuró:

```yaml
navbar:
  pinned: true
```

## Semántica de build, publish y deploy

Se aclaró el comportamiento real:

```text
build
  fuentes locales → _outputs/

publish
  _outputs/ → publish/

deploy
  estado local → commit → push
```

`publish` no construye las fuentes. Solo prepara `publish/` a partir de resultados previamente construidos.

Se modificó `iasi-dev deploy` para crear un único commit por repositorio:

```text
deploy
├── commit único del estado local completo
└── push
```

Con `--full`:

```text
deploy --full
├── build
├── publish
├── commit único del estado completo
└── push
```

El commit incluye conjuntamente fuentes, archivos derivados y `publish/`. La User Guide de IASI Tools Dev se actualizó para reflejar esta semántica.

Para cambios en fuentes, estilos, JavaScript o configuración de una publicación, el comando recomendado es:

```bash
iasi-dev deploy --full iasi-home
```

## Estado final

- El visor integrado funciona para documentación y libros externos publicados.
- La navbar permanece visible durante el desplazamiento.
- La clasificación entre productos, subproductos y artefactos está establecida.
- El directorio de páginas se llama `pages/subproducts/`.
- La landing Todos dispone de subnavbar y banner final.
- La landing de IASI Quarto tiene un diseño renovado.
- `iasi-dev deploy` genera un solo commit por repositorio.
- La web compila correctamente con el perfil HTML.
