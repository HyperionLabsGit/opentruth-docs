# Comprendre les scores

OpenTruth produit plusieurs scores pour chaque claim verifie. Voici comment les interpreter.

## Score de validite (validity_score)

Le score principal : **a quel point le claim est-il soutenu par les sources ?**

| Plage | Label | Signification |
|-------|-------|---------------|
| 0.75 - 1.00 | **Haute** | Bien confirme par des sources fiables |
| 0.50 - 0.74 | **Moyenne** | Partiellement confirme |
| 0.00 - 0.49 | **Basse** | Peu de confirmation |
| — | **Conteste** | Sources contradictoires |

### Comment c'est calcule

```
validity_score = cross_ref × 0.6 + qualite_moyenne × 0.4
```

- **cross_ref** (60%) : pourcentage de sources qui confirment vs contredisent, pondere par leur fiabilite
- **qualite_moyenne** (40%) : moyenne de (fiabilite × pertinence) pour chaque source

---

## Correspondance (correspondence)

Pour chaque paire claim/source, le systeme determine si la source :

| Valeur | Signification | Impact sur le score |
|--------|---------------|---------------------|
| **confirms** | La source confirme le claim | +1.0 (pondere par fiabilite) |
| **partial** | Confirmation partielle | +0.5 |
| **no_data** | La source ne traite pas ce sujet | 0.0 |
| **contradicts** | La source contredit le claim | -1.0 |

### Comment c'est determine

OpenTruth utilise deux methodes complementaires :

1. **Agent CrewAI** : Un agent IA (GPT-5) analyse le claim et la source pendant l'enrichissement et determine la correspondance
2. **NLI (Natural Language Inference)** : Un modele specialise (mDeBERTa-v3) compare mathematiquement le texte du claim avec le texte de la source

En production, c'est l'evaluation de l'agent qui est utilisee. Le NLI sert de verification secondaire.

---

## Confiance NLI (nli_confidence)

La confiance du modele NLI dans sa prediction (0-1).

| Plage | Interpretation |
|-------|----------------|
| 0.80+ | Tres confiant — prediction fiable |
| 0.50-0.79 | Moderement confiant — a verifier |
| < 0.50 | Peu confiant — prediction incertaine |

**Note** : Sur des textes politiques longs en francais, la confiance NLI est naturellement plus basse que sur des phrases courtes. Un score de 0.60 est considere comme acceptable.

---

## Score de fiabilite (reliability_score)

La fiabilite de la source (0-1), basee sur son type :

| Type de source | Score |
|----------------|-------|
| Institutionnelle (G1.1) | 0.95 |
| Fact-checker certifie (G1.5) | 0.90 |
| Academique (G1.2) | 0.85 |
| Media majeur (G1.3) | 0.70 |
| Source interne (G1.6) | 0.60 |
| Media regional (G1.4) | 0.55 |
| Blog / Autre (G1.7-G1.8) | 0.30 |

---

## Score de pertinence (relevance_score)

A quel point la source parle du meme sujet que le claim (0-1).

Calcule par similarite cosinus entre les embeddings du claim et de la source (OpenAI text-embedding-3-large, 3072 dimensions).

| Plage | Interpretation |
|-------|----------------|
| 0.80+ | Tres pertinent — meme sujet precis |
| 0.60-0.79 | Pertinent — sujet proche |
| 0.40-0.59 | Tangentiel — lien indirect |
| < 0.40 | Non pertinent — filtre automatiquement |

---

## Score video (validity_score video)

Chaque video recoit un score global base sur l'ensemble de ses claims :

| Label | Condition |
|-------|-----------|
| **Vrai** | Au moins un claim confirme, aucun contredit |
| **Faux** | Au moins un claim contredit |
| **Non verifiable** | Aucun claim confirme ni contredit |

---

## Exemple concret

> Claim : "Le chomage en France est a 7.1% au T3 2025"

| Source | Type | Fiabilite | Pertinence | Correspondance |
|--------|------|-----------|------------|----------------|
| INSEE | G1.1 Institutionnelle | 0.95 | 0.92 | confirms |
| Eurostat | G1.1 Institutionnelle | 0.95 | 0.85 | confirms |
| Le Monde | G1.3 Media majeur | 0.70 | 0.78 | confirms |

**Resultat** : validity_score = 0.88, verdict = **H1.1 Vrai**

3 sources fiables concordantes → haute confiance.
