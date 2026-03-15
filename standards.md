# Alignement IFCN / EFCSN

OpenTruth s'aligne sur les standards internationaux de fact-checking pour garantir la credibilite et l'interoperabilite de sa plateforme.

## IFCN — International Fact-Checking Network

L'IFCN (Poynter Institute) est la reference mondiale pour la certification des fact-checkers.

### Notre score : 17/25 (68%)

| Critere | Status | Detail |
|---------|--------|--------|
| Engagement de non-partisanite | ✅ | Doctrine : "Nous structurons, nous n'evaluons pas" |
| Transparence des sources | ✅ | Toutes les sources affichees avec URLs |
| Transparence du financement | ❌ | Page de financement pas encore creee |
| Transparence de la methodologie | ✅ | Pipeline documente publiquement |
| Politique de corrections | ⚠️ | Systeme de feedback beta, pas de politique formelle |
| Organisation legale | ❌ | Pas encore d'entite juridique |

### Strategie

OpenTruth vise le track **IFCN Technology Partner** — un programme pour les plateformes technologiques qui soutiennent le fact-checking, sans necessiter d'equipe editoriale humaine.

---

## EFCSN — European Fact-Checking Standards Network

L'EFCSN (Conseil de l'Europe) est le standard europeen, plus prescriptif que l'IFCN.

### Notre score : 16/30 (53%)

| Article | Critere | Status |
|---------|---------|--------|
| Art. 1 | Organisation et gouvernance | ⚠️ Pas d'entite legale |
| Art. 2 | Non-partisanite | ✅ Doctrine neutralite |
| Art. 3 | Transparence | ✅ Pipeline et sources publics |
| Art. 4 | Methodologie | ✅ Pipeline automatise documente |
| Art. 5 | Droit de reponse | ❌ Pas encore implemente |
| Art. 6 | Ethique | ⚠️ Pas de statement IA formelle |
| Art. 7 | Mecanisme de plainte | ❌ Pas encore implemente |

### Differences avec l'IFCN

- L'EFCSN est **plus prescriptif** (sanctions, renouvellement)
- L'EFCSN ne prescrit **pas** de taxonomie de verdicts — chaque org garde la sienne
- Scope europeen, inclut la Suisse via le Conseil de l'Europe

---

## EDMO — European Digital Media Observatory

L'EDMO coordonne les hubs regionaux de fact-checking en Europe.

### Positionnement OpenTruth

OpenTruth se positionne comme **partenaire technologique** des hubs EDMO, pas comme concurrent des fact-checkers humains :

- **Hub BeLux** (RTBF Faky) : partenaire potentiel pour le francophone
- **GADMO** (DE/AT/CH) : couvre la region DACH, recherche des outils tech
- **DE-FACTO** (France) : hub francais, inclut AFP Factuel

### Ce qu'on apporte aux hubs

1. Pipeline automatise de pre-verification (triage des claims)
2. Graphe de connaissances cross-referencant les verifications
3. API ClaimReview pour l'interoperabilite
4. Analyse video native (pas seulement texte)

---

## Gaps a combler (pre-certification)

### Organisationnels (prioritaire)

| Gap | Action | Priorite |
|-----|--------|----------|
| Entite legale | Creer une association ou Sarl en Suisse | P1 |
| Board editorial | Constituer un comite consultatif (academiques, journalistes) | P2 |
| Page de financement | Publier les sources de financement | P2 |
| Statement ethique IA | Rediger une charte d'utilisation responsable de l'IA | P2 |
| Mecanisme de plainte | Implementer un formulaire de contestation des verdicts | P3 |
| Droit de reponse | Permettre aux sujets des claims de repondre | P3 |

### Techniques (deja couverts)

| Critere | Status |
|---------|--------|
| Methodologie documentee | ✅ Cette documentation |
| Sources transparentes | ✅ URLs affichees |
| Taxonomie structuree | ✅ I1-I11, H1.1-H1.8 |
| ClaimReview interop | ✅ API + feed DataFeed |
| Non-partisanite | ✅ Doctrine dans le code |

---

## Calendrier de certification

| Etape | Quand | Objectif |
|-------|-------|----------|
| Google ClaimReview | Immediat | Enregistrer dans Search Console |
| IFCN Technology Partner | Q3 2026 | Reconnaissance internationale |
| EFCSN membre | Q1 2027 | Apres 6 mois operationnels |
