# La etiqueta `<head>` — la guía SEO, GEO y redes sociales

🇫🇷 [Version française (originale) →](./README.md) · 🇬🇧 [English version →](./README.en.md)

**Todo lo que debe (y no debe) vivir en el `<head>` de tus páginas, clasificado por lo que te aporta: lo que lee Google, lo que leen las IA (ChatGPT, Perplexity, Gemini), lo que muestran las redes sociales y lo que determina la velocidad.** Con plantillas listas para copiar y los errores que cuestan posiciones. Por [Bernard David Corroy](https://www.david-corroy.com/), consultor SEO & GEO.

Las guías existentes enumeran las etiquetas. Esta te dice **cuáles importan, para quién y en qué orden** — porque un `<head>` no es un inventario: es tu tarjeta de presentación ante tres lectores: los buscadores, las IA y los humanos que comparten tus enlaces.

---

## 1. El mínimo vital — 6 líneas, sin excusas

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>El título de la página — 50 a 60 caracteres</title>
<meta name="description" content="La promesa de la página en 140-160 caracteres, con la palabra clave principal al inicio.">
<link rel="canonical" href="https://www.ejemplo.es/esta-pagina/">
<link rel="icon" href="/favicon.ico" sizes="32x32">
```

Tres reglas innegociables:

- `charset` en **primera posición** — el navegador solo lo busca en los primeros 1.024 bytes.
- `viewport` sin `user-scalable=no` ni `maximum-scale=1`: bloquear el zoom es un fallo de accesibilidad que Lighthouse penaliza.
- Un solo `<title>`, un solo `canonical`. Los duplicados (tema + plugin SEO escribiendo cada uno el suyo) envían señales contradictorias — y Google decide sin ti.

## 2. Lo que lee Google

### El título — el elemento más rentable del head

```html
<title>Palabra clave principal: la promesa — Marca</title>
```

- 50-60 caracteres visibles; más allá, corte a ~580 píxeles.
- La palabra clave al principio pesa más que al final.
- Un título por página, único en todo el sitio — los duplicados se diagnostican en Search Console.
- Google reescribe ~60 % de los títulos que considera malos: un título reescrito es un síntoma, no una fatalidad.

### La descripción — no es factor de posicionamiento, pero sí tu tasa de clics

```html
<meta name="description" content="Respuesta directa a la intención de búsqueda, una cifra o una prueba, una llamada implícita al clic. 140-160 caracteres.">
```

### Las directivas robots — el arma que mata en ambos sentidos

```html
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
```

- Un `noindex` olvidado tras el lanzamiento = el sitio entero invisible. **Es el error n.º 1 de las auditorías**, y pasa desapercibido durante meses.
- `max-image-preview:large` condiciona el formato grande en Google Discover.
- La cabecera HTTP `X-Robots-Tag` tiene prioridad sobre la etiqueta: ante un comportamiento inexplicable, revisa las dos.

### El canonical — una sola verdad por contenido

```html
<link rel="canonical" href="https://www.ejemplo.es/pagina/">
```

- URL **absoluta**, con el protocolo y el subdominio correctos (www o no — el que hayas elegido en todas partes).
- Autorreferente en las páginas normales: es lo que neutraliza los parámetros de tracking (`?utm_…`).
- Trampa clásica: un canonical apuntando a la home en todo el sitio (plugin mal configurado) = desindexación masiva.

### hreflang — solo si eres multilingüe

```html
<link rel="alternate" hreflang="es" href="https://www.ejemplo.es/pagina/">
<link rel="alternate" hreflang="en" href="https://www.ejemplo.com/pagina/">
<link rel="alternate" hreflang="x-default" href="https://www.ejemplo.com/pagina/">
```

Regla de oro: la reciprocidad. Si la página ES declara la página EN, la página EN debe declarar la página ES — si no, ambas declaraciones se ignoran.

## 3. Lo que leen las IA (la sección GEO que las demás guías no tienen)

Cuando ChatGPT, Perplexity o Gemini citan una página, se apoyan primero en lo que sus crawlers (GPTBot, ClaudeBot, PerplexityBot, Google-Extended) extraen de ella — y el `<head>` es su primera lectura.

### Tu título y tu descripción son tu pitch ante las IA

Los motores de respuesta suelen tomar título + descripción **tal cual** para decidir si la página merece una lectura completa. Un título vago («Inicio — Empresa García») te elimina antes incluso de analizar el contenido.

### El JSON-LD: decirles a las máquinas quién eres

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Nombre Apellido",
  "jobTitle": "Tu oficio preciso",
  "url": "https://www.ejemplo.es/",
  "sameAs": [
    "https://www.linkedin.com/in/tuperfil/",
    "https://www.wikidata.org/entity/QXXXXX",
    "https://github.com/tucuenta"
  ]
}
</script>
```

- El array `sameAs` es la columna vertebral de tu **entidad**: cada perfil coherente listado aumenta la confianza de las IA cuando hablan de ti.
- `Organization` o `Person` + `WebSite` en la home, `Article` en los contenidos, `FAQPage` donde haya preguntas y respuestas reales.
- Enlaza tus bloques entre sí con `@id` en lugar de repetir la misma información.

### Lo que NO se decide en el head (aunque todos lo busquen ahí)

- El acceso de los crawlers de IA se configura en `robots.txt` (GPTBot, ClaudeBot, PerplexityBot…), no en una meta.
- El archivo `llms.txt` (el mapa del sitio para las IA) vive en la raíz, como robots.txt.
- La etiqueta `meta keywords` está muerta desde 2009. Su presencia delata un sitio mal mantenido — elimínala.

## 4. Lo que muestran las redes — Open Graph y Twitter Cards

Cada vez que alguien comparte tu página sin imagen ni título propio, pierdes un clic. El bloque completo:

```html
<meta property="og:type" content="website">
<meta property="og:title" content="El título para compartir (puede diferir del título SEO)">
<meta property="og:description" content="La descripción para compartir.">
<meta property="og:url" content="https://www.ejemplo.es/pagina/">
<meta property="og:image" content="https://www.ejemplo.es/img/og-pagina.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:locale" content="es_ES">
<meta property="og:site_name" content="Tu marca">
<meta name="twitter:card" content="summary_large_image">
```

- Imagen: **1200 × 630 px**, menos de 300 KB, texto legible en miniatura. Una sola imagen bien hecha sirve en todas partes (LinkedIn, Facebook, X, WhatsApp, Slack, iMessage).
- URL de imagen **absoluta** — las URL relativas rompen la vista previa en la mitad de las plataformas.
- Prueba con el Post Inspector de LinkedIn y el validador de X: las cachés de compartición se purgan allí, no en tu sitio.

## 5. La velocidad — las etiquetas que ganan milisegundos

```html
<!-- Abrir pronto la conexión hacia un dominio de terceros crítico -->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<!-- Precargar LA recurso crítico (uno o dos, no diez) -->
<link rel="preload" href="/fonts/titulo.woff2" as="font" type="font/woff2" crossorigin>

<!-- Resolución DNS anticipada para dominios secundarios -->
<link rel="dns-prefetch" href="https://stats.ejemplo.es">
```

- `preload` es un bisturí, no una pistola de pintura: cada preload retrasa todo lo demás. Resérvalo para la fuente del título o la imagen LCP.
- Aloja tus fuentes **en tu propio servidor**: un viaje DNS + TLS a Google Fonts cuesta más que los bytes de la fuente.
- El CSS crítico sigue siendo bloqueante, y está bien así. Nunca pongas en async el CSS de algo visible al cargar (banner de cookies incluido) — hola, destello de contenido sin estilo.

## 6. Identidad visual — el set de favicons moderno

Cuatro archivos bastan en 2026:

```html
<link rel="icon" href="/favicon.ico" sizes="32x32">
<link rel="icon" href="/icon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="/apple-touch-icon.png"><!-- 180×180 -->
<link rel="manifest" href="/manifest.webmanifest">
```

- Google exige un favicon de al menos **48×48** (múltiplos de 48 recomendados) para mostrarlo en sus resultados — y puede tardar semanas en descubrirlo en un sitio reciente.
- `theme-color` tiñe la barra del navegador móvil: `<meta name="theme-color" content="#0d1220">`.

## 7. Las trampas caras — vistas en auditoría

| Trampa | Síntoma | Corrección |
|---|---|---|
| `noindex` de preproducción olvidado | Sitio invisible, cero impresiones en GSC | Revisar `robots` + X-Robots-Tag en CADA despliegue |
| Doble title / doble canonical | Google reescribe o ignora | Un solo emisor (el plugin SEO, no el tema) |
| Canonical hacia la home en todas partes | Todo desindexado salvo la home | Canonical autorreferente página a página |
| `og:image` ausente o relativa | Comparticiones sin visual | URL absoluta, 1200×630 |
| `charset` después de scripts | Caracteres rotos aleatorios | charset en la línea 1 del head |
| `user-scalable=no` | Penalización de accesibilidad | Dejar el zoom libre |
| 15 preload «por si acaso» | LCP peor, no mejor | 1-2 preload máximo, medidos |
| `meta keywords` todavía presente | Señal de sitio abandonado | Eliminar |
| hreflang sin reciprocidad | Declaraciones ignoradas | Cada idioma declara todos los demás |
| JSON-LD inválido (coma, comilla) | Resultados enriquecidos perdidos sin aviso | Validar en cada cambio (Rich Results Test) |

## 8. Plantillas listas para copiar

### Página de servicio / negocio

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Oficio + ciudad: la promesa — Marca</title>
  <meta name="description" content="Qué haces, para quién, dónde, con una cifra como prueba.">
  <meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
  <link rel="canonical" href="https://www.ejemplo.es/servicio/">
  <link rel="icon" href="/favicon.ico" sizes="32x32">
  <link rel="icon" href="/icon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">
  <meta property="og:type" content="website">
  <meta property="og:title" content="Oficio + ciudad: la promesa">
  <meta property="og:description" content="La versión para compartir de tu descripción.">
  <meta property="og:url" content="https://www.ejemplo.es/servicio/">
  <meta property="og:image" content="https://www.ejemplo.es/img/og-servicio.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:locale" content="es_ES">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "LocalBusiness",
    "name": "Marca", "url": "https://www.ejemplo.es/",
    "areaServed": "Tu zona", "sameAs": ["https://www.linkedin.com/company/…"] }
  </script>
