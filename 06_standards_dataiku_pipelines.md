# Standard Corporate - Dataiku Pipelines

## 1. En-tete du document

- **Titre**: Standard Corporate - Regles et meilleures pratiques pour la construction de pipelines dans Dataiku
- **Version**: 1.0
- **Date**: 2026-06-10
- **Auteur / Role**: Equipe Data Engineering & Data Governance
- **Validation / Approbation**: Lead Data Engineer, Data Architect, Head of BI, RSSI (si applicable)
- **Public cible**: Data Engineers, Analytics Engineers, Data Analysts avances, ML Engineers
- **Commanditaire**: Direction Data & Performance / DSI
- **Statut**: Applicable a l'echelle de l'entreprise
- **Perimetre**: Tous les projets Dataiku (flows, recipes, scenarios, bundles, deploiements)
- **Objectif du document**: Definir les standards de conception, d'industrialisation et d'exploitation des pipelines Dataiku pour garantir qualite, fiabilite, securite, performance et maintenabilite.

## 1.1 Cadre editorial et niveau de norme

- **Niveau normatif**: Ce document est prescriptif. Les exigences sont applicables par defaut a tout pipeline Dataiku.
- **Interpretation des termes**:
  - **MUST**: exigence obligatoire, non negociable hors derogation formelle.
  - **SHOULD**: exigence fortement recommandee; toute exception doit etre justifiee et documentee.
  - **MAY**: option autorisee selon le contexte, sans contradiction avec les exigences MUST/SHOULD.
- **Mode de redaction**: formulations courtes, orientees execution, auditabilite et exploitabilite.
- **Gestion des exceptions**: toute derogation doit inclure un motif, une duree de validite, un plan de remediation et un approbateur nomme.
- **Precedence**: en cas de conflit, ce standard s'applique apres les obligations legales/reglementaires et avant les pratiques locales d'equipe.

## 2. Vision, principes directeurs et resultats attendus

### 2.1 Vision
Construire des pipelines Dataiku industrialises, lisibles et resilients, capables de servir des usages analytiques et operationnels avec des SLA maitrises.

### 2.2 Principes directeurs
1. **Pipeline by Design**: chaque flow doit etre concu avec un objectif metier explicite et des points de controle qualite.
2. **Reproductibilite**: executions deterministes, versionnees et relancables.
3. **Separation des responsabilites**: donnees, logique de transformation, orchestration et deploiement clairement separes.
4. **Observabilite native**: metriques, logs, alertes et preuves d'execution accessibles.
5. **Security & Compliance**: minimisation des donnees sensibles et controle strict des acces.
6. **Performance et cout**: execution efficace, dependances optimales, ressources proportionnees.

### 2.3 Resultats attendus
- Reduction des echecs de scenarios et des incidents de production.
- Diminution du temps de debug et de reprise.
- Acceleration du delivery data avec meilleure qualite.
- Homogeneite de conception entre equipes.

## 3. Roles et responsabilites (RACI simplifie)

| Activite | Data Engineer | Analytics Engineer | Data Steward | Platform Admin Dataiku | Metier (PO) |
|---|---|---|---|---|---|
| Design du flow et recipes | A/R | R | C | I | C |
| Definitions regles qualite | R | C | A/R | I | C |
| Orchestration (scenarios, triggers) | A/R | C | I | C | I |
| Validation securite/acces | C | I | C | A/R | I |
| Validation metier des sorties | C | C | C | I | A/R |
| Deploiement bundle et monitoring | R | C | I | A/R | I |

## 4. Standard de construction des pipelines Dataiku (regles obligatoires)

### 4.1 Structuration des projets Dataiku
- **MUST** organiser les projets par domaine metier et cas d'usage, pas par utilisateur.
- **MUST** nommer datasets, recipes, scenarios avec conventions stables et explicites.
- **SHOULD** separer zones `raw`, `staging`, `curated`, `serving` dans le flow.
- **SHOULD** isoler les objets experimentaux hors chaine de production.

### 4.2 Design des flows
- **MUST** maintenir un flow lisible avec trajectoires de transformation claires.
- **MUST** eviter les dependances circulaires.
- **MUST** limiter les branches mortes ou non maintenues.
- **SHOULD** favoriser modularite par sous-ensembles logiques.

### 4.3 Recipes et transformations
- **MUST** choisir le type de recipe adapte (visual, SQL, Python, Spark) selon volume et complexite.
- **MUST** centraliser la logique critique dans des recettes versionnees et reusables.
- **MUST** documenter les hypotheses metier dans les recettes non triviales.
- **SHOULD** minimiser duplication de logique entre recettes.

### 4.4 Gestion des schemas et contrats de donnees
- **MUST** definir schema attendu (types, contraintes, nullabilite) pour datasets critiques.
- **MUST** detecter et gerer les schema drift avant propagation.
- **MUST** versionner les changements structurants.
- **SHOULD** maintenir un data contract lie au projet Dataiku.

### 4.5 Data quality et controles
- **MUST** appliquer checks de qualite sur etapes critiques (nulles, duplicats, bornes, referentiel).
- **MUST** bloquer ou isoler les donnees invalides selon politique de criticite.
- **SHOULD** stocker les rejets dans un dataset de quarantaine.
- **SHOULD** publier KPI de qualite (taux rejet, anomalies, fraicheur).

### 4.6 Scenarios et orchestration
- **MUST** definir scenarios idempotents et relancables.
- **MUST** parametrer retries, timeouts, et notifications sur echec.
- **MUST** separer scenarios de build, de controle, et de publication si necessaire.
- **SHOULD** utiliser triggers temporels et event-based de maniere explicite.

