# ClaimReview et interoperabilite

OpenTruth exporte ses verdicts au format **ClaimReview** (schema.org), le standard utilise par Google, Facebook et les fact-checkers certifies.

## Qu'est-ce que ClaimReview ?

ClaimReview est un balisage JSON-LD qui permet aux moteurs de recherche d'afficher les resultats de fact-checking directement dans les resultats de recherche.

Quand un utilisateur cherche un claim verifie, Google peut afficher :

```
✅ Vrai — Verifie par OpenTruth
Source : INSEE, Eurostat (2 sources concordantes)
```

---

## Mapping des verdicts

OpenTruth convertit automatiquement ses verdicts H1 en ratings ClaimReview :

| Verdict OpenTruth | ratingValue | alternateName | Affiche dans Google |
|-------------------|-------------|---------------|---------------------|
| H1.1 Vrai | 5 | True | ✅ True |
| H1.2 Plutot vrai | 4 | Mostly True | ✅ Mostly True |
| H1.3 Melange | 3 | Mixed | ⚠️ Mixed |
| H1.4 Trompeur | 2 | Misleading | ⚠️ Misleading |
| H1.5 Faux | 1 | False | ❌ False |
| H1.6 Non verifiable | — | Unverifiable | Exclu |
| H1.7 Opinion | — | Opinion | Exclu |
| H1.8 Satire | — | Satire | Exclu |

Les verdicts H1.6-H1.8 sont exclus du ClaimReview car ils ne representent pas des verifications factuelles.

---

## API ClaimReview

### Claim individuel

```
GET /api/v1/claims/{claim_id}/claimreview
```

Retourne le JSON-LD ClaimReview pour un claim specifique :

```json
{
  "@context": "https://schema.org",
  "@type": "ClaimReview",
  "datePublished": "2026-03-15",
  "url": "https://app.opentruth.ch/claims/abc123",
  "claimReviewed": "Le chomage en France est a 7.1%",
  "author": {
    "@type": "Organization",
    "name": "OpenTruth",
    "url": "https://opentruth.ch"
  },
  "reviewRating": {
    "@type": "Rating",
    "ratingValue": 5,
    "bestRating": 5,
    "worstRating": 1,
    "alternateName": "True"
  },
  "itemReviewed": {
    "@type": "Claim",
    "author": {
      "@type": "Person",
      "name": "Emmanuel Macron"
    },
    "datePublished": "2025-12-15"
  }
}
```

### Feed DataFeed (Google Fact Check Tools)

```
GET /api/v1/claims/feed
```

Retourne un DataFeed avec tous les claims verifies, compatible avec Google Fact Check Tools et Google Search Console.

---

## Integration avec Google

### Etapes pour apparaitre dans Google

1. ✅ API ClaimReview implementee
2. ✅ Feed DataFeed disponible
3. ⬜ Enregistrer le site dans Google Search Console
4. ⬜ Soumettre le feed via Fact Check Markup Tool
5. ⬜ Attendre l'indexation Google (quelques semaines)

### Fait automatiquement

Chaque nouveau claim verifie est automatiquement disponible via l'API ClaimReview — pas d'action manuelle requise.

---

## Compatibilite

| Plateforme | Methode | Status |
|------------|---------|--------|
| Google Search | ClaimReview JSON-LD | ✅ API prete |
| Google Fact Check Explorer | DataFeed | ✅ Feed pret |
| Facebook / Meta | ClaimReview | ✅ Compatible |
| ClaimBuster | ClaimReview | ✅ Compatible |
| EDMO aggregateur | ClaimReview + EFCSN metadata | ⚠️ Metadata a ajouter |
