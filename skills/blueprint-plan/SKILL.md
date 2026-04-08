---
name: blueprint-plan
description: "You MUST use this before any non-trivial implementation. Produces an ultra-detailed blueprint (CDC) where every step is copy-pasteable, every file is named, every line is specified. Zero ambiguity. Zero 'adapt as needed'. The engineer executing this should be able to work mechanically."
---

# Blueprint Plan (CDC)

## Principe fondamental

> **"Le cerveau planifie, les mains executent."**
> Tu es le cerveau. Tu produis un blueprint si precis qu'un developpeur junior pourrait l'executer sans poser une seule question.

**Announce at start:** "Je produis le blueprint avant de toucher au code."

## Quand utiliser cette skill

- Avant TOUTE feature non-triviale (plus de 3 fichiers ou plus de 30 min de travail)
- Avant une refactorisation
- Avant une migration
- Avant un changement d'architecture
- **JAMAIS commencer a coder sans blueprint sur une tache complexe**

## Le process

### Phase 1 — Analyse (OBLIGATOIRE)

Avant d'ecrire le moindre plan :

1. **Comprendre le POURQUOI** — Quel probleme on resout ? Pour qui ?
2. **Explorer l'existant** — Lire les fichiers concernes, comprendre les patterns en place
3. **Lister TOUTES les approches possibles** (minimum 3)
4. **Comparer** chaque approche sur : fiabilite, simplicite, maintenabilite, performance, cout en tokens
5. **Choisir la meilleure** — pas la premiere, LA MEILLEURE
6. **Justifier le choix** en 2-3 phrases

> **REGLE DE FER : Si Romain doit te suggerer la bonne solution, c'est que t'as merde.**
> C'est TON job de trouver la meilleure approche en amont.

### Phase 2 — Blueprint (le CDC)

Produire un document avec cette structure EXACTE :

```markdown
# [Nom de la feature] — Blueprint

**Objectif** : [1 phrase]
**Approche choisie** : [nom] — [pourquoi]
**Approches rejetees** : [liste avec raison du rejet]
**Fichiers impactes** : [liste complete]
**Estimation** : [nombre d'etapes]

---

## Etape 1 : [Titre]

**Fichier** : `chemin/exact/du/fichier.ts`
**Action** : Creer | Modifier | Supprimer

```typescript
// Code COMPLET a ecrire ou modifier
// Pas de "..." ou "reste du code"
// Pas de "adaptez selon votre contexte"
// Le code exact, copier-collable
```

**Verification** : `commande exacte pour verifier que ca marche`
**Resultat attendu** : `output exact attendu`

---

## Etape 2 : [Titre]
...
```

### Regles du blueprint

1. **Chaque etape = UNE action** (2-5 min max)
2. **Code complet** — pas de placeholder, pas de TODO, pas de "..."
3. **Chemins absolus** — `src/app/api/route.ts`, jamais "le fichier de l'API"
4. **Verification a chaque etape** — commande + output attendu
5. **Ordre d'execution strict** — les dependances sont resolues dans l'ordre
6. **Zero ambiguite** — un developpeur qui ne connait PAS le projet doit pouvoir executer
7. **Commits atomiques** — indiquer quand committer et avec quel message

### Ce qui est INTERDIT dans un blueprint

- "Adaptez selon votre contexte"
- "Similaire a ce qu'on a fait pour X"
- "Ajoutez les imports necessaires"
- "Completez avec la logique metier"
- "..." ou tout placeholder
- Tout ce qui demande au lecteur de DEVINER

### Phase 3 — Review du blueprint

Avant de passer a l'execution :

1. Relire le blueprint du debut a la fin
2. Pour chaque etape : est-ce que le code est COMPLET et COPIER-COLLABLE ?
3. Les verifications couvrent-elles le RESULTAT (pas juste "le fichier existe") ?
4. L'ordre est-il correct ? Pas de dependance circulaire ?
5. Manque-t-il des etapes ? (migrations DB, env vars, config, tests)

## Sauvegarde

Sauvegarder le blueprint dans : `docs/blueprints/YYYY-MM-DD-[feature-name].md`
Si le dossier n'existe pas, le creer.

## Apres le blueprint

Dire a l'utilisateur :
> "Blueprint pret. [N] etapes. Je peux executer maintenant ou tu veux relire d'abord ?"

Si l'utilisateur valide → utiliser la skill `blueprint-execute` pour executer.
