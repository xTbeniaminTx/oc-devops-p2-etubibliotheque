# EtudiantFrontend

Application Angular 19 de la bibliothèque, pour les agents.

- port : `4200`
- les appels `/api` sont redirigés vers le back-end sur le port `8080` (`proxy.conf.json`)

Prérequis et ordre de démarrage : voir le [README à la racine](../README.md).
Le back-end doit être démarré avant le front-end.

## Démarrage

Depuis ce dossier :

```
npm install
npm run start
```

Puis ouvrir http://localhost:4200.
La page se recharge automatiquement à chaque modification du code.

## Écrans

- `/register` : création d'un compte agent

## Tests unitaires

Les tests utilisent Jest :

```
npm test
```

## Build

```
npm run build
```

Les fichiers compilés sont placés dans `dist/`.
