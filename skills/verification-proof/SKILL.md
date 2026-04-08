---
name: verification-proof
description: "Use BEFORE claiming any work is complete. Prove it works with concrete evidence. The word 'should' is banned. No declaration without proof. No 'probably OK'. Run the command. Show the output. THEN say it's done."
---

# Verification Proof

## Principe

> **Aucune declaration de completion sans PREUVE de verification fraiche.**
> Le mot "devrait" est un red flag. Si tu dis "ca devrait marcher", c'est que t'as PAS verifie.

**Announce at start:** "Je verifie avec preuves avant de declarer que c'est fait."

## La Loi d'Airain

**INTERDICTIONS ABSOLUES :**
- Dire "c'est fait" sans output de commande qui le prouve
- Dire "ca devrait marcher"
- Dire "probablement OK"
- Dire "j'ai verifie" sans montrer la preuve
- Dire "100% clean" sans verification exhaustive
- Batch les completions ("tout est fait") — verifier CHAQUE element

## Verification goal-backward (3 niveaux)

Au lieu de verifier "est-ce que les taches sont faites ?", verifier "est-ce que le RESULTAT est atteint ?" :

### Niveau 1 — EXISTS
Le fichier/service/config est **physiquement present** ?
```bash
# Exemples de verification EXISTS
ls -la path/to/file.ts        # Le fichier existe ?
cat path/to/file.ts | head -5  # Il a du contenu ?
```

### Niveau 2 — SUBSTANTIVE
C'est du **vrai contenu** (pas un stub, pas un TODO, pas un placeholder) ?
```bash
# Red flags a grep
grep -rn "TODO\|FIXME\|coming soon\|placeholder\|not implemented" path/
# Si un de ces patterns est present → PAS substantive
```

### Niveau 3 — WIRED
C'est **connecte, utilise, fonctionnel** dans le systeme ?
```bash
# Exemples de verification WIRED
curl http://localhost:3000/api/endpoint   # L'API repond ?
npm test                                   # Les tests passent ?
npx prisma db push --dry-run              # Le schema DB est valide ?
```

## Protocole de verification

Pour chaque delivrable :

```
□ EXISTS — Le fichier/service est physiquement present ?
   → Preuve : [output de ls/cat]
□ SUBSTANTIVE — C'est du vrai contenu, pas un stub ?
   → Preuve : [output de grep TODO/FIXME = vide]
□ WIRED — C'est connecte et fonctionnel ?
   → Preuve : [output de test/curl/build]
□ Mot "devrait" utilise ? → Si oui, STOP, verifier
□ PROVEN — J'ai une preuve fraiche ?
   → Preuve : [commande + output exact]
```

## Detection anti-stubs

Red flags a chercher ACTIVEMENT dans le code produit :

- Commentaires `TODO`, `FIXME`, `XXX`, "a faire", "coming soon"
- Fonctions vides ou qui ne font que `console.log`
- Valeurs hardcodees la ou du dynamique est attendu
- Configs copiees-collees sans adaptation au contexte
- `any` en TypeScript la ou un type precis est attendu
- `catch (e) {}` — error swallowing
- Tests qui ne testent rien (`expect(true).toBe(true)`)

## Rationalisations interdites

| Ce que tu te dis | La realite |
|-----------------|-----------|
| "C'est trop simple pour verifier" | Le code simple casse aussi |
| "Je verifierai apres" | Non. Verifier maintenant |
| "Ca marchait la derniere fois" | L'etat a peut-etre change |
| "C'est juste un changement cosmetique" | Les changements cosmetiques cassent des builds |
| "Le test prendrait trop de temps" | Le debug prendra 10x plus |
| "C'est probablement OK" | "Probablement" = pas verifie |

## Format de rapport final

Quand tu declares un travail termine :

```
VERIFICATION COMPLETE :
- [N] fichiers crees/modifies
- [N] verifications EXISTS passees
- [N] verifications SUBSTANTIVE passees
- [N] verifications WIRED passees
- Build : [PASS/FAIL + output]
- Tests : [PASS/FAIL + output]
- Zero TODO/FIXME/placeholder detecte

PREUVES :
[Coller les outputs de commande]
```

## Regle d'or

> **Remplace chaque "devrait" par une commande qui PROUVE.**
> "Ca devrait marcher" → `npm test` → "Tests: 14 passed, 0 failed"
