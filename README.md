# RAW ZINE — Independent Visual Culture & Research

Proyecto de maquetación web desarrollado íntegramente con HTML5 semántico y CSS3 moderno bajo la metodología BEM (Block, Element, Modifier). El sitio presenta la identidad editorial y visual de RAW ZINE, una publicación independiente centrada en la investigación cultural, la autoproducción y la contracultura.

Proceso de Diseño (Figma & Figma Dev Mode)

El desarrollo del sitio web está inspirado íntegramente en mi wireframe y prototipo previo diseñado en Figma. Para trasladar el diseño al código con total precisión, utilicé Figma Dev Mode para inspeccionar y extraer directamente los parámetros clave:

* Iconografía vectorial: Extracción limpia del código de los iconos SVG para integrarlos directamente en el HTML.
* Palette: Obtención exacta de los códigos HEX de la paleta cromática (`#000000`, `#d4ff00`, `#1a1a1a`, `#ffffff`) para definir el sistema visual.
* Maquetación y espaciados: Consulta de dimensiones, márgenes, paddings, proporciones de cuadrícula y alineación de los elementos para reproducir fielmente la estructura.

------------------------------------------------------

(CSS)

Para reducir el número de peticiones HTTP, acelerar los tiempos de carga y mantener un código escalable, toda la base estructural y los componentes transversales del sitio se han unificado en una única hoja de estilos vinculada globalmente (`global.css`), acompañada únicamente por los estilos específicos de cada vista (`index.css`, `studio.css`, `aboutUs.css` , `artworks.css`):

* Variables Globales y Resets Base (`:root`):
  * Centralización de la paleta cromática (`--color-brand: #D4FF00`, `--color-black: #000000`, `--color-white: #FFFFFF`, `--color-surface: #ECE6F0`, `--color-card-bg: #2C2C2C`), familias tipográficas (`--font-brand`, `--font-primary`), pesos y espaciados (`--gap-grid`).
  * Normalización global (`box-sizing: border-box`, eliminación de márgenes residuales y control de desbordamiento `overflow-x: hidden`) junto con la clase de accesibilidad `.sr-only` para lectores de pantalla.
* Tipografías Locales en Formato WOFF2 (`@font-face`):
  * Integración local de Monomaniac One (Regular 400) e Inter (Regular 400, Medium 500 y Bold 700) en formato `.woff2` desde `../ASSETS/`, eliminando dependencias externas.
* Header y Menú Navegable sin JavaScript (`.header`, `.nav-overlay`):
  * Cabecera flexible con logotipo cuadrado en contraste invertido (`.header__logo`), buscador integrado (`.header__search`) y navegación horizontal en escritorio.
  * Menú móvil mediante "Checkbox Hack" (`:checked`): Transformación animada del icono hamburguesa en `X` fija, despliegue de capa superpuesta con desenfoque (`backdrop-filter: blur(4px)`), bloqueo del scroll del fondo (`body:has(.header__toggle:checked)`) y ocultación automática del menú cuando se abre una ventana modal (`body:has(:target) .header__hamburger`).
* Footer Estructurado y Adaptable (`.footer`):
  * Pie de página compartido compuesto por un bloque de marca en blanco puro (`.footer__brand`) con tipografía fluida (`clamp()`) y una cuadrícula de información (`.footer__container`) que pasa de 2×2 columnas en móvil a 4 columnas alineadas en desktop (`min-width: 992px`).
* Validación y Rendimiento:
  * Código validado en W3C HTML Validator y W3C CSS Validator, y auditado con Google Lighthouse (precarga de fuentes críticas con `rel="preload"`, reducción de hojas CSS globales a un solo archivo `global.css`, carga diferida de estilos no críticos y atributos `aria-label`).

----------------------------------------------------------

(HTML)

1. Homepage (`index.html`)
   * Página principal de presentación del proyecto editorial.
   * Integra el banner superior con efecto neón animado (`<h1>`), el *marquee* infinito de noticias, el carrusel 3D interactivo de artículos destacados con lightboxes a pantalla completa y el bloque de valores y métricas de la publicación (+300 Articles, +2.000 Authors, 100% Autoproduction).
