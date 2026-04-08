---
name: blueprint-execute
description: "Execute a blueprint mechanically, step by step. No improvisation. No deviation. Follow the plan exactly. Verify at each step. Report when complete."
---

# Blueprint Execute

## Principe

> **Tu es les mains. Le cerveau a deja travaille.**
> Execute le blueprint mecaniquement. Pas d'improvisation. Pas de "j'ai une meilleure idee en cours de route".

**Announce at start:** "J'execute le blueprint etape par etape."

## Le process

### Etape 0 — Charger le blueprint

1. Lire le fichier blueprint (`docs/blueprints/*.md`)
2. Compter le nombre d'etapes
3. Creer un TodoWrite avec toutes les etapes
4. **Si quelque chose n'est pas clair** → STOP. Demander AVANT d'executer.

### Pour chaque etape

1. **Marquer in_progress** dans le TodoWrite
2. **Lire l'etape** completement avant d'agir
3. **Executer** exactement ce qui est ecrit — meme code, meme fichier, meme commande
4. **Verifier** avec la commande de verification du blueprint
5. **Comparer** le resultat obtenu avec le resultat attendu
6. **Si OK** → marquer completed, passer a la suivante
7. **Si KO** → STOP IMMEDIAT. Ne pas improviser un fix.

### Quand STOP

**Arret immediat si :**
- La verification echoue
- Un fichier mentionne dans le blueprint n'existe pas
- Une dependance manque
- Le code du blueprint a une erreur de syntaxe
- Le resultat ne correspond pas a l'attendu

**Action en cas de stop :**
1. Reporter l'erreur exacte (output de commande)
2. Identifier l'etape qui echoue
3. Dire : "Etape [N] echouee. [erreur]. Le blueprint doit etre corrige avant de continuer."
4. NE PAS essayer de fixer. C'est le job du cerveau (blueprint-plan), pas des mains.

### Regle des 3 tentatives

Si 3 tentatives echouent sur la meme etape :
- Ce n'est PAS un bug, c'est un probleme d'ARCHITECTURE
- STOP total
- Reporter a l'utilisateur avec les 3 erreurs
- Suggerer de revenir a `blueprint-plan` pour repenser l'approche

### Commits

Committer quand le blueprint l'indique. Message exact du blueprint. Si le blueprint ne specifie pas de commit, committer a chaque groupe logique d'etapes.

### Completion

Quand toutes les etapes sont terminees :

1. Lancer TOUTES les verifications une derniere fois (regression check)
2. Reporter :
   - Nombre d'etapes executees
   - Nombre de verifications passees
   - Tout ecart par rapport au blueprint (meme mineur)
3. Proposer un code review via l'agent `blueprint-reviewer`

## Ce qui est INTERDIT pendant l'execution

- Modifier le blueprint
- Ajouter du code qui n'est pas dans le blueprint
- Sauter une etape de verification
- Dire "ca devrait marcher" sans preuve
- Improviser un fix quand une etape echoue
- Refactoriser "en passant"
- Ajouter une feature "bonus"
