---
name: systematic-debug
description: "Use when ANYTHING fails or behaves unexpectedly. 4 phases in strict order: logs → config → source code → experimentation. NEVER skip a phase. NEVER guess. NEVER retry blindly. The last modification is ALWAYS the first suspect."
---

# Systematic Debug

## Principe

> **Debug en 4 phases. Dans l'ordre. JAMAIS sauter de phase.**
> Repeter une commande qui echoue = gaspillage pur. Comprendre POURQUOI avant de reessayer.

**Announce at start:** "Je debug systematiquement en 4 phases."

## Les 4 phases (ORDRE STRICT)

### Phase 1 — LOGS / STDERR
```
Lire les logs. Lire les erreurs. Lire les stack traces.
La reponse est presque toujours LA.
```
- Quelle est l'erreur EXACTE ?
- Quel fichier, quelle ligne ?
- Quel message d'erreur complet (pas juste le titre) ?

### Phase 2 — CONFIG / STATE
```
Verifier l'etat actuel du systeme.
Ce qui marchait hier ne marche peut-etre plus aujourd'hui.
```
- Les fichiers de config sont-ils corrects ?
- Les variables d'environnement sont-elles presentes ?
- Les dependances sont-elles installees et a la bonne version ?
- L'etat de la DB est-il coherent ?

### Phase 3 — SOURCE CODE
```
Lire le code source. Pas deviner. LIRE.
```
- Que fait REELLEMENT le code (pas ce qu'il devrait faire) ?
- Y a-t-il un bug de logique ?
- Les types sont-ils corrects ?
- Les imports sont-ils presents ?

### Phase 4 — EXPERIMENTATION CIBLEE
```
Seulement apres avoir compris le probleme.
Un test CIBLE, pas un essai a l'aveugle.
```
- Hypothese claire AVANT de tester
- Un seul changement a la fois
- Verifier le resultat
- Si ca echoue, revenir en Phase 1 avec les nouvelles infos

## Regles de fer

### Derniere modification = premier suspect
**TOUJOURS** investiguer la derniere modification en premier. Pas partir dans des explorations larges. Qu'est-ce qui a change en dernier ? C'est presque toujours la cause.

### Ne JAMAIS repeter une commande qui echoue
Si `X` echoue, analyser POURQUOI avant de relancer `X`. Relancer la meme commande = definition de la folie.

### Regle des 3 tentatives
3 echecs sur la meme approche → STOP. Changer de strategie. C'est un probleme d'architecture, pas un bug.

### Niveaux de confiance
Toute affirmation technique a un niveau :
- **HIGH** — Verifie via docs officielles ou code source
- **MEDIUM** — Croise avec plusieurs sources
- **LOW** — Source unique ou memoire

**Ne JAMAIS presenter du LOW comme certitude.** "Je n'ai pas trouve X" est une information valide.

### Rechercher AVANT d'essayer
Face a un probleme inconnu, la PREMIERE action est une recherche (docs, GitHub issues, web). JAMAIS tenter des fixes a l'aveugle. 10 min de recherche economise des heures de boucles.

## Classification des imprevus

Quand un probleme imprévu survient pendant une tache :

| Type | Action | Permission |
|------|--------|------------|
| Bug, erreur, vulnerabilite | Fix immediat + documenter | Auto |
| Critique manquant (validation, auth) | Ajout immediat + documenter | Auto |
| Bloquant (deps manquantes, config absente) | Fix immediat + documenter | Auto |
| **Architectural** (nouveau service, changement de schema) | **STOP → demander a l'utilisateur** | Manuel |

Si ca touche correctness/securite/completion → fix auto.
Si ca change l'architecture → STOP et demander.

## Anti-patterns

| Ce que tu fais | Ce que tu devrais faire |
|---------------|----------------------|
| Retenter la meme commande | Lire l'erreur d'abord |
| Chercher sur Google immediatement | Lire les logs d'abord |
| Modifier le code au hasard | Comprendre le bug d'abord |
| "Ca marchait avant" | Verifier ce qui a change |
| Ignorer un warning | Les warnings deviennent des erreurs |