2. About Us(`aboutUs.html`)**
   * Sección dedicada al manifiesto editorial (The Magazine, Roots, Ethics), la barra de valores fundamentales (Selected Network, Research, Auto Production, Against Discrimination, Cultural Purposes) y la cuadrícula de artículos destacados (`.studio-grid`).
3. Artworks(`artworks.html`)**
   * Página visual encabezada por una sección Hero a pantalla completa con imagen de fondo oscurecida mediante degradado (`linear-gradient`), llamadas a la acción hacia los artículos y la galería.
4. Studio (`studio.html`)
    * r


---------------------------------------------------------------------------

Componentes reutilizables

* Header & Navegación Responsive (`.header`, `.nav-overlay`):
  * Cabecera flexible reutilizable con logotipo cuadrado en contraste invertido (`.header__logo`), barra de búsqueda funcional con `<button type="submit">` accesible y menú navegable.
  * Menú móvil sin JavaScript (Checkbox Hack): Al activar `.header__toggle`, el icono hamburguesa se transforma en una `X` fija (`position: fixed`), despliega el menú superpuesto con desenfoque (`backdrop-filter: blur(4px)`) y bloquea el scroll del fondo mediante `body:has(.header__toggle:checked)`.
* Hero Banner Neón & Animaciones Tipográficas (`.hero-banner`, `.hero`):
  * Titular `<h1>` con efecto de brillo neón multicapa (`text-shadow`) y ciclo de desvanecimiento infinito de 5 segundos (`@keyframes neon-fade`), además de tipografía fluida con `clamp()`.
* Marquee Infinito (`.marquee`):
  * Cinta horizontal animada de noticias destacadas con elementos duplicados ocultos a lectores de pantalla (`aria-hidden="true"`)
* Carousel 3D Cilíndrico (`.carousel`):
  * Galería tridimensional construida con `transform-style: preserve-3d` y rotaciones en el eje Y (`rotateY`), con giro automático y pausa al pasar el cursor (`:hover`).
* Lightboxes Modales con `:target` (`.lightbox`):**
  * Ventanas modales a pantalla completa para la lectura de artículos sin necesidad de JavaScript.
  * Incluye ocultación automática de barras de desplazamiento (`scrollbar-width: none` / `::-webkit-scrollbar`) y ocultación inteligente del icono hamburguesa del header mientras una lightbox está activa (`body:has(:target)`).
* Barra de Valores y Métricas (`.values-bar`, `.metrics-bar`):**
  * Tarjetas interactivas con inversión de color y escalado al pasar el cursor, adaptadas mediante Media Queries para mantenerse alineadas tanto en escritorio como en tableta (iPad).
* Footer Estructurado (`.footer`):
  * Pie de página reutilizable organizado en columnas semánticas (`.footer__column`) con información de contacto, ubicación, enlaces legales y redes sociales.

-----------------------------------------------------

Cambios Realizados Respecto al Diseño Original

* Inversión cromática y efecto neón en la cabecera principal: Se sustituyó el bloque superior de fondo amarillo con texto oscuro por un fondo negro puro (`#000000`) con tipografía `<h1>` en amarillo fluo (`#d4ff00`), resplandor neón y animación de desvanecimiento continuo de 5 segundos.(`studio.html`)
* Eliminación de márgenes y espacios blancos residuales: Se ajustaron los resets de `body`, `header` y `.main-content` para unir la barra de navegación superior con el banner principal y los bordes laterales de la ventana.
* Mejoras de Semántica, Validación W3C y Accesibilidad:
  * Sustitución de `<a type="submit">` por `<button type="submit">` en el buscador.
  * Incorporación de encabezados `<h1>` por página y títulos `<h2>` ocultos visualmente mediante `.sr-only` en secciones estructurales para cumplir al 100% con el validador W3C.
  * Corrección de rutas relativas y caracteres tipográficos.
