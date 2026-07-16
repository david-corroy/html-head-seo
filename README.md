# La balise `<head>` — le guide SEO, GEO & réseaux sociaux

🇬🇧 [English version →](./README.en.md) · 🇪🇸 [Versión en español →](./README.es.md)

**Tout ce qui doit (et ne doit pas) vivre dans le `<head>` de vos pages, classé par ce que ça rapporte : ce que Google lit, ce que les IA lisent (ChatGPT, Perplexity, Gemini), ce que les réseaux sociaux affichent, et ce qui fait la vitesse.** Avec les templates prêts à coller et les pièges qui coûtent des positions. Par [Bernard David Corroy](https://www.david-corroy.com/), consultant SEO & GEO.

Les guides existants listent les balises. Celui-ci vous dit **lesquelles comptent, pour qui, et dans quel ordre** — parce qu'un `<head>` n'est pas un inventaire, c'est votre carte de visite auprès de trois lecteurs : les moteurs, les IA, et les humains qui partagent vos liens.

---

## 1. Le minimum vital — 6 lignes, aucune excuse

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Le titre de la page — 50 à 60 caractères</title>
<meta name="description" content="La promesse de la page en 140-160 caractères, avec le mot-clé principal au début.">
<link rel="canonical" href="https://www.exemple.fr/cette-page/">
<link rel="icon" href="/favicon.ico" sizes="32x32">
```

Trois règles non négociables :

- `charset` en **première position** — le navigateur ne le cherche que dans les 1 024 premiers octets.
- `viewport` sans `user-scalable=no` ni `maximum-scale=1` : bloquer le zoom est une faute d'accessibilité que Lighthouse sanctionne.
- Un seul `<title>`, un seul `canonical`. Les doublons (thème + plugin SEO qui écrivent chacun le leur) créent des signaux contradictoires — et Google tranche sans vous.

## 2. Ce que Google lit

### Le titre — l'élément le plus rentable du head

```html
<title>Mot-clé principal : la promesse — Marque</title>
```

- 50-60 caractères affichés ; au-delà, coupure aux ~580 pixels.
- Le mot-clé au début pèse plus qu'à la fin.
- Un title par page, unique sur tout le site — les doublons se diagnostiquent dans la Search Console.
- Google réécrit ~60 % des titles qu'il juge mauvais : si le vôtre est réécrit, c'est un symptôme, pas une fatalité.

### La description — pas un critère de classement, mais votre taux de clic

```html
<meta name="description" content="Réponse directe à l'intention de recherche, un chiffre ou une preuve, un appel implicite au clic. 140-160 caractères.">
```

### Les directives robots — l'arme qui tue dans les deux sens

```html
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
```

- `noindex` oublié après une mise en prod = le site entier invisible. **C'est l'erreur n°1 constatée en audit**, et elle passe inaperçue des mois.
- `max-image-preview:large` conditionne l'affichage grand format dans Discover.
- Le `X-Robots-Tag` (en-tête HTTP) a priorité sur la balise : en cas de comportement inexpliqué, vérifiez les deux.

### Le canonical — une seule vérité par contenu

```html
<link rel="canonical" href="https://www.exemple.fr/page/">
```

- URL **absolue**, avec le bon protocole et le bon sous-domaine (www ou pas — celui que vous avez choisi partout).
- Auto-référent sur les pages normales : c'est lui qui neutralise les paramètres de tracking (`?utm_…`).
- Piège classique : un canonical qui pointe vers la home sur tout le site (mauvaise config de plugin) = désindexation massive.

### hreflang — seulement si vous êtes multilingue

```html
<link rel="alternate" hreflang="fr" href="https://www.exemple.fr/page/">
<link rel="alternate" hreflang="en" href="https://www.exemple.com/page/">
<link rel="alternate" hreflang="x-default" href="https://www.exemple.com/page/">
```

Règle d'or : la réciprocité. Si la page FR déclare la page EN, la page EN doit déclarer la page FR, sinon les deux déclarations sont ignorées.

## 3. Ce que les IA lisent (le volet GEO que les autres guides n'ont pas)

Quand ChatGPT, Perplexity ou Gemini citent une page, ils s'appuient d'abord sur ce que leurs crawlers (GPTBot, ClaudeBot, PerplexityBot, Google-Extended) en retirent — et le `<head>` est leur première lecture.

### Le title et la description sont votre pitch auprès des IA

Les moteurs de réponse reprennent souvent title + description **tels quels** pour décider si la page mérite d'être lue en entier. Un title vague (« Accueil — Société Dupont ») vous élimine avant même l'analyse du contenu.

### Le JSON-LD : dire aux machines qui vous êtes

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Prénom Nom",
  "jobTitle": "Votre métier précis",
  "url": "https://www.exemple.fr/",
  "sameAs": [
    "https://www.linkedin.com/in/votreprofil/",
    "https://www.wikidata.org/entity/QXXXXX",
    "https://github.com/votrecompte"
  ]
}
</script>
```

- Le tableau `sameAs` est la colonne vertébrale de votre **entité** : chaque profil cohérent listé augmente la confiance des IA quand elles parlent de vous.
- `Organization` ou `Person` + `WebSite` sur la home, `Article` sur les contenus, `FAQPage` là où il y a de vraies questions-réponses.
- Reliez vos blocs entre eux avec `@id` plutôt que de répéter les mêmes informations.

### Ce qui ne se joue PAS dans le head (mais qu'on y cherche à tort)

- L'accès des crawlers IA se règle dans `robots.txt` (GPTBot, ClaudeBot, PerplexityBot…), pas dans une meta.
- Le fichier `llms.txt` (le plan du site pour les IA) vit à la racine, comme robots.txt.
- La balise `meta keywords` est morte depuis 2009. Sa présence signale un site mal entretenu — supprimez-la.

## 4. Ce que les réseaux affichent — Open Graph & Twitter Cards

Chaque partage sans image ni titre propre est un clic perdu. Le bloc complet :

```html
<meta property="og:type" content="website">
<meta property="og:title" content="Le titre pour le partage (peut différer du title SEO)">
<meta property="og:description" content="La description pour le partage.">
<meta property="og:url" content="https://www.exemple.fr/page/">
<meta property="og:image" content="https://www.exemple.fr/img/og-page.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:locale" content="fr_FR">
<meta property="og:site_name" content="Votre marque">
<meta name="twitter:card" content="summary_large_image">
```

- Image : **1200 × 630 px**, moins de 300 Ko, texte lisible en miniature. Une seule image bien faite sert partout (LinkedIn, Facebook, X, WhatsApp, Slack, iMessage).
- URL d'image **absolue** — les URLs relatives cassent l'aperçu sur la moitié des plateformes.
- Testez avec l'inspecteur de partage de LinkedIn (Post Inspector) et le validateur de X : les caches de partage se purgent là-bas, pas chez vous.

## 5. La vitesse — les balises qui font gagner des millisecondes

```html
<!-- Ouvrir la connexion tôt vers un domaine tiers critique -->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<!-- Précharger LA ressource critique (une ou deux, pas dix) -->
<link rel="preload" href="/fonts/titre.woff2" as="font" type="font/woff2" crossorigin>

<!-- Résolution DNS anticipée pour les domaines secondaires -->
<link rel="dns-prefetch" href="https://stats.exemple.fr">
```

- `preload` est un scalpel, pas un pistolet à peinture : chaque preload retarde tout le reste. Réservez-le à la police du titre ou à l'image LCP.
- Hébergez vos polices **chez vous** : un aller-retour DNS + TLS vers Google Fonts coûte plus cher que les octets de la police.
- Le CSS critique reste bloquant, et c'est normal. Ne passez jamais en async le CSS d'un élément visible au chargement (bandeau cookies compris) — bonjour le flash de contenu brut.

## 6. Identité visuelle — le set de favicons moderne

Quatre fichiers suffisent en 2026 :

```html
<link rel="icon" href="/favicon.ico" sizes="32x32">
<link rel="icon" href="/icon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="/apple-touch-icon.png"><!-- 180×180 -->
<link rel="manifest" href="/manifest.webmanifest">
```

- Google exige une favicon d'au moins **48×48** (multiple de 48 recommandé) pour l'afficher dans ses résultats — et il peut mettre des semaines à la découvrir sur un site récent.
- `theme-color` colore la barre du navigateur mobile : `<meta name="theme-color" content="#0d1220">`.

## 7. Les pièges qui coûtent cher — vus en audit

| Piège | Symptôme | Correction |
|---|---|---|
| `noindex` de pré-production oublié | Site invisible, zéro impression GSC | Vérifier `robots` + X-Robots-Tag à CHAQUE mise en prod |
| Double title / double canonical | Google réécrit ou ignore | Un seul émetteur (le plugin SEO, pas le thème) |
| Canonical vers la home partout | Désindexation de tout sauf la home | Canonical auto-référent page par page |
| `og:image` absente ou relative | Partages sans visuel | URL absolue, 1200×630 |
| `charset` après des scripts | Caractères cassés aléatoires | charset en ligne 1 du head |
| `user-scalable=no` | Accessibilité sanctionnée | Laisser le zoom libre |
| 15 preload « au cas où » | LCP dégradé au lieu d'amélioré | 1-2 preload maximum, mesurés |
| `meta keywords` encore présente | Signal de site à l'abandon | Supprimer |
| hreflang sans réciprocité | Déclarations ignorées | Chaque langue déclare toutes les autres |
| JSON-LD invalide (virgule, guillemet) | Résultats enrichis perdus sans alerte | Valider à chaque modif (Rich Results Test) |

## 8. Templates prêts à coller

### Site vitrine / page de service

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Métier + ville : la promesse — Marque</title>
  <meta name="description" content="Ce que vous faites, pour qui, où, avec une preuve chiffrée.">
  <meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
  <link rel="canonical" href="https://www.exemple.fr/service/">
  <link rel="icon" href="/favicon.ico" sizes="32x32">
  <link rel="icon" href="/icon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">
  <meta property="og:type" content="website">
  <meta property="og:title" content="Métier + ville : la promesse">
  <meta property="og:description" content="La version partage de votre description.">
  <meta property="og:url" content="https://www.exemple.fr/service/">
  <meta property="og:image" content="https://www.exemple.fr/img/og-service.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:locale" content="fr_FR">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "LocalBusiness",
    "name": "Marque", "url": "https://www.exemple.fr/",
    "areaServed": "Votre zone", "sameAs": ["https://www.linkedin.com/company/…"] }
  </script>
