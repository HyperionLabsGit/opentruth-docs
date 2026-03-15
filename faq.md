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
