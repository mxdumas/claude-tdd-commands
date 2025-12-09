# Claude TDD Commands

Commandes Claude Code pour un workflow Test-Driven Development structuré.

## Installation

### Linux / macOS

```bash
curl -fsSL https://raw.githubusercontent.com/mxdumas/claude-tdd-commands/master/install.sh | bash
```

### Windows (PowerShell)

```powershell
irm https://raw.githubusercontent.com/mxdumas/claude-tdd-commands/master/install.ps1 | iex
```

### Manuel

Cloner le repo et copier le dossier `commands/tdd/` vers `.claude/commands/tdd/` dans votre projet.

## Commandes

### Initialisation (`/tdd:init:*`)

Pour démarrer un nouveau projet avec le workflow TDD.

| Commande | Description |
|----------|-------------|
| `/tdd:init:1-project` | Découverte du projet, définition des epics |
| `/tdd:init:2-architecture` | Définition de l'architecture technique |
| `/tdd:init:3-standards` | Définition des conventions de code |
| `/tdd:init:4-readme` | Génération du README |

### Flow TDD (`/tdd:flow:*`)

Pour le cycle de développement.

| Commande | Description |
|----------|-------------|
| `/tdd:flow:status` | Affiche l'état actuel (epic, tâche, progression) |
| `/tdd:flow:next` | Exécute automatiquement la prochaine étape |
| `/tdd:flow:1-analyze` | Analyse la tâche, écrit les specs |
| `/tdd:flow:2-test` | Écrit les tests (RED) |
| `/tdd:flow:3-dev` | Implémente le code (GREEN) |
| `/tdd:flow:4-review` | Valide standards et couverture |
| `/tdd:flow:5-docs` | Documente, met à jour CHANGELOG |
| `/tdd:flow:6-done` | Commit et mise à jour du state |
| `/tdd:flow:quickfix` | Correction rapide (worktree séparé) |

## Workflow

```
┌─────────────────────────────────────────────────────────────────────────┐
│  INIT (une fois)                                                        │
│  /tdd:init:1-project → 2-architecture → 3-standards → 4-readme          │
└─────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────┐
│  FLOW (par tâche)                                                       │
│  /tdd:flow:1-analyze → 2-test → 3-dev → 4-review → 5-docs → 6-done     │
│       ↑                                                      │          │
│       └──────────────────────────────────────────────────────┘          │
└─────────────────────────────────────────────────────────────────────────┘
```

## Structure créée

```
projet/
├── .claude/commands/tdd/    # Commandes installées
├── docs/
│   ├── state.json           # État du projet
│   ├── current-task.md      # Specs de la tâche en cours
│   ├── epics/               # Définition des epics
│   │   ├── e0-foundation.md
│   │   └── e1-feature.md
│   └── dev/
│       ├── architecture.md
│       └── standards.md
├── CLAUDE.md
├── CHANGELOG.md
└── README.md
```

## Quickfix (corrections rapides)

Pour les petites corrections (<20 lignes) sans passer par le cycle TDD complet.

```bash
# Setup worktree (une seule fois)
git worktree add ../projet-quickfix -b quickfix

# Dans le worktree
/tdd:flow:quickfix "fix typo in error message"
```

Chaque quickfix crée sa propre branche et PR vers la branche courante.

## Raccourcis

- `/tdd:flow:status` - Où en suis-je ?
- `/tdd:flow:next` - Continue automatiquement
- `/tdd:flow:quickfix "desc"` - Correction rapide

## License

MIT
