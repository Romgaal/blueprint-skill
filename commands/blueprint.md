---
allowed-tools: Bash(*), Read(*), Write(*), Edit(*), Glob(*), Grep(*), Agent(*)
description: "Full blueprint workflow: plan → execute → verify → review"
---

Lancer le workflow complet de blueprint :

1. **Analyser** la demande de l'utilisateur
2. **Invoke** la skill `blueprint-plan` pour produire le CDC
3. **Presenter** le blueprint a l'utilisateur pour validation
4. **Si valide** → invoke `blueprint-execute` pour executer etape par etape
5. **A la fin** → invoke `verification-proof` pour prouver que tout est fait
6. **Optionnel** → lancer l'agent `blueprint-reviewer` pour un review complet

A chaque transition entre phases, informer l'utilisateur et attendre sa validation.
