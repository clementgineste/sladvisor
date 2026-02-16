# PRD — SLAdvisor

Version: 1.0  
Date: 2026-02-16  
Branche: `PRD`  
Statut: Draft (working document)

## 1. Contexte et ambition

SLAdvisor est un conseiller SLA statique (single-page, zéro backend) qui traduit des besoins métier (criticité, RTO/RPO, budget, support) en recommandation de tier de disponibilité et en implications techniques/opérationnelles.

Ambition du produit:
- passer d'un **calculateur utile** à un **outil de décision de référence** pour arbitrer fiabilité, coûts et complexité;
- rendre explicites les compromis (ex: budget faible vs SLA élevé);
- conserver l'ADN du projet: simplicité d'usage, transparence, déploiement statique.

## 2. Sources utilisées pour ce PRD

- Code actuel: `index.html` (app, moteur de calcul, exports, tables de référence).
- Vision/contraintes “agent”:
  - `AGENTS.md` (architecture, stack, conventions, priorités MVP/v2).
- Backlog GitHub:
  - Issues ouvertes: #13, #10, #8, #7.
  - Issues fermées pertinentes: #1, #2, #3, #4, #5, #6, #9, #11, #12.
- Milestones GitHub:
  - `v2.0 - Major features` (open)
  - `Infrastructure` (open)

## 3. État actuel du produit

### 3.1 Fonctionnel

Le produit couvre déjà:
- wizard 6 étapes (criticality, downtime tolerance, support window, budget, RTO, RPO);
- moteur de scoring déterministe + mapping vers tiers `Bronze` → `Diamond`;
- calcul d'indisponibilité (annuelle/mensuelle/hebdo);
- architecture, multiplicateur de coût, pénalités, alertes d'incohérence;
- table de comparaison de tous les tiers;
- références SLA cloud (AWS/GCP/Azure) affichées;
- partage via URL params;
- export Markdown + PDF;
- dark mode.

### 3.2 Technique

- 1 fichier `index.html`, Alpine.js + Tailwind CDN, jsPDF CDN.
- logique centrale en fonctions pures (`computeSLARecommendation`, `detectWarnings`, `formatDuration`).
- déploiement GitHub Pages, sans build.

### 3.3 Points forts

- compréhension rapide de la recommandation;
- expérience fluide et interactive;
- très faible coût de maintenance opérationnelle;
- time-to-deploy quasi nul.

### 3.4 Limites principales

