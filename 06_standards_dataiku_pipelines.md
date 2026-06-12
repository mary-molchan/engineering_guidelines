# Standard Corporate — Dataiku Pipelines

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Règles, architecture et meilleures pratiques pour la construction de pipelines dans Dataiku |
| **Version** | 2.0 |
| **Date** | 2026-06-12 |
| **Auteur / Rôle** | Équipe Data Engineering & Data Governance |
| **Validation / Approbation** | Lead Data Engineer, Data Architect, Head of BI, Platform Admin Dataiku, RSSI si applicable |
| **Public cible** | Data Engineers, Analytics Engineers, Data Analysts avancés, ML Engineers, Data Stewards, Platform Admins Dataiku |
| **Commanditaire** | Direction Data & Performance / DSI |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Tous les projets Dataiku DSS : flows, datasets, recipes, scenarios, triggers, reporters, metrics, checks, bundles, automation nodes, API services, déploiements, monitoring et exploitation |
| **Objectif du document** | Définir les standards de conception, d’industrialisation, de sécurité, de qualité, de performance, de déploiement et d’exploitation des pipelines Dataiku afin de garantir fiabilité, traçabilité, maintenabilité et valeur métier. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les exigences s’appliquent par défaut à tout pipeline Dataiku créé, maintenu, déployé, exécuté ou monitoré dans l’organisation.

Sont concernés :

- projets Dataiku ;
- flows ;
- datasets ;
- managed folders ;
- visual recipes ;
- SQL recipes ;
- Python recipes ;
- Spark recipes ;
- scenarios ;
- triggers ;
- reporters ;
- metrics ;
- checks ;
- bundles ;
- project deployments ;
- automation nodes ;
- API services ;
- code environments ;
- connexions ;
- variables ;
- dashboards de monitoring ;
- jobs de production ;
- pipelines analytiques ;
- pipelines ML/AI lorsque Dataiku est utilisé comme orchestrateur.

---

## 1.2 Interprétation des termes

| Terme | Signification |
|---|---|
| **MUST** | Exigence obligatoire, non négociable hors dérogation formelle. |
| **SHOULD** | Exigence fortement recommandée ; toute exception doit être justifiée et documentée. |
| **MAY** | Option autorisée selon le contexte, sans contradiction avec les exigences MUST/SHOULD. |
| **MUST NOT** | Interdiction explicite. |
| **Exception documentée** | Écart temporaire validé par un approbateur nommé, avec justification, durée de validité et plan de remédiation. |

---

## 1.3 Mode de rédaction

Les règles sont orientées :

- exécution industrielle ;
- auditabilité ;
- exploitabilité ;
- sécurité ;
- qualité des données ;
- performance ;
- maîtrise des coûts ;
- reproductibilité ;
- gouvernance ;
- maintenabilité ;
- support de production.

---

## 1.4 Gestion des exceptions

Toute dérogation doit inclure :

- le projet concerné ;
- le flow ou pipeline concerné ;
- le motif de l’écart ;
- l’impact qualité ;
- l’impact sécurité ;
- l’impact performance ;
- l’impact exploitation ;
- l’impact coût ;
- la durée de validité ;
- le plan de remédiation ;
- l’approbateur nommé ;
- la date de révision.

---

## 1.5 Précedence

En cas de conflit, l’ordre de priorité est :

1. obligations légales, réglementaires et contractuelles ;
2. politiques sécurité / RSSI ;
3. politiques Data Governance ;
4. standards Data Architecture ;
5. standards plateforme Dataiku ;
6. présent standard Dataiku Pipelines ;
7. pratiques locales d’équipe.

---

## 1.6 Sources officielles Dataiku utilisées

Ce standard s’appuie sur la documentation officielle Dataiku DSS, notamment :

