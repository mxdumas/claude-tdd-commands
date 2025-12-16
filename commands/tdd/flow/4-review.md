# /tdd:flow:4-review

Revue critique du code.

## Instructions

### 1. Charger le contexte

Read `.tdd-context.md` (lightweight).

Verify `docs/state.json`: `current.phase` must be "review".

### 2. Build et tests

```bash
dotnet build && dotnet test
```

Si échec, corriger d'abord.

### 3. Vérifier le coverage

**Lancer les commandes de coverage:**

```bash
rm -rf ./TestResults
dotnet test --collect:"XPlat Code Coverage" --results-directory ./TestResults
reportgenerator -reports:"./TestResults/*/coverage.cobertura.xml" -targetdir:"./TestResults/CoverageReport" -reporttypes:TextSummary
cat ./TestResults/CoverageReport/Summary.txt
```

**Lire le baseline** de `.tdd-context.md` (section `📊 Baseline`).

**Vérifier les seuils:**
- Line coverage (Spotlight.Core): ≥ 80%
- Branch coverage: ≥ 60%
- Pas de régression vs baseline
- Nouvelles classes: ≥ 90%

**Analyser le résultat:**
- Coverage actuel: [X.X]%
- Baseline: [Y.Y]%
- Delta: [+/- Z.Z]%
- Status: ✓ Pass / ❌ Fail (avec raison)
- Classes sous 80%: [liste si applicable]

Si coverage échoue: ajouter tests manquants avant de continuer.

### 4. Revue du code

Use git diff --name-only to list changed files.

Pour chaque fichier modifié/créé:

**A. Adéquation avec le besoin**
- Le code fait-il exactement ce qui était demandé?
- Respect du scope de `.tdd-context.md > Décisions > Scope`?
- Pas de gold plating (features non demandées)?

**B. Design - Signaux d'alerte**
- Classe > 200 lignes, méthode > 30 lignes
- Plus de 3 niveaux d'indentation
- Plus de 4 paramètres
- Noms génériques (data, manager, helper)

**C. Robustesse**
- Erreurs gérées correctement?
- Ressources libérées (IDisposable, using)?
- Cas limites de `.tdd-context.md > Edge cases` couverts?

**D. Conformité standards (de .tdd-context.md > Conventions clés)**
- [ ] File-scoped namespaces
- [ ] Records pour modèles immuables
- [ ] Nullable enabled (pas de suppression avec !)
- [ ] CancellationToken sur async
- [ ] XML docs en français

### 5. Revue des tests

**Coverage manquante?**
- Chaque `if` doit avoir un test pour chaque branche
- Chaque `throw` doit avoir un test qui le déclenche

**Types de tests appropriés?**
| Question | Si oui → |
|----------|----------|
| Y a-t-il des I/O fichiers non testés en intégration ? | Ajouter test dans `IntegrationTests` |
| Les interactions multi-composants sont-elles couvertes ? | Ajouter test d'intégration |

### 6. Corrections

**Mineurs:** Corriger directement (renommages, formatage, tests simples manquants).

**Majeurs:** Présenter le problème et demander confirmation avant de corriger.

### 7. Mettre à jour .tdd-context.md

Ajouter section après `## 📊 Baseline`:

```markdown
### Coverage final
- Line: [X.X]% (baseline: [Y.Y]%, delta: [+/-Z.Z]%)
- Branch: [X.X]%
- Nouvelles classes: [X.X]%
- Status: ✓ Thresholds met

### Review
- Corrections mineures: [N] appliquées
- Design: ✓ OK
- Standards: ✓ Conformes
```

### 8. Finaliser

Garder `current.phase` = "review".

```
## REVIEW: [E1] T4 - Titre

**Build:** ✓ OK
**Tests:** [N]/[N] passed ✓

**Coverage:**
- Line: [X.X]% (baseline: [Y.Y]%, +[Z.Z]%) ✓
- Branch: [X.X]% ✓
- Nouvelles classes: [X.X]% ✓

**Corrections:** [N] mineures appliquées
**Standards:** ✓ Conformes

Lancer `/tdd:flow:5-docs` pour documenter.
```

## Questions utiles

- Si je modifie ce code dans 6 mois, qu'est-ce qui me poserait problème?
- Ce test pourrait-il passer alors que le code est cassé?
- Est-ce la solution la plus simple qui fonctionne?
