# Plan d'action — Guide de démonstration Bob × IBM Planning Analytics BUILD

## Vue d'ensemble

**Objectif** : Produire un fichier `guide-bob-pa-build.md` complet, en français, destiné à une équipe BUILD PA. Ce guide contient 20 exercices répartis sur 8 domaines. Chaque exercice propose un énoncé + un prompt à exécuter + le vrai output produit par Bob sur le serveur `24Retail`, que le participant compare avec son propre résultat.

**Approche** : Construire le guide domaine par domaine. Pour chaque exercice, Bob exécute d'abord le prompt sur `24Retail` pour capturer le vrai résultat, puis rédige la fiche de l'exercice avec cet output intégré.

**Fichier de sortie** : `guide-bob-pa-build.md`

**Serveur de démo** : `24Retail` (MCP `pa-tz-server` + `pa-tz-cube`)

**Nb exercices** :
- Domaines 1, 4, 5, 6, 7, 8 → 2 exercices chacun (12 au total)
- Domaines 2 (TurboIntegrator) et 3 (Règles TM1) → 4 exercices chacun (8 au total)
- **Total : 20 exercices**

---

## Template de fiche exercice (appliqué à chaque exercice)

Chaque fiche contient les rubriques suivantes :

1. **Objectif** — ce que le participant apprend / démontre
2. **Contexte** — situation BUILD dans laquelle cet exercice se produit
3. **Prompt à lancer** — texte exact copiable dans Bob
4. **Étapes** — déroulé pas à pas de l'interaction avec Bob
5. **Résultat de référence** — vrai output produit par Bob sur 24Retail (code, analyse, artefact)
6. **Avec Bob** — temps, ce qui est automatisé, niveau de qualité
7. **Sans Bob** — temps, outils requis, points de friction, risques
8. **Points de discussion** — questions pour animer le débat post-exercice

---

## Sous-tâches

### Sous-tâche 0 — Squelette du fichier Markdown
- **Intent** : Créer `guide-bob-pa-build.md` avec l'introduction, les prérequis, les 8 sections de domaine vides (titres + descriptions), la conclusion vide et les annexes.
- **Expected Outcomes** : Le fichier existe, est navigable via ancres Markdown, et peut être enrichi domaine par domaine sans conflit.
- **Todo List** :
  - [ ] Créer `guide-bob-pa-build.md`
  - [ ] Rédiger l'introduction (contexte, cible, mode d'emploi du guide, conventions)
  - [ ] Rédiger la section Prérequis (Bob + MCP, serveur 24Retail, connaissances PA, timing)
  - [ ] Créer les 8 sections domaine avec titres, descriptions et placeholders d'exercices
  - [ ] Créer la section Conclusion (vide, à remplir en sous-tâche 9)
  - [ ] Créer les 4 annexes (vides, à remplir en sous-tâche 9)
- **Status** : [ ] pending

---

### Sous-tâche 1 — Domaine 1 : Architecture & Modélisation (2 exercices)
- **Intent** : Montrer comment Bob aide un architecte BUILD à analyser un modèle existant et à proposer une architecture cible.
- **Exercices** :
  - `1.1` — Reverse-engineering d'un modèle existant : *"Analyse la structure complète du serveur 24Retail et donne-moi une vue d'ensemble de l'architecture du modèle de données"*
  - `1.2` — Proposition d'architecture : *"En te basant sur le cube Revenue de 24Retail, propose une architecture optimisée pour un nouveau cube de reporting consolidé intégrant Supply Chain"*
- **Expected Outcomes** : Les 2 fiches sont rédigées avec le vrai output Bob capturé sur 24Retail.
- **Todo List** :
  - [ ] Exécuter le prompt 1.1 sur 24Retail, capturer l'output
  - [ ] Rédiger la fiche 1.1 complète
  - [ ] Exécuter le prompt 1.2 sur 24Retail, capturer l'output
  - [ ] Rédiger la fiche 1.2 complète
  - [ ] Intégrer les 2 fiches dans le MD
- **Status** : [ ] pending

---

