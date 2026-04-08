---
name: challenge-assumptions
description: "You MUST use this before committing to any technical approach. Compare minimum 3 options. Kill cognitive biases. Pick the BEST solution, not the first one that comes to mind. If you skip this, you WILL waste time."
---

# Challenge Assumptions

## Principe

> **Ne jamais choisir une techno par habitude. Comparer minimum 3 options.**
> La premiere idee est rarement la meilleure. La bonne solution est celle que tu trouves APRES avoir challenge tes propres biais.

**Announce at start:** "Je challenge les approches avant de choisir."

## Quand utiliser

- Avant de choisir une stack technique
- Avant de choisir une architecture
- Avant de choisir un pattern de code
- Quand tu es "sur" de la bonne approche (surtout la)
- Quand l'utilisateur propose une approche et que tu veux valider

## Le process

### 1. Lister les options (minimum 3)

Pour chaque option, documenter :

| Critere | Option A | Option B | Option C |
|---------|----------|----------|----------|
| **Fiabilite** | ? | ? | ? |
| **Simplicite** | ? | ? | ? |
| **Maintenabilite** | ? | ? | ? |
| **Performance** | ? | ? | ? |
| **Cout (tokens/temps/argent)** | ? | ? | ? |
| **Risques** | ? | ? | ? |

### 2. Detecter les biais cognitifs

Checklist de pieges mentaux a verifier ACTIVEMENT :

| Biais | Question a se poser |
|-------|-------------------|
| **Biais de familiarite** | "Je choisis ca parce que c'est le mieux, ou parce que je connais deja ?" |
| **Biais d'ancrage** | "La premiere option que j'ai vue influence-t-elle mon jugement ?" |
| **Biais de confirmation** | "Est-ce que je cherche des raisons pour valider mon choix initial ?" |
| **Biais du cout irrecuperable** | "Est-ce que je persiste parce que j'ai deja investi du temps ?" |
| **Biais d'optimisme** | "Est-ce que je sous-estime les risques de mon option preferee ?" |
| **Effet de halo** | "Est-ce que la popularite d'un outil me fait surestimer sa qualite ?" |

### 3. Stress-test de l'option choisie

Avant de valider, poser ces questions :

- **Et si ca casse ?** — Comment on rollback ? Combien de temps ?
- **Et dans 6 mois ?** — Est-ce que ca scale ? Est-ce que c'est maintenable ?
- **Et si l'utilisateur change d'avis ?** — A quel point c'est flexible ?
- **Le fix le plus simple existe-t-il ?** — Un edit config (2 min) vs un rebuild (40 min) ?
- **Quelqu'un l'a-t-il deja fait ?** — Chercher AVANT de reinventer

### 4. Decision finale

Format de sortie :

```
DECISION : [Option choisie]
RAISON : [2-3 phrases]
REJETEES : [Options rejetees + pourquoi]
RISQUES ACCEPTES : [Ce qui pourrait quand meme mal tourner]
PLAN B : [Si ca foire, on fait quoi]
```

## Regles

- **Minimum 3 options.** "Il n'y a qu'une seule facon" est presque toujours faux.
- **Rechercher AVANT d'essayer.** 10 min de recherche economise des heures de boucles inutiles.
- **Fix minimal d'abord.** Estimer le cout AVANT d'agir.
- **Si l'utilisateur propose une approche** : la challenger avec respect. "C'est une option. J'ai aussi regarde X et Y. Voici mon analyse..." Ne pas valider aveuglement.
- **JAMAIS dire "c'est la seule solution"** sans avoir explore les alternatives.

## Anti-patterns a detecter

| Ce que tu penses | Ce que tu devrais penser |
|-----------------|------------------------|
| "C'est evident" | "Pourquoi c'est evident ? Quelles alternatives ?" |
| "Tout le monde fait ca" | "Est-ce que tout le monde a raison ?" |
| "C'est trop simple pour comparer" | "Le code simple casse aussi" |
| "On a toujours fait comme ca" | "Est-ce que le contexte a change ?" |
| "Ca devrait marcher" | "PROUVE-LE" |
