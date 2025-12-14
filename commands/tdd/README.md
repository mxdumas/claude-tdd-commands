# TDD Commands

Commandes Claude Code pour le workflow Test-Driven Development.

## Vue d'ensemble

```
/init (natif) → /tdd:init:* → /tdd:flow:*
```

## Initialisation (`/tdd:init:*`)

Commandes pour initialiser un nouveau projet TDD.

| Commande | Description | Crée |
|----------|-------------|------|
| `/tdd:init:1-project` | Découverte du projet, définition des epics | `docs/state.json`, `docs/epics/*.md` |
| `/tdd:init:2-architecture` | Architecture technique | `docs/dev/architecture.md` |
| `/tdd:init:3-standards` | Conventions de code | `docs/dev/standards.md` |
| `/tdd:init:4-readme` | README final | `README.md` |

**Flow recommandé :**
```
/init → /tdd:init:1-project → /tdd:init:2-architecture → /tdd:init:3-standards → /tdd:init:4-readme
```

Chaque commande est indépendante et pose les questions nécessaires si le contexte manque.

## Flow TDD (`/tdd:flow:*`)

Commandes pour le cycle de développement TDD.

| Commande | Phase | Description |
|----------|-------|-------------|
| `/tdd:flow:status` | - | Affiche l'état actuel (epic, tâche, phase, progression) |
| `/tdd:flow:next` | - | Exécute automatiquement la prochaine étape |
| `/tdd:flow:1-analyze` | Plan | Analyse la tâche, écrit specs dans `docs/current-task.md` |
| `/tdd:flow:2-test` | RED | Écrit les tests, capture coverage baseline |
| `/tdd:flow:3-dev` | GREEN | Implémente jusqu'à ce que tous les tests passent |
| `/tdd:flow:4-review` | Validate | Vérifie standards, coverage, qualité |
| `/tdd:flow:5-docs` | Document | Documente, met à jour CHANGELOG |
| `/tdd:flow:6-done` | Commit | Commit, met à jour state.json |

**Cycle :**
```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   1-analyze → 2-test → 3-dev → 4-review → 5-docs → 6-done  │
│       ↑                                              │      │
│       └──────────────────────────────────────────────┘      │
│                     (tâche suivante)                        │
└─────────────────────────────────────────────────────────────┘
```

## Code Coverage

Le coverage est intégré au flow TDD :

**Quand :**
- `2-test` : Capture le baseline avant d'écrire les tests
- `4-review` : Valide que le coverage respecte les seuils

**Commandes :**
```bash
# Exécuter tests avec coverage
dotnet test --collect:"XPlat Code Coverage" --results-directory ./TestResults

# Générer rapport texte
reportgenerator -reports:"./TestResults/*/coverage.cobertura.xml" -targetdir:"./TestResults/CoverageReport" -reporttypes:TextSummary

# Afficher résumé
cat ./TestResults/CoverageReport/Summary.txt
```

**Seuils minimaux :**
| Métrique | Seuil |
|----------|-------|
| Line coverage (Spotlight.Core) | ≥ 80% |
| Branch coverage | ≥ 60% |
| Nouvelles classes | ≥ 90% |

**Prérequis :**
```bash
dotnet tool install -g dotnet-reportgenerator-globaltool
```

## Structure des fichiers

```
docs/
├── state.json          # État du projet (epic, tâche, phase)
├── current-task.md     # Specs de la tâche en cours
├── epics/
│   ├── e0-foundation.md
│   └── e1-feature.md
└── dev/
    ├── architecture.md
    ├── standards.md
    └── api/
```

## state.json

```json
{
  "current": {
    "epic": "E1",
    "task": "T2",
    "phase": "dev"
  },
  "epics": {
    "E0": { "status": "completed", "completed": ["T1", "T2", "T3"] },
    "E1": { "status": "in_progress", "completed": ["T1"] }
  }
}
```

## Quickfix (`/tdd:quickfix`)

Pour les corrections rapides qui ne nécessitent pas le processus TDD complet.

```bash
# Setup worktree (une seule fois)
git worktree add ../spotlight-quickfix -b quickfix

# Dans le worktree, lancer la commande
/tdd:flow:quickfix "fix typo in error message"
```

**Principe:** Une branche `quickfix` persistante qui accumule les commits. Une seule PR vers la branche courante (epic1, epic2, etc.).

**Quand utiliser:**
- Changements < 20 lignes
- Corrections évidentes (typos, imports, null checks)
- Pas de nouvelle fonctionnalité

**Workflow:**
- Fenêtre 1 (`spotlight`) → TDD normal
- Fenêtre 2 (`spotlight-quickfix`) → `/tdd:quickfix`
- Merger la PR quand prêt → `git pull` dans fenêtre 1

Voir `/tdd:quickfix` pour les détails.

## Raccourcis

- `/tdd:flow:status` - Où en suis-je ?
- `/tdd:flow:next` - Continue automatiquement
- `/tdd:flow:quickfix "desc"` - Correction rapide (worktree)
