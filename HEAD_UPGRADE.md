# Optional: Head improvements

If you want to upgrade the `<head>` of `public/index.html` with proper SEO + social-share meta tags, paste this between the existing `<title>` and the Google Fonts `<link>` tag.

```html
<!-- Canonical -->
<link rel="canonical" href="https://kevinvarend.com/" />

<!-- Favicon (replace path once you upload favicon files to /public) -->
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />

<!-- Theme color (matches J3D.AI indigo) -->
<meta name="theme-color" content="#2A2947" />

<!-- Open Graph -->
<meta property="og:type" content="website" />
<meta property="og:url" content="https://kevinvarend.com/" />
<meta property="og:title" content="Kevin Varend — Founder, J3D.AI · Co-Founder, House of Collaboration" />
<meta property="og:description" content="Building the architecture of trust between Europe, Asia and beyond — through technology, summits, and statecraft." />
<meta property="og:image" content="https://kevinvarend.com/og-image.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="og:locale" content="en_GB" />

<!-- Twitter / X -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:url" content="https://kevinvarend.com/" />
<meta name="twitter:title" content="Kevin Varend — Founder, J3D.AI" />
<meta name="twitter:description" content="Building the architecture of trust between Europe, Asia and beyond." />
<meta name="twitter:image" content="https://kevinvarend.com/og-image.jpg" />

<!-- LinkedIn / Author -->
<meta name="author" content="Kevin Varend" />
<link rel="me" href="https://www.linkedin.com/in/kevinvarend/" />

<!-- JSON-LD: Person schema for rich results -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Kevin Varend",
  "url": "https://kevinvarend.com",
  "image": "https://kevinvarend.com/og-image.jpg",
  "jobTitle": "Co-Founder & Managing Director",
  "worksFor": {
    "@type": "Organization",
    "name": "J3D.AI Labs OÜ",
    "url": "https://j3d.ai"
  },
  "sameAs": [
    "https://www.linkedin.com/in/kevinvarend/",
    "https://www.instagram.com/kevin.varend/"
  ]
}
</script>
```

## Assets to drop into `/public`

For the meta tags above to work, generate and upload:

| File | Size | Purpose |
|------|------|---------|
| `favicon.svg` | any | Modern browsers — vector |
| `favicon-32x32.png` | 32×32 | Tab icon fallback |
| `apple-touch-icon.png` | 180×180 | iOS home screen |
| `og-image.jpg` | 1200×630 | LinkedIn / X share preview |

Easiest path: use https://realfavicongenerator.net — upload one square PNG, it generates the full set.

The OG image specifically is high-leverage. Make a single 1200×630 image: dark indigo background, "Kevin Varend" in Cormorant Garamond italic, plus "Founder · J3D.AI Labs" underneath in Nunito caps. Use the J3D.AI palette. This is what shows up every time anyone shares the URL on LinkedIn or in a DM.
