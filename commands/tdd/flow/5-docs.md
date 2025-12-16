# /tdd:flow:5-docs

Documente la tâche complétée.

## Instructions

### 1. Charger le contexte

Read `.tdd-context.md` (lightweight).

Verify `docs/state.json`: `current.phase` must be "review".

### 2. Mettre à jour CHANGELOG.md

**Ajouter entrée** sous la section appropriée (Added/Changed/Fixed):

- Format: `- [Module]: description of change`
- Être **spécifique** (mentionner classes/methods)
- Écrire du point de vue utilisateur/développeur
- Voir exemples de bonnes entrées en fin de document

**Exemple:**
```markdown
### Added
- GDTF import: extract color wheels with CIE xyY values and gobo images
- `FixtureType.Wheels` collection for accessing fixture wheel definitions
```

### 3. Vérifier les XML docs

**Lire les fichiers créés** (de `.tdd-context.md > Fichiers > Créer`).

**Si APIs publiques:**
- Vérifier que tous les types/méthodes publics ont XML docs en français
- Ajouter ceux qui manquent

**Rappel format:**
```csharp
/// <summary>
/// Description en français.
/// </summary>
public class Foo { }
```

### 4. Évaluer si ADR nécessaire

**Lire `.tdd-context.md > Décisions`.**

**Créer ADR si:**
- Choix entre plusieurs approches valides
- Décision qui impacte plusieurs modules
- Trade-off significatif (performance vs simplicité)

**Ne pas créer si:**
- Implémentation standard sans alternative
- Décision locale à un fichier
- Choix évident sans trade-off

**Si ADR nécessaire:**
- Créer dans `docs/dev/decisions/`
- Utiliser template si existe
- Numérotation: prochain numéro disponible

### 5. Vérifier docs existantes (optionnel)

**Seulement si changements significatifs**, vérifier si ces docs doivent être mises à jour:

| Document | Mettre à jour si... |
|----------|---------------------|
| `docs/dev/api/{module}.md` | Nouvelle API publique importante |
| `docs/dev/architecture.md` | Nouveau composant ou flux majeur |
| `README.md` | Changement visible utilisateur |

Si mise à jour nécessaire: faire maintenant.

### 6. Mettre à jour .tdd-context.md

Ajouter section finale:

```markdown
## 📚 Documentation
- ✓ CHANGELOG updated ([Added/Changed/Fixed])
- ✓ XML docs complete
- ✓ ADR created: [NNN-title] / Not needed
- ✓ Other docs: [list] / None
```

### 7. Mettre à jour phase

```json
{
  "current": {
    "phase": "docs"
  }
}
```

### 8. Rapport

```
## Documentation: [E1] T4 - Titre

### Mis à jour
- `CHANGELOG.md` - Section [Added/Changed/Fixed]
- XML docs - [N] ajoutés / Déjà complets

### Créé
- `docs/dev/decisions/[NNN-title].md` / Pas d'ADR nécessaire

### Vérifié (pas de changement nécessaire)
- `docs/dev/architecture.md`
- `README.md`

Lancer `/tdd:flow:6-done` pour committer et finaliser.
```

## Bonnes pratiques CHANGELOG

**Bon:**
```markdown
### Added
- GDTF import: extract color wheels with CIE xyY values and gobo images
- `FixtureType.Wheels` collection for accessing fixture wheel definitions

### Changed
- `GdtfImporter.Import()` now extracts channel functions with DMX ranges
```

**Mauvais:**
```markdown
### Added
- Added wheels
- New feature
```

## Quand créer un ADR

**Créer:**
- Choix entre plusieurs approches valides
- Décision qui impacte plusieurs modules
- Trade-off significatif (performance vs simplicité)

**Ne pas créer:**
- Implémentation standard sans alternative
- Décision locale à un fichier
- Choix évident sans trade-off