### Sous-tâche 2 — Domaine 2 : Développement TurboIntegrator (4 exercices)
- **Intent** : Démontrer la capacité de Bob à écrire, déployer et optimiser des processus TI complets — c'est le domaine le plus riche en valeur pour une équipe BUILD.
- **Exercices** :
  - `2.1` — ETL chargement de données : *"Écris un processus TI qui charge des données depuis un fichier CSV externe vers le cube Revenue de 24Retail"*
  - `2.2` — Spread mensuel avec clé saisonnière : *"Crée et déploie un processus TI Copy_Revenue_Units qui ventile un total annuel pUnitFcst sur les 12 mois du cube Revenue de 24Retail selon une clé saisonnière paramétrable"*
  - `2.3` — Calcul agrégé multi-cubes : *"Écris un processus TI qui lit les données du cube Supply Chain et écrit les coûts agrégés dans le cube Income Statement de 24Retail"*
  - `2.4` — Optimisation d'un processus existant : *"Analyse les processus Copy_Revenue_Compare_Units et Copy_Revenue_Spread_Units de 24Retail et réécris-les en un processus unifié optimisé"*
- **Expected Outcomes** : 4 fiches avec code TI réel déployable, validé syntaxiquement par le MCP.
- **Todo List** :
  - [ ] Exécuter et capturer l'output du prompt 2.1 (génération de code uniquement)
  - [ ] Rédiger la fiche 2.1
  - [ ] Exécuter le prompt 2.2, déployer sur 24Retail via `create_tm1_process` + `update_tm1_process`, capturer le résultat de déploiement
  - [ ] Rédiger la fiche 2.2
  - [ ] Exécuter et capturer l'output du prompt 2.3
  - [ ] Rédiger la fiche 2.3
  - [ ] Exécuter le prompt 2.4 (analyse + réécriture), capturer l'output
  - [ ] Rédiger la fiche 2.4
  - [ ] Intégrer les 4 fiches dans le MD
- **Relevant Context** : Cube Revenue : dimensions = organization, Channel, product, Month, Year, Version, Revenue. Membres Channel : 10/Retail, 20/Internet, 30/Distribution. Membres Version : Version 1/Budget, Actual, Forecast, Predictive. Membres Month : Jan→Dec, Q1→Q4, Year.
- **Status** : [ ] pending

---

### Sous-tâche 3 — Domaine 3 : Règles TM1 (4 exercices)
- **Intent** : Montrer que Bob peut générer des règles TM1 correctes et expliquées, avec feeders — une tâche réputée complexe et source d'erreurs.
- **Exercices** :
  - `3.1` — KPI dérivé et projection : *"Génère les règles TM1 pour calculer Gross Margin % et un Run Rate (projection annuelle depuis YTD) dans le cube Revenue de 24Retail"*
  - `3.2` — Conversion de devise : *"Génère une règle TM1 qui applique les taux du cube Exchange Rates de 24Retail pour convertir les valeurs Revenue de Local en USD"*
  - `3.3` — Variance Budget vs Actual : *"Génère les règles TM1 pour calculer automatiquement les membres Variance et Variance% entre Version 1/Budget et Actual dans le cube Revenue"*
  - `3.4` — Feeders : *"Pour les règles générées en 3.1, génère les FEEDERS corrects et explique pourquoi chacun est nécessaire"*
- **Expected Outcomes** : 4 fiches avec code de règles TM1 réel, commenté, avec feeders.
- **Note** : Les règles sont générées (code livrable) mais non déployées — le MCP ne permet pas d'écrire les fichiers .rux directement.
- **Todo List** :
  - [ ] Exécuter et capturer l'output du prompt 3.1
  - [ ] Rédiger la fiche 3.1
  - [ ] Consulter la structure du cube Exchange Rates, puis exécuter et capturer l'output du prompt 3.2
  - [ ] Rédiger la fiche 3.2
  - [ ] Exécuter et capturer l'output du prompt 3.3
  - [ ] Rédiger la fiche 3.3
  - [ ] Exécuter et capturer l'output du prompt 3.4 (en lien avec 3.1)
  - [ ] Rédiger la fiche 3.4
  - [ ] Intégrer les 4 fiches dans le MD
