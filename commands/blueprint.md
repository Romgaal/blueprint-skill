---
allowed-tools: Bash(*), Read(*), Write(*), Edit(*), Glob(*), Grep(*), Agent(*)
description: "Full blueprint workflow: plan → execute → verify → review"
---

Lancer le workflow complet de blueprint :

1. **Analyser** la demande de l'utilisateur

2. **Invoke** la skill `blueprint-plan` pour produire le CDC.
   Elle enchaine d'elle-meme :
   - Phase 0 — capture verbatim des requirements
   - Phase 0.5 — **lever les ambiguites** : questions a l'utilisateur, inconnues verifiees, decisions a trancher
   - Phase 1 — **invoke `challenge-assumptions`** pour le choix de l'approche (3 options minimum)
   - Phase 2 — le CDC (avec critere de succes global)

   > ⚠️ **Si la Phase 0.5 remonte des questions ou des decisions a trancher → S'ARRETER et attendre
   > les reponses de l'utilisateur.** Ne jamais planifier sur des suppositions : un blueprint parfait
   > sur un objectif devine est inutile.

3. **Presenter** le blueprint a l'utilisateur pour validation

4. **Si valide** → invoke `blueprint-execute` pour executer etape par etape

5. **A la fin** → invoke `verification-proof` pour prouver que tout est fait — y compris le
   **critere de succes global** du blueprint, pas seulement les verifications d'etape

6. **Optionnel** → lancer l'agent `blueprint-reviewer` pour un review complet
   (implementation vs plan **+ qualite du plan lui-meme**)

A chaque transition entre phases, informer l'utilisateur et attendre sa validation.
