# SkyCheck ✈️

Web map affichant le trafic dans l'espace aérien de France métropolitaine, mise à jour automatiquement toutes les 10 minutes.

**Démo :** https://ziiiipr.github.io/sky_check/

---

## Fonctionnement

Page GitHub affichant les données d'un ping régulier (10 min) de l'API OpenSky network. L'API OpenSky n'autorisant pas les requêtes directes depuis un navigateur (CORS), la meilleure alternative a été un cron job via GitHub actions qui écrit ses données dans un geojson.

```
GitHub Actions (cron, toutes les 10 min)
        │
        ▼
scripts/fetch_flights.py interroge OpenSky, écrit docs/data/flights.geojson
        │
        ▼
Commit / push automatique
        │
        ▼
GitHub Pages + Leaflet (docs/index.html) charge le geojson et affiche les avions.
```

## Stack technique

| Composant | Rôle |
|---|---|
| Python (`requests`) | Interroge l'API OpenSky côté serveur |
| GitHub Actions | Planifie l'exécution (cron) et commit le résultat automatiquement |
| Leaflet | Affichage de la carte et des avions côté client |
| GitHub Pages | Hébergement statique du site & données |
