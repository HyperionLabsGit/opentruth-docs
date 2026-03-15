# Verdicts (H1.1-H1.8)

Apres verification, chaque claim recoit un verdict. OpenTruth utilise 8 niveaux de verdict, repartis en deux groupes.

## Claims verifiables

### H1.1 — Vrai 🟢

L'affirmation est confirmee par au moins 2 sources fiables concordantes.

> *Exemple : "Le taux de chomage en France est de 7,1%" — confirme par l'INSEE et Eurostat.*

### H1.2 — Plutot vrai 🟢

L'affirmation est globalement confirmee, avec des nuances mineures.

> *Exemple : "La France a reduit ses emissions de CO2" — vrai, mais le pourcentage exact differe selon les sources.*

### H1.3 — Melange / Partiellement faux 🟡

L'affirmation contient des elements vrais ET des elements faux.

> *Exemple : "Le budget de la defense a double en 10 ans" — il a augmente significativement, mais pas double.*

### H1.4 — Trompeur 🟠

Techniquement vrai, mais omet un contexte crucial qui change l'interpretation.

> *Exemple : "Le chomage a baisse de 20%" — vrai sur une periode choisie, mais il avait augmente de 30% l'annee precedente.*

Signaux typiques : chiffres cherry-picks, citations tronquees, generalisations abusives.

### H1.5 — Faux 🔴

L'affirmation est contredite par les sources fiables.

> *Exemple : "La Suisse fait partie de l'Union europeenne" — faux, la Suisse n'est pas membre de l'UE.*

### H1.6 — Non verifiable ⚪

Les preuves disponibles sont insuffisantes pour conclure.

> *Exemple : "Des negociations secretes ont eu lieu" — aucune source ne confirme ni ne dement.*

## Claims non verifiables

### H1.7 — Opinion / Interpretation 🟣

Pas une affirmation factuelle verifiable : opinion, prediction ou jugement de valeur.

> *Exemple : "Ce gouvernement est le pire de la Ve Republique."*

OpenTruth identifie les opinions mais ne les verifie pas — ce ne sont pas des faits.

### H1.8 — Satire / Parodie 🩷

Contenu clairement satirique ou parodique.

> *Exemple : Tout contenu du Gorafi, The Onion, NordPresse.*

OpenTruth detecte la satire pour eviter de la traiter comme une affirmation serieuse.

---

## Comprendre les couleurs

| Couleur | Signification |
|---------|---------------|
| 🟢 Vert | Confirme (H1.1, H1.2) |
| 🟡 Jaune | Melange (H1.3) |
| 🟠 Orange | Trompeur (H1.4) |
| 🔴 Rouge | Faux (H1.5) |
| ⚪ Gris | Non verifiable (H1.6) |
| 🟣 Violet | Opinion (H1.7) |
| 🩷 Rose | Satire (H1.8) |
