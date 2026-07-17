---
name: crash-test
description: "Use to audit an EXISTING product, UI, system or codebase: find frictions, risks and weaknesses across UX, ergonomics, design, security, performance and debt. Exhaustive brainstorm of findings, then 3 distinct solutions per finding, then a mandatory re-pass. Produces findings and options — never fixes them itself (hands off to blueprint-plan). Do NOT use to plan new work (that is blueprint-plan), nor to debug one specific failure (that is systematic-debug)."
---

# Crash-test (audit d'un existant)

## Principe fondamental

> **"On audite ce qui EST, pas ce qu'on aimerait avoir."**
> Le crash-test **constate** et **propose des options**. Il ne repare rien.
> Reparer pendant l'audit = scope creep + audit bacle. La reparation, c'est `blueprint-plan`.

**Announce at start:** "Je lance le crash-test. Je constate d'abord, je ne repare rien avant la fin."

## Quand utiliser cette skill

- Auditer un produit / une UI / un systeme / un codebase existant
- Avant une refonte : savoir ce qui cloche VRAIMENT (pas ce qu'on suppose)
- Apres une livraison : trouver ce qu'on a rate
- **PAS** pour planifier une construction neuve → `blueprint-plan`
- **PAS** pour debugger une panne precise → `systematic-debug`

## Phase 1 — Brainstorm exhaustif (constater, ne rien filtrer)

Balayer **tous** les axes. Ne pas s'arreter au premier qui parle :

| Axe | Ce qu'on traque |
|---|---|
| **Ergonomie / UX** | frictions, clics inutiles, parcours qui casse, absence de feedback, etats vides/erreur non geres |
| **Design** | hierarchie visuelle, coherence, lisibilite, accessibilite (contraste, focus, clavier) |
| **Securite** | surface d'attaque, auth/permissions, donnees exposees, injection, secrets en clair |
| **Performance** | temps de reponse, poids, requetes inutiles, N+1, cache absent ou mal pose |
| **Anti-fragilite** | que se passe-t-il si X tombe / si l'input est hostile / si la charge x10 ? |
| **Dette** | doublons, code mort, config divergente, contournements qui trainent |

**Regles de cette phase :**
1. **Un finding doit etre REPRODUCTIBLE** — donner le chemin exact pour le constater (clic, commande, ligne).
   *Une impression n'est pas un finding.* "Ca fait brouillon" → non. "3 CTA de meme poids visuel se disputent l'attention en haut de page" → oui.
2. **Chercher l'enjeu SOUS-JACENT** — le symptome n'est pas le probleme. "L'user ne trouve pas le bouton"
   est peut-etre "l'architecture de l'info est fausse". Traiter le symptome laisse le probleme.
3. **Ne rien filtrer pendant le brainstorm** — on juge en Phase 2, pas maintenant.
4. **Ne RIEN reparer.** Si une correction evidente demange : la noter, continuer.

## Phase 2 — 3 solutions par finding

Pour **chaque** finding, produire **3 solutions reellement DISTINCTES**.

> 3 variantes de la meme idee ≠ 3 solutions. Si les 3 attaquent le probleme au meme endroit,
> c'est qu'il en manque : remonter d'un cran (et si on supprimait le besoin ? et si on changeait le flux ?).

Pour chacune, dire : **le cout**, **le risque**, **ce qu'elle sacrifie**.

Criteres de tri (dans cet ordre) :
1. **Anti-fragile** — resiste a l'imprevu, ne casse pas au prochain changement
2. **Simplicite** — moins de pieces mobiles bat plus malin
3. **Zero dette** — aucune solution qui cree un contournement a payer plus tard
4. **UX** — simple et intuitif pour l'utilisateur final, pas pour le dev

Puis **recommander UNE** solution et dire **pourquoi elle bat les 2 autres**. Pas de "ca depend".

## Phase 3 — La repasse (obligatoire)

Quand tout est ecrit, **tout relire** et trancher pour chaque point :

- [ ] Ce finding **tient-il** encore ? (ou c'etait un biais / une preference personnelle ?)
- [ ] Est-il **reproductible** ? Sinon → le supprimer, ce n'est pas un finding.
- [ ] La solution recommandee est-elle **la bonne**, ou juste **la plus facile a implementer** ?
- [ ] Ai-je confondu **volume et rigueur** ? 20 findings tiedes valent moins que 5 qui font mal.
      Fusionner ce qui se recoupe, supprimer ce qui est deja couvert ailleurs.
- [ ] Y a-t-il un finding **majeur noye** au milieu de details cosmetiques ? → le remonter.

> **Regle : la sortie doit etre PRIORISEE par impact, jamais par ordre de decouverte.**

## Phase 4 — Auto-challenge (les angles morts)

Se retourner contre soi, honnetement :

- **Qu'est-ce que je n'ai PAS regarde ?** Un axe evite parce qu'il etait penible a auditer ?
- **Quel finding ai-je adouci** parce qu'il remet en cause un choix precedent (le mien, ou celui de l'utilisateur) ?
- **Ou suis-je alle vite** parce que "ca avait l'air ok" ? Ce sont les zones ou les bugs vivent.
- **Est-ce que je propose ce qui est JUSTE, ou ce qui me semble juste ?** Ce n'est pas la meme chose :
  le second est confortable, le premier se prouve.

> **La qualite prime sur la vitesse d'execution.** Un audit rapide et flatteur ne vaut rien.
> Si un finding derange, il se dit quand meme — surtout s'il derange.

## Si tu delegues (subagents)

Tu as le droit d'utiliser toutes les ressources utiles. Mais :
- **C'est TOI le boss** : tu definis exactement ce que tu attends, et tu **verifies** ce qui revient.
- **Un subagent qui confirme ton analyse sans la challenger n'a rien apporte.** Demande-lui de
  *demonter*, pas de valider.
- **Tu ne relaies jamais un resultat que tu n'as pas verifie toi-meme.** Sa conclusion est une
  hypothese jusqu'a ce que tu la prouves.

## Sortie attendue

```markdown
# Crash-test : [cible] — [date]

**Perimetre audite** : [ce qui a ete regarde] | **Non couvert** : [ce qui ne l'a pas ete, et pourquoi]

## Findings (pries par impact)

### F1 — [titre] · [CRITIQUE | IMPORTANT | MINEUR] · axe: [UX/secu/perf/...]
**Constat** : [le fait]
**Repro** : [le chemin exact pour le voir]
**Enjeu sous-jacent** : [le vrai probleme derriere le symptome]

| Solution | Cout | Risque | Sacrifie |
|---|---|---|---|
| A. [...] | | | |
| B. [...] | | | |
| C. [...] | | | |

**Recommandation** : [A/B/C] — [pourquoi elle bat les autres]
```

## Ce qui est INTERDIT

- Un finding sans **repro** → c'est une opinion, pas un finding
- 3 variantes de la meme idee presentees comme "3 solutions"
- **Reparer** pendant l'audit
- Gonfler la liste pour faire nombre (le volume n'est pas de la rigueur)
- Adoucir un constat parce qu'il remet en cause un choix precedent
- "Ca depend" en guise de recommandation

## Apres le crash-test

Presenter les findings pries a l'utilisateur, puis :
> "Crash-test termine. [N] findings, [M] critiques. Tu veux que je transforme lesquels en plan ?"

Sur validation → lancer `/blueprint` (ou la skill `blueprint-plan`) sur les solutions retenues :
il refera une passe complete — lever les ambiguites, choisir l'approche, CDC — avant `blueprint-execute`.

**Les findings recurrents** (le meme piege qui revient d'un audit a l'autre) → les ajouter dans
`references/gotchas.md`. Un piege vu deux fois est un piege de methode, pas de hasard.
