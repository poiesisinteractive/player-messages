# Beyond the Pier — Nouvelles du port

Ce dépôt **public** sert le fichier `messages.json` que le jeu lit au démarrage. Les messages s'affichent dans le panneau
« Nouvelles du port » de l'accueil : notes de version, appels à playtests, enquêtes.

- **Adresse lue par le jeu** : `https://raw.githubusercontent.com/poiesisinteractive/player-messages/main/messages.json`.
  Elle est réglée dans `Mods/Core/Values/PlayerMessages/Source.json`, champ `remoteUrl`, du dépôt du jeu.
- **Sans réseau** : le jeu affiche les messages livrés dans le build (`Mods/Core/Values/PlayerMessages/Messages.json`).
  Une modification ici atteint donc les joueurs **sans nouvelle version du jeu**.
- **La démo a son propre fichier** (#1410) : `messages-demo.json`, lu par les builds de démo (`demoBuild` dans
  `Mods/Config.json`) à l'adresse `https://raw.githubusercontent.com/poiesisinteractive/player-messages/main/messages-demo.json`
  (champ `demoRemoteUrl` de `Source.json`). Les joueurs de la démo ne voient jamais `messages.json`, ni l'inverse. Sans
  réseau, la démo affiche `MessagesDemo.json`, livré dans son build. Ses versions s'écrivent `demo 0.1` : l'étape démo
  vient après l'alpha et avant la bêta (`alpha < demo < beta < rc < sortie`).

## Format

```json
{
  "format": 1,
  "messages": [
    {
      "id": "release-alpha-3.1.1",
      "date": "2026-11-11",
      "type": "release",
      "minVersion": "alpha 3.1",
      "maxVersion": "alpha 3",
      "texts": { "FR": { "title": "…", "body": "…", "items": ["…"] }, "EN": { }, "ES": { }, "DE": { } }
    }
  ]
}
```

- **`id`** : unique et stable. Le jeu s'en sert pour savoir quels messages le joueur a déjà vus.
- **`type`** : `release`, `announcement`, `playtest`, `survey`, `event`, `maintenance` ou `update`.
- **`texts`** : les quatre langues FR, EN, ES et DE.
- **`minVersion` / `maxVersion`** : facultatifs, bornes incluses. Une borne ne compare que ce qu'elle précise : `"maxVersion": "alpha 3"` couvre toutes les alpha 3.x.
- **`image` / `link`** : facultatifs, en https seulement.

Spécification complète : `docs/Messages_Joueurs.md` dans le dépôt du jeu.

## Règles
- Un JSON invalide est ignoré par le jeu. Vérifiez-le avant de pousser, par exemple avec `python3 -m json.tool messages.json`.
- Les messages les plus récents vont en tête de liste.
- Taille maximale du fichier : 256 Ko (`maxFeedBytes`).
