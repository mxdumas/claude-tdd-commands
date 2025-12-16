# Commande: /tdd:flow:next

Exécute automatiquement la prochaine étape du flow TDD selon la phase actuelle.

## Instructions

### 1. Charger l'état

Lire `docs/state.json`.

Si le fichier n'existe pas → afficher :
```
Projet non initialisé. Lancer `/tdd:init:1-project` d'abord.
```

### 2. Déterminer et exécuter la prochaine étape

| Phase actuelle | Action |
|----------------|--------|
| `null` | Exécuter `/tdd:flow:1-analyze` |
| `analyze` | Exécuter `/tdd:flow:1-analyze` (continuer/terminer) |
| `test` | Exécuter `/tdd:flow:2-test` |
| `dev` | Exécuter `/tdd:flow:3-dev` |
| `review` | Exécuter `/tdd:flow:4-review` |
| `docs` | Exécuter `/tdd:flow:5-docs` |

**Note:** Après `docs`, la phase reste `docs` jusqu'à ce que `/tdd:flow:6-done` soit lancé manuellement (commit = action explicite).

### 3. Exécuter la commande

Charger et exécuter les instructions de la commande correspondante.

## Notes

- `/tdd:flow:6-done` n'est jamais appelé automatiquement (commit = décision utilisateur)
- Si epic terminé, suggérer de passer au suivant
- Raccourci pratique pour ne pas avoir à retenir quelle commande lancer
- Toutes les phases (sauf 1-analyze) utilisent `.tdd-context.md` pour le contexte
