# /tdd:flow:2-test

Écrit des tests significatifs (phase RED).

## Instructions

### 1. Vérifier
- `docs/state.json` : `current.phase` doit être "test"
- Charger `docs/current-task.md`
- Lire les tests similaires existants pour comprendre les patterns

### 2. Capturer le coverage baseline

Avant d'écrire les tests, capturer le coverage actuel pour comparaison :

```bash
# Nettoyer les anciens résultats
rm -rf ./TestResults

# Exécuter avec coverage
dotnet test --collect:"XPlat Code Coverage" --results-directory ./TestResults

# Générer le rapport texte
reportgenerator -reports:"./TestResults/*/coverage.cobertura.xml" -targetdir:"./TestResults/CoverageReport" -reporttypes:TextSummary

# Afficher le résumé
cat ./TestResults/CoverageReport/Summary.txt
```

**Noter le coverage baseline** (line coverage %) pour comparaison à l'étape review.

### 3. Réflexion avant d'écrire

**Comportement métier :**
- Quels sont les scénarios d'utilisation réels ?
- Quelles règles métier doivent être respectées ?
- Quels invariants ne doivent jamais être violés ?

**Contrat API :**
- Pré-conditions (ce que l'appelant doit garantir)
- Post-conditions (ce que le code garantit)
- Effets de bord
- Modes d'échec possibles

**Edge cases réels :**
- Cas limites en production
- Fichiers malformés / données corrompues
- États incohérents possibles

### 4. Type de tests à créer

**Décider où placer les tests :**

| Type | Projet | Quand l'utiliser |
|------|--------|------------------|
| **Unitaire** | `Spotlight.Core.Tests` | Logique isolée, calculs, transformations, validation |
| **Intégration** | `Spotlight.Core.IntegrationTests` | I/O fichiers, multi-composants, scénarios end-to-end |

**Indices pour choisir :**
- Besoin de vrais fichiers sur disque ? → Intégration
- Teste plusieurs classes ensemble ? → Intégration
- Logique pure sans dépendances externes ? → Unitaire
- Validation d'entrées / cas d'erreur ? → Unitaire

### 5. Catégories de tests

**Priorité haute :**
- **Comportement métier** - Ce que fait le code pour l'utilisateur
- **Contrat API** - Pré/post-conditions, validation
- **Invariants** - Règles qui ne doivent jamais être violées

**Priorité moyenne :**
- **Scénarios réels** - Cas d'utilisation complets
- **Intégration** - Interactions entre composants
- **Gestion d'erreurs** - Récupération, messages explicites

### 6. Nommage

```csharp
// Bon : Décrit un comportement
[Fact]
public void Import_WithMultipleFixtures_GroupsThemByUniverse()

// Mauvais : Décrit une mécanique
[Fact]
public void Import_CallsParser_ThenCreatesFixture()
```

Pattern : `Action_Context_ExpectedResult`

### 7. Structure

```csharp
[Fact]
public void Import_WithColorWheel_ExtractsAllSlots()
{
    // Arrange - Contexte explicite
    var path = GetTestFile("fixture-with-wheel.gdtf");

    // Act - Action unique
    var result = _importer.Import(path);

    // Assert - Vérifications significatives
    var wheel = result.Wheels.Single(w => w.Type == WheelType.Color);
    Assert.Equal(8, wheel.Slots.Count);
    Assert.All(wheel.Slots, slot => Assert.NotEmpty(slot.Name));
}
```

### 8. Tests paramétrés

```csharp
[Theory]
[InlineData("file.gdtf", "GDTF")]
[InlineData("file.mvr", "MVR")]
public void Import_DifferentFormats_SetsCorrectSource(string file, string expected)
```

### 9. Ce qu'il faut éviter

- `Assert.NotNull(result)` seul (ne teste rien de significatif)
- Tester l'implémentation interne (mocks qui vérifient les appels)
- Noms génériques (`Test1`, `Add_Works`)
- Dépendance à l'ordre des éléments

### 10. Checklist par méthode publique

| Catégorie | Vérifié ? |
|-----------|-----------|
| Comportement nominal | |
| Paramètres null/vide | |
| Valeurs invalides/extrêmes | |
| Invariants du domaine | |
| Cas limites réalistes | |
| Modes d'erreur | |

### 11. Écrire les tests

Créer les fichiers de `docs/current-task.md`. Les tests doivent **compiler mais ÉCHOUER**.

### 12. Vérifier

```bash
dotnet build  # Doit passer
dotnet test   # Doit ÉCHOUER (RED)
```

### 13. Finaliser

Mettre `current.phase` = "dev".

```
## RED: [E1] T4 - Titre

**Tests créés:** 18 tests dans 2 fichiers

**Par catégorie:**
- Comportement: 6
- Contrat API: 4
- Invariants: 3
- Edge cases: 3
- Erreurs: 2

**Coverage baseline:** 85.2% (avant nouveaux tests)
**Build:** OK
**Tests:** 0/18 passed (RED) ✓

Lancer `/tdd:flow:3-dev` pour implémenter (GREEN).
```