</head>
```

### Article de blog

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>La question que l'article résout — Marque</title>
  <meta name="description" content="La réponse en une phrase, puis ce que le lecteur saura faire à la fin.">
  <meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">
  <link rel="canonical" href="https://www.exemple.fr/blog/article/">
  <link rel="icon" href="/favicon.ico" sizes="32x32">
  <meta property="og:type" content="article">
  <meta property="og:title" content="La question que l'article résout">
  <meta property="og:image" content="https://www.exemple.fr/img/og-article.png">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "Article",
    "headline": "La question que l'article résout",
    "datePublished": "2026-07-16", "dateModified": "2026-07-16",
    "author": { "@type": "Person", "name": "Prénom Nom", "url": "https://www.exemple.fr/a-propos/" } }
  </script>
</head>
```

## 9. La checklist de mise en prod

- [ ] `charset` + `viewport` en tête, zoom libre
- [ ] `<title>` unique, 50-60 caractères, mot-clé au début
- [ ] description 140-160 caractères, orientée clic
- [ ] `robots` = index,follow (et PAS le noindex de la préprod)
- [ ] canonical absolu, auto-référent
- [ ] OG complet + image 1200×630 absolue + twitter:card
- [ ] JSON-LD valide, `sameAs` cohérent avec vos vrais profils
- [ ] favicon ≥ 48×48 + SVG + apple-touch-icon
- [ ] preconnect/preload : uniquement le critique, mesuré
- [ ] hreflang réciproque (si multilingue)
- [ ] zéro `meta keywords`, zéro balise en double

---

## Crédits & licence

Rédaction 100 % originale. L'idée d'un répertoire dédié au `<head>` a été popularisée par [HEAD de Josh Buchea](https://github.com/joshbuchea/HEAD) — allez-y pour l'inventaire exhaustif des balises ; venez ici pour savoir lesquelles comptent et pourquoi.

Licence **CC BY 4.0** : réutilisation libre à condition de créditer « Bernard David Corroy — [david-corroy.com](https://www.david-corroy.com/) ».

**L'auteur** : Bernard David Corroy, consultant SEO & GEO indépendant — [Site](https://www.david-corroy.com/) · [Audit express gratuit](https://www.david-corroy.com/audit-express/) · [Prompts GEO](https://github.com/david-corroy/prompts-geo) · [LinkedIn](https://www.linkedin.com/in/bernard-david-corroy/) · [Wikidata](https://www.wikidata.org/entity/Q140472682)
