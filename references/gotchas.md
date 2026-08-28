# Gotchas — les pieges de planification deja payes

> Charge a la demande par `blueprint-plan`. Ce ne sont pas des principes theoriques :
> chaque entree est une erreur reellement commise, datee, avec ce qu'il aurait fallu faire.
> **Apres chaque plan rate : ajouter le piege ici.** C'est le contenu a plus fort signal du skill.

---

## 1. Resumer le brief au lieu de le capturer verbatim
**Vecu** : une description detaillee de l'utilisateur resumee en 3 lignes. La compaction de conversation
est passee. Les details ont ete perdus **definitivement** — impossible de les retrouver.
**Lecon** : les mots exacts, dans un fichier, AVANT tout. Un resume est une perte de donnees irreversible.
→ Phase 0.

## 2. Supposer un mecanisme au lieu de le verifier (2026-07-17)
**Vecu** : diagnostic "les skills modifies ne tournent pas" — base sur la lecture d'un chemin d'install,
sans jamais verifier ce que le systeme charge REELLEMENT. Conclusion **inversee** : ils tournaient.
L'action corrective proposee etait donc fausse.
**Lecon** : un chemin lu ≠ un comportement observe. Toute affirmation sur "comment ca marche" se prouve
par une observation, sinon c'est une hypothese deguisee en fait. → Phase 0.5, volet 2.

## 3. "Je suppose que / probablement / ca devrait"
**Vecu** : recurrent. Chaque occurrence de ces mots dans un plan a fini en surprise a l'execution.
**Lecon** : ces mots sont le symptome d'une inconnue non levee. Remplacer par une commande qui PROUVE,
ou par une question a l'utilisateur. Jamais par une supposition confortable.

## 4. Sur-fragmenter les recommandations (2026-07-17)
**Vecu** : 5 ameliorations proposees la ou 3 suffisaient — deux se recoupaient, une etait deja couverte
ailleurs. Le volume donnait une impression de rigueur ; en realite ca diluait le signal.
**Lecon** : plus d'items ≠ meilleure analyse. Avant de livrer une liste : fusionner ce qui se recoupe,
supprimer ce qui existe deja. Un skill (comme un plan) qui grossit perd en force.

## 5. Foncer sur la premiere approche qui marche
**Vecu** : la premiere idee implementee, puis decouverte d'une approche 3x plus simple — apres coup.
**Lecon** : 3 options minimum, comparees, AVANT de figer quoi que ce soit. C'est le job de
`challenge-assumptions` — l'invoquer, pas le contourner "parce que c'est evident".

## 6. Scope creep : le code "bonus"
**Vecu** : un fichier non prevu modifie "tant qu'a faire", un refactor glisse dans une feature.
Resultat : diff illisible, bug introduit hors perimetre, review impossible.
**Lecon** : le blueprint liste les fichiers impactes. **Tout fichier hors liste = deviation a signaler**,
meme un commentaire. La transparence prime sur l'initiative.

## 7. Escalader avant d'avoir essaye le fix minimal
**Vecu** : rebuild d'image / restart de daemon pour un probleme qu'un edit de config d'une ligne reglait.
Cout : 40 min au lieu de 2, et un risque de casse ajoute.
**Lecon** : estimer le cout AVANT d'agir. Toujours le geste le moins invasif qui resout. L'escalade se
merite : niveau N+1 seulement si N a REELLEMENT echoue.

## 8. Planifier sans auditer l'existant
**Vecu** : plan bati pour ajouter une chose... deja presente. Et une "correction" proposee qui aurait
casse un reglage volontaire.
**Lecon** : lire l'existant AVANT de proposer. Un plan qui ne cite aucun fichier lu est un plan devine.

## 9. Le plan parfait sur le mauvais objectif
**Vecu** : blueprint impeccable — etapes precises, code complet — pour un besoin mal compris.
Zero erreur d'execution, resultat inutile.
**Lecon** : la precision du plan ne rattrape JAMAIS un objectif faux. C'est le seul defaut qu'aucune
phase suivante ne rattrape. → Phase 0.5, volet 1.

## 10. L'ancre textuelle ne vit pas dans la fonction annoncee (2026-08-26)
**Vecu** : blueprint sept-findings, etape 2.3 ancre 3 — libelle « rattachement (fin de `rattacher()`) »,
mais l'ancre textuelle (`return {"ok": True, "vid": cur.lastrowid}`) vivait dans `creer_variante()`,
qui ne rattache rien. L'execution mecanique a suivi l'ancre : invalidation posee au mauvais endroit,
commentaire faux dans le code, vrais points de rattachement non couverts (rattrapes par un filtre
live au service — par construction, pas par intention).
**Lecon** : au plan, verifier pour CHAQUE ancre (grep -n) qu'elle est bien DANS la fonction que le
libelle annonce. A l'execution : libelle et ancre divergents = STOP et rapport, jamais application
silencieuse de l'un des deux.

## 11. Mutation manuelle a taille constante = __pycache__ empoisonne (2026-08-26)
**Vecu** : protocole « muter `POIDS .32→.99`, pytest doit echouer, restaurer en finally ». La mutation
garde la MEME taille de fichier et la restauration tombe dans la MEME seconde → le .pyc compile pendant
la mutation reste valide aux yeux de Python (validation mtime seconde + taille). Reproduit en review :
pytest 4 failed sur source PROPRE, API dev servant des scores faux (x100 sans redistribution), alors
que `git diff` etait vide.
**Lecon** : tout protocole de mutation Python purge `__pycache__` en finally OU tourne avec
`PYTHONDONTWRITEBYTECODE=1`. `git diff` vide ne prouve pas l'etat reel : le bytecode n'est pas dans git.

## 12. Amender un chiffre a UN endroit quand il vit a TROIS (2026-08-26)
**Vecu** : seuil F5 amende 450→500 Ko dans la verification dev, laisse a « < 450 000 » dans le point
de deploiement ET le critere de succes global. La prod sert 488 561 octets : le critere global etait
infaisable tel qu'ecrit (et le gzip cense compenser n'etait pas actif). Meme classe : la note « la base
de tests n'a pas de table posts » (1.2) et « le script du blueprint initial » (4.1) referencent des
etats supprimes par leurs propres amendements.
**Lecon** : un amendement = grep de la valeur/du texte dans TOUT le document, chaque occurrence
tranchee explicitement. Un critere de succes global qui depend d'un seuil amende se re-derive, il ne
se laisse pas en l'etat.

## 13. Specifier du concurrent sans derouler le « pendant » (2026-08-26)
**Vecu** : le code du blueprint (F2, cache pre-calcule) ecrit `quand=now` en fin de calcul sans
regarder si une invalidation est arrivee PENDANT les ~5 s du calcul : un detachement pendant un
recalcul est ecrase et le reel disparait des propositions jusqu'a la collecte suivante (jusqu'a 3 h).
Meme classe : pendant le tout premier calcul (synchrone), une 2e requete sert une liste vide au lieu
d'attendre. La course etait dans le code SPECIFIE par le plan — l'execution etait conforme.
**Lecon** : tout plan qui specifie cache + invalidation + thread deroule explicitement le scenario
« evenement pendant le calcul » : jeton d'epoque lu avant le calcul et compare avant d'ecrire, ou
re-verification de l'invalidation en fin de calcul.