- [The Flow — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/flow/index.html)
- [Visual recipes — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/other_recipes/index.html)
- [Automation scenarios — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/scenarios/index.html)
- [Scenario steps — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/scenarios/steps.html)
- [Launching a scenario / triggers — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/scenarios/triggers.html)
- [Reporting on scenario runs — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/scenarios/reporters.html)
- [Variables in scenarios — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/scenarios/variables.html)
- [Metrics, checks and Data Quality — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/metrics-check-data-quality/index.html)
- [Metrics — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/metrics-check-data-quality/metrics.html)
- [Checks — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/metrics-check-data-quality/checks.html)
- [Testing a project — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/scenarios/test_scenarios.html)
- [Production deployments and bundles — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/deployment/index.html)
- [Creating bundles — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/deployment/creating-bundles.html)
- [Deploying bundles with the Project Deployer — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/deployment/deploying-bundles.html)
- [Main project permissions — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/security/permissions.html)
- [Connections security — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/security/connections.html)
- [User profiles — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/security/user-profiles.html)
- [Dataiku API Deployer / API node deployment infrastructures — Dataiku DSS documentation](https://doc.dataiku.com/dss/latest/apinode/api-deployment-infrastructures.html)

---

## 1.7 Définitions clés

| Terme | Définition |
|---|---|
| **DSS** | Dataiku Data Science Studio, plateforme utilisée pour créer, orchestrer, déployer et monitorer des projets data. |
| **Project** | Conteneur logique regroupant flow, datasets, recipes, scenarios, notebooks, dashboards, variables et configurations. |
| **Flow** | Représentation visuelle des datasets et recipes ; il permet de comprendre les dépendances et le lineage des traitements. |
| **Dataset** | Objet Dataiku représentant une source, une table, un fichier, une sortie intermédiaire ou un résultat consommable. |
| **Managed Folder** | Espace de fichiers géré par Dataiku, souvent utilisé pour stocker artefacts, exports, modèles ou fichiers intermédiaires. |
| **Recipe** | Étape de transformation créant un ou plusieurs outputs à partir d’un ou plusieurs inputs. |
| **Visual Recipe** | Recipe configurable via interface graphique, adaptée aux transformations analytiques courantes. |
| **Code Recipe** | Recipe utilisant SQL, Python, R, Spark ou autre code pour des traitements avancés. |
| **Scenario** | Orchestration Dataiku permettant d’exécuter des étapes, builds, checks, scripts, notifications et automatisations. |
| **Trigger** | Condition de lancement automatique d’un scenario : temporelle, dataset changé, SQL trigger, Python trigger, API, etc. |
| **Reporter** | Mécanisme de notification de scenario run : email, messaging ou autre canal configuré. |
| **Metric** | Mesure calculée automatiquement sur un objet du flow : nombre de lignes, fraîcheur, taille, statistiques, métriques custom. |
| **Check** | Contrôle validant qu’une metric respecte une condition attendue. |
| **Bundle** | Archive versionnée d’un projet créée sur Design Node pour déploiement vers Automation Node. |
| **Design Node** | Environnement Dataiku utilisé pour concevoir et modifier les projets. |
| **Automation Node** | Environnement Dataiku utilisé pour exécuter des projets en production. |
| **Project Deployer** | Composant permettant de gérer les Automation nodes, déployer des bundles et monitorer les projets déployés. |
| **API Deployer** | Composant permettant de gérer et déployer des services API sur API nodes ou infrastructures Kubernetes. |
| **Code Environment** | Environnement Python/R isolant dépendances, packages et versions. |
| **Connection** | Configuration d’accès à une source ou cible : SQL, filesystem, cloud storage, API, etc. |
| **Variables** | Paramètres de projet, instance ou scenario utilisés pour rendre les flows configurables. |
| **Partitioning** | Mécanisme permettant de découper les datasets selon date, pays, domaine ou autre clé. |
| **Build** | Exécution d’un ou plusieurs datasets/recipes du flow. |
| **Idempotence** | Capacité d’un pipeline à être relancé sans créer d’effets secondaires indésirables. |
| **Quarantine dataset** | Dataset isolant les lignes invalides ou rejetées. |
| **SLA** | Engagement de disponibilité, fraîcheur, durée d’exécution ou qualité. |
| **MTTD** | Mean Time To Detect : délai moyen de détection d’incident. |
| **MTTR** | Mean Time To Recover : délai moyen de résolution/restauration. |

---

## 2. Vision, principes directeurs et résultats attendus

## 2.1 Vision

Construire des pipelines Dataiku industrialisés, lisibles, résilients et gouvernés, capables de servir des usages analytiques, opérationnels et ML avec des SLA maîtrisés.

Un projet Dataiku de production ne doit pas être une succession informelle de recettes créées au fil de l’eau. Il doit être conçu comme un produit data exploitable :

- compréhensible par un nouveau membre de l’équipe ;
- relançable après incident ;
- observable ;
- documenté ;
- sécurisé ;
- testé ;
- versionné ;
- déployé via processus contrôlé ;
- aligné sur les besoins métier ;
- maintenable dans le temps.

---

## 2.2 Principes directeurs

1. **Pipeline by Design**  
   Chaque flow doit être conçu avec un objectif métier explicite, un grain cible et des points de contrôle qualité.

2. **Reproductibilité**  
   Les exécutions doivent être déterministes, versionnées, relançables et auditées.

3. **Separation of Concerns**  
   Données, logique de transformation, orchestration, qualité, sécurité et déploiement doivent être clairement séparés.

4. **Observable by Default**  
   Scenarios, recipes, datasets et outputs critiques doivent produire des métriques, logs, alertes et preuves d’exécution.

5. **Security & Compliance by Design**  
   Les accès, connexions, credentials et données sensibles doivent être contrôlés dès la conception.

6. **Quality Gates**  
   Les contrôles qualité doivent être intégrés au flow et aux scenarios, non ajoutés manuellement après incident.

7. **Performance and Cost Awareness**  
   Le moteur d’exécution, les volumes, les partitions, les connexions et la fréquence doivent être adaptés au workload réel.

8. **Promotion Controlled**  
   Les passages en production doivent se faire via bundles versionnés, testés et approuvés.

9. **Minimal Manual Production Changes**  
   Les modifications manuelles en production doivent être interdites hors procédure d’urgence.

10. **Business Trust**  
    Les sorties critiques doivent être validées, documentées et réconciliables avec les définitions métier.

---

## 2.3 Résultats attendus

L’application de ce standard doit permettre de :

- réduire les échecs de scenarios ;
- réduire les incidents de production ;
- diminuer le temps de debug ;
- accélérer la reprise après incident ;
- améliorer la qualité des outputs ;
- standardiser les flows entre équipes ;
- réduire les duplications de logique ;
- améliorer la traçabilité ;
- sécuriser les données sensibles ;
- améliorer les performances ;
- maîtriser les coûts compute ;
- faciliter le passage Design Node → Automation Node ;
- renforcer la confiance des métiers dans les livrables Dataiku.

---

## 2.4 Indicateurs de succès

| Indicateur | Cible recommandée |
|---|---|
| Pipelines critiques avec scenario de production | 100 % |
| Pipelines critiques avec checks qualité | 100 % |
| Pipelines critiques avec alerting/reporters | 100 % |
| Pipelines critiques avec owner documenté | 100 % |
| Pipelines critiques déployés via bundle | 100 % |
| Pipelines critiques relançables | 100 % |
| Credentials en clair dans code/recipes | 0 |
| Flows de production avec branches mortes | 0 ou exceptions documentées |
| Scenarios récurrents en échec sans action | 0 |
| Projets production sans runbook | 0 |
| Temps moyen de debug incident | Réduction continue |
| Taux de succès scenarios | ≥ 99 % sur pipelines critiques, ou SLA spécifique |
| Taux de rejets qualité non expliqués | Réduction continue |
| Déploiements PROD hors bundle | 0 hors urgence documentée |

---

## 3. Rôles et responsabilités

## 3.1 RACI simplifié

| Activité | Data Engineer | Analytics Engineer | Data Analyst avancé | ML Engineer | Data Steward | Platform Admin Dataiku | RSSI / Security | Métier / PO |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Cadrage besoin et SLA | C | C | C | C | C | I | I | A/R |
| Design du flow | A/R | R | C | C | C | I | I | C |
| Développement recipes | A/R | A/R | R | R | C | I | I | I |
| Data contracts | R | R | C | C | A/R | I | C | C |
| Règles qualité | R | C | C | C | A/R | I | C | C |
| Scenarios / orchestration | A/R | R | C | C | I | C | I | I |
| Connexions et credentials | C | I | I | I | I | A/R | C | I |
| Permissions projet | C | C | I | C | C | A/R | C | I |
| Sécurité données sensibles | C | C | I | C | A/R | C | A/R | C |
| Validation métier outputs | C | C | C | C | C | I | I | A/R |
| Bundles et promotion | R | C | I | C | I | A/R | I | I |
| Monitoring production | R | C | I | C | C | A/R | I | I |
| Incident management | A/R | C | I | C | C | C | C | I |
| Documentation | R | R | C | C | A/R | C | C | C |

---

## 3.2 Ownership obligatoire

Chaque projet Dataiku de production doit avoir :

- un owner métier ;
- un owner technique ;
- un backup owner ;
- un data steward si données critiques ;
- un support contact ;
- un niveau de criticité ;
- un SLA ;
- un niveau de confidentialité ;
- une liste des connexions utilisées ;
- une liste des datasets critiques ;
- une liste des scenarios de production ;
- une stratégie de déploiement ;
- un runbook ;
- un changelog ;
- une date de dernière revue.

Aucun projet Dataiku critique ne doit être en production sans ownership explicite.

---

## 4. Architecture cible Dataiku

## 4.1 Architecture logique

```text
Sources
  ↓
Connections Dataiku
  ↓
Raw datasets
  ↓
Staging recipes
  ↓
Curated datasets
  ↓
Business / Serving datasets
  ↓
Dashboards / Exports / APIs / ML / BI
  ↓
Monitoring, Metrics, Checks, Scenarios
```

---

## 4.2 Architecture environnementale

```text
Design Node
  - développement
  - tests
  - recette
  - création de bundles

Project Deployer
  - gestion des bundles
  - déploiement vers Automation Node
  - monitoring des projets déployés

Automation Node
  - exécution production
  - scenarios actifs
  - datasets de production
  - logs et historique

API Deployer / API Node
  - déploiement de services API
  - scoring temps réel
  - endpoints opérationnels
```

---

## 4.3 Standard environnement

| Environnement | Usage | Règles |
|---|---|---|
| **DEV** | Développement et exploration contrôlée | Données échantillonnées ou non sensibles si possible |
| **TEST/UAT** | Validation technique et métier | Données représentatives, contrôles renforcés |
| **PROD / Automation Node** | Exécution officielle | Accès restreint, pas de modification manuelle hors procédure |
| **SANDBOX** | Expérimentation | Durée limitée, non connecté aux chaînes critiques |
| **API Node** | Exposition services temps réel | Sécurité, versioning et monitoring spécifiques |

---

## 4.4 Principes de séparation

- Design Node sert à construire, tester, corriger.
- Automation Node sert à exécuter de façon contrôlée.
- Les connexions DEV/TEST/PROD doivent être séparées.
- Les variables doivent permettre adaptation par environnement.
- Les datasets de production ne doivent pas être modifiés manuellement depuis Design Node.
- Les bundles doivent être utilisés pour promotion.
- Les API services doivent passer par API Deployer si exposés en production.

---

## 5. Structuration des projets Dataiku

## 5.1 Règles de découpage des projets

### Obligations

- Organiser les projets par domaine métier et cas d’usage, pas par utilisateur.
- Éviter les projets “fourre-tout”.
- Éviter les projets personnels pour pipelines de production.
- Définir un objectif clair par projet.
- Limiter le périmètre pour maintenir lisibilité et supportabilité.
- Documenter les dépendances inter-projets.
- Isoler les expérimentations hors chaînes de production.

---

## 5.2 Typologie recommandée

| Type de projet | Usage | Exemples |
|---|---|---|
| **Ingestion project** | Collecte et préparation initiale | CRM ingestion, ERP ingestion |
| **Transformation project** | Curated / business logic | Sales curated, Finance reconciliation |
| **Serving project** | Outputs pour BI/API/export | Sales marts, customer scoring output |
| **ML project** | Feature engineering, training, scoring | Churn model, fraud detection |
| **Monitoring project** | Observabilité, qualité, SLA | Data quality dashboard |
| **Sandbox project** | Exploration temporaire | Prototype segmentation |

---

## 5.3 Taille et complexité

Un projet doit être refactorisé si :

- le flow devient illisible ;
- les branches n’ont plus de lien fonctionnel ;
- les temps de build augmentent fortement ;
- plusieurs owners métier coexistent sans gouvernance ;
- les scenarios deviennent trop nombreux ;
- les permissions deviennent difficiles à gérer ;
- la promotion par bundle devient risquée ;
- le support ne peut plus diagnostiquer rapidement.

---

## 5.4 Nommage des projets

Format recommandé :

```text
<DOMAIN>_<USE_CASE>_<ENV_OPTIONAL>
```

Exemples :

```text
SALES_DAILY_PIPELINE
FINANCE_RECONCILIATION
CUSTOMER_CHURN_SCORING
OPS_DATA_QUALITY_MONITORING
```

### Règles

- Nom stable.
- Pas de nom personnel.
- Pas d’acronyme obscur.
- Domaine explicite.
- Usage principal visible.
- Environnement indiqué si l’architecture l’exige.

---

## 5.5 Documentation projet

Chaque projet production doit contenir une page de documentation ou wiki incluant :

- objectif ;
- owners ;
- SLA ;
- périmètre fonctionnel ;
- sources ;
- outputs ;
- flow overview ;
- scenarios ;
- triggers ;
- metrics/checks ;
- connexions ;
- données sensibles ;
- procédures de reprise ;
- dépendances ;
- changelog ;
- liens vers data contracts.

---

## 6. Design des flows

## 6.1 Principes

Le flow est l’artefact central de compréhension du pipeline. Il doit rester lisible, navigable et explicable.

### Règles

- Le flow doit représenter une trajectoire logique.
- Les dépendances circulaires sont interdites.
- Les branches mortes doivent être supprimées ou isolées.
- Les objets expérimentaux doivent être hors chaîne production.
- Les datasets critiques doivent être identifiables.
- Les zones fonctionnelles doivent être nommées et documentées.
- Les branches de contrôle qualité doivent être visibles.
- Les outputs consommés doivent être clairement indiqués.
- Le lineage doit permettre de remonter aux sources.

---

## 6.2 Couches recommandées dans le flow

```text
RAW
  ↓
STAGING
  ↓
CURATED
  ↓
SERVING / MART
  ↓
EXPORT / API / BI / ML
```

### RAW

Usage :

- données sources brutes ou quasi brutes ;
- ingestion initiale ;
- minimum de transformation ;
- traçabilité.

Règles :

- conserver metadata source ;
- ne pas utiliser directement pour BI ;
- ne pas appliquer de logique métier lourde ;
- documenter source et fréquence.

### STAGING

Usage :

- typage ;
- nettoyage technique ;
- normalisation de formats ;
- détection erreurs ;
- préparation des joins.

Règles :

- conversions explicites ;
- gestion des valeurs invalides ;
- contrôles de schéma ;
- pas de logique métier complexe non documentée.

### CURATED

Usage :

- entités métier fiables ;
- harmonisation ;
- déduplication ;
- règles qualité ;
- référentiels ;
- historisation si nécessaire.

Règles :

- data contract ;
- checks qualité ;
- ownership ;
- outputs réutilisables.

### SERVING / MART

Usage :

- datasets consommables ;
- exports ;
- BI ;
- scoring ;
- reporting ;
- APIs.

Règles :

- grain documenté ;
- colonnes utiles uniquement ;
- sécurité appliquée ;
- performance adaptée ;
- définitions métier validées.

---

## 6.3 Flow readability

### Bon flow

Un bon flow :

- se lit de gauche à droite ou haut en bas ;
- a des noms explicites ;
- évite les croisements inutiles ;
- sépare les couches ;
- contient peu d’objets orphelins ;
- expose les points de contrôle ;
- permet de comprendre rapidement source → transformation → output.

### Mauvais flow

Un mauvais flow :

- mélange exploration et production ;
- contient des recettes dupliquées ;
- a des datasets sans owner ;
- contient des branches mortes ;
- n’a pas de conventions ;
- dépend de notebooks manuels ;
- ne montre pas les contrôles qualité ;
- ne permet pas de relancer facilement une étape.

---

## 6.4 Flow zones

Recommandation :

Créer des zones ou conventions visuelles pour :

- sources ;
- préparation ;
- qualité ;
- transformations métier ;
- outputs ;
- quarantine ;
- monitoring ;
- ML ;
- exports.

---

## 6.5 Objets expérimentaux

Les objets expérimentaux doivent :

- être dans un projet sandbox ;
- ou être préfixés `EXP_`;
- avoir une date d’expiration ;
- ne pas être dépendance d’un output PROD ;
- être supprimés après validation/rejet ;
- ne pas être inclus dans le bundle production sauf justification.

---

## 7. Conventions de nommage

## 7.1 Datasets

Format recommandé :

```text
<layer>_<domain>_<entity>_<grain_or_purpose>
```

Exemples :

```text
raw_sales_orders
stg_sales_orders_typed
cur_sales_orders_validated
cur_sales_customer_deduped
srv_sales_daily_kpi
quarantine_sales_orders_invalid
ops_sales_pipeline_audit
```

---

## 7.2 Recipes

Format recommandé :

```text
rcp_<action>_<input>_to_<output>
```

Exemples :

```text
rcp_type_raw_orders_to_stg_orders
rcp_dedupe_stg_customers_to_cur_customers
rcp_aggregate_cur_sales_to_srv_daily_kpi
rcp_validate_stg_orders_to_quarantine_orders
```

---

## 7.3 Scenarios

Format recommandé :

```text
sc_<frequency>_<domain>_<purpose>
```

Exemples :

```text
sc_daily_sales_build
sc_hourly_customer_ingestion
sc_weekly_finance_reconciliation
sc_daily_data_quality_monitoring
sc_on_demand_sales_reprocess
```

---

## 7.4 Metrics and checks

Format recommandé :

```text
chk_<object>_<rule>
metric_<object>_<measurement>
```

Exemples :

```text
chk_sales_orders_not_null_order_id
chk_sales_orders_amount_positive
metric_sales_orders_row_count
metric_sales_orders_max_order_date
```

---

## 7.5 Variables

Format recommandé :

```text
<domain>_<purpose>_<parameter>
```

Exemples :

```text
sales_input_connection
sales_output_connection
sales_default_start_date
pipeline_run_mode
dq_error_threshold
```

---

## 7.6 Règles générales

- Pas de nom personnel.
- Pas de nom temporaire en production.
- Pas d’abréviation obscure.
- Pas d’espace si convention technique.
- Noms stables après mise en production.
- Renommage impactant doit être documenté.
- Les outputs consommés doivent avoir les noms les plus stables.

---

## 8. Recipes et transformations

## 8.1 Choix du type de recipe

| Type de recipe | Usage recommandé | Points d’attention |
|---|---|---|
| **Visual recipe** | Transformations standards, lisibles, accessibles | Bonne pour collaboration, mais surveiller complexité |
| **SQL recipe** | Pushdown vers base, gros volumes relationnels | Optimiser SQL, éviter logique opaque |
| **Python recipe** | Logique avancée, API, traitements spécifiques | Gérer code env, tests, performance |
| **Spark recipe** | Gros volumes distribués | Justifier overhead, config clusters |
| **Sync recipe** | Mouvement de données | Contrôler volumes et sécurité |
| **Prepare recipe** | Nettoyage léger et typage | Éviter accumulation de règles non documentées |
| **Join recipe** | Jointures simples | Tester cardinalité |
| **Group recipe** | Agrégations | Documenter grain |
| **Stack recipe** | Union de datasets homogènes | Tester schéma |
| **Window recipe** | Fonctions analytiques | Clarifier partition/order |

---

## 8.2 Règles de choix moteur

### In-database / SQL pushdown

À privilégier lorsque :

- les données sont dans une base SQL ;
- le volume est important ;
- la base cible est performante ;
- les transformations sont relationnelles ;
- éviter les mouvements de données est critique.

### Local engine

À limiter à :

- petits volumes ;
- prototypes ;
- transformations simples ;
- fichiers locaux contrôlés.

### Spark

À utiliser lorsque :

- volumes très importants ;
- besoin distribué ;
- infrastructure disponible ;
- coût/complexité justifiés.

### Python

À utiliser lorsque :

- logique non faisable facilement en visual/SQL ;
- appels API ;
- librairies spécifiques ;
- traitement algorithmique ;
- transformation custom.

---

## 8.3 Règles obligatoires recipes

- Choisir le type de recipe selon volume, complexité et moteur cible.
- Documenter toute recipe non triviale.
- Éviter la duplication de logique.
- Éviter les recipes sans output clair.
- Éviter les recipes qui mélangent plusieurs responsabilités.
- Tester les recipes critiques.
- Nommer les recipes explicitement.
- Maintenir les dépendances code env.
- Éviter l’écriture directe dans outputs sensibles sans contrôle.
- Vérifier la cardinalité des joins.
- Vérifier les volumes avant/après transformation.
- Éviter les traitements manuels hors scenario.

---

## 8.4 Visual recipes

### Avantages

- lisibles par profils non développeurs ;
- auditables visuellement ;
- adaptées aux transformations courantes ;
- favorisent collaboration.

### Risques

- logique métier dispersée ;
- accumulation d’étapes ;
- difficulté de revue si recipe trop longue ;
- duplication entre projets ;
- performance insuffisante si engine mal choisi.

### Règles

- Limiter la complexité d’une recipe.
- Documenter les règles métier.
- Ajouter checks sur outputs.
- Éviter la reproduction de règles déjà centralisées.
- Refactoriser en plusieurs recipes si nécessaire.

---

## 8.5 SQL recipes

### Règles

- SQL lisible et conforme au standard SQL corporate.
- Pas de `SELECT *` en production.
- Colonnes explicitement listées.
- Joins testés.
- CTE nommées clairement.
- Filtrer tôt.
- Éviter les scans inutiles.
- Utiliser variables si besoin.
- Ajouter commentaires d’intention.
- Tester performance sur volume représentatif.

---

## 8.6 Python recipes

### Règles

- Utiliser un code environment validé.
- Ne pas stocker credentials dans le code.
- Utiliser les connexions/variables Dataiku prévues.
- Structurer le code en fonctions.
- Logger les étapes importantes.
- Gérer erreurs explicitement.
- Éviter `print` non structuré en production.
- Éviter de charger en mémoire des volumes massifs.
- Écrire les outputs via API Dataiku de manière contrôlée.
- Versionner les dépendances.
- Ajouter tests unitaires si logique critique.

### Template recommandé

```python
import dataiku
import pandas as pd
from datetime import datetime, timezone

def validate_input_schema(df: pd.DataFrame) -> None:
    required_columns = {"order_id", "customer_id", "amount"}
    missing_columns = required_columns - set(df.columns)
    if missing_columns:
        raise ValueError(f"Missing required columns: {missing_columns}")

def main():
    input_dataset = dataiku.Dataset("stg_sales_orders")
    output_dataset = dataiku.Dataset("cur_sales_orders_validated")

    df = input_dataset.get_dataframe()
    validate_input_schema(df)

    df["amount"] = pd.to_numeric(df["amount"], errors="coerce")
    df["quality_status"] = df["amount"].apply(
        lambda value: "VALID" if pd.notnull(value) and value >= 0 else "INVALID_AMOUNT"
    )
    df["processed_at"] = datetime.now(timezone.utc)

    valid_df = df[df["quality_status"] == "VALID"].copy()
    output_dataset.write_with_schema(valid_df)

if __name__ == "__main__":
    main()
```

---

## 8.7 Spark recipes

### Règles

- Justifier Spark par volume ou traitement.
- Éviter Spark pour petits datasets.
- Configurer ressources adaptées.
- Partitionner correctement.
- Éviter collect massif vers driver.
- Tester sur volumes représentatifs.
- Monitorer temps et coûts.
- Documenter configuration.

---

## 8.8 Code environments

### Règles

- Les code environments doivent être gérés par plateforme ou owners autorisés.
- Les versions de packages doivent être pinées si pipeline critique.
- Les dépendances doivent être documentées.
- Les environnements obsolètes doivent être retirés.
- Les changements de code env doivent être testés avant production.
- L’usage d’un code env doit être restreint aux groupes autorisés si nécessaire.
- Les erreurs de permission code env doivent être traitées comme incidents de configuration.

---

## 9. Schémas, data contracts et schema drift

## 9.1 Data contract obligatoire

Chaque dataset critique doit avoir un data contract.

### Contenu minimal

- nom fonctionnel ;
- nom technique ;
- owner métier ;
- owner technique ;
- source ;
- output ;
- grain ;
- schéma ;
- types ;
- nullabilité ;
- contraintes ;
- qualité attendue ;
- SLA ;
- règles de sécurité ;
- stratégie d’évolution ;
- règles de rétention ;
- consommateurs ;
- plan de support.

---

## 9.2 Schéma attendu

Pour les datasets critiques, documenter :

| Élément | Exemple |
|---|---|
| Nom colonne | `order_id` |
| Type | `string` |
| Nullable | `false` |
| Description | Identifiant commande |
| Règle qualité | Non null, unique par ligne commande |
| Classification | Internal |
| Source | CRM |
| Transformation | Trim + uppercase |
| Owner | Sales Data Steward |

---

## 9.3 Schema drift

### Règles

- Détecter schema drift avant propagation.
- Bloquer breaking changes.
- Alerter owner.
- Isoler données non conformes.
- Documenter évolution.
- Versionner data contract.
- Tester downstream impact.
- Informer consommateurs.

### Matrice de compatibilité

| Changement | Statut par défaut | Action |
|---|---|---|
| Ajout colonne nullable | Compatible sous condition | Documenter |
| Ajout colonne non nullable | Breaking | Validation requise |
| Suppression colonne | Breaking | Plan migration |
| Renommage colonne | Breaking | Plan migration |
| Changement type compatible | À valider | Tests |
| Changement type incompatible | Bloquant | Correction source |
| Changement grain | Bloquant majeur | Revalidation métier |
| Changement sémantique | Bloquant majeur | Versioning |

---

## 9.4 Gestion des types

### Règles

- Types définis explicitement.
- Conversion date/nombre documentée.
- Valeurs invalides isolées.
- Nulls interprétés selon data contract.
- Colonnes sensibles classifiées.
- Colonnes techniques standardisées.

---

## 9.5 Colonnes techniques recommandées

| Colonne | Usage |
|---|---|
| `source_system` | Origine |
| `source_file_name` | Fichier source |
| `source_row_number` | Ligne source |
| `ingested_at` | Date ingestion |
| `processed_at` | Date transformation |
| `run_id` | Identifiant scenario run |
| `quality_status` | Statut qualité |
| `quality_error_reason` | Raison rejet |
| `record_hash` | Détection changement |
| `is_current` | Historisation |
| `valid_from` | Début validité |
| `valid_to` | Fin validité |

---

## 10. Data quality, metrics et checks

## 10.1 Principes

La qualité des données doit être intégrée au flow et automatisée via metrics, checks et scenarios.

Dataiku permet de calculer des metrics sur des objets du flow et d’utiliser des checks pour vérifier que ces métriques respectent des conditions attendues. Ces contrôles doivent devenir des quality gates pour les pipelines critiques.

---

## 10.2 Types de contrôles minimaux

| Type | Exemple |
|---|---|
| Schéma | colonnes attendues, types, ordre si critique |
| Complétude | taux de nulls |
| Unicité | clé primaire, clé métier |
| Validité | bornes, formats, accepted values |
| Fraîcheur | max date source, délai ingestion |
| Volume | nombre de lignes, variation vs baseline |
| Référentiel | codes pays, devises, statuts |
| Duplicats | doublons techniques/métier |
| Reconciliation | source total vs target total |
| Distribution | outliers, dérive |
| Sécurité | absence de PII non attendue |

---

## 10.3 Quality gates

### Règles

- Les checks critiques doivent bloquer la publication ou l’étape suivante.
- Les checks warning doivent alerter sans bloquer.
- Les seuils doivent être métier ou statistiques, non arbitraires.
- Les checks doivent être visibles dans les scenarios.
- Les résultats doivent être historisés.
- Les rejets doivent être analysables.

---

## 10.4 Exemple de matrice de qualité

| Dataset | Check | Sévérité | Seuil | Action |
|---|---|---|---|---|
| `stg_sales_orders` | `order_id not null` | Critical | invalid_count = 0 | Stop pipeline |
| `stg_sales_orders` | `amount >= 0` | Critical | invalid_count = 0 | Quarantine + stop si > seuil |
| `cur_sales_orders` | row count variation | Warning | variation < 20 % | Alert |
| `srv_sales_daily_kpi` | freshness | Critical | max_date = D-1 ou D | Stop publication |
| `cur_customers` | duplicate customer_id | Critical | duplicate_count = 0 | Stop pipeline |

---

## 10.5 Quarantine datasets

### Règles

- Les données invalides doivent être isolées.
- Les pipelines ne doivent pas supprimer silencieusement les erreurs.
- Les datasets de quarantaine doivent être standardisés.
- Les rejets doivent être monitorés.
- Un owner doit traiter les rejets.
- Les rejets récurrents doivent produire une action corrective.

### Colonnes recommandées

```text
run_id
source_system
source_dataset
source_row_identifier
rejected_at
error_code
error_message
raw_payload
quality_rule
severity
resolution_status
resolved_at
resolved_by
```

---

## 10.6 Dataset de résultats qualité

```text
dq_result
- run_id
- project_key
- dataset_name
- check_name
- severity
- status
- checked_at
- invalid_count
- threshold
- sample_link
- owner
```

---

## 10.7 Scenarios qualité

Un scenario qualité peut :

- calculer metrics ;
- exécuter checks ;
- arrêter le pipeline si critical ;
- envoyer notification ;
- écrire résultat dans dataset d’audit ;
- lancer un scenario downstream uniquement si qualité OK.

---

## 11. Scenarios et orchestration

## 11.1 Principes

Un scenario de production doit être conçu comme une orchestration fiable, relançable et observable.

### Règles obligatoires

- Scenarios critiques idempotents.
- Scenarios nommés selon convention.
- Triggers documentés.
- Retries configurés si pertinent.
- Timeouts définis.
- Reporters configurés.
- Échec notifié.
- Logs accessibles.
- Étapes séparées logiquement.
- Build principal précédé de pre-checks si source critique.
- Post-checks après build.
- Publication uniquement après qualité validée.
- Runbook associé.

---

## 11.2 Structure scenario recommandée

```text
1. Initialize run context
2. Pre-check sources
3. Check schema drift
4. Build raw/staging
5. Run staging quality checks
6. Build curated
7. Run curated quality checks
8. Build serving outputs
9. Run reconciliation checks
10. Publish / activate downstream
11. Send success/failure report
12. Write audit log
```

---

## 11.3 Scenarios séparés par responsabilité

### Recommandation

Créer des scenarios séparés lorsque le pipeline devient complexe :

| Scenario | Rôle |
|---|---|
| `sc_daily_sales_precheck` | Vérification sources et schémas |
| `sc_daily_sales_build` | Build principal |
| `sc_daily_sales_quality` | Checks qualité |
| `sc_daily_sales_publish` | Publication outputs |
| `sc_daily_sales_reprocess` | Relance contrôlée |
| `sc_daily_sales_monitoring` | Surveillance SLA |

---

## 11.4 Triggers

Dataiku permet de lancer des scenarios manuellement, par API ou via triggers.

### Types courants

- time-based trigger ;
- dataset change trigger ;
- SQL trigger ;
- Python trigger ;
- trigger after scenario ;
- manual trigger ;
- API trigger.

### Règles

- Les triggers doivent être documentés.
- Les triggers automatiques doivent être actifs uniquement en production validée.
- Les triggers concurrents doivent être évalués.
- Les runs parallèles non maîtrisés doivent être évités.
- Les dépendances cross-project doivent être explicites.
- Les triggers doivent tenir compte des fenêtres source.
- Les triggers doivent éviter la double exécution.

---

## 11.5 Retries

### Règles

- Les retries doivent être utilisés pour erreurs transitoires.
- Les retries ne doivent pas masquer un problème structurel.
- Le nombre de retries doit être limité.
- Les délais entre retries doivent être adaptés.
- Les échecs après retries doivent alerter.
- Les retries doivent rester idempotents.

---

## 11.6 Timeouts

### Règles

- Tout scenario critique doit avoir une durée attendue.
- Les étapes longues doivent avoir seuils d’alerte.
- Les timeouts doivent être réalistes.
- Les timeouts doivent déclencher diagnostic.
- Les dégradations progressives doivent être monitorées.

---

## 11.7 Reporters

Les reporters doivent informer les équipes sur :

- succès ;
- échec ;
- qualité dégradée ;
- SLA manqué ;
- retard ;
- volume anormal ;
- changement important.

### Règles

- Les notifications doivent être actionnables.
- Les messages doivent inclure projet, scenario, run, erreur, owner, lien logs.
- Les succès peuvent être résumés.
- Les échecs critiques doivent alerter canal support.
- Les alertes répétées sans action doivent être traitées.

### Template message

```text
[DATAIKU][FAILURE] sc_daily_sales_build

Project: SALES_DAILY_PIPELINE
Environment: PROD
Scenario run: ${scenarioRunId}
Failed step: Build curated sales
Error: <error_message>
Impact: Sales dashboard may be stale
SLA: J+0 07:00 CET
Owner: data.engineering@company.com
Runbook: <link>
Logs: <link>
```

---

## 11.8 Variables de scenario

### Règles

- Utiliser les variables pour paramètres d’exécution.
- Ne pas stocker de secrets en variables simples.
- Documenter les variables.
- Éviter les variables globales ambiguës.
- Séparer variables environnementales et fonctionnelles.
- Valider les valeurs obligatoires au début du scenario.

---

## 12. Variables, paramètres et configuration

## 12.1 Types de variables

| Type | Usage |
|---|---|
| Project variables | Paramètres propres au projet |
| Global variables | Paramètres instance-wide, à limiter |
| Scenario variables | Paramètres définis pendant un run |
| Environment variables | Paramètres par environnement |
| Bundle variables | Paramètres utilisés à la promotion |

---

## 12.2 Règles

- Centraliser les paramètres.
- Éviter les valeurs hardcodées.
- Ne pas mettre de secrets dans variables visibles.
- Documenter les variables critiques.
- Utiliser conventions de nommage.
- Tester variables lors du déploiement.
- Prévoir valeurs par défaut sûres.
- Séparer DEV/TEST/PROD.

---

## 12.3 Exemple de variables projet

```json
{
  "env": "prod",
  "domain": "sales",
  "input_connection": "snowflake_sales_prd",
  "output_connection": "snowflake_mart_prd",
  "dq_critical_threshold": 0,
  "dq_warning_row_variation_pct": 20,
  "default_timezone": "Europe/Paris",
  "sla_freshness_hour_cet": "07:00"
}
```

---

## 12.4 Anti-patterns

- Chemins hardcodés dans recipes.
- Connexions DEV dans projet PROD.
- Dates fixes oubliées.
- Paramètres dupliqués dans plusieurs recipes.
- Secrets en clair.
- Variables non documentées.
- Variables globales modifiées sans analyse d’impact.

---

## 13. Gestion des environnements et promotion

## 13.1 Principes

Dataiku distingue typiquement un Design Node pour concevoir les projets et un Automation Node pour exécuter les projets en production. La promotion doit être contrôlée par bundles et Project Deployer lorsque disponible.

---

## 13.2 Bundles

### Règles

- Tout déploiement production doit passer par bundle versionné.
- Le bundle doit avoir un numéro de version.
- Le bundle doit être associé à un changelog.
- Le bundle doit être testé avant activation.
- Le bundle doit être approuvé par owner technique.
- Les changements breaking doivent être signalés.
- Les variables et connexions doivent être vérifiées.
- Le rollback doit être possible par bundle précédent.
- Les bundles obsolètes doivent être archivés selon politique.

---

## 13.3 Versioning de bundles

Format recommandé :

```text
v<major>.<minor>.<patch>
```

Exemples :

```text
v1.0.0 initial production release
v1.1.0 new curated dataset
v1.1.1 bug fix in amount parsing
v2.0.0 breaking schema change
```

---

## 13.4 Project Deployer

Le Project Deployer doit être utilisé pour :

- gérer les Automation nodes ;
- déployer les bundles ;
- activer les versions ;
- monitorer les projets déployés ;
- vérifier l’état de santé des déploiements.

### Règles

- Les droits Project Deployer doivent être limités.
- Les déploiements doivent être tracés.
- Les changements d’infrastructure doivent être contrôlés.
- Les projets critiques doivent avoir un statut de monitoring.
- Les déploiements manuels hors processus doivent être interdits.

---

## 13.5 Matrice de compatibilité environnement

Chaque projet doit documenter :

| Élément | DEV | TEST/UAT | PROD |
|---|---|---|---|
| Connexion source |  |  |  |
| Connexion cible |  |  |  |
| Datasets critiques |  |  |  |
| Variables |  |  |  |
| Code env |  |  |  |
| Scenarios actifs |  |  |  |
| Triggers actifs |  |  |  |
| Groupes autorisés |  |  |  |
| Secrets / credentials |  |  |  |

---

## 13.6 Déploiement en production

### Checklist promotion

- bundle créé ;
- changelog mis à jour ;
- tests Design Node passés ;
- UAT validée ;
- data quality checks passés ;
- sécurité validée ;
- variables PROD validées ;
- connexions PROD validées ;
- code env PROD validé ;
- scenarios PROD validés ;
- triggers planifiés ;
- reporters configurés ;
- runbook mis à jour ;
- rollback identifié ;
- approbation obtenue.

---

## 13.7 Rollback

### Règles

- Le rollback par bundle précédent doit être documenté.
- Les changements de données doivent avoir stratégie séparée.
- Les migrations destructives doivent avoir plan spécifique.
- Le rollback doit être testé pour pipelines critiques.
- Les dépendances aval doivent être informées.

---

## 14. Sécurité, permissions et connexions

## 14.1 Modèle de sécurité

Dataiku DSS s’appuie sur un modèle de permissions par groupes. Les user profiles ne doivent pas être considérés comme une fonctionnalité de sécurité ; la sécurité doit être gérée via groupes et permissions.

### Règles

- Utiliser groupes pour gérer les accès.
- Éviter les permissions individuelles.
- Appliquer least privilege.
- Limiter les droits admin.
- Séparer designers, reviewers, operators, consumers.
- Réviser les accès périodiquement.
- Documenter les groupes par projet.
- Limiter capacité d’exécuter du code non isolé.
- Limiter création de connexions personnelles.
- Contrôler accès aux code environments.

---

## 14.2 Rôles projet recommandés

| Groupe | Droits typiques |
|---|---|
| `grp_dataiku_platform_admin` | Administration plateforme |
| `grp_project_owner_<domain>` | Gestion projet |
| `grp_data_engineer_<domain>` | Développement recipes/scenarios |
| `grp_analytics_engineer_<domain>` | Développement analytique |
| `grp_data_steward_<domain>` | Validation qualité/documentation |
| `grp_business_viewer_<domain>` | Consultation outputs/dashboards |
| `grp_prod_operator` | Exécution/surveillance production |
| `grp_security_reviewer` | Revue sécurité |

---

## 14.3 Connexions

### Règles

- Les connexions doivent être créées par admins ou owners autorisés.
- Les connexions PROD doivent être séparées de DEV/TEST.
- Les credentials doivent être stockés dans mécanismes sécurisés.
- Les connexions personnelles doivent être limitées.
- Les connexions à données sensibles doivent être restreintes.
- Les connexions doivent avoir owner.
- Les connexions inutilisées doivent être revues.
- Les mouvements entre connexions doivent être contrôlés.
- Les sync recipes entre zones de sécurité différentes doivent être validées.

---

## 14.4 Données sensibles

### Obligations

- Identifier données sensibles.
- Minimiser colonnes sensibles.
- Pseudonymiser/masquer si possible.
- Restreindre accès datasets.
- Éviter export non contrôlé.
- Éviter copies sandbox.
- Protéger logs contre exposition de secrets.
- Documenter finalité.
- Appliquer politiques RGPD/locales.
- Impliquer RSSI/DPO si nécessaire.

---

## 14.5 Secrets

### Interdits

- credentials dans code Python ;
- tokens dans variables visibles ;
- clés API dans notebooks ;
- secrets dans descriptions de recipes ;
- exports de fichiers de configuration avec secrets ;
- partage de credentials par email/chat.

### Obligations

- Utiliser mécanismes sécurisés de Dataiku/plateforme.
- Restreindre accès aux connexions.
- Rotations selon politique.
- Désactivation après fin de besoin.
- Audit des usages.

---

## 14.6 Code execution security

Les droits d’exécution de code doivent être contrôlés, en particulier pour :

- Python recipes ;
- notebooks ;
- plugins ;
- code environments ;
- execution containers ;
- connexions sensibles ;
- API services.

### Règles

- Restreindre exécution de code aux groupes autorisés.
- Interdire code non isolé si politique sécurité l’exige.
- Valider dépendances externes.
- Éviter appels réseau non contrôlés.
- Scanner ou reviewer code critique.
- Journaliser exécutions critiques.

---

## 15. Performance et scalabilité

## 15.1 Principes

La performance d’un pipeline Dataiku dépend :

- du moteur choisi ;
- du volume ;
- de la localisation des données ;
- du pushdown ;
- des partitions ;
- du nombre de recipes ;
- des mouvements de données ;
- de la fréquence ;
- de la concurrence ;
- du code env ;
- de la configuration infrastructure ;
- des connexions sources/cibles.

---

## 15.2 Règles fondamentales

- Adapter engine au workload.
- Privilégier pushdown SQL lorsque possible.
- Éviter mouvements inutiles de données.
- Ne pas utiliser local engine pour gros volumes.
- Partitionner les datasets volumineux si pertinent.
- Incrémentaliser les traitements.
- Éviter recalcul complet si non nécessaire.
- Mesurer durées par étape.
- Identifier goulots d’étranglement.
- Optimiser d’abord les étapes les plus coûteuses.
- Éviter recettes dupliquées recalculant la même chose.
- Séparer workloads expérimentaux et production.

---

## 15.3 Pushdown SQL

### À privilégier lorsque

- source/cible SQL performante ;
- transformations relationnelles ;
- volume élevé ;
- coût de transfert important ;
- Snowflake/BigQuery/SQL engine optimisé disponible.

### Règles

- Écrire SQL conforme au standard.
- Vérifier les requêtes générées si visual recipe.
- Éviter extraction locale inutile.
- Tester temps d’exécution.
- Monitorer coût côté moteur.

---

## 15.4 Partitioning

### Cas d’usage

- données temporelles volumineuses ;
- reprocessing partiel ;
- refresh incrémental ;
- calcul par région/pays ;
- traitement parallèle ;
- SLA serré.

### Règles

- Choisir clé de partition pertinente.
- Éviter trop de petites partitions.
- Documenter stratégie.
- Tester builds partiels.
- Gérer late arriving data.
- Monitorer partitions manquantes.
- Prévoir reprocess par partition.

---

## 15.5 Incremental builds

### Règles

- Définir watermark.
- Définir fenêtre de recouvrement.
- Éviter doublons.
- Gérer late data.
- Journaliser partitions traitées.
- Tester rerun.
- Tester rollback.
- Comparer volumes.

---

## 15.6 Performance anti-patterns

- Build complet quotidien sur très gros volume sans besoin.
- Sync inutile entre deux systèmes alors qu’un pushdown est possible.
- Python local sur millions de lignes.
- Multiples recipes recalculant la même agrégation.
- Scenarios déclenchés trop fréquemment.
- Datasets intermédiaires énormes jamais nettoyés.
- Joins many-to-many non testés.
- Exports massifs non contrôlés.
- Code env instable.
- Triggers concurrents générant conflits.

---

## 16. Logging, monitoring et exploitation

## 16.1 Principes

Les pipelines Dataiku critiques doivent être monitorés comme des services de production.

### À monitorer

- statut scenario ;
- durée scenario ;
- durée par étape ;
- échecs ;
- retries ;
- fraîcheur datasets ;
- volumes ;
- taux de rejets ;
- checks qualité ;
- dérive schéma ;
- usage connexions ;
- coûts compute si disponible ;
- logs erreurs ;
- fréquence des reruns ;
- incidents ;
- SLA breaches.

---

## 16.2 Audit dataset

Créer un dataset d’audit par domaine ou plateforme.

```text
pipeline_run_audit
- run_id
- project_key
- scenario_name
- environment
- started_at
- ended_at
- status
- failed_step
- trigger_type
- rows_read
- rows_written
- rows_rejected
- freshness_timestamp
- quality_status
- error_message
- bundle_version
- owner
```

---

## 16.3 Monitoring scenario

### Statuts à suivre

- success ;
- failed ;
- warning ;
- aborted ;
- timeout ;
- skipped ;
- running too long ;
- no data ;
- data quality failed ;
- partial success.

---

## 16.4 Alertes minimales

- scenario failure ;
- scenario duration > SLA ;
- dataset freshness breach ;
- check qualité critical failed ;
- volume anormal ;
- schema drift ;
- taux de rejet élevé ;
- source indisponible ;
- connexion échouée ;
- bundle activation failed ;
- API service unhealthy ;
- code env failure ;
- permissions error.

---

## 16.5 Dashboard Platform Ops

Un dashboard d’exploitation Dataiku doit inclure :

- taux de succès scenarios ;
- durée moyenne ;
- top scenarios lents ;
- échecs par projet ;
- échecs par type ;
- SLA freshness ;
- checks qualité ;
- taux de rejet ;
- datasets non rafraîchis ;
- projets sans owner ;
- scenarios sans reporter ;
- bundles actifs ;
- dernières promotions ;
- incidents ouverts ;
- MTTR ;
- MTTD.

---

## 16.6 Runbook par pipeline critique

Chaque pipeline critique doit avoir :

- description ;
- owner ;
- SLA ;
- sources ;
- outputs ;
- scenario principal ;
- triggers ;
- dependencies ;
- common failures ;
- diagnostic steps ;
- rerun procedure ;
- rollback ;
- escalation ;
- contacts ;
- logs location ;
- dashboard monitoring ;
- known limitations.

---

## 17. Tests et validation

## 17.1 Types de tests

| Type | Objectif |
|---|---|
| Unit test | Tester une logique isolée |
| Schema test | Vérifier colonnes/types |
| Data quality test | Nulls, duplicats, bornes |
| Integration test | Vérifier flow complet |
| Regression test | Comparer avant/après |
| Performance test | Vérifier durée/ressources |
| Security test | Vérifier accès et masquage |
| UAT | Validation métier |
| Smoke test | Vérification rapide post-déploiement |
| Reprocessing test | Capacité à relancer |

---

## 17.2 Test scenarios

Dataiku permet de structurer des scenarios de test et d’en suivre les résultats via les fonctionnalités de monitoring de projet.

### Règles

- Les projets critiques doivent avoir un scenario de test.
- Les tests doivent être exécutés avant bundle PROD.
- Les tests doivent couvrir checks qualité.
- Les tests doivent valider outputs.
- Les tests doivent être visibles dans la documentation.
- Les tests échoués doivent bloquer promotion si critique.

---

## 17.3 Jeux de test

### Règles

- Utiliser données représentatives.
- Couvrir cas nominaux.
- Couvrir cas invalides.
- Couvrir volumes minimaux.
- Couvrir late data.
- Couvrir doublons.
- Couvrir nulls.
- Couvrir changements de schéma.
- Éviter PII réelle si non nécessaire.

---

## 17.4 Tests de non-régression

Comparer avant/après :

- row count ;
- distinct count ;
- KPI critiques ;
- volumes par partition ;
- taux de nulls ;
- taux de rejet ;
- max date ;
- distributions ;
- outputs de référence.

---

## 17.5 Smoke tests post-déploiement

Après activation bundle :

- scenario principal démarre ;
- connexions fonctionnent ;
- code env fonctionne ;
- premier dataset critique build ;
- checks qualité passent ;
- reporter fonctionne ;
- logs accessibles ;
- outputs visibles ;
- temps d’exécution acceptable.

---

## 18. CI/CD et versioning

## 18.1 Principes

Dataiku doit être intégré dans un cycle de livraison contrôlé :

- développement en Design Node ;
- revue ;
- tests ;
- bundle ;
- déploiement Automation Node ;
- activation ;
- monitoring ;
- rollback si nécessaire.

---

## 18.2 Versioning recommandé

À versionner :

- code recipes ;
- notebooks production si utilisés ;
- SQL ;
- Python ;
- variables templates ;
- documentation ;
- data contracts ;
- test definitions ;
- scenario definitions si exportables ;
- bundle versions ;
- changelog ;
- runbooks.

---

## 18.3 Repository recommandé

```text
dataiku-project/
│
├── README.md
├── CHANGELOG.md
├── docs/
│   ├── architecture.md
│   ├── data_contracts.md
│   ├── runbook.md
│   ├── security.md
│   └── deployment.md
│
├── sql/
│   ├── recipes/
│   └── tests/
│
├── python/
│   ├── recipes/
│   ├── libraries/
│   └── tests/
│
├── config/
│   ├── variables.dev.json
│   ├── variables.uat.json
│   └── variables.prod.template.json
│
├── quality/
│   ├── checks.md
│   └── dq_rules.yaml
│
└── release/
    ├── release_checklist.md
    └── rollback_plan.md
```

---

## 18.4 Changelog

```markdown
# Changelog — Dataiku Project

## 2026-06-12 — v2.0.0

### Added
- Nouveau scenario sc_daily_sales_quality.
- Dataset de quarantaine quarantine_sales_orders_invalid.
- Checks fraîcheur et duplicats.

### Changed
- Passage de build complet à build incrémental.
- Optimisation recipe SQL pour pushdown Snowflake.

### Fixed
- Correction parsing amount sur commandes ERP.

### Security
- Suppression PII email dans serving dataset.
- Restriction connexion Snowflake PROD au groupe data engineering.

### Breaking changes
- Renommage colonne revenue_amount en sales_amount_eur.
```

---

## 18.5 Pull Request checklist

```markdown
# Pull Request Checklist — Dataiku Pipeline

## Flow

- [ ] Flow lisible
- [ ] Pas de branche morte
- [ ] Couches raw/staging/curated/serving respectées
- [ ] Objets expérimentaux isolés

## Recipes

- [ ] Noms explicites
- [ ] Logique critique documentée
- [ ] Pas de duplication inutile
- [ ] Engine adapté au volume
- [ ] Code env validé si Python/R

## Data quality

- [ ] Checks critiques définis
- [ ] Quarantine prévue si nécessaire
- [ ] Tests nulls/duplicats/bornes
- [ ] Réconciliation source/cible

## Scenarios

- [ ] Scenario idempotent
- [ ] Triggers documentés
- [ ] Retries/timeouts définis
- [ ] Reporters configurés
- [ ] Runbook mis à jour

## Security

- [ ] Pas de credentials en clair
- [ ] Connexions conformes
- [ ] Permissions minimales
- [ ] Données sensibles protégées

## Deployment

- [ ] Bundle versionné
- [ ] Variables par environnement validées
- [ ] Smoke tests prévus
- [ ] Rollback documenté
```

---

## 19. ML et API pipelines dans Dataiku

## 19.1 Périmètre

Lorsque Dataiku est utilisé pour ML/AI ou API services, les mêmes principes de pipelines s’appliquent, avec contrôles supplémentaires.

---

## 19.2 ML pipelines

### Règles

- Séparer feature engineering, training, evaluation, scoring.
- Versionner datasets d’entraînement.
- Documenter target, features et exclusions.
- Contrôler fuite de données.
- Monitorer performance modèle.
- Monitorer drift si applicable.
- Versionner modèle.
- Valider métriques métier.
- Définir rollback modèle.
- Sécuriser scoring outputs.

---

## 19.3 API services

### Règles

- Déployer via API Deployer.
- Sécuriser endpoints.
- Éviter endpoint public sans justification.
- Versionner services.
- Tester charge et latence.
- Monitorer erreurs.
- Prévoir rollback.
- Documenter inputs/outputs.
- Masquer données sensibles dans logs.
- Définir SLA.

---

## 20. Checklist qualité avant passage en production

## 20.1 Qualité fonctionnelle

- [ ] Objectif métier clair.
- [ ] Outputs validés par PO.
- [ ] KPI concordants avec définitions officielles.
- [ ] Jeux de test représentatifs.
- [ ] Cas limites testés.
- [ ] Data contract validé.
- [ ] Grain documenté.
- [ ] Consumers identifiés.

---

## 20.2 Robustesse technique

- [ ] Scenario relançable.
- [ ] Idempotence validée.
- [ ] Gestion erreurs configurée.
- [ ] Retries définis.
- [ ] Timeouts définis.
- [ ] Pre-checks sources.
- [ ] Post-checks qualité.
- [ ] Reprocessing documenté.
- [ ] Rollback documenté.
- [ ] Logs exploitables.

---

## 20.3 Gouvernance et documentation

- [ ] Owner métier.
- [ ] Owner technique.
- [ ] Backup owner.
- [ ] SLA.
- [ ] Runbook.
- [ ] Data contracts.
- [ ] Changelog.
- [ ] Documentation flow.
- [ ] Connexions listées.
- [ ] Dépendances documentées.
- [ ] Bundle versionné.

---

## 20.4 Sécurité

- [ ] Permissions minimales.
- [ ] Groupes vérifiés.
- [ ] Connexions sécurisées.
- [ ] Credentials non exposés.
- [ ] Données sensibles identifiées.
- [ ] Masquage/pseudonymisation appliqué.
- [ ] Exports contrôlés.
- [ ] Code execution permissions vérifiées.
- [ ] API sécurisée si applicable.

---

## 20.5 Performance

- [ ] Engine adapté.
- [ ] Pushdown utilisé si pertinent.
- [ ] Partitioning évalué.
- [ ] Incrémentalité évaluée.
- [ ] Temps d’exécution compatible SLA.
- [ ] Volumes testés.
- [ ] Étapes lentes identifiées.
- [ ] Coûts compute estimés.
- [ ] Concurrence évaluée.
- [ ] Monitoring en place.

---

## 20.6 Exploitation

- [ ] Scenarios actifs en PROD.
- [ ] Triggers actifs et vérifiés.
- [ ] Reporters configurés.
- [ ] Alertes testées.
- [ ] Monitoring dashboard disponible.
- [ ] Runbook accessible.
- [ ] Support contact défini.
- [ ] Escalation définie.
- [ ] Smoke test post-déploiement prévu.

---

## 21. Lignes rouges

Les pratiques suivantes sont interdites pour les pipelines Dataiku corporate :

- construire des pipelines critiques sans checks qualité ;
- déployer en production sans bundle versionné ;
- modifier manuellement un projet production hors procédure ;
- laisser des credentials en clair dans code, variables ou notebooks ;
- maintenir des recipes dupliquées non gouvernées ;
- ignorer des échecs répétés de scenario ;
- utiliser user profiles comme mécanisme de sécurité ;
- accorder des permissions projet à des utilisateurs individuels sans justification ;
- utiliser une connexion PROD dans un projet sandbox ;
- copier des données sensibles vers sandbox sans anonymisation ;
- exposer des outputs contenant PII sans contrôle ;
- lancer des builds complets coûteux sans justification ;
- utiliser Python local pour gros volumes sans validation ;
- laisser des branches mortes dans un flow production ;
- publier des outputs sans owner ;
- activer des triggers concurrents non maîtrisés ;
- supprimer des outputs critiques sans rollback ;
- créer des API services publics sans sécurité ;
- ignorer les checks de fraîcheur ;
- ne pas documenter les variables d’environnement ;
- ne pas avoir de runbook pour pipeline critique.

---

## 22. Astuces expertes et gains rapides

## 22.1 Pre-check avant build principal

Un scenario de pre-check peut vérifier :

- disponibilité source ;
- connexion ;
- schéma ;
- fraîcheur ;
- volume minimal ;
- droits ;
- code env ;
- variables obligatoires.

Cela évite de lancer un build long voué à l’échec.

---

## 22.2 Dataset de quarantaine standard

Isoler les erreurs permet de :

- préserver le pipeline ;
- mesurer la qualité ;
- fournir des exemples au métier ;
- accélérer correction source ;
- éviter suppression silencieuse.

---

## 22.3 Naming orienté support

Inclure domaine, objet et rôle de l’étape dans le nom réduit fortement le temps de diagnostic.

---

## 22.4 Variables centralisées

Centraliser les paramètres :

- connexions ;
- dates ;
- seuils ;
- run modes ;
- chemins ;
- SLA.

---

## 22.5 Scenarios découpés

Un scenario unique géant est difficile à diagnostiquer. Découper par responsabilité permet :

- rerun ciblé ;
- logs plus clairs ;
- alerting spécifique ;
- ownership plus précis.

---

## 22.6 Monitoring des temps par étape

Le suivi des durées par step identifie :

- goulots ;
- régressions ;
- lenteurs sources ;
- dérives volumes ;
- coûts anormaux.

---

## 22.7 Checklist release Dataiku

Une checklist de release évite :

- oubli de variables PROD ;
- triggers non actifs ;
- reporters absents ;
- bundle incorrect ;
- code env manquant ;
- connexion mal mappée ;
- checks non exécutés.

---

## 22.8 Flow pruning

Supprimer régulièrement :

- datasets temporaires ;
- recipes abandonnées ;
- notebooks obsolètes ;
- scenarios désactivés ;
- branches expérimentales ;
- variables inutilisées.

---

## 22.9 Project template

Créer un template corporate de projet avec :

- structure de flow ;
- scenarios standard ;
- variables standard ;
- documentation ;
- datasets audit ;
- checks qualité ;
- reporters ;
- runbook.

---

## 22.10 Quality score

Créer un score qualité par projet :

```text
Project Quality Score =
ownership completeness
+ checks coverage
+ documentation coverage
+ scenario success rate
+ freshness compliance
+ security compliance
```

---

## 23. Mode opératoire standard

## 23.1 SOP — Création d’un pipeline Dataiku

1. Cadrer besoin métier.
2. Définir outputs attendus.
3. Identifier SLA.
4. Identifier sources.
5. Identifier contraintes sécurité.
6. Définir data contract.
7. Concevoir flow cible.
8. Définir couches raw/staging/curated/serving.
9. Définir conventions de nommage.
10. Créer datasets sources.
11. Créer recipes.
12. Ajouter checks qualité.
13. Créer quarantine datasets.
14. Créer scenario principal.
15. Ajouter pre-checks.
16. Ajouter post-checks.
17. Ajouter reporters.
18. Tester sur données représentatives.
19. Valider métier.
20. Préparer bundle.
21. Déployer en Automation Node.
22. Lancer smoke test.
23. Activer triggers.
24. Monitorer première exécution.
25. Documenter.

---

## 23.2 SOP — Mise en production

1. Vérifier readiness checklist.
2. Créer bundle versionné.
3. Rédiger changelog.
4. Exécuter test scenario.
5. Valider variables.
6. Valider connexions.
7. Valider code env.
8. Valider permissions.
9. Publier bundle.
10. Déployer via Project Deployer.
11. Activer bundle.
12. Lancer smoke test.
13. Vérifier logs.
14. Vérifier outputs.
15. Vérifier reporters.
16. Activer triggers.
17. Communiquer release.
18. Monitorer post-release.

---

## 23.3 SOP — Incident pipeline

1. Identifier scenario et run.
2. Identifier étape échouée.
3. Lire logs.
4. Vérifier source.
5. Vérifier connexions.
6. Vérifier code env.
7. Vérifier schema drift.
8. Vérifier checks qualité.
9. Vérifier volumes.
10. Évaluer impact métier.
11. Stopper downstream si nécessaire.
12. Relancer si idempotent.
13. Appliquer correction.
14. Informer parties prenantes.
15. Documenter incident.
16. Ajouter prévention.

---

## 23.4 SOP — Reprocessing

1. Définir périmètre à retraiter.
2. Valider impact.
3. Désactiver triggers si nécessaire.
4. Créer backup ou snapshot si nécessaire.
5. Fixer paramètres de run.
6. Lancer scenario reprocess.
7. Vérifier volumes.
8. Vérifier checks qualité.
9. Vérifier outputs.
10. Réactiver triggers.
11. Communiquer fin.
12. Documenter.

---

## 23.5 SOP — Revue trimestrielle

1. Revoir ownership.
2. Revoir permissions.
3. Revoir connexions.
4. Revoir scenarios actifs.
5. Revoir failures.
6. Revoir SLA.
7. Revoir checks qualité.
8. Revoir branches mortes.
9. Revoir objets sandbox.
10. Revoir code env.
11. Revoir performance.
12. Revoir coûts.
13. Revoir documentation.
14. Mettre à jour plan d’amélioration.

---

## 24. Gouvernance documentaire

## 24.1 Révision du standard

Ce standard doit être revu :

- trimestriellement ;
- après incident majeur ;
- après régression pipeline ;
- après incident sécurité ;
- après évolution Dataiku majeure ;
- après changement d’architecture ;
- après adoption d’un nouveau mode de déploiement ;
- après feedback récurrent des équipes.

---

## 24.2 Documentation obligatoire par projet

```markdown
# Dataiku Project Documentation

## Project

Project key:
Project name:
Domain:
Environment:

## Objective

Business objective:

## Owners

Business owner:
Technical owner:
Backup:
Data steward:

## SLA

Freshness:
Runtime:
Success rate:
Support hours:

## Flow overview

Raw:
Staging:
Curated:
Serving:

## Critical datasets

| Dataset | Layer | Grain | Owner | SLA |
|---|---|---|---|---|

## Scenarios

| Scenario | Trigger | SLA | Reporter | Runbook |
|---|---|---|---|---|

## Quality checks

| Dataset | Check | Severity | Action |
|---|---|---|---|

## Connections

| Connection | Environment | Usage | Owner |
|---|---|---|---|

## Security

Sensitive data:
Groups:
Restrictions:

## Deployment

Bundle:
Automation node:
Rollback:

## Incident runbook

Link:

## Changelog

Link:
```

---

## 24.3 Registre des dérogations

```markdown
# Dérogation Dataiku

## Projet concerné

Project key:

## Règle concernée

Règle du standard:

## Motif

Pourquoi la dérogation est nécessaire:

## Impact

- Qualité:
- Sécurité:
- Performance:
- Coût:
- Exploitation:

## Durée de validité

Date de fin:

## Plan de remédiation

Actions:

## Approbateur

Nom / rôle:

## Date de revue

Date:
```

---

## 24.4 Post-mortem incident

```markdown
# Post-mortem Dataiku Pipeline

## Résumé

Project:
Scenario:
Date:
Sévérité:
Impact:

## Timeline

- Detection:
- First response:
- Mitigation:
- Recovery:

## Cause racine

Description:

## Facteurs contributifs

- Source:
- Schema:
- Code:
- Connexion:
- Permissions:
- Scenario:
- Infrastructure:
- Monitoring:

## Impact métier

Description:

## Ce qui a fonctionné

Points positifs:

## Ce qui n’a pas fonctionné

Points faibles:

## Actions correctives

| Action | Owner | Deadline | Statut |
|---|---|---|---|

## Mise à jour standard requise

Oui/Non:
```

---

## 25. Annexes

## 25.1 Template minimal de pipeline Dataiku

```yaml
project_key: SALES_PIPELINE
project_name: Sales Daily Pipeline
domain: SALES
environment: PROD

owners:
  technical: data.engineering@company.com
  business: sales.ops@company.com
  data_steward: sales.data.steward@company.com
  support: data-support@company.com

sla:
  freshness: "J+0 07:00 CET"
  runtime_max_minutes: 90
  success_rate: ">= 99%"
  support_hours: "Business hours"

flow_layers:
  raw:
    - raw_sales_orders
  staging:
    - stg_sales_orders_typed
  curated:
    - cur_sales_orders_validated
  serving:
    - srv_sales_daily_kpi

quality_checks:
  - dataset: stg_sales_orders_typed
    rule: "order_id not null"
    severity: critical
    action: stop_pipeline
  - dataset: cur_sales_orders_validated
    rule: "transaction_id unique"
    severity: critical
    action: stop_pipeline
  - dataset: srv_sales_daily_kpi
    rule: "max_sales_date >= current_date - 1"
    severity: critical
    action: alert_and_stop_publication
  - dataset: cur_sales_orders_validated
    rule: "amount >= 0"
    severity: critical
    action: quarantine_invalid_rows

orchestration:
  main_scenario: sc_daily_sales_build
  precheck_scenario: sc_daily_sales_precheck
  quality_scenario: sc_daily_sales_quality
  retries: 2
  timeout_minutes: 90
  trigger: "daily 06:00 CET"
  reporter_channel: "data-platform-alerts"

security:
  pii_present: false
  restricted_groups:
    - grp_data_sales_engineering
    - grp_sales_ops_viewers
  connections:
    input: snowflake_sales_prd
    output: snowflake_mart_prd

deployment:
  design_node: dss-design
  automation_node: dss-automation-prd
  bundle_version: v1.0.0
  rollback_bundle: v0.9.0
```

---

## 25.2 Template scenario standard

```text
Scenario: sc_daily_<domain>_<pipeline>_build

Step 1 — Initialize
- Set run_id
- Validate variables
- Log start

Step 2 — Pre-check
- Check source availability
- Check schema
- Check freshness
- Check minimum volume

Step 3 — Build staging
- Build raw/staging datasets

Step 4 — Staging checks
- Run metrics
- Run checks
- Stop if critical failure

Step 5 — Build curated
- Build curated datasets

Step 6 — Curated checks
- Nulls
- Duplicates
- Referential integrity
- Reconciliation

Step 7 — Build serving
- Build BI/API/ML outputs

Step 8 — Publish
- Activate downstream outputs if checks OK

Step 9 — Report
- Send success/failure notification

Step 10 — Audit
- Write run status
```

---

## 25.3 Template runbook

```markdown
# Runbook — Dataiku Pipeline

## Pipeline

Project:
Scenario:
Environment:
Owner:
SLA:

## Common failures

### Source unavailable

Symptoms:
Actions:
Escalation:

### Schema drift

Symptoms:
Actions:
Escalation:

### Data quality failure

Symptoms:
Actions:
Escalation:

### Code environment failure

Symptoms:
Actions:
Escalation:

### Permission error

Symptoms:
Actions:
Escalation:

## Rerun procedure

1.
2.
3.

## Rollback procedure

1.
2.
3.

## Contacts

Business:
Technical:
Platform:
Security:

## Links

Project:
Scenario logs:
Monitoring dashboard:
Documentation:
```

---

## 25.4 Template release checklist

```markdown
# Release Checklist — Dataiku

## Bundle

- [ ] Bundle created
- [ ] Version number assigned
- [ ] Changelog updated
- [ ] Breaking changes documented

## Tests

- [ ] Test scenario passed
- [ ] Data quality checks passed
- [ ] UAT completed
- [ ] Smoke test plan ready

## Configuration

- [ ] Variables validated
- [ ] Connections mapped
- [ ] Code environment available
- [ ] Permissions checked
- [ ] Reporters configured
- [ ] Triggers reviewed

## Security

- [ ] No clear credentials
- [ ] Sensitive data protected
- [ ] Groups validated
- [ ] Exports controlled

## Operations

- [ ] Runbook updated
- [ ] Monitoring ready
- [ ] Alerting tested
- [ ] Rollback bundle identified
- [ ] Support informed
```

---

## 25.5 Template monitoring dataset

```text
dataiku_pipeline_monitoring
- project_key
- scenario_name
- run_id
- environment
- bundle_version
- started_at
- ended_at
- duration_seconds
- status
- failed_step
- trigger_type
- rows_processed
- rows_rejected
- data_quality_status
- freshness_status
- sla_status
- owner
- error_message
```

---

## 25.6 Template data quality rules

```yaml
dataset: cur_sales_orders_validated
owner: sales.data.steward@company.com
rules:
  - name: order_id_not_null
    type: not_null
    column: order_id
    severity: critical
    action: stop_pipeline

  - name: transaction_id_unique
    type: uniqueness
    columns:
      - transaction_id
    severity: critical
    action: stop_pipeline

  - name: amount_positive
    type: accepted_range
    column: amount_eur
    condition: "amount_eur >= 0"
    severity: critical
    action: quarantine

  - name: max_order_date_freshness
    type: freshness
    column: order_date
    threshold: ">= current_date - 1"
    severity: critical
    action: alert_and_stop_publication
```

---

## 25.7 Template permissions matrix

```markdown
# Permissions Matrix — Dataiku Project

| Group | Project permission | Connections | Datasets | Scenarios | Notes |
|---|---|---|---|---|---|
| grp_data_engineer_sales | Write project content | sales_dev, sales_prd controlled | read/write | run/edit | Engineering team |
| grp_sales_ops_viewers | Read dashboards | none | read serving only | none | Business users |
| grp_prod_operator | Limited ops | prod connections via service | read logs | run scenarios | Production support |
| grp_platform_admin | Admin | all managed | admin | admin | Platform team |
```

---

## 25.8 Confidentialité

Certaines données, exemples, noms de projets, noms de connexions, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, aux règles de gouvernance, aux politiques de sécurité, aux licences Dataiku, aux capacités activées dans l’instance et aux besoins métier de l’organisation.

---

## 25.9 Résumé exécutif du standard

Un pipeline Dataiku corporate doit être :

- orienté métier ;
- lisible dans le flow ;
- structuré par couches ;
- testé ;
- sécurisé ;
- documenté ;
- observable ;
- idempotent ;
- relançable ;
- performant ;
- versionné ;
- déployé par bundle ;
- monitoré en production ;
- maintenable dans le temps.
La qualité d’un pipeline Dataiku ne se mesure pas seulement au fait qu’il exécute une transformation. Elle se mesure à sa capacité à produire durablement des données fiables, sécurisées, contrôlées, explicables et exploitables par l’organisation.
