# /tdd:flow:4-review

Revue critique du code.

## Instructions

### 1. Vérifier
- `docs/state.json` : `current.phase` doit être "review"
- Charger `docs/current-task.md`, `docs/dev/standards.md`

### 2. Build et tests

```bash
dotnet build && dotnet test
```

Si échec → corriger d'abord.

### 3. Revue du code

Lire **chaque fichier modifié/créé**.

#### A. Adéquation avec le besoin

- Le code fait-il exactement ce qui était demandé ?
- Y a-t-il du scope creep ou des oublis ?
- Les décisions de l'analyse sont-elles respectées ?

#### B. Design

**Questions à se poser :**
- Responsabilité : Classe/méthode fait une seule chose ?
- Couplage : Trop dépendant d'autres modules ?
- Testabilité : Facile à tester en isolation ?

**Signaux d'alerte :**
- Classe > 200 lignes, méthode > 30 lignes
- Plus de 3 niveaux d'indentation
- Plus de 4 paramètres
- Noms génériques (`data`, `manager`, `helper`)

#### C. Robustesse

- Erreurs gérées correctement ?
- Ressources libérées (IDisposable, using) ?
- Cas limites couverts ?

### 4. Revue des tests

**Couverture à vérifier :**
- Comportements métier
- Validation inputs
- Invariants
- Cas limites
- Gestion erreurs

**Tests manquants ?**
- Chaque `if` → test pour chaque branche ?
- Chaque `throw` → test qui le déclenche ?

### 5. Conformité standards

**Checklist :**
- [ ] File-scoped namespaces
- [ ] Records pour modèles
- [ ] Nullable (pas de `!`)
- [ ] CancellationToken sur async

### 6. Corrections

**Mineurs :** Corriger directement (renommages, formatage, tests simples manquants).

**Majeurs :** Présenter le problème et demander confirmation avant de corriger.

### 7. Finaliser

Garder `current.phase` = "review".

```markdown
## Review: [E1] T4 - Titre

**Build:** OK
**Tests:** 18/18 passed

**Corrections appliquées:**
- [Liste]

**Couverture tests:** OK

Lancer `/tdd:flow:5-docs` pour documenter.
```

## Questions utiles

- "Si je modifie ce code dans 6 mois, qu'est-ce qui me poserait problème ?"
- "Ce test pourrait-il passer alors que le code est cassé ?"
- "Est-ce la solution la plus simple qui fonctionne ?"
