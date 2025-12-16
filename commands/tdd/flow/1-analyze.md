# /tdd:flow:1-analyze

Analyse interactive pour préparer la prochaine tâche TDD.

## Instructions

### 1. Charger l'état
Lis `docs/state.json`. Si `current.phase` != `null` → erreur, suggérer la bonne commande.

### 2. Déterminer la tâche
- Prochaine tâche non complétée dans `current.epic`
- Si epic complété → passer au prochain

### 3. Explorer le contexte (délégué à agent)

**Lancer agent d'exploration** avec Task tool (model='haiku', subagent_type='Explore'):

```
Analyze the codebase for task [Epic] [Task ID].

Context to explore:
1. Read docs/dev/architecture.md, docs/dev/standards.md
2. Read docs/epics/E{N}.md (epic overview - lightweight)
3. Read docs/epics/E{N}/T{M}.md (task details - specific to this task only)
4. Find files/classes that will be impacted by this task
5. Identify existing patterns used in similar code
6. Find similar existing tests
7. Check for relevant samples in docs/samples/ or tests/*/TestData/
8. Check for relevant ADRs in docs/dev/decisions/

Return a concise summary (max 1000 tokens) with:
- Task understanding (1-2 sentences from E{N}/T{M}.md)
- Files to create (list with brief description)
- Files to modify (list with brief description)
- Existing patterns to follow (2-3 key patterns with file paths)
- Key conventions relevant to this task (extract from standards.md)
- Similar tests found (paths only, max 2-3 examples)
- Risks/complexity points (2-3 items)
- Edge cases to consider (2-3 items)

DO NOT include full file contents. Only paths and brief descriptions.
Load ONLY the specific task file (E{N}/T{M}.md), not the entire epic.
```

**Attendre le résumé de l'agent** (résultat condensé ~500-1000 tokens).

### 4. Analyse silencieuse

À partir du résumé de l'agent, identifier :
1. **Impact** - Quels fichiers/modules seront touchés ?
2. **Dépendances** - De quoi dépend cette tâche ? Qu'est-ce qui en dépendra ?
3. **Risques** - Points de complexité ou d'incertitude
4. **Edge cases** - Cas limites à considérer

### 5. Présenter l'analyse

```
## Analyse: [E1] T4 - Titre

### Ce que j'ai compris
[Résumé de la tâche et son contexte]

### Impact identifié
- Fichiers à créer: X
- Fichiers à modifier: Y
- Dépendances: Z

### Points d'attention
1. [Point technique ou architectural notable]
2. [Risque ou complexité identifiée]
3. [Ambiguïté dans la spec]

### Questions ouvertes
[Liste des questions]
```

### 6. Discussion interactive

Engager une **conversation naturelle** pour clarifier.

**Types de questions :**

| Catégorie | Exemple |
|-----------|---------|
| Clarification | "La spec mentionne X, mais le code fait Y. Quelle approche ?" |
| Architecture | "Deux façons : A (simple) ou B (extensible). Préférence ?" |
| Scope | "Tout faire maintenant ou commencer par le minimum ?" |
| Edge cases | "Que doit-il se passer si [cas limite] ?" |
| Données | "As-tu des fichiers de test spécifiques ?" |

**Règles :**
- Questions **ouvertes** (pas oui/non)
- **Itérer** sur les réponses
- **Challenger** les hypothèses si nécessaire
- Continuer jusqu'à compréhension claire du scope, des décisions, des priorités

### 7. Synthèse des décisions

```
## Décisions finales

### Scope
- [Ce qui est inclus]
- [Ce qui est exclu]

### Architecture
- [Choix techniques retenus]

### Cas limites
- [Comment les gérer]
```

Demander confirmation avant de continuer.

### 8. Mettre à jour state.json

```json
{ "current": { "epic": "E1", "task": "T4", "phase": "analyze" } }
```

### 9. Créer .tdd-context.md (structure stricte)

**IMPORTANT:** Toujours suivre cette structure exacte. Les phases suivantes dépendent de ces sections.

```markdown
# [E1] T4 - Titre de la tâche

## 📋 Session
- Started: 2025-12-15 10:30
- Epic: E1
- Task: T4
- Docs loaded: ✓ architecture.md, standards.md, epic E1

## 🎯 Objectif
[Description en 1-2 phrases de ce qui doit être fait]

## 🛠 Décisions
- **Approche:** [Décision architecturale principale en 1 phrase]
- **Pattern:** [Pattern principal à suivre, avec référence fichier]
- **Scope:** [Ce qui est inclus/exclu]

## 📁 Fichiers

### Créer
- `path/to/file.cs` - [Description brève]
- `tests/path/to/test.cs` - [Description brève]

### Modifier
- (liste ou "Aucun")

### Code existant pertinent
- `path/to/similar.cs` - [Pourquoi c'est pertinent, pattern à suivre]

## 🧪 Tests ([N] prévus)

### Specs
1. [Comportement 1 à tester]
2. [Comportement 2 à tester]
3. [Edge case à tester]

### Exemples de tests similaires
- `tests/path/to/similar_test.cs` - [Pattern de test à suivre]

## 💻 Implémentation

### Approche
[Comment implémenter, en 2-3 phrases]

### Code pattern (si pertinent)
```csharp
// Exemple de pattern à suivre
public class Example
{
    // Structure attendue
}
```

## 📖 Conventions clés
- Namespaces: file-scoped
- Records: for immutable data
- Async: CancellationToken last param
- XML docs: French
- [Autre convention pertinente à cette tâche]

## ⚠️ Risques
- [Risque 1]
- [Risque 2]

## 🔍 Edge cases
- [Cas limite 1]
- [Cas limite 2]

## 📝 Notes
[Autres points d'attention ou contexte important]
```

### 10. Finaliser

Mettre `current.phase` = "test".

```
## Prête: [E1] T4 - Titre

**Scope:** [Résumé en 1 ligne]
**Décisions clés:** [Liste]
**Tests prévus:** X tests
**Code prévu:** Y fichiers
**Contexte:** .tdd-context.md créé avec toutes les décisions

Lancer `/tdd:flow:2-test` pour écrire les tests (RED).
```

## Adapter la profondeur

| Tâche simple | Tâche complexe |
|--------------|----------------|
| 1-2 questions | Discussion approfondie |
| Patterns évidents | Exploration des alternatives |
| Agent Haiku | Agent Sonnet si très complexe |