</head>
```

### Artículo de blog

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>La pregunta que el artículo resuelve — Marca</title>
  <meta name="description" content="La respuesta en una frase, y luego lo que el lector sabrá hacer al final.">
  <meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
  <link rel="canonical" href="https://www.ejemplo.es/blog/articulo/">
  <link rel="icon" href="/favicon.ico" sizes="32x32">
  <meta property="og:type" content="article">
  <meta property="og:title" content="La pregunta que el artículo resuelve">
  <meta property="og:image" content="https://www.ejemplo.es/img/og-articulo.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "Article",
    "headline": "La pregunta que el artículo resuelve",
    "datePublished": "2026-07-16", "dateModified": "2026-07-16",
    "author": { "@type": "Person", "name": "Nombre Apellido", "url": "https://www.ejemplo.es/sobre-mi/" } }
  </script>
</head>
```

## 9. La checklist de salida a producción

- [ ] `charset` + `viewport` al principio, zoom libre
- [ ] `<title>` único, 50-60 caracteres, palabra clave al inicio
- [ ] descripción de 140-160 caracteres, orientada al clic
- [ ] `robots` = index,follow (y NO el noindex de preproducción)
- [ ] canonical absoluto y autorreferente
- [ ] OG completo + imagen 1200×630 absoluta + twitter:card
- [ ] JSON-LD válido, `sameAs` coherente con tus perfiles reales
- [ ] favicon ≥ 48×48 + SVG + apple-touch-icon
- [ ] preconnect/preload: solo lo crítico, medido
- [ ] hreflang recíproco (si eres multilingüe)
- [ ] cero `meta keywords`, cero etiquetas duplicadas

---

## Créditos y licencia

Redacción 100 % original. La idea de un repositorio dedicado al `<head>` la popularizó [HEAD de Josh Buchea](https://github.com/joshbuchea/HEAD) — ve allí para el inventario exhaustivo de etiquetas; ven aquí para saber cuáles importan y por qué.

Licencia **CC BY 4.0**: reutilización libre a condición de acreditar «Bernard David Corroy — [david-corroy.com](https://www.david-corroy.com/)».

**El autor**: Bernard David Corroy, consultor SEO & GEO independiente — [Web](https://www.david-corroy.com/) · [Auditoría exprés gratuita](https://www.david-corroy.com/audit-express/) (FR) · [Prompts GEO](https://github.com/david-corroy/prompts-geo) · [LinkedIn](https://www.linkedin.com/in/bernard-david-corroy/) · [Wikidata](https://www.wikidata.org/entity/Q140472682)
