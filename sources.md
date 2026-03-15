# Sources et fiabilite (G1-G3)

OpenTruth classe chaque source utilisee pour la verification selon trois dimensions : le **type** (G1), le **type de preuve** (G2) et la **qualite** (G3).

## G1 — Types de sources

Chaque source est classee dans l'une des 8 categories suivantes, ordonnees par fiabilite decroissante :

### G1.1 — Institutionnelle (fiabilite : 0.95)

Sources officielles d'Etat, organisations internationales, registres parlementaires.

> *Exemples : admin.ch, assemblee-nationale.fr, europa.eu, who.int, onu.org*

Ces sources ont la plus haute fiabilite car elles representent des donnees primaires officielles.

### G1.2 — Academique (fiabilite : 0.85)

Publications scientifiques, rapports universitaires, meta-analyses.

> *Exemples : arxiv.org, pubmed.ncbi.nlm.nih.gov, hal.science, scholar.google.com*

### G1.3 — Media majeur (fiabilite : 0.70)

Agences de presse internationales et medias de reference.

> *Exemples : AFP, Reuters, Le Monde, NZZ, SWI swissinfo.ch, RTS*

### G1.4 — Media regional (fiabilite : 0.55)

Medias locaux, specialises ou regionaux.

> *Exemples : 24heures.ch, Tribune de Geneve, France 3 Regions*

### G1.5 — Fact-checker certifie (fiabilite : 0.90)

Organisations de fact-checking certifiees IFCN ou EFCSN.

> *Exemples : AFP Factuel, Les Decodeurs, Correctiv, Full Fact*

### G1.6 — Source interne (fiabilite : 0.60)

Sources internes au pipeline OpenTruth (claims deja verifiees, graphe de connaissances).

### G1.7 — Blog / Opinion (fiabilite : 0.30)

Blogs, tribunes, editoriaux, contenus d'opinion.

> *Exemples : Medium, blogs personnels, tribunes dans les journaux*

### G1.8 — Autre (fiabilite : 0.30)

Sources non classifiees ou de fiabilite inconnue.

> *Exemples : forums, reseaux sociaux, sites personnels*

---

## G2 — Types de preuves

Chaque source apporte un type de preuve different :

| Code | Type | Description |
|------|------|-------------|
| G2.1 | Donnees primaires | Statistiques officielles, registres, bases de donnees |
| G2.2 | Document officiel | Lois, reglements, proces-verbaux, rapports |
| G2.3 | Publication scientifique | Etude peer-reviewed, meta-analyse |
| G2.4 | Reportage factuel | Article de presse factuel (pas editorial) |
| G2.5 | Analyse d'expert | Avis d'expert, rapport d'analyse |
| G2.6 | Temoignage | Citation directe, interview, declaration |

---

## G3 — Qualite de la source

Trois criteres evaluent la qualite de chaque source :

### G3.1 — Independance

La source est-elle independante du sujet qu'elle couvre ?

| Niveau | Score | Exemple |
|--------|-------|---------|
| Forte | 0.90 | Institut statistique national (INSEE, OFS) |
| Moyenne | 0.60 | Media reconnu couvrant la politique |
| Faible | 0.30 | Blog du parti politique concerne |

### G3.2 — Proximite

A quel point la source est-elle proche du fait original ?

| Niveau | Score | Exemple |
|--------|-------|---------|
| 1 (primaire) | 0.95 | Proces-verbal officiel du debat |
| 2 (secondaire) | 0.70 | Article de presse citant le proces-verbal |
| 3 (tertiaire) | 0.40 | Blog resumant l'article de presse |

### G3.3 — Methodologie

La source suit-elle une methodologie rigoureuse ?

| Niveau | Score | Exemple |
|--------|-------|---------|
| Forte | 0.90 | Etude peer-reviewed, donnees officielles |
| Moyenne | 0.55 | Article journalistique avec sources citees |
| Faible | 0.25 | Post sur les reseaux sociaux sans sources |

### Score G3 final

Le score G3 combine les trois dimensions :

```
G3 = Independance × 0.35 + Proximite × 0.35 + Methodologie × 0.30
```