### 4.7 Gestion des environnements et promotion
- **MUST** separer DEV/TEST/PROD avec regles de promotion explicites.
- **MUST** promouvoir via bundles versionnes et approuves.
- **MUST** eviter modifications manuelles directes en production hors procedure d'urgence.
- **SHOULD** maintenir une matrice de compatibilite des connexions et secrets.

### 4.8 Securite et conformite
- **MUST** appliquer principe du moindre privilege sur projets, connexions et datasets.
- **MUST** proteger les credentials via mecanismes securises de plateforme.
- **MUST** masquer ou pseudonymiser les donnees sensibles en fonction du besoin.
- **SHOULD** tracer acces et executions sur actifs critiques.

### 4.9 Performance et scalabilite
- **MUST** adapter moteur d'execution (in-database, Spark, local) au workload.
- **MUST** limiter mouvements de donnees inutiles entre engines.
- **SHOULD** privilegier pushdown SQL quand possible.
- **SHOULD** partitionner et incrementaliser les traitements volumineux.

### 4.10 Logging, monitoring et exploitation
- **MUST** centraliser logs d'execution et historique des scenarios.
- **MUST** configurer alertes sur echec, retard SLA et degradation qualite.
- **SHOULD** definir runbook d'incident par pipeline critique.
- **SHOULD** realiser revues periodiques de sante des pipelines.

## 5. Checklist Qualite avant passage en production

### 5.1 Qualite fonctionnelle
- [ ] Resultats valides sur jeux de test representatifs.
- [ ] KPI metier concordants avec definitions officielles.

### 5.2 Robustesse technique
- [ ] Scenarios relancables sans effet de bord.
- [ ] Gestion des erreurs et notifications activee.

### 5.3 Gouvernance et documentation
- [ ] Ownership, dependances, SLA et runbook documentes.
- [ ] Data contracts et schemas a jour.

### 5.4 Securite
- [ ] Acces minimaux verifies.
- [ ] Donnees sensibles protegees selon policy.

### 5.5 Performance
- [ ] Temps d'execution compatible SLA.
- [ ] Couts compute/stockage estimes et acceptables.

## 6. Bonnes pratiques d'implementation

### 6.1 Design flow efficace
- Commencer par une vue cible du flow puis descendre par etapes atomiques.
- Garder une convention de nommage uniforme pour faciliter navigation et support.

### 6.2 Reutilisation
- Transformer les logiques repetitives en composants reutilisables (recipes standards, macros, templates).
- Factoriser configurations communes (variables projet/globales).

### 6.3 Testabilite
- Ajouter jeux de tests minimaux automatises sur datasets cle.
- Comparer volumes, distinct counts et KPI avant/apres changement.

### 6.4 CI/CD et deploiement
- Versionner projets et bundles dans Git.
- Exiger revue par pair avant promotion en PROD.
- Integrer smoke tests post-deploiement.

### 6.5 Pilotage operationnel
- Suivre MTTD/MTTR des incidents pipelines.
- Mesurer taux de succes scenarios et taux de rerun.
- Prioriser ameliorations selon impact metier et cout.

## 7. Lignes rouges (interdits)

- Construire des pipelines critiques sans checks qualite.
- Deployer en production sans bundle versionne ni validation.
- Laisser des credentials en clair dans code/recipes.
- Maintenir des recipes dupliquees non gouvernees.
- Ignorer des echecs repetes de scenario sans action corrective.

## 8. Astuces expertes et gains rapides (lifehacks)

1. **Pre-check avant build principal**: detecte rapidement indisponibilite source et schema drift.
2. **Dataset de quarantaine standard**: evite blocage total du pipeline en cas d'anomalies.
3. **Naming orientee support**: inclure domaine, objet, et role de l'etape dans le nom.
4. **Variables de projet centralisees**: simplifient promotion multi-environnements.
5. **Scenarios decoupes par responsabilite**: plus simples a rerun et a diagnostiquer.
6. **Monitoring des temps par etape**: identifie rapidement les goulots d'etranglement.
7. **Checklist de release Dataiku**: reduit regressions lors des promotions.

## 9. Mode operatoire standard (SOP)

1. Cadrage besoin metier, SLA, contraintes securite.
2. Design du flow cible et des data contracts.
3. Construction des recipes et checks qualite.
4. Orchestration scenarios, retries et alertes.
5. Tests fonctionnels, techniques, securite et performance.
6. Promotion par bundle versionne vers production.
7. Monitoring continu, revue des incidents et optimisation.

## 10. Gouvernance documentaire

- Revision trimestrielle de ce standard.
- Toute derogation doit etre approuvee et tracee.
- Les incidents majeurs et post-mortems alimentent une mise a jour obligatoire.

## 11. Annexes

### 11.1 Template minimal de pipeline Dataiku

```yaml
project_key: SALES_PIPELINE
owner_technical: data.engineering@company.com
owner_business: sales.ops@company.com
sla:
  freshness: "J+0 07:00 CET"
  success_rate: ">= 99%"
flow_layers:
  - raw
  - staging
  - curated
  - serving
quality_checks:
  - not_null: order_id
  - uniqueness: transaction_id
  - accepted_range: amount >= 0
orchestration:
  scenario: sc_daily_sales_build
  retries: 2
  timeout_minutes: 90
security:
  pii_masking: true
  restricted_groups:
    - grp_data_finance
```

### 11.2 Confidentialite
Certaines donnees, exemples et captures peuvent etre anonymises ou legerement modifies afin de proteger les informations internes et sensibles.