D'après l'audit (#13):
- robustesse export/copie (échecs silencieux possibles);
- validation URL params insuffisante;
- marge d'amélioration a11y sémantique;
- hardening sécurité côté dépendances CDN/CSP;
- données cloud SLA statiques à gouverner dans le temps.

## 4. Problème produit à résoudre

Aujourd'hui, beaucoup d'équipes demandent un niveau de SLA sans mesurer:
- le coût réel (infra + équipe + process);
- l'effort opérationnel continu;
- les contraintes architecturales minimales;
- les compromis explicites entre disponibilité, vélocité et budget.

SLAdvisor doit devenir un **cadre de décision** et pas uniquement un “score calculator”.

## 5. Utilisateurs cibles

### 5.1 Personas

1. Founder / PM / PO
- besoin: cadrer rapidement une promesse de disponibilité réaliste.
- risque actuel: sur-promettre un SLA irréaliste.

2. Engineering Manager / Tech Lead
- besoin: argumenter les implications architecture/process face au business.
- risque actuel: discussions abstraites, sans référentiel commun.

3. SRE / Ops
- besoin: traduire l'objectif SLA en exigences observabilité, astreinte, runbooks.
- risque actuel: attentes élevées sans investissement correspondant.

4. Avant-vente / consultant cloud
- besoin: support pédagogique pour ateliers client.
- risque actuel: recommandations peu traçables.

### 5.2 Jobs To Be Done

- “Quand un stakeholder demande 99.99%, je veux visualiser immédiatement ce que cela implique pour éviter une promesse irréaliste.”
- “Quand je définis RTO/RPO, je veux comprendre le tier atteignable avec mon budget actuel.”
- “Quand je partage une recommandation, je veux un livrable clair (URL/PDF/Markdown) exploitable en réunion.”

## 6. Vision produit v2

Construire un **dashboard SLA interactif** (issue #10) qui:
- expose la logique de bout en bout (SLA ↔ architecture ↔ pratiques opérationnelles ↔ coûts);
- inclut un mode reverse pleinement exploitable;
- conserve le wizard comme entrée guidée (onboarding débutant), puis ouvre sur une vue experte.

## 7. Objectifs & non-objectifs

### 7.1 Objectifs

- O1: améliorer la qualité de décision (moins de promesses SLA irréalistes).
- O2: accroître la crédibilité du résultat (robustesse + transparence + sources).
- O3: élargir l'usage (bilingue FR/EN, partage, export utilisable en comités).
- O4: maintenir une architecture statique simple et durable.

### 7.2 Non-objectifs

- N1: ne pas devenir un outil de monitoring runtime.
- N2: ne pas simuler exhaustivement toutes les architectures cloud possibles.
- N3: pas de backend dans cette phase (sauf décision explicite future).

## 8. Exigences produit

## 8.1 Exigences fonctionnelles (FR)

F1. Recommendation SLA
- calculer un tier + score + impacts (downtime, architecture, coût, pénalités, alertes).

F2. Explainability
- afficher les facteurs qui tirent la recommandation vers le haut/bas.

F3. Dashboard expert (v2)
- vue transversale par tier: architecture, déploiement, observabilité, incidents, error budget, résilience.

F4. Reverse mode
- partir des contraintes actuelles (budget/équipe/infra) et afficher le tier max atteignable + gaps vers tier supérieur.

F5. i18n FR/EN
- bascule de langue + persistance préférence + fallback.

F6. Export/partage
- lien partageable stable;
- export Markdown/PDF fiable et complet (pas de contenu tronqué).

F7. Références cloud
- exposer sources + date de vérification.

### 8.2 Exigences non fonctionnelles (NFR)

NFR1. Performance
- chargement initial < 2s en conditions standard desktop (hors contraintes réseau extrêmes).

NFR2. Accessibilité
- navigation clavier complète;
- structure ARIA cohérente;
- contraste conforme WCAG AA sur éléments critiques.

NFR3. Fiabilité
- pas d'échec silencieux sur actions critiques (copie/export).

NFR4. Sécurité
- réduction surface supply-chain (SRI/CSP/pinning selon faisabilité GitHub Pages).

NFR5. Maintenabilité
- conserver un coeur de calcul testable;
- introduire une stratégie de test minimale (issue #7).

## 9. Principes UX

- Clarté d'abord: chaque chiffre doit être contextualisé.
- Progressif: novice (wizard) -> expert (dashboard).
- Causalité visible: modifier un input -> effet immédiat partout.
- Surcharge maîtrisée: densité informative oui, confusion non.
- Partage prêt réunion: URL, PDF, Markdown lisibles sans explication orale.

## 10. Métriques de succès

### 10.1 Produit

- taux d'utilisation de la section “comparison/dashboard” après recommandation.
- taux de partage (clic `Share`) et d'export (Markdown/PDF).
- taux de sessions avec modification d'au moins 2 paramètres (signe d'exploration).

### 10.2 Qualité

- taux d'erreurs JS console (objectif: ~0 en usage normal).
- taux d'échec actions copie/export (objectif: visible + géré).
- couverture de scénarios critiques du moteur (tests unitaires).

### 10.3 Adoption

- trafic récurrent mensuel;
- temps moyen passé dans l'outil;
- nombre de issues feedback orientées amélioration vs bug bloquant.

## 11. Roadmap et milestones

### Milestone `v2.0 - Major features`

Objectif: passer du calculateur au dashboard décisionnel.

- M2.1 Dashboard interactif SLA (issue #10)
  - vue unique par tier avec implications techniques/opérationnelles;
  - reverse mode intégré dans la vue expert;
  - visualisation error budget.

- M2.2 i18n FR/EN (issue #8)
  - dictionnaire de traduction central;
  - switch FR/EN persistant;
  - couverture complète des textes UI + exports + messages.

### Milestone `Infrastructure`

Objectif: fiabiliser le produit à coût minimal.

- M-I1 Tests moteur + smoke CI (issue #7)
  - tests unitaires fonctions pures;
  - smoke Playwright en CI.

- M-I2 Qualité/robustesse issue #13
  - pagination PDF;
  - clipboard robuste;
  - validation stricte URL params;
  - ajustements a11y et hardening sécurité.

## 12. Backlog structuré (issue-driven)

### 12.1 Must-have (priorité haute)

- #13 (qualité/robustesse) — sous-lots:
  - PDF pagination + `try/finally`;
  - clipboard feedback fiable;
  - validation URL enum.
- #10 (dashboard v2) — MVP dashboard:
  - sections architecture/deployment/ops/error budget;
  - interactions live liées aux entrées.
- #7 (tests) — socle qualité continue.

### 12.2 Should-have

- #8 (i18n) version complète (UI + exports + warnings).
- gouvernance des données cloud SLA (`lastVerifiedAt`, routine de revue).

### 12.3 Could-have

- simulation multi-scenarios comparés (A/B/C) en localStorage.
- export “executive summary” 1-page orienté management.

## 13. Architecture cible (sans backend)

Principes:
- conserver un `index.html` unique pour déploiement statique;
- extraire logiquement les blocs dans le fichier (sections bien marquées);
- ajouter une couche de “domain model” claire:
  - `inputs`;
  - `scoring`;
  - `derivedInsights`;
  - `uiState`.

Évolution recommandée:
- objet `translations` central;
- objet `tierBlueprint` enrichi (ops, observabilité, déploiement, coût équipe);
- helpers robustes (`safeCopy`, `safeExportPDF`, `parseAndValidateParams`).

## 14. Risques et mitigation

R1. Complexité UI (dashboard dense)
- mitigation: versionner en paliers (MVP dashboard puis enrichissements).

R2. Régression sur moteur de calcul
- mitigation: tests unitaires de mapping + snapshots de sorties clés.

R3. Dette a11y en UI custom
- mitigation: simplifier sémantique, check clavier/SR à chaque release.

R4. Données cloud obsolètes
- mitigation: date de fraîcheur visible + revue trimestrielle.

R5. Drift de langue (FR/EN)
- mitigation: clés i18n uniques + checklist de couverture.

## 15. Critères d'acceptation globaux (release-level)

- CA1: chaque recommandation est explicable (score + raisons + alertes).
- CA2: exports et partage ne mentent pas (succès/échec explicites).
- CA3: aucun texte utilisateur “orphelin” hors système i18n (objectif v2 i18n complet).
- CA4: smoke CI vert sur push + tests moteur passent.
- CA5: dashboard v2 permet de comprendre concrètement les implications d'un tier.

## 16. Décisions ouvertes pour discussion

1. Positionnement de la langue par défaut:
- Option A: auto-détection navigateur;
- Option B: anglais par défaut;
- Option C: FR par défaut sur domaine actuel puis switch manuel.

2. Niveau de détail dashboard v2 en première release:
- Option A: architecture + incidents + coût;
- Option B: ajouter aussi observabilité + error budget;
- Option C: full scope #10 dès le départ.

3. Niveau d'investissement tests:
- Option A: unit tests moteur uniquement;
- Option B: unit + smoke Playwright minimal;
- Option C: inclure parcours e2e principaux.

4. Stratégie sécurité CDN:
- Option A: durcissement minimal (CSP simple);
- Option B: SRI/pinning quand possible;
- Option C: auto-hébergement des assets externes.

## 17. Plan d'exécution proposé (90 jours)

- Sprint 1 (semaines 1-2)
  - lot robustesse #13 prioritaire;
  - base tests unitaires moteur.

- Sprint 2 (semaines 3-4)
  - smoke CI + stabilisation QA;
  - design système i18n + migration textes.

- Sprint 3-4 (semaines 5-8)
  - dashboard v2 MVP (vue implications + reverse mode visible);
  - instrumentation métriques d'usage basiques.

- Sprint 5-6 (semaines 9-12)
  - enrichissements dashboard;
  - finition accessibilité/sécurité;
  - documentation release + communication.

## 18. Traçabilité PRD -> GitHub

- Vision dashboard: issue #10, milestone `v2.0 - Major features`
- i18n: issue #8, milestone `v2.0 - Major features`
- Qualité/robustesse: issue #13 (open)
- Tests/CI: issue #7, milestone `Infrastructure`

---

Ce document est volontairement “gros” et orienté exécution. Il sert de base de discussion, puis de pilotage. La prochaine étape est de le découper en epic + sous-issues actionnables avec Definition of Done par lot.
