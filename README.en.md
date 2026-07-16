# The `<head>` tag — the SEO, GEO & social guide

🇫🇷 [Version française (originale) →](./README.md) · 🇪🇸 [Versión en español →](./README.es.md)

**Everything that should (and shouldn't) live in your pages' `<head>`, sorted by what it earns you: what Google reads, what AI engines read (ChatGPT, Perplexity, Gemini), what social networks display, and what drives speed.** With copy-paste templates and the mistakes that cost rankings. By [Bernard David Corroy](https://www.david-corroy.com/), SEO & GEO consultant.

Existing guides list the tags. This one tells you **which ones matter, for whom, and in what order** — because a `<head>` is not an inventory, it's your calling card for three readers: search engines, AI engines, and the humans who share your links.

---

## 1. The vital minimum — 6 lines, no excuses

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>The page title — 50 to 60 characters</title>
<meta name="description" content="The page's promise in 140-160 characters, main keyword up front.">
<link rel="canonical" href="https://www.example.com/this-page/">
<link rel="icon" href="/favicon.ico" sizes="32x32">
```

Three non-negotiable rules:

- `charset` comes **first** — browsers only look for it within the first 1,024 bytes.
- `viewport` without `user-scalable=no` or `maximum-scale=1`: blocking zoom is an accessibility failure that Lighthouse penalizes.
- One `<title>`, one `canonical`. Duplicates (theme + SEO plugin each writing their own) send contradictory signals — and Google decides without you.

## 2. What Google reads

### The title — the highest-ROI element in the head

```html
<title>Main keyword: the promise — Brand</title>
```

- 50-60 characters displayed; beyond that, truncation at ~580 pixels.
- A keyword at the beginning weighs more than at the end.
- One title per page, unique across the site — duplicates show up in Search Console.
- Google rewrites ~60% of titles it judges poor: a rewritten title is a symptom, not fate.

### The description — not a ranking factor, but your click-through rate

```html
<meta name="description" content="Direct answer to the search intent, a number or a proof, an implicit call to click. 140-160 characters.">
```

### Robots directives — the weapon that kills both ways

```html
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
```

- A forgotten `noindex` after going live = the whole site invisible. **It's the #1 mistake found in audits**, and it goes unnoticed for months.
- `max-image-preview:large` gates large-format display in Google Discover.
- The `X-Robots-Tag` HTTP header overrides the meta tag: when behavior makes no sense, check both.

### The canonical — one truth per piece of content

```html
<link rel="canonical" href="https://www.example.com/page/">
```

- **Absolute** URL, with the right protocol and subdomain (www or not — whichever you chose everywhere).
- Self-referencing on normal pages: that's what neutralizes tracking parameters (`?utm_…`).
- Classic trap: a canonical pointing to the homepage on every page (plugin misconfiguration) = mass deindexing.

### hreflang — only if you're multilingual

```html
<link rel="alternate" hreflang="en" href="https://www.example.com/page/">
<link rel="alternate" hreflang="fr" href="https://www.example.fr/page/">
<link rel="alternate" hreflang="x-default" href="https://www.example.com/page/">
```

Golden rule: reciprocity. If the EN page declares the FR page, the FR page must declare the EN page back — otherwise both declarations are ignored.

## 3. What AI engines read (the GEO section other guides don't have)

When ChatGPT, Perplexity or Gemini cite a page, they rely first on what their crawlers (GPTBot, ClaudeBot, PerplexityBot, Google-Extended) extract from it — and the `<head>` is their first read.

### Your title and description are your pitch to the AIs

Answer engines often take title + description **verbatim** to decide whether the page deserves a full read. A vague title ("Home — Smith & Co") eliminates you before your content is even parsed.

### JSON-LD: telling machines who you are

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "First Last",
  "jobTitle": "Your precise job",
  "url": "https://www.example.com/",
  "sameAs": [
    "https://www.linkedin.com/in/yourprofile/",
    "https://www.wikidata.org/entity/QXXXXX",
    "https://github.com/youraccount"
  ]
}
</script>
```

- The `sameAs` array is the backbone of your **entity**: every consistent profile listed raises AI confidence when they talk about you.
- `Organization` or `Person` + `WebSite` on the homepage, `Article` on content, `FAQPage` where real Q&As exist.
- Link your blocks together with `@id` instead of repeating the same information.

### What is NOT decided in the head (but people look for there anyway)

- AI crawler access is controlled in `robots.txt` (GPTBot, ClaudeBot, PerplexityBot…), not in a meta tag.
- The `llms.txt` file (the sitemap for AIs) lives at the root, like robots.txt.
- The `meta keywords` tag has been dead since 2009. Its presence signals a poorly maintained site — remove it.

## 4. What social networks display — Open Graph & Twitter Cards

Every share without its own image and title is a lost click. The complete block:

```html
<meta property="og:type" content="website">
<meta property="og:title" content="The share title (may differ from the SEO title)">
<meta property="og:description" content="The share description.">
<meta property="og:url" content="https://www.example.com/page/">
<meta property="og:image" content="https://www.example.com/img/og-page.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:locale" content="en_US">
<meta property="og:site_name" content="Your brand">
<meta name="twitter:card" content="summary_large_image">
```

- Image: **1200 × 630 px**, under 300 KB, text readable as a thumbnail. One well-made image works everywhere (LinkedIn, Facebook, X, WhatsApp, Slack, iMessage).
- **Absolute** image URL — relative URLs break the preview on half the platforms.
- Test with LinkedIn's Post Inspector and X's card validator: share caches are purged there, not on your site.

## 5. Speed — the tags that earn milliseconds

```html
<!-- Open the connection early to a critical third-party domain -->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<!-- Preload THE critical resource (one or two, not ten) -->
<link rel="preload" href="/fonts/heading.woff2" as="font" type="font/woff2" crossorigin>

<!-- Early DNS resolution for secondary domains -->
<link rel="dns-prefetch" href="https://stats.example.com">
```

- `preload` is a scalpel, not a paint gun: every preload delays everything else. Reserve it for the heading font or the LCP image.
- Host your fonts **yourself**: a DNS + TLS round-trip to Google Fonts costs more than the font's bytes.
- Critical CSS stays render-blocking, and that's fine. Never async the CSS of anything visible at load (cookie banner included) — hello flash of unstyled content.

## 6. Visual identity — the modern favicon set

Four files are enough in 2026:

```html
<link rel="icon" href="/favicon.ico" sizes="32x32">
<link rel="icon" href="/icon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="/apple-touch-icon.png"><!-- 180×180 -->
<link rel="manifest" href="/manifest.webmanifest">
```

- Google requires a favicon of at least **48×48** (multiples of 48 recommended) to show it in search results — and it can take weeks to discover it on a recent site.
- `theme-color` tints the mobile browser bar: `<meta name="theme-color" content="#0d1220">`.

## 7. The expensive traps — seen in audits

| Trap | Symptom | Fix |
|---|---|---|
| Staging `noindex` left in place | Site invisible, zero GSC impressions | Check `robots` + X-Robots-Tag at EVERY deploy |
| Double title / double canonical | Google rewrites or ignores | One emitter only (the SEO plugin, not the theme) |
| Canonical to the homepage everywhere | Everything deindexed except home | Self-referencing canonical per page |
| Missing or relative `og:image` | Shares without a visual | Absolute URL, 1200×630 |
| `charset` after scripts | Random broken characters | charset on line 1 of the head |
| `user-scalable=no` | Accessibility penalty | Leave zoom free |
| 15 "just in case" preloads | LCP worse, not better | 1-2 preloads max, measured |
| `meta keywords` still present | Abandoned-site signal | Remove |
| hreflang without reciprocity | Declarations ignored | Every language declares all the others |
| Invalid JSON-LD (comma, quote) | Rich results silently lost | Validate on every change (Rich Results Test) |

## 8. Copy-paste templates

### Service / business page

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Trade + city: the promise — Brand</title>
  <meta name="description" content="What you do, for whom, where, with one number as proof.">
  <meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
  <link rel="canonical" href="https://www.example.com/service/">
  <link rel="icon" href="/favicon.ico" sizes="32x32">
  <link rel="icon" href="/icon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">
  <meta property="og:type" content="website">
  <meta property="og:title" content="Trade + city: the promise">
  <meta property="og:description" content="The share version of your description.">
  <meta property="og:url" content="https://www.example.com/service/">
  <meta property="og:image" content="https://www.example.com/img/og-service.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:locale" content="en_US">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "LocalBusiness",
    "name": "Brand", "url": "https://www.example.com/",
    "areaServed": "Your area", "sameAs": ["https://www.linkedin.com/company/…"] }
  </script>
</head>
```

### Blog article

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>The question the article answers — Brand</title>
  <meta name="description" content="The answer in one sentence, then what the reader will know how to do.">
  <meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
  <link rel="canonical" href="https://www.example.com/blog/article/">
  <link rel="icon" href="/favicon.ico" sizes="32x32">
  <meta property="og:type" content="article">
  <meta property="og:title" content="The question the article answers">
  <meta property="og:image" content="https://www.example.com/img/og-article.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "Article",
    "headline": "The question the article answers",
    "datePublished": "2026-07-16", "dateModified": "2026-07-16",
    "author": { "@type": "Person", "name": "First Last", "url": "https://www.example.com/about/" } }
  </script>
</head>
```

## 9. The complete inventory — every remaining tag, and what it's actually worth

Everything else that can live in a `<head>`, beyond the essentials of sections 1-6 — sorted into three verdicts: ✅ useful in specific cases, 🟠 niche, ❌ dead (remove it on sight).

### Identity & discovery

| Tag | Role | Verdict |
|---|---|---|
| `<link rel="me" href="…">` | Cross-ownership proof of your profiles (Mastodon verifies it) — a free **entity** signal | ✅ point it at your key profiles |
| `<meta name="fediverse:creator" content="@you@instance">` | Credits the author on Mastodon/Fediverse shares | ✅ if you're active there |
| `<link rel="alternate" type="application/rss+xml" href="/feed/">` | Declares your RSS/Atom feed — still read by aggregators AND some AI crawlers | ✅ if you publish regularly |
| `<meta name="google-site-verification">` / `msvalidate.01` | GSC / Bing Webmaster verification — prefer the DNS method to keep the head lean | 🟠 works, but DNS is cleaner |
| `<link rel="pingback">` / `rel="webmention"` | Blog-to-blog notifications | 🟠 niche (IndieWeb) |
| `<link rel="search" type="application/opensearchdescription+xml">` | OpenSearch | ❌ abandoned by browsers |
| `<meta name="generator" content="WordPress 6.x">` | Advertises your CMS and its version | ❌ remove: free intel for attackers |
| `rel="author"` / `rel="publisher"` | Ex-Google+ links | ❌ died with Google+ |
| `<meta name="geo.position">` / `ICBM` | Meta geolocation | ❌ ignored by every engine — local SEO happens in your Google profile and JSON-LD |

### Mobile, PWA & dressing

| Tag | Role | Verdict |
|---|---|---|
| `<meta name="theme-color" media="(prefers-color-scheme: dark)" content="…">` | Different browser-bar color in dark mode | ✅ clean finishing touch |
| `<meta name="color-scheme" content="light dark">` | Announces the page handles both modes (avoids the white flash in dark mode) | ✅ if your site has a dark mode |
| `<meta name="format-detection" content="telephone=no">` | Stops iOS from turning every digit string into a phone link | 🟠 useful on pages with references/prices |
| `apple-mobile-web-app-capable` / `-status-bar-style` / `-title` | Dressing when the site is pinned to the iOS home screen | 🟠 PWA only |
| `<link rel="apple-touch-startup-image">` | iOS PWA launch screen (one variant per screen size…) | 🟠 demanding PWA |
| `msapplication-TileColor` + `browserconfig.xml` | Windows 8/10 tiles | ❌ legacy, zero impact left |

### Advanced performance & security

| Tag | Role | Verdict |
|---|---|---|
| `<script type="speculationrules">` | The official successor to prefetch/prerender: pre-renders likely next pages (near-instant navigation) | ✅ the real 2026 lever, Chrome/Edge |
| `<link rel="modulepreload" href="/app.js">` | Preloads an ES module with its dependencies | ✅ modular-JS sites |
| `<link rel="prefetch">` / `rel="prerender"` | The old speculation hints | 🟠 superseded by speculationrules |
| `integrity="sha384-…" crossorigin` on external scripts | SRI: a tampered third-party script is rejected | ✅ whenever a script comes from a CDN |
| `<meta name="referrer" content="strict-origin-when-cross-origin">` | Referrer policy (the HTTP header remains preferable) | 🟠 fine as a fallback |
| `<meta http-equiv="Content-Security-Policy">` | CSP as a meta — no `frame-ancestors`, no reporting in meta form | 🟠 the HTTP header does it better |
| `<base href="…">` | Redefines the base of ALL relative URLs (links, images, scripts…) | ❌ classic trap — silent breakage, avoid |
| `<meta http-equiv="refresh" content="5;url=…">` | Client-side redirect | ❌ always prefer a server-side 301 |

### Extended Open Graph

| Tag | Role | Verdict |
|---|---|---|
| `article:published_time` / `article:modified_time` / `article:author` | Article metadata for shares (also read by some parsers) | ✅ on blogs |
| `og:image:alt` | Alt text for the share image (accessibility) | ✅ two seconds of work |
| `og:video` / `og:audio` | Embedded media in the share | 🟠 niche |
| `fb:app_id` | Link to a Facebook app | ❌ pointless without a Facebook app |
| `<meta name="rating" content="adult">` | Adult-content self-labeling (SafeSearch) | 🟠 mandatory… if applicable |

## 10. The go-live checklist

- [ ] `charset` + `viewport` first, zoom free
- [ ] unique `<title>`, 50-60 characters, keyword up front
- [ ] description 140-160 characters, click-oriented
- [ ] `robots` = index,follow (and NOT the staging noindex)
- [ ] absolute, self-referencing canonical
- [ ] complete OG + absolute 1200×630 image + twitter:card
- [ ] valid JSON-LD, `sameAs` consistent with your real profiles
- [ ] favicon ≥ 48×48 + SVG + apple-touch-icon
- [ ] preconnect/preload: critical only, measured
- [ ] reciprocal hreflang (if multilingual)
- [ ] zero `meta keywords`, zero duplicate tags

---

## License

100% original writing. License **CC BY 4.0**: free reuse provided you credit "Bernard David Corroy — [david-corroy.com](https://www.david-corroy.com/)".

**The author**: Bernard David Corroy, independent SEO & GEO consultant — [Website](https://www.david-corroy.com/) · [Free express audit](https://www.david-corroy.com/audit-express/) (FR) · [GEO Prompts](https://github.com/david-corroy/prompts-geo) · [LinkedIn](https://www.linkedin.com/in/bernard-david-corroy/) · [Wikidata](https://www.wikidata.org/entity/Q140472682)