- **Relevant Context** : Membres Revenue mesures = Units Sold, Unit Price, Unit Cost, Gross Revenue, Cost of Sales, Gross Margin, Gross Margin %. Membres Month inclut YTD et FcstMonth. Cube Exchange Rates existe sur 24Retail.
- **Status** : [ ] pending

---

### Sous-tâche 4 — Domaine 4 : Intégration (2 exercices)
- **Intent** : Montrer comment Bob aide à concevoir le mapping et les processus d'intégration depuis des sources externes réelles (SAP, fichiers).
- **Exercices** :
  - `4.1` — Mapping source → cube : *"Je reçois un export SAP avec les colonnes CompanyCode, MaterialNumber, FiscalPeriod, FiscalYear, Amount, Quantity. Génère le mapping vers le cube Revenue de 24Retail et le processus TI de chargement correspondant"*
  - `4.2` — Gestion des erreurs ETL : *"Améliore le processus TI généré en 4.1 pour ajouter une gestion robuste des erreurs : lignes rejetées, fichier de log, compteur d'erreurs, ProcessBreak si seuil dépassé"*
- **Expected Outcomes** : 2 fiches avec mapping documenté et code TI avec gestion d'erreurs complète.
- **Todo List** :
  - [ ] Exécuter et capturer l'output du prompt 4.1
  - [ ] Rédiger la fiche 4.1
  - [ ] Exécuter et capturer l'output du prompt 4.2 (en continuité avec 4.1)
  - [ ] Rédiger la fiche 4.2
  - [ ] Intégrer les 2 fiches dans le MD
- **Status** : [ ] pending

---

### Sous-tâche 5 — Domaine 5 : Migration (2 exercices)
- **Intent** : Montrer comment Bob accompagne un consultant face à un modèle legacy à migrer — analyse de risques, identification des dépendances, plan structuré.
- **Exercices** :
  - `5.1` — Audit de modèle legacy : *"Audite le serveur 24Retail et identifie les points de fragilité, les anti-patterns et les risques avant une migration vers une nouvelle version de PA"*
  - `5.2` — Plan de migration : *"Génère un plan de migration structuré pour porter le modèle 24Retail vers un nouveau serveur, en listant les dépendances entre cubes, dimensions et processus, avec l'ordre de migration recommandé"*
- **Expected Outcomes** : 2 fiches avec audit réel basé sur les métadonnées 24Retail et plan de migration actionnable.
- **Todo List** :
  - [ ] Exécuter et capturer l'output du prompt 5.1
  - [ ] Rédiger la fiche 5.1
  - [ ] Exécuter et capturer l'output du prompt 5.2
  - [ ] Rédiger la fiche 5.2
  - [ ] Intégrer les 2 fiches dans le MD
- **Status** : [ ] pending

---

### Sous-tâche 6 — Domaine 6 : Tests & Validation (2 exercices)
- **Intent** : Montrer comment Bob détecte des anomalies dans les données réelles et génère des cas de test pour les processus TI livrés.
- **Exercices** :
  - `6.1` — Détection d'anomalies : *"Analyse les données du cube Revenue de 24Retail et détecte les valeurs aberrantes ou incohérentes : outliers, ratios anormaux, données manquantes par rapport à d'autres versions"*
  - `6.2` — Génération de cas de test : *"Génère un jeu de cas de test complet pour valider le processus Copy_Revenue_Units que nous avons créé, couvrant les cas nominaux, les cas limites (pUnitFcst=0, pChannel vide) et les cas d'erreur"*
- **Expected Outcomes** : 2 fiches avec résultats d'analyse réels (outlier detection sur 24Retail) et cas de test documentés.
- **Todo List** :
  - [ ] Exécuter `perform_outlier_detection` sur le cube Revenue, capturer l'output
  - [ ] Rédiger la fiche 6.1
  - [ ] Exécuter et capturer l'output du prompt 6.2
  - [ ] Rédiger la fiche 6.2
  - [ ] Intégrer les 2 fiches dans le MD
- **Status** : [ ] pending

---

