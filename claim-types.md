# Types de claims (I1-I11)

Quand OpenTruth analyse un discours politique, chaque affirmation est classifiee dans l'une des 11 categories suivantes.

## I1 — Attribution

Une affirmation attribuee a une personne ou une organisation.

> *Exemple : "Le ministre a declare que le deficit serait reduit de 3%."*

Ce type de claim necessite de verifier qui a dit quoi, et si la citation est exacte.

## I2 — Evenement

Une affirmation concernant un evenement passe ou present.

> *Exemple : "Un vote a eu lieu a l'Assemblee nationale le 5 mars."*

Verification : l'evenement s'est-il produit comme decrit ?

## I3 — Definition factuelle

Une affirmation presentee comme un fait etabli.

> *Exemple : "La Suisse est un Etat federal compose de 26 cantons."*

Ce type de claim est generalement verifiable dans des sources officielles.

## I4 — Interpretation

Une affirmation qui interprete des faits avec un jugement.

> *Exemple : "Cette politique a ete un echec total."*

Les interpretations melangent faits et opinions — OpenTruth verifie la base factuelle, pas le jugement.

## I5 — Juridique

Une affirmation concernant le droit ou la legalite.

> *Exemple : "Cette loi interdit le cumul des mandats."*

Verification dans les textes juridiques officiels (codes, lois, jurisprudence).

## I6 — Scientifique

Une affirmation basee sur la science ou la recherche.

> *Exemple : "Les etudes montrent que les vaccins reduisent la transmission."*

Verification dans la litterature scientifique (publications, meta-analyses, rapports d'agences).

## I7 — Statistique

Une affirmation contenant des chiffres ou des donnees quantitatives.

> *Exemple : "Le chomage est a 7,1% au troisieme trimestre 2025."*

Verification dans les bases de donnees statistiques officielles (INSEE, OFS, Eurostat).

## I8 — Comparatif

Une affirmation comparant deux elements (pays, periodes, politiques).

> *Exemple : "La France depense plus pour la defense que l'Allemagne."*

Necessite de verifier les deux cotes de la comparaison.

## I9 — Predictif / Promesse

Une affirmation portant sur le futur ou un engagement.

> *Exemple : "Nous reduirons les impots d'ici 2027."*

Les promesses ne sont pas verifiables au moment ou elles sont faites — OpenTruth les identifie et les suit dans le temps.

## I10 — Historique

Une affirmation portant sur un fait historique.

> *Exemple : "En 1990, le mur de Berlin etait deja tombe."*

Verification dans les sources historiques et encyclopediques.

## I11 — Causal

Une affirmation etablissant un lien de cause a effet.

> *Exemple : "L'immigration cause le chomage."*

Le type le plus complexe : OpenTruth verifie si le lien causal est soutenu par des donnees, ou s'il s'agit d'une correlation presentee comme une causalite.
