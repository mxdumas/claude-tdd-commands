# /tdd:flow:3-dev

Implémente le code pour faire passer les tests (phase GREEN).

**Code propre dès le départ.** Simple ≠ laid. Minimal = pas de superflu, pas de dette technique.

## Instructions

### 1. Charger le contexte

Read `.tdd-context.md` (lightweight, all context already extracted).

Verify `docs/state.json`: `current.phase` must be "dev".

### 2. Lire les tests d'abord

Lire les fichiers de tests créés en phase RED.

Comprendre :
- API attendue (signatures, types)
- Comportements vérifiés
- Cas d'erreur à gérer

### 3. Implémenter

**Ordre :**
1. Créer fichiers listés dans `.tdd-context.md > Fichiers > Créer` (code, pas tests)
2. Modifier fichiers listés dans "Modifier" (si applicable)

**Principes :**

| Principe | Description |
|----------|-------------|
| **YAGNI** | N'implémenter que ce qui est testé |
| **KISS** | Solution la plus simple qui fonctionne |

**Faire :**
- Code propre et lisible dès le départ
- Suivre conventions de `.tdd-context.md > Conventions clés`
- Suivre pattern de `.tdd-context.md > Implémentation`
- Noms clairs

**Ne pas faire :**
- Fonctionnalités non testées
- Optimisation prématurée
- Gold plating (features bonus non demandées)
- Sur-engineering (abstractions inutiles)

### 4. Faire passer les tests

```bash
dotnet test
```

**Si échec :**
1. Lire le message d'erreur
2. Corriger l'implémentation (pas le test)
3. Re-tester

**Itérer** jusqu'à 100% pass.

### 5. Vérification finale

```bash
dotnet build && dotnet test  # 100% pass requis
```

### 6. Mettre à jour .tdd-context.md

Ajouter section après `## 💻 Implémentation`:

```markdown
### Résultat GREEN
- ✓ Fichiers créés: [liste]
- ✓ Fichiers modifiés: [liste ou "Aucun"]
- ✓ Tests: [N]/[N] passed (GREEN) ✓
```

### 7. Finaliser

Mettre `current.phase` = "review" dans state.json.

```
## GREEN: [E1] T4 - Titre

**Fichiers créés:** [N]
**Fichiers modifiés:** [N]
**Tests:** [N]/[N] passed ✓

Lancer `/tdd:flow:4-review` pour la revue.
```

## Anti-patterns

```csharp
// ❌ Gold plating - fonctionnalité non demandée
public void Import(string path, bool validate = true, ILogger? logger = null)
// → Implémenter seulement ce que les tests demandent

// ❌ Sur-engineering
public interface IProcessor { }
public class Processor : IProcessor { }
public class ProcessorFactory { }
// → Direct et simple si pas testé autrement

// ❌ Optimisation prématurée
public class CachedImporter // Pas de test de cache = pas de cache
```

## Situations difficiles

**Test ambigu:**
Demander à l'utilisateur quelle approche suivre.

**Problème de design révélé:**
- Si simple: résoudre maintenant
- Si complexe: demander avant de continuer
