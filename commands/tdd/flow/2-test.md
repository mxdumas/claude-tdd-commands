# /tdd:flow:2-test

Écrit des tests significatifs (phase RED).

## Instructions

### 1. Charger le contexte

Read `.tdd-context.md` (lightweight, all context already extracted).

DO NOT read architecture.md, standards.md, or epic files - everything is in .tdd-context.md.

Verify `docs/state.json`: `current.phase` must be "test".

### 2. Capturer le coverage baseline

```bash
rm -rf ./TestResults
dotnet test --collect:"XPlat Code Coverage" --results-directory ./TestResults
reportgenerator -reports:"./TestResults/*/coverage.cobertura.xml" -targetdir:"./TestResults/CoverageReport" -reporttypes:TextSummary
cat ./TestResults/CoverageReport/Summary.txt
```

**Ajouter à .tdd-context.md** (après la section `## 📖 Conventions clés`):

```markdown
## 📊 Baseline
- Coverage: [X.X]%
- Tests: [N] tests
```

### 3. Écrire les tests (RED phase)

**Contexte à charger:**
1. Specs de tests (section `🧪 Tests` de .tdd-context.md)
2. Conventions (section `📖 Conventions clés`)
3. Tests similaires (lire les exemples mentionnés pour comprendre les patterns)

**Écrire les tests qui:**
- Suivent le nommage: `Action_Context_ExpectedResult`
- Utilisent la structure Arrange/Act/Assert
- Couvrent les comportements des specs (pas les détails d'implémentation)
- Incluent les edge cases de .tdd-context.md
- Respectent les conventions (XML docs en français, etc.)

**Exigences RED:**
- Tests DOIVENT compiler
- Tests DOIVENT échouer (pas d'implémentation encore)

**Créer les fichiers** listés dans `.tdd-context.md` section "Fichiers > Créer".

**Organiser par catégorie:**
- Comportement principal
- Contrats API
- Edge cases
- Gestion d'erreurs

### 4. Build et vérifier RED

```bash
dotnet build  # Doit passer
dotnet test   # Doit ÉCHOUER (RED)
```

Si tests passent → problème, les tests ne testent rien.
Si tests ne compilent pas → corriger.

### 5. Mettre à jour .tdd-context.md

Ajouter section après `## 🧪 Tests`:

```markdown
### Résultat RED
- ✓ Tests créés: [N] tests
- ✓ Build: OK
- ✓ Tests: 0/[N] passed (RED) ✓
```

### 6. Finaliser

Mettre `current.phase` = "dev" dans state.json.

```
## RED: [E1] T4 - Titre

**Tests créés:** [N] tests dans [M] fichiers

**Par catégorie:**
- Comportement: [N]
- Contrat API: [N]
- Edge cases: [N]
- Erreurs: [N]

**Coverage baseline:** [X.X]% (capturé)
**Build:** OK
**Tests:** 0/[N] passed (RED) ✓

Lancer `/tdd:flow:3-dev` pour implémenter (GREEN).
```

## Principes de tests (rappel pour l'agent)

**Bon test:**
```csharp
[Fact]
public void Import_WithMultipleFixtures_GroupsThemByUniverse()
{
    // Arrange
    var path = GetTestFile("multi-fixture.gdtf");

    // Act
    var result = _importer.Import(path);

    // Assert
    var universes = result.Fixtures.GroupBy(f => f.Universe);
    Assert.Equal(2, universes.Count());
}
```

**Mauvais test:**
```csharp
[Fact]
public void Test1() // ❌ Nom générique
{
    var result = _importer.Import("file.gdtf");
    Assert.NotNull(result); // ❌ Ne teste rien de significatif
}
```
