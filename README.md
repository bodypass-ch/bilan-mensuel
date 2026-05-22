# Bilan Mensuel — Body Pass

Page perso "Wrapped-style" pour les porteurs Body Pass, rendue mensuellement (envoi le 15 de chaque mois).

## URL pattern

```
https://bodypass-ch.github.io/bilan-mensuel/?t=<USER_TOKEN>
```

`USER_TOKEN` = `sha256("bodypass-{carte_num}")[:10]`, partagé avec les autres campagnes du moteur multi-campagnes (notations, utilisation).

## Structure

```
bilan-mensuel/
├── index.html          # Page consommatrice — fetch data/<token>.json
├── data/
│   ├── demo.json       # Sample payload pour tests visuels
│   └── <token>.json    # Un fichier par porteur, pushé par le moteur
├── .nojekyll
└── README.md
```

## Pattern data

```json
{
  "name": "Prénom",
  "window": { "start": "22 avril 2026", "end": "22 mai 2026" },
  "stats": { "visites": 4, "top_category": "Spa", "economies_chf": 180 },
  "visites": [ { "etab_name", "category", "canton", "visit_count", "last_visit_fr", "etab_photo_url", "etab_url_public" } ],
  "recos": [ { "slot", "badge", "etab_name", "category", "canton", "blurb", "etab_photo_url", "etab_url_public" } ]
}
```

## Génération

Le module `campaigns/bilan_30d/build.py` dans `bodypass-campaigns-engine` calcule pour chaque porteur du segment Mailchimp "Bilan Mensuel" (`STATUT_CRT=active` ET `JOURSDEPV<=30`) :

- Stats des 30 derniers jours glissants
- Top 4 établissements visités (dédup, triés par visit_count desc)
- 3 propositions : favorite_category, same_canton, novelty

Le moteur est déclenché par le workflow n8n `bp-moteur-campagnes` une fois par mois (14 à 23h00).

## Modification de l'HTML

⚠️ Le bot n8n ne touche **jamais** à `index.html`. Toute modification du template = commit manuel direct.

Voir aussi le repo source `bodypass-ch/utilisation-30d` pour le pattern parent.
