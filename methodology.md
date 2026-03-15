# Methodologie de verification

Ce document detaille chaque phase du pipeline de verification OpenTruth.

## Vue d'ensemble

```
Video YouTube
    ↓
Phase 0 : Analyse des metadonnees
    ↓
Phase 1 : Transcription
    ↓
Phase 2 : Consolidation
    ↓
Phase 3 : Enrichissement (CrewAI — 11 agents IA)
    ↓
Phase 4 : Scoring NLI
    ↓
Phase 5 : Vectorisation et graphe de connaissances
    ↓
Resultat : Claims verifies avec verdicts
```

---

## Phase 0 — Analyse des metadonnees

**Agent** : Metadata Analyzer (Groq Llama 3.3-70B)

Avant meme de transcrire, le systeme analyse les metadonnees YouTube :

- **Titre** : indices sur le sujet et le speaker
- **Chaine** : type de source (officielle, media, personnelle)
- **Description** : contexte additionnel
- **Date de publication** : contexte temporel

Cette analyse fournit le contexte pour toutes les phases suivantes.

---

## Phase 1 — Transcription

**Methode primaire** : YouTube Transcript API (gratuit, rapide)
**Fallback** : OpenAI Whisper-1 (si pas de transcription YouTube)

Le resultat est une serie de segments bruts avec timestamps.

---

## Phase 2 — Consolidation

Les segments bruts sont fusionnes en segments thematiques coherents :

1. **Fusion** : segments avec gap < 0.5s combines
2. **Ponctuation** : restauration avec DeepPunctuation
3. **Decoupage** : spaCy fr_core_news_sm pour le decoupage en phrases
4. **Detection de topics** : embeddings MiniLM-L12-v2 + similarite cosinus pour detecter les changements de sujet

**Resultat** : 10-30 segments thematiques par video.

---

## Phase 3 — Enrichissement

C'est le coeur du systeme. **11 agents IA** travaillent en parallele et en sequence.

### Phase 3a — Agents paralleles (5 agents)

| Agent | Modele | Role |
|-------|--------|------|
| **Summarizer** | Groq Llama 3.3-70B | Resume factuel du segment (2-3 phrases) |
| **Speaker Detector** | Groq Llama 3.3-70B | Identifie qui parle (nom, fonction, parti) |
| **Topic Analyzer** | Groq Llama 3.3-70B | Extrait les themes avec scores de pertinence |
| **Entity Detector** | Groq Llama 3.3-70B | Detecte les entites nommees (personnes, organisations, lieux) |
| **Forensic Analyzer** | Groq Llama 3.3-70B + GPT-4o Vision | Analyse les frames video pour detecter des manipulations visuelles |

### Phase 3b — Pipeline de claims (3 agents, sequentiel)

| Agent | Modele | Role |
|-------|--------|------|
| **Claim Extractor** | OpenAI GPT-5.2 | Extrait les affirmations verifiables, les classe (I1-I11) |
| **Source Finder** | Groq Llama 3.3-70B | Recherche 4-6 sources web par claim (Brave Search + scraping) |
| **Claim Verifier** | Groq Llama 3.3-70B | Evalue la correspondance claim/source, attribue un verdict |

### Outils utilises par les agents

| Outil | Usage |
|-------|-------|
| Brave Search / SearXNG | Recherche web pour trouver des sources |
| Web Scraper (Crawl4AI) | Extraction du contenu des pages web |
| Batch Scraper | Scraping parallele de multiples URLs |
| Analyse forensique hybride | Analyse de frames video (Python + GPT-4o Vision) |

---

## Phase 4 — Scoring NLI

**Modele** : mDeBERTa-v3-base-xnli-multilingual-nli-2mil7

Pour chaque paire claim/source, le modele NLI determine :

| Prediction | Correspondance OpenTruth |
|------------|--------------------------|
| ENTAILMENT | confirms |
| CONTRADICTION | contradicts |
| NEUTRAL (haute pertinence) | partial |
| NEUTRAL (basse pertinence) | no_data |

### Seuils de confiance

- Confiance minimum : 0.40 (en dessous = no_data)
- Entailment minimum : 0.40
- Contradiction minimum : 0.50 (barre plus haute)
- Pertinence pour partial : > 0.80

### Score video final

```
validity_score = cross_ref_agreement × 0.6 + qualite_moyenne × 0.4
```

---

## Phase 5 — Vectorisation et graphe

### Vectorisation (pgvector)

Chaque segment est vectorise avec OpenAI text-embedding-3-large (3072 dimensions) :

- **Double vectorisation** : texte original + resume
- **Stockage** : Supabase pgvector (tables `segment_vectors`, `source_vectors`)
- **Usage** : recherche semantique RAG pour les requetes utilisateurs

### Graphe de connaissances (Neo4j)

Les entites et relations extraites sont ajoutees au graphe :

- 7 types de noeuds (Person, Organization, Location, Concept, Event, Claim, Source)
- 11 types de relations (MENTIONS, CLAIMS, VERIFIED_BY, etc.)
- Deduplication par similarite (cosinus > 0.95)

Voir [Ontologie et graphe de connaissances](ontology.md) pour les details.

---

## Controle qualite

### Quality gate

Apres l'enrichissement, un controle qualite verifie que le pipeline a produit des resultats :

- Si 0 claims ET 0 sources → status = `enrichment_failed`
- Si au moins 1 claim OU 1 source → status = `enriched`

### Transparence

Pour chaque verdict, OpenTruth fournit :

- Les sources utilisees (URLs cliquables)
- Le score de confiance NLI
- La fiabilite de chaque source
- Le raisonnement de l'agent verificateur
- Le type de claim (I1-I11)
