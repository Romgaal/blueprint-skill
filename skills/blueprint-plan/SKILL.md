---
name: blueprint-plan
description: "Produces the full spec (CDC) before writing code, for any task spanning several files or over ~30 min: feature, refactor, migration, architecture change. First resolves ambiguities (asks the user what is missing, verifies unknowns), delegates the approach choice to challenge-assumptions, then specifies every step as copy-pasteable code with its verification. Use before blueprint-execute. Do NOT use for a trivial single-file fix."
---

# Blueprint Plan (CDC)

## Principe fondamental

> **"Le cerveau planifie, les mains executent."**
> Tu es le cerveau. Tu produis un blueprint si precis qu'un developpeur junior pourrait l'executer sans poser une seule question.

**Toute l'intelligence est dans le plan. L'execution est mecanique.**
Si un junior echoue a executer ton plan, c'est que le plan etait mauvais — pas que le junior est nul. Le plan doit etre assez detaille pour qu'un junior l'execute sans reflechir et sans prendre une seule decision.

**Announce at start:** "Je produis le blueprint avant de toucher au code."

## Quand utiliser cette skill

- Avant TOUTE feature non-triviale (plus de 3 fichiers ou plus de 30 min de travail)
- Avant une refactorisation
- Avant une migration
- Avant un changement d'architecture
- **JAMAIS commencer a coder sans blueprint sur une tache complexe**

## Le process

### Phase 0 — Capture des requirements (CRITIQUE)

**Ecrire les MOTS EXACTS de l'utilisateur.** Pas un resume. Pas une reformulation. Ses mots, verbatim.

Pourquoi : la compaction de conversation DETRUIT tout ce qui n'est pas ecrit dans un fichier. Si tu resumes en 3 lignes une description detaillee, les details sont PERDUS a jamais. C'est arrive. Ca ne doit plus arriver.

Creer ou mettre a jour : `docs/requirements/[feature-name].md` avec la description complete de l'utilisateur.

### Phase 0.5 — Lever les ambiguites (AVANT de planifier)

> **Un blueprint parfait bati sur un brief incomplet = "garbage in, precise garbage out".**
> La precision du plan ne rattrape JAMAIS un objectif mal compris. C'est le seul defaut qu'aucune
> etape suivante ne peut corriger.

Phase 0 capture ce que l'utilisateur a **dit**. Cette phase va chercher ce qu'il **n'a pas dit**.
**Inverser la dynamique** : ce n'est pas a lui de deviner ce dont tu as besoin, c'est a toi d'aller le chercher.

**Volet 1 — Ce qui manque cote utilisateur.** Passer le brief au crible :

| Dimension | Ce qu'on cherche | Si absent → demander |
|---|---|---|
| **Objectif** | Le RESULTAT vise, pas la description de la tache | "Qu'est-ce qui doit etre VRAI quand c'est fini ?" |
| **Contexte** | Ce que je dois savoir pour ne pas deviner | "Y a-t-il un existant / une contrainte que j'ignore ?" |
| **Contraintes** | Perimetre, format, non-negociables, interdits | "Qu'est-ce que je ne dois SURTOUT pas casser ni faire ?" |
| **Mode** | D'un bloc, ou par paliers valides ? | "Je fais tout et je montre a la fin, ou etape par etape ?" |

**Volet 2 — Ce que MOI je ne sais pas.** Lister les inconnues techniques qui changent le plan :
- "L'API X supporte-t-elle Y ?", "cette colonne existe-t-elle deja ?" → **verifier AVANT** de planifier autour
- Une hypothese non verifiee = un plan bati sur du sable. La lever coute 2 min, la subir coute la session.

**Sortie obligatoire de cette phase :**
1. **Questions posees** a l'utilisateur — et on **attend ses reponses**. On ne devine pas, on ne "part sur une hypothese raisonnable".
2. **Inconnues levees** — chacune par une commande qui PROUVE, pas par une supposition.
3. **Decisions a trancher** — la liste des choix qui engagent l'utilisateur (perimetre, arbitrage cout/qualite, irreversibilite), presentee **AVANT** de planifier, pas apres coup.

> **REGLE : si tu te surprends a ecrire "je suppose que...", "probablement", ou "on partira du principe que..." dans le blueprint → cette phase n'a pas ete faite. Retourne la faire.**

Si le brief est complet et sans inconnue : le dire en une ligne ("brief complet, aucune ambiguite") et enchainer. Cette phase ne doit pas devenir de la ceremonie.

### Phase 1 — Choisir l'approche

**Invoke la skill `challenge-assumptions`.** C'est exactement son job : minimum 3 options, comparaison,
biais tues, meilleur choix justifie. **Ne PAS refaire son travail ici.**

Avant de l'invoquer : **explorer l'existant** (lire les fichiers concernes, comprendre les patterns en
place) — sinon les options comparees seront hors-sol.

Ce qu'on garde en sortie, pour le blueprint :
- **Approche choisie** + justification en 2-3 phrases
- **Approches rejetees** + raison du rejet

> **REGLE DE FER : Si l'utilisateur doit te suggerer la bonne solution, c'est que t'as merde.**
> C'est TON job de trouver la meilleure approche en amont.

### Phase 2 — Blueprint (le CDC)

Produire un document avec cette structure EXACTE :

```markdown
# [Nom de la feature] — Blueprint

**Objectif (resultat vise)** : [ce qui doit etre VRAI quand c'est fini — pas la tache a faire]
**Critere de succes global** : [LE test end-to-end qui prouve que l'objectif est atteint]
**Approche choisie** : [nom] — [pourquoi]
**Approches rejetees** : [liste avec raison du rejet]
**Decisions validees par l'utilisateur** : [liste] | ou "brief complet, aucune ambiguite"
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

> **Objectif vs critere de succes** : l'objectif dit *ce qu'on vise*, le critere de succes dit
> *comment on PROUVE qu'on y est*. Les verifications d'etape prouvent que chaque brique est posee ;
> le critere global prouve que le mur tient. Les deux sont obligatoires.

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
4. Le **critere de succes global** est-il prouvable par une commande ?
5. L'ordre est-il correct ? Pas de dependance circulaire ?
6. Manque-t-il des etapes ? (migrations DB, env vars, config, tests)
7. Reste-t-il un "je suppose" quelque part ? → retour Phase 0.5

## Pieges connus

Avant de produire le blueprint, lire **`references/gotchas.md`** : les erreurs de planification
deja commises sur de vrais projets. Elles se repetent. C'est le contenu a plus fort signal de
cette skill — l'enrichir apres chaque plan rate (piege + date + ce qu'il aurait fallu faire).

## Sauvegarde

Sauvegarder le blueprint dans : `docs/blueprints/YYYY-MM-DD-[feature-name].md`
Si le dossier n'existe pas, le creer.

## Apres le blueprint

Dire a l'utilisateur :
> "Blueprint pret. [N] etapes. Critere de succes : [X]. Je peux executer maintenant ou tu veux relire d'abord ?"

Si l'utilisateur valide → utiliser la skill `blueprint-execute` pour executer.
