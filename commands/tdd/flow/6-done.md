# /tdd:flow:6-done

Commit les changements et finalise la tâche.

## Instructions

### 1. Charger le contexte

Read `.tdd-context.md` (lightweight).

Verify `docs/state.json`: `current.phase` must be "docs".

### 2. Vérification finale

```bash
dotnet build && dotnet test  # Tout doit passer
```

Si échec: ne pas committer, retourner en phase appropriée.

### 3. Analyser les changements

```bash
# En parallèle
git status
git diff --stat
git log -3 --oneline
```

Vérifier:
- Fichiers modifiés correspondent à `.tdd-context.md > Fichiers`?
- Pas de fichiers temporaires ou debug?
- Style de commit des derniers commits?

### 4. Mettre à jour state.json

```json
{
  "current": {
    "epic": "E1",
    "task": null,
    "phase": null
  },
  "epics": {
    "E1": {
      "status": "in_progress",
      "completed": ["T1", "T2", "T3", "T4"]  // Ajouter la tâche
    }
  }
}
```

Vérifier si l'epic est complété (comparer avec liste des tâches dans epic file).

**Si toutes les tâches complétées:**
- Mettre `epics[E{N}].status` = "completed"
- Passer `current.epic` au prochain epic

### 5. Commit message

Format: `E{N}: {description courte}`

Basé sur:
- `.tdd-context.md > Objectif`
- Style des commits récents
- git diff

```bash
git add .

git commit -m "$(cat <<'EOF'
E{N}: {description courte de la tâche}

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
EOF
)"
```

### 6. Vérifier le commit

```bash
git log -1 --stat
git status  # Doit être clean
```

### 7. Nettoyer .tdd-context.md (optionnel)

Supprimer `.tdd-context.md` ou garder pour historique (votre choix).

### 8. Créer PR si epic terminé

**Seulement si epic completé** et remote configuré:

```bash
# Vérifier remote
git remote -v

# Si remote existe
git push -u origin epic{N}

# Créer PR
gh pr create --title "Epic E{N}: {nom}" --body "$(cat <<'EOF'
## Summary

{Description de ce que l'epic accomplit, de .tdd-context.md de la dernière tâche}

## Tasks completed

{Liste des tâches de l'epic}

## Test plan
- [x] `dotnet build` passes
- [x] `dotnet test` passes

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

### 9. Rapport final

**Tâche terminée, epic en cours:**
```
## ✓ Done: [E1] T4 - Titre

**Commit:** [hash] E{N}: {description}
**Epic E1:** [X]/[Y] tasks completed

**Next:** `/tdd:flow:1-analyze` pour T{X+1}
```

**Epic terminé + PR:**
```
## ✓ Epic E{N} terminée

**Tâches:** T1-T{X} (all completed)
**PR:** #{N} - {URL}

**Next epic:** E{N+1} - {nom}

Review/merge la PR, puis `/tdd:flow:1-analyze`
```

## Notes

- NE PAS push automatiquement (laisser l'utilisateur décider)
- Si pre-commit hook modifie des fichiers:
  - Vérifier que le commit est le nôtre (git log -1 --format='%an %ae')
  - Si oui: amend avec les changements du hook
  - Si non: créer nouveau commit
