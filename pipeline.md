# Comment ca marche

OpenTruth verifie automatiquement les affirmations politiques en 5 etapes.

## Etape 1 — Extraction

OpenTruth analyse la video (discours, debat, interview) et extrait le texte via la transcription automatique. Le texte est decoupe en segments thematiques.

**Resultat** : segments de texte avec timestamps.

## Etape 2 — Detection

Pour chaque segment, le systeme detecte :
- **Qui parle** (identification du locuteur)
- **Les sujets abordes** (themes politiques)
- **Les affirmations factuelles** (claims verifiables)

Chaque claim est classifie dans l'un des [11 types](claim-types.md) (I1-I11).

## Etape 3 — Recherche de sources

Pour chaque claim, OpenTruth recherche des sources fiables :
- Sources officielles (gouvernement, parlement, statistiques nationales)
- Sources academiques (publications scientifiques)
- Sources presse (agences de presse, medias reconnus)

Le systeme utilise une hierarchie de fiabilite : les sources officielles sont privilegiees.

## Etape 4 — Verification

Le systeme compare chaque claim avec les sources trouvees en utilisant l'Intelligence Artificielle (NLI — Natural Language Inference) :

- **Confirme** : la source dit la meme chose que le claim
- **Contredit** : la source dit le contraire
- **Partiel** : la source confirme une partie seulement
- **Pas de donnees** : la source ne traite pas ce sujet

## Etape 5 — Verdict

En combinant les resultats de toutes les sources, un [verdict](verdicts.md) est attribue (H1.1 a H1.8).

Le verdict prend en compte :
- Le **nombre de sources** qui confirment ou contredisent
- La **fiabilite** de chaque source (officielle > presse > blog)
- La **pertinence** de chaque source par rapport au claim

---

## Transparence

Pour chaque verdict, OpenTruth fournit :
- Les sources utilisees (avec URLs)
- Le score de confiance
- Le raisonnement detaille
- Le type de claim (I1-I11)

Vous pouvez toujours verifier vous-meme en consultant les sources originales.
