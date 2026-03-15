# Ontologie et graphe de connaissances

OpenTruth construit un **graphe de connaissances** (Knowledge Graph) dans Neo4j pour tracker les relations entre personnes, organisations, claims et sources a travers le temps.

## Pourquoi un graphe ?

Un graphe permet de repondre a des questions que les bases de donnees classiques ne peuvent pas :

- *"Quels claims a fait Macron sur l'immigration en 2025 ?"*
- *"Quelles sources contredisent les affirmations de ce depute ?"*
- *"Y a-t-il des patterns dans les claims faux d'un parti ?"*

---

## Types de noeuds (entites)

Le graphe contient 7 types d'entites :

| Type | Description | Exemple |
|------|-------------|---------|
| **Person** | Individu identifie | Emmanuel Macron, Mathilde Panot |
| **Organization** | Organisation, parti, institution | LFI, INSEE, UE |
| **Location** | Lieu geographique | Assemblee nationale, Berne, Bruxelles |
| **Concept** | Concept abstrait, theme politique | Immigration, deficit budgetaire, climat |
| **Event** | Evenement date | Vote de confiance du 5 mars 2026 |
| **Claim** | Affirmation factuelle verifiee | "Le chomage est a 7.1%" |
| **Source** | Source utilisee pour la verification | Article INSEE, rapport Eurostat |

### Enrichissement des claims

Chaque noeud Claim porte des metadonnees de verification :

- `claim_type` : I1-I11 (type de claim)
- `validity_score` : 0-1 (score de validite)
- `validity_label` : high / medium / low / contested

### Contexte temporel

Tous les noeuds sont tagges avec `video_date` (date de publication de la video). Cela permet de suivre l'evolution des discours dans le temps.

---

## Types de relations

Le graphe utilise 11 types de relations :

| Relation | De → Vers | Exemple |
|----------|-----------|---------|
| **MENTIONS** | Person → Concept | Macron MENTIONS immigration |
| **SPEAKS_ABOUT** | Person → Event | Panot SPEAKS_ABOUT vote de confiance |
| **WORKS_FOR** | Person → Organization | Dupont WORKS_FOR Assemblee nationale |
| **LOCATED_IN** | Event → Location | Debat LOCATED_IN Assemblee nationale |
| **DATED** | Event → (date) | Vote DATED 2026-03-05 |
| **PART_OF** | Person → Organization | Le Pen PART_OF RN |
| **RELATED_TO** | Concept → Concept | Immigration RELATED_TO securite |
| **CAUSES** | Concept → Concept | Inflation CAUSES baisse pouvoir achat |
| **INFLUENCES** | Person → Concept | Ministre INFLUENCES politique energetique |
| **CLAIMS** | Person → Claim | Depute CLAIMS "le chomage a baisse" |
| **VERIFIED_BY** | Claim → Source | Claim VERIFIED_BY article INSEE |

### Doctrine de neutralite

Les relations suivantes sont **interdites** dans le graphe :

- ~~SUPPORTS~~ (impliquerait un jugement ideologique)
- ~~OPPOSES~~ (idem)
- ~~AGREES_WITH~~ (idem)

OpenTruth ne juge pas si une position est "bonne" ou "mauvaise" — il structure les faits.

---

## Deduplication

Quand le meme claim apparait dans plusieurs videos, le graphe le fusionne :

- Similarite cosinus > 0.95 entre les embeddings → meme noeud
- Les doublons sont merges dans le noeud canonique
- Les relations (sources, speakers) sont preservees

Cela permet de suivre un claim a travers le temps et differents speakers.

---

## Requetes typiques

### Tous les claims d'un speaker

```cypher
MATCH (p:Person {name: "Emmanuel Macron"})-[:CLAIMS]->(c:Claim)
RETURN c.text, c.claim_type, c.validity_label
ORDER BY c.video_date DESC
```

### Sources qui contredisent un claim

```cypher
MATCH (c:Claim)-[:VERIFIED_BY]->(s:Source {correspondence: "contradicts"})
WHERE c.text CONTAINS "chomage"
RETURN c.text, s.url, s.title
```

### Themes les plus abordes par un parti

```cypher
MATCH (p:Person)-[:PART_OF]->(o:Organization {name: "LFI"})
MATCH (p)-[:MENTIONS]->(concept:Concept)
RETURN concept.name, COUNT(*) AS mentions
ORDER BY mentions DESC
LIMIT 10
```
