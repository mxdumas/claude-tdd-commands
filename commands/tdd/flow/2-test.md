# Commande: /test

Écrit les tests définis dans le plan (phase RED du TDD).

## Instructions

1. **Charger l'état** : Lis `docs/state.json`

2. **Vérifier la phase** :
   - Si `current.phase` != "test" → afficher erreur
   - Si `null` → suggérer `/analyze`
   - Si autre → suggérer la bonne commande

3. **Charger le plan** : `docs/current-task.md`

4. **Créer les fichiers de tests** :
   - Créer les fichiers listés dans "Tests à créer"
   - Utiliser le code de la section "Tests (RED)"
   - Les tests doivent compiler mais ÉCHOUER (pas d'implémentation)

5. **Assurer la couverture complète** :

   ## CHECKLIST OBLIGATOIRE

   Pour **CHAQUE méthode publique**, cocher tous les cas applicables :

   ### ✅ Happy Path
   - [ ] Cas nominal avec entrées valides
   - [ ] Chaque paramètre optionnel testé (valeur par défaut + valeur explicite)
   - [ ] Retour attendu vérifié (type, valeur, propriétés)

   ### ✅ Null / Empty
   - [ ] Chaque paramètre `string` testé avec `null` → `ArgumentNullException`
   - [ ] Chaque paramètre `string` testé avec `""` → `ArgumentException` ou comportement défini
   - [ ] Chaque paramètre objet testé avec `null` → `ArgumentNullException`
   - [ ] Collections vides testées → comportement défini

   ### ✅ Valeurs invalides / extrêmes
   - [ ] IDs/clés inexistants → `null`, exception, ou no-op documenté
   - [ ] Valeurs hors limites (négatifs, > max, overflow)
   - [ ] Caractères spéciaux dans les strings (espaces, unicode, chemins invalides)
   - [ ] Fichiers inexistants → `FileNotFoundException`

   ### ✅ État inattendu
   - [ ] Ressource modifiée entre deux appels
   - [ ] Ressource supprimée entre deux appels
   - [ ] Appels concurrents (si applicable)

   ### ✅ Exceptions explicites
   - [ ] Chaque `throw` dans le code a un test correspondant
   - [ ] Type d'exception vérifié avec `Assert.Throws<T>()`
   - [ ] Message d'exception vérifié si pertinent (`Assert.Contains()`)
   - [ ] Paramètres qui évitent l'exception (ex: `overwrite=true`)

   ### Exemple complet
   ```csharp
   // === Happy path ===
   [Fact] public void Add_ValidItem_AddsToCollection() { }
   [Fact] public void Add_WithOptionalParam_UsesProvidedValue() { }

   // === Null / Empty ===
   [Fact] public void Add_NullName_ThrowsArgumentNullException() { }
   [Fact] public void Add_EmptyName_ThrowsArgumentException() { }
   [Fact] public void Add_NullCollection_ThrowsArgumentNullException() { }

   // === Valeurs invalides ===
   [Fact] public void Get_NonExistentKey_ReturnsNull() { }
   [Fact] public void Add_NegativeQuantity_ThrowsArgumentOutOfRangeException() { }
   [Fact] public void Import_InvalidPath_ThrowsFileNotFoundException() { }

   // === État inattendu ===
   [Fact] public void Update_ItemDeletedConcurrently_ThrowsInvalidOperationException() { }

   // === Exceptions explicites ===
   [Fact] public void Add_DuplicateKey_ThrowsInvalidOperationException() { }
   [Fact] public void Add_DuplicateKey_WithOverwrite_Succeeds() { }
   ```

6. **Vérifier la compilation** :
   ```bash
   dotnet build
   ```
   - Si erreur de compilation → corriger les tests

7. **Exécuter les tests** :
   ```bash
   dotnet test
   ```
   - Les tests doivent ÉCHOUER (RED)
   - C'est normal et attendu à cette étape

8. **Mettre à jour state.json** : `current.phase` = "dev"

9. **Afficher le résumé** :
   ```
   ## RED: [E0] T3 - Titre

   **Tests créés:** 12 tests dans 2 fichiers
   - Happy path: 5
   - Edge cases: 4
   - Exceptions: 3

   **Build:** OK
   **Tests:** 0/12 passed (RED) ✓

   Les tests échouent comme attendu.

   Lancer `/dev` pour implémenter (GREEN).
   ```

## Flow TDD
```
/analyze → /test (RED) → /dev (GREEN) → /review → /docs → /done
```

## Notes
- Ne pas implémenter le code métier à cette étape
- Les tests doivent être complets et vérifier TOUS les comportements
- Chaque branche du code (if/else, try/catch) doit avoir un test
- Si un test passe déjà, c'est suspect → vérifier qu'il teste vraiment quelque chose
- Nommer les tests selon le pattern: `{Method}_{Scenario}_{ExpectedResult}`
