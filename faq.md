# FAQ

## Questions generales

### Qu'est-ce qu'OpenTruth ?

OpenTruth est une infrastructure automatisee de verification du discours politique. Le systeme analyse des videos YouTube de discours, debats et sessions parlementaires pour extraire, verifier et noter les affirmations factuelles.

### OpenTruth est-il neutre politiquement ?

Oui. Notre doctrine fondamentale est : "Nous n'evaluons pas le discours politique. Nous le structurons en donnees." Les sources sont classees par fiabilite (officielle, academique, presse), jamais par alignement politique.

### Quelles langues sont supportees ?

Pour la beta : **francais** et **anglais**. L'architecture est concue pour le multilinguisme — l'allemand sera ajoute prochainement (pertinent pour le contexte suisse).

### D'ou viennent les sources ?

OpenTruth recherche automatiquement des sources sur le web, en privilegiant :
1. Les sources officielles (sites gouvernementaux, statistiques nationales, registres parlementaires)
2. Les publications academiques
3. Les agences de presse reconnues (AFP, Reuters)

## Comprendre les resultats

### Pourquoi un claim est-il "Non verifiable" (H1.6) ?

Cela signifie que les sources disponibles ne permettent pas de conclure. Ce n'est pas un jugement — c'est un constat que les preuves sont insuffisantes a ce jour.

### Quelle est la difference entre "Faux" (H1.5) et "Trompeur" (H1.4) ?

- **Faux** : l'affirmation est directement contredite par les faits
- **Trompeur** : l'affirmation est techniquement vraie, mais omet un contexte crucial qui en change le sens

Exemple trompeur : "Le chomage a baisse de 20%" — vrai sur la periode choisie, mais il avait augmente de 30% l'annee precedente.

### Pourquoi certains claims sont-ils classes "Opinion" (H1.7) ?

Les opinions, predictions et jugements de valeur ne sont pas des faits verifiables. OpenTruth les identifie pour les separer des affirmations factuelles. Exemples :
- "Ce gouvernement est incompetent" → opinion
- "L'economie va s'effondrer" → prediction
- "C'est la meilleure reforme" → jugement de valeur

### Le score de confiance, c'est quoi ?

C'est un indicateur de la solidite du verdict, base sur :
- Le nombre de sources concordantes
- La fiabilite des sources (officielle = plus fiable qu'un blog)
- La pertinence des sources par rapport au claim

Un score eleve signifie que plusieurs sources fiables s'accordent.

## Beta

### Comment signaler une erreur ?

Pendant la beta, utilisez le bouton de feedback dans l'application. Chaque signalement est examine pour ameliorer le systeme.

### Les resultats sont-ils parfaits ?

Non. OpenTruth est un outil d'aide a la verification, pas un oracle. Le systeme peut :
- Manquer des nuances contextuelles
- Ne pas trouver de sources pour certains claims
- Classifier incorrectement un type de claim

C'est pourquoi nous fournissons toujours les sources et le raisonnement — vous pouvez verifier vous-meme.

### Mes donnees sont-elles protegees ?

Oui. OpenTruth est base en Suisse et respecte la LPD (Loi federale sur la protection des donnees) et le RGPD. Consultez notre [politique de confidentialite](https://opentruth.ch/privacy) pour plus de details.

## Questions techniques

### Quel modele d'IA est utilise pour la verification ?

OpenTruth utilise plusieurs modeles specialises :
- **GPT-5.2** (OpenAI) pour l'extraction des claims et la classification
- **Llama 3.3-70B** (Groq) pour le resume, la detection de speakers et la recherche de sources
- **mDeBERTa-v3** pour le scoring NLI (Natural Language Inference)
- **text-embedding-3-large** (OpenAI) pour la recherche semantique

### Pourquoi le score de confiance NLI est-il parfois bas ?

Le modele NLI a ete entraine principalement sur des phrases courtes en anglais (XNLI). Sur des textes politiques longs en francais, la confiance est naturellement plus basse. Un score NLI de 0.60 est considere comme acceptable pour notre cas d'usage. Nous travaillons sur un fine-tuning specifique pour ameliorer ce score.

### Comment fonctionne la recherche de sources ?

Le systeme utilise une approche multi-sources :
1. Recherche web via Brave Search ou SearXNG (auto-heberge)
2. Scraping du contenu des pages trouvees
3. Filtrage par pertinence (embeddings + similarite cosinus)
4. Hierarchie de fiabilite : sources officielles > academiques > presse > autres

### Les sources sont-elles en temps reel ?

Oui. A chaque verification, le systeme effectue des recherches web en temps reel. Il n'utilise pas une base de donnees statique de faits.

### Que se passe-t-il si aucune source ne confirme ni ne contredit ?

Le claim recoit le verdict **H1.6 Non verifiable**. Cela ne signifie pas qu'il est faux — seulement que les preuves disponibles sont insuffisantes. Le verdict peut changer si de nouvelles sources deviennent disponibles.

### OpenTruth peut-il verifier des videos en anglais ?

Oui. Le pipeline supporte le francais et l'anglais. L'allemand sera ajoute prochainement (pertinent pour le contexte suisse). Le modele NLI est multilingue (26 langues).

### Qu'est-ce que le graphe de connaissances ?

OpenTruth construit un [graphe de connaissances](ontology.md) qui connecte les personnes, organisations, claims et sources. Cela permet de suivre l'evolution des discours dans le temps et de detecter des patterns (ex: un politicien qui repete un claim deja verifie comme faux).

### Les verdicts sont-ils definitifs ?

Non. Un verdict peut evoluer si :
- De nouvelles sources deviennent disponibles
- Un utilisateur signale une erreur (feedback beta)
- Le contexte change (ex: une promesse se realise)

Le systeme conserve l'historique des verdicts.