### Sous-tâche 7 — Domaine 7 : Documentation (2 exercices)
- **Intent** : Montrer que Bob génère automatiquement une documentation technique de qualité livrable depuis le modèle réel — tâche chronophage et souvent négligée en BUILD.
- **Exercices** :
  - `7.1` — Documentation d'architecture : *"Génère la documentation technique complète du cube Revenue de 24Retail : dimensions, membres, règles de gestion métier, flux de données entrants, processus TI associés"*
  - `7.2` — Spécification de processus TI : *"Génère la spécification fonctionnelle et technique du processus Copy_Revenue_Spread_Units de 24Retail comme si tu rédigeais un livrable de projet pour un client grand compte"*
- **Expected Outcomes** : 2 fiches avec documentation réelle prête à être livrée — niveau livrable projet.
- **Todo List** :
  - [ ] Exécuter et capturer l'output du prompt 7.1
  - [ ] Rédiger la fiche 7.1
  - [ ] Exécuter et capturer l'output du prompt 7.2
  - [ ] Rédiger la fiche 7.2
  - [ ] Intégrer les 2 fiches dans le MD
- **Status** : [ ] pending

---

### Sous-tâche 8 — Domaine 8 : Déploiement (2 exercices)
- **Intent** : Montrer comment Bob assiste le déploiement en production et le monitoring post-déploiement — étapes souvent mal couvertes en BUILD.
- **Exercices** :
  - `8.1` — Checklist de déploiement : *"Génère une checklist de déploiement complète pour promouvoir le modèle 24Retail de l'environnement de développement vers la production IBM Planning Analytics, avec les vérifications pré et post-déploiement"*
  - `8.2` — Monitoring post-déploiement : *"Monitore le serveur 24Retail en temps réel : liste les processus actifs, vérifie l'état des threads, identifie les processus qui présentent des erreurs dans leurs logs"*
- **Expected Outcomes** : 2 fiches avec checklist réelle et résultat de monitoring live du serveur 24Retail.
- **Todo List** :
  - [ ] Exécuter et capturer l'output du prompt 8.1
  - [ ] Rédiger la fiche 8.1
  - [ ] Exécuter `get_tm1_server_process_threads` + `get_tm1_server_process_execution_error_logs`, capturer l'output
  - [ ] Rédiger la fiche 8.2
  - [ ] Intégrer les 2 fiches dans le MD
- **Status** : [ ] pending

---

### Sous-tâche 9 — Conclusion & Annexes
- **Intent** : Clore le guide avec une synthèse honnête de la valeur de Bob (gains ET limites), une matrice de valeur, et les annexes de référence utiles à toute équipe BUILD.
- **Expected Outcomes** : Le fichier est complet, navigable, et prêt à être partagé à une équipe BUILD.
- **Todo List** :
  - [ ] Rédiger la Conclusion : matrice 8 domaines × (gain temps estimé / risque réduit / limite Bob honnête)
  - [ ] Rédiger Annexe A : Glossaire PA/TM1 (cube, dimension, TI, règle, feeder, sparse, sandbox…)
  - [ ] Rédiger Annexe B : Inventaire des outils MCP disponibles (pa-tz-cube + pa-tz-server)
  - [ ] Rédiger Annexe C : Template vierge d'une fiche exercice (Markdown copiable)
  - [ ] Rédiger Annexe D : Structure du serveur 24Retail utilisé pour les démos (cubes, dimensions clés, membres)
  - [ ] Relecture finale : vérifier cohérence des ancres, numérotation, prompts copiables, liens internes
- **Status** : [ ] pending

---

## Ordre d'exécution

```
ST0 → ST1 → ST2 → ST3 → ST4 → ST5 → ST6 → ST7 → ST8 → ST9
```

**Points d'attention** :
- ST2 exercice 2.2 : déploiement réel sur 24Retail — confirmer avant exécution
- ST3 : code de règles généré mais non déployé (limitation MCP sur les .rux) — acceptable
- ST9 : matrice de limites honnêtes — être transparent sur ce que Bob ne fait pas

---

## Questions de validation

1. ST2 exercice 2.2 prévoit un **déploiement réel** d'un processus sur 24Retail — confirmes-tu que c'est OK ?
2. Pour ST3, les règles TM1 sont **générées mais non déployées** (le MCP ne supporte pas l'écriture des .rux) — acceptable pour les exercices ?
3. La conclusion inclura une section **"Limites de Bob"** honnête — tu confirmes qu'on reste transparent ?
