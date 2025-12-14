# /tdd:flow:4-review

Revue critique du code.

## Instructions

### 1. Vérifier

Lire docs/state.json : current.phase doit être "review". Charger docs/current-task.md et docs/dev/standards.md.

### 2. Build et tests

Exécuter: dotnet build && dotnet test

Si échec, corriger d'abord.

### 3. Vérifier le coverage

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

**Seuils minimaux :**
| Métrique | Seuil | Action si échec |
|----------|-------|-----------------|
| Line coverage (Spotlight.Core) | ≥ 80% | Ajouter tests manquants |
| Branch coverage | ≥ 60% | Tester les branches non couvertes |
| Nouveau code | 100% | Chaque ligne ajoutée doit être testée |

**Vérifier :**
- Le coverage n'a pas régressé vs baseline (noté à l'étape 2-test)
- Les nouvelles classes ont une couverture ≥ 90%
- Pas de classe à 0% (sauf DTOs simples)

### 4. Revue du code

Lire chaque fichier modifié/créé.

A. Adéquation avec le besoin:
- Le code fait-il exactement ce qui était demandé?
- Y a-t-il du scope creep ou des oublis?
- Les décisions de l'analyse sont-elles respectées?

B. Design - Questions à se poser:
- Responsabilité: Classe/méthode fait une seule chose?
- Couplage: Trop dépendant d'autres modules?
- Testabilité: Facile à tester en isolation?

C. Design - Signaux d'alerte:
- Classe > 200 lignes, méthode > 30 lignes
- Plus de 3 niveaux d'indentation
- Plus de 4 paramètres
- Noms génériques (data, manager, helper)

D. Robustesse:
- Erreurs gérées correctement?
- Ressources libérées (IDisposable, using)?
- Cas limites couverts?

### 5. Revue des tests

**Couverture à vérifier:** comportements métier, validation inputs, invariants, cas limites, gestion erreurs.

**Tests manquants?** Chaque if doit avoir un test pour chaque branche. Chaque throw doit avoir un test qui le déclenche.

**Types de tests appropriés?**
| Question | Si oui → |
|----------|----------|
| Y a-t-il des I/O fichiers non testés en intégration ? | Ajouter test dans `IntegrationTests` |
| Les interactions multi-composants sont-elles couvertes ? | Ajouter test d'intégration |
| La logique pure est-elle testée unitairement ? | Garder dans `Core.Tests` |

### 6. Conformité standards

Checklist: file-scoped namespaces, records pour modèles, nullable (pas de !), CancellationToken sur async.

### 7. Corrections

Mineurs: Corriger directement (renommages, formatage, tests simples manquants).
Majeurs: Présenter le problème et demander confirmation avant de corriger.

### 8. Finaliser

Garder current.phase = "review". Afficher un résumé:

```
## REVIEW: [E1] T4 - Titre

**Build:** OK
**Tests:** 18/18 passed ✓

**Coverage:**
- Line: 85.2% (baseline: 84.0%) ✓
- Branch: 70.4% ✓
- Nouvelles classes: 100% ✓

**Corrections:** 2 mineures appliquées

Lancer `/tdd:flow:5-docs` pour documenter.
```

## Questions utiles

- Si je modifie ce code dans 6 mois, qu'est-ce qui me poserait problème?
- Ce test pourrait-il passer alors que le code est cassé?
- Est-ce la solution la plus simple qui fonctionne?
