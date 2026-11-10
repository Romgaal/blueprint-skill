---
name: crash-test
description: "Audit an existing product, UI, system or codebase across UX, design, security, performance, resilience and debt. Separate proven findings, supported risks, opportunities and unknowns; compare 3 distinct solutions per retained problem, challenge the evidence and hand off to blueprint-plan. Produces an audit, never repairs. Not for planning new work or debugging one specific failure."
---

# Crash-test (audit d'un existant)

## Principe fondamental

> **"On audite ce qui EST, pas ce qu'on aimerait avoir."**
> Le crash-test **constate** et **propose des options**. Il ne repare rien.
> Reparer pendant l'audit = scope creep + audit bacle. La reparation, c'est `blueprint-plan`.

**Announce at start:** "Je lance le crash-test : preuves, options et repasse critique. Les correctifs passent ensuite par le blueprint."

## Quand utiliser cette skill

- Auditer un produit / une UI / un systeme / un codebase existant
- Avant une refonte : savoir ce qui cloche VRAIMENT (pas ce qu'on suppose)
- Apres une livraison : trouver ce qu'on a rate
- **PAS** pour planifier une construction neuve → `blueprint-plan`
- **PAS** pour debugger une panne precise → `systematic-debug`

## Phase 0 — Cadrer et borner l'audit

Avant les essais, fixer d'apres le contexte :
- **Cible et version** : sources, environnement et date ; verifier ce qui est effectivement servi avant d'y transferer une preuve locale ou ancienne.
- **Usages critiques** : utilisateurs/roles, parcours, etats et formats pertinents ; resultat attendu et critere observable de reussite.
- **Couverture** : ce qui sera examine, ce qui ne le sera pas et pourquoi. Pour la securite, identifier les donnees a proteger, points d'entree et frontieres de confiance.

Demander seulement les inconnues qui changent le perimetre ou l'autorisation ; poursuivre les controles independants. Une demande d'audit n'autorise pas des paiements, messages externes, changements de production ou installations.

Les essais susceptibles de detruire, saturer ou modifier des donnees se font sur une copie isolee, avec un cas temoin, des limites de duree/volume et une condition d'arret. Distinguer un arret impose par le banc d'un echec du produit. Si l'isolement ou une autorisation manque, conserver ce point comme non teste et preciser le controle restant.

## Phase 1 — Explorer les six axes et qualifier les preuves

Balayer **tous** les axes. Ne pas s'arreter au premier qui parle :

| Axe | Ce qu'on traque |
|---|---|
| **Ergonomie / UX** | frictions, clics inutiles, parcours qui casse, absence de feedback, etats vides/erreur non geres |
| **Design** | hierarchie visuelle, coherence, lisibilite, accessibilite (contraste, focus, clavier) |
| **Securite** | surface d'attaque, auth/permissions, donnees exposees, injection, secrets, dependances affectees et chemins d'attaque accessibles |
| **Performance** | temps de reponse, poids, requetes inutiles, N+1, cache absent ou mal pose |
| **Resistance aux incidents** | panne, delai, reprise, doublon, annulation, concurrence, donnee invalide ou charge accrue ; essais bornes et isoles |
| **Dette** | doublons, code mort, config divergente, contournements qui trainent |

**Regles de cette phase :**
1. **Un fait doit etre REPRODUCTIBLE** — donner les preconditions, le geste ou la commande, le resultat attendu et le resultat observe. Joindre une preuve datee (sortie, capture, mesure ou source precise) et sa cible/version. Une impression esthetique seule ne prouve pas une friction.
2. **Chercher l'enjeu SOUS-JACENT** — le symptome n'est pas le probleme. "L'user ne trouve pas le bouton"
   est peut-etre "l'architecture de l'info est fausse". Traiter le symptome laisse le probleme.
3. **Ne rien filtrer pendant le brainstorm** — on juge en Phase 2, pas maintenant.
4. **Ne RIEN reparer.** Si une correction evidente demange : la noter, continuer.

Classer les observations sans les confondre :

| Statut | Ce qu'il faut fournir |
|---|---|
| **Fait reproduit** | Observation et preuve verifiable ; portee exacte du resultat |
| **Risque etaye** | Indice concret, conditions necessaires, partie non demontree et prochain controle pour confirmer ou ecarter le risque |
| **Opportunite** | Besoin utilisateur, benefice attendu et experience permettant de tester l'idee ; ce n'est pas un defaut etabli |
| **Inconnue** | Information manquante, decision qu'elle empeche et moyen de la lever |

Pour chaque probleme, indiquer **HIGH / MEDIUM / LOW** et justifier la confiance : preuve directe concordante, preuve partielle ou indirecte, indice faible. La confiance et la gravite sont deux dimensions distinctes. Une hypothese sans indice ne devient pas un finding pour remplir le rapport.

En securite, rapprocher l'avis actuel et sa source de la version installee, de sa configuration et du chemin accessible. Un score de scanner ou une dependance presente n'est pas une exploitation demontree ; les alertes propagees dans le graphe ne sont pas autant d'attaques distinctes. Un risque serieux non testable sans danger reste visible comme risque.

Garder une courte liste d'opportunites lorsque l'examen en revele : changer le flux, supprimer un besoin, simplifier le produit. Preciser comment les eprouver, sans inventer leur efficacite ni imposer une refonte hors mandat.

## Phase 2 — 3 solutions par finding

Pour **chaque probleme retenu** (fait ou risque etaye), produire **3 solutions reellement DISTINCTES**. Pour un risque non confirme, rendre le choix conditionnel au controle qui leve l'inconnue : ne pas recommander une correction comme si l'incident etait prouve.

> 3 variantes de la meme idee ≠ 3 solutions. Si les 3 attaquent le probleme au meme endroit,
> c'est qu'il en manque : remonter d'un cran (et si on supprimait le besoin ? et si on changeait le flux ?).

Pour chacune, dire : **le cout**, **le risque**, **ce qu'elle sacrifie**.

Criteres de tri (dans cet ordre) :
1. **Resistance aux incidents** — modes de panne, reprise et retour arriere explicites, sans promettre l'absence de toute panne
2. **Simplicite** — moins de pieces mobiles bat plus malin
3. **Maintenance** — eviter les contournements durables, expliciter le cout d'entretien et les compromis ; toute complexite ajoutee doit repondre a un besoin demontre
4. **UX** — simple et intuitif pour l'utilisateur final, pas pour le dev

Puis **recommander UNE** solution et dire **pourquoi elle bat les 2 autres**. Pas de "ca depend".

Donner pour ce choix un **critere de reussite observable**, le **controle de non-regression** pertinent, les **inconnues bloquantes** et le **responsable** de la suite (role si la personne est inconnue). Ce sont les entrees du blueprint, pas encore son implementation.

## Phase 3 — La repasse (obligatoire)

Quand tout est ecrit, **tout relire** et trancher pour chaque point :

- [ ] Ce finding **tient-il** encore ? (ou c'etait un biais / une preference personnelle ?)
- [ ] La preuve correspond-elle a la cible et aux conditions annoncees ? Sinon → reclasser en risque etaye/inconnue, ou retirer l'affirmation sans fondement ; ne pas effacer un risque serieux faute de test destructif.
- [ ] Pour chaque point majeur, **quel resultat infirmerait ma conclusion ?** Chercher ce contre-exemple et le tester quand c'est possible ; noter le resultat ou la limite, puis corriger la conclusion si necessaire.
- [ ] Le cas temoin passe-t-il ? L'echec vient-il du produit, d'un prealable omis, d'un mock ou du banc de test ?
- [ ] La solution recommandee est-elle **la bonne**, ou juste **la plus facile a implementer** ?
- [ ] Ai-je confondu **volume et rigueur** ? 20 findings tiedes valent moins que 5 qui font mal.
      Fusionner ce qui se recoupe, supprimer ce qui est deja couvert ailleurs.
- [ ] Y a-t-il un finding **majeur noye** au milieu de details cosmetiques ? → le remonter.

> **Prioriser** par impact utilisateur/metier, exposition ou frequence, et difficulte de recuperation ; afficher la confiance a cote. Justifier les inconnues sans fabriquer un score precis. Un risque grave mais incertain peut etre prioritaire a investiguer. L'ordre de decouverte ne determine pas l'ordre du rapport.

## Phase 4 — Auto-challenge (les angles morts)

Se retourner contre soi, honnetement :

- **Qu'est-ce que je n'ai PAS regarde ?** Un axe evite parce qu'il etait penible a auditer ?
- **Quel finding ai-je adouci** parce qu'il remet en cause un choix precedent (le mien, ou celui de l'utilisateur) ?
- **Ou suis-je alle vite** parce que "ca avait l'air ok" ? Ce sont les zones ou les bugs vivent.
- **Est-ce que je propose ce qui est JUSTE, ou ce qui me semble juste ?** Ce n'est pas la meme chose :
  le second est confortable, le premier se prouve.

> **La qualite prime sur la vitesse d'execution.** Un audit rapide et flatteur ne vaut rien.
> Si un finding derange, il se dit quand meme — surtout s'il derange.

Clore par les angles morts, les points reclasses/ecartes et les preuves manquantes. Ne pas promettre un audit exhaustif, zero biais ou une securite absolue. Ne pas prolonger les essais sans question precise restante ; une verification supplementaire doit pouvoir changer une conclusion ou une decision.

## Si tu delegues (subagents)

Si la delegation est disponible et autorisee :
- **C'est TOI le boss** : tu definis exactement ce que tu attends, et tu **verifies** ce qui revient.
- **Un subagent qui confirme ton analyse sans la challenger n'a rien apporte.** Demande-lui de
  *demonter*, pas de valider.
- **Tu ne relaies jamais un resultat que tu n'as pas verifie toi-meme.** Sa conclusion est une
  hypothese jusqu'a ce que tu la prouves.

## Sortie attendue

```markdown
# Crash-test : [cible] — [date]

**Cible/version/environnement** : [references et date]
**Usages et criteres de reussite** : [parcours/roles/etats attendus]
**Perimetre audite** : [ce qui a ete regarde] | **Non couvert** : [ce qui ne l'a pas ete, et pourquoi]

## Problemes retenus (tries par impact)

### F1 — [titre] · [CRITIQUE | IMPORTANT | MINEUR] · axe: [UX/secu/perf/...]
**Statut / confiance** : [fait reproduit ou risque etaye] · [HIGH/MEDIUM/LOW + raison]
**Constat et impact** : [fait ou risque, utilisateurs touches, exposition/frequence, recuperabilite]
**Preuve / repro** : [source et artefact dates, preconditions, geste, attendu, observe ; limite locale/publique/simulee]
**Enjeu sous-jacent** : [le vrai probleme derriere le symptome]
**Refutation** : [contre-exemple cherche, resultat ou controle restant]

| Solution | Cout | Risque | Sacrifie |
|---|---|---|---|
| A. [...] | | | |
| B. [...] | | | |
| C. [...] | | | |

**Recommandation** : [A/B/C] — [pourquoi elle bat les autres]
**Pour le blueprint** : [critere observable, non-regression, inconnues, responsable]

## Opportunites et inconnues
[Si pertinentes : benefice attendu et experience de validation / information manquante et moyen de la lever]

## Repasse et limites
[Points reclasses/ecartes et pourquoi ; angles morts et preuves manquantes]
```

## Ce qui est INTERDIT

- Un fait sans preuve, un risque presente comme incident certain, une opportunite presentee comme resultat acquis
- 3 variantes de la meme idee presentees comme "3 solutions"
- **Reparer** pendant l'audit
- Gonfler la liste pour faire nombre (le volume n'est pas de la rigueur)
- Adoucir un constat parce qu'il remet en cause un choix precedent
- "Ca depend" en guise de recommandation

## Apres le crash-test

Presenter les problemes tries, en distinguant les faits des risques etayes, puis proposer les priorites a transformer en plan. Ne pas compter les inconnues ou les opportunites comme des incidents prouves.

Sur validation des solutions, ou si cette suite est deja explicitement autorisee, lancer `/blueprint` (ou la skill `blueprint-plan`) :
il refera une passe complete — lever les ambiguites, choisir l'approche, CDC — avant `blueprint-execute`.

**Les findings recurrents** (le meme piege qui revient d'un audit a l'autre) → les consigner dans le suivi du projet ; proposer leur integration aux [gotchas du plugin](../../references/gotchas.md) si generalisables. Modifier le plugin seulement dans le perimetre autorise. Un piege vu deux fois est un piege de methode, pas de hasard.
