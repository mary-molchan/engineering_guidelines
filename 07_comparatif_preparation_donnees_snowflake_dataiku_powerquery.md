# Standard Corporate — Répartition de la préparation des données entre Snowflake, Dataiku et Power Query avant reporting Power BI

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Répartition des responsabilités de préparation des données entre Snowflake, Dataiku et Power Query avant reporting Power BI |
| **Version** | 2.0 |
| **Date** | 2026-06-12 |
| **Auteur / Rôle** | Équipe Data Engineering, Analytics Engineering & BI Governance |
| **Validation / Approbation** | Lead Data Engineer, Data Architect, Head of BI, Data Governance Officer, Platform Owner Snowflake, Platform Owner Dataiku |
| **Public cible** | Data Engineers, Analytics Engineers, BI Developers, Data Analysts avancés, Data Architects, Product Owners Data, Data Stewards |
| **Commanditaire** | Direction Data & Performance / DSI / Direction Métier |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Tous les projets Power BI consommant des données préparées via Snowflake, Dataiku DSS, Power Query, Dataflows ou semantic models Power BI |
| **Objectif du document** | Définir une répartition claire, gouvernée et industrialisable des transformations entre Snowflake, Dataiku et Power Query afin de maximiser performance, fiabilité, gouvernance, maintenabilité, réutilisation et qualité des modèles Power BI. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les règles s’appliquent par défaut à tout projet de reporting Power BI utilisant une ou plusieurs couches de préparation de données en amont.

Sont concernés :

- pipelines Snowflake ;
- pipelines Dataiku DSS ;
- Power Query dans Power BI Desktop ;
- Power Query dans Dataflows / Dataflows Gen2 si applicable ;
- semantic models Power BI ;
- datasets de reporting ;
- vues contractuelles ;
- tables certifiées ;
- data products analytiques ;
- flux d’alimentation BI ;
- chaînes d’industrialisation DEV / TEST / PROD.

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

Les règles sont rédigées selon une logique :

- architecture data ;
- performance BI ;
- gouvernance des transformations ;
- maintenabilité ;
- industrialisation ;
- réduction de la dette technique ;
- séparation des responsabilités ;
- fiabilité des KPI ;
- sécurité ;
- qualité des données ;
- expérience utilisateur Power BI.

---

## 1.4 Précedence

En cas de conflit, l’ordre de priorité est :

1. obligations légales, réglementaires et contractuelles ;
2. politiques sécurité, confidentialité et RGPD ;
3. standards Data Governance ;
4. standards Data Architecture ;
5. standards Snowflake corporate ;
6. standards Dataiku corporate ;
7. standards Power BI corporate ;
8. présent standard de répartition des responsabilités ;
9. pratiques locales d’équipe.

---

## 1.5 Sources officielles de référence

Ce document s’appuie sur les documentations officielles suivantes :

### Snowflake

- Snowflake Documentation: https://docs.snowflake.com/
- Dynamic Tables: https://docs.snowflake.com/en/user-guide/dynamic-tables/overview
- Decision guide for Dynamic Tables: https://docs.snowflake.com/en/user-guide/dynamic-tables/decision-guide
- Streams and Tasks: https://docs.snowflake.com/en/user-guide/data-pipelines-intro
- Query Performance Optimization: https://docs.snowflake.com/en/user-guide/performance-query-options
- Micro-partitions and Clustering: https://docs.snowflake.com/en/user-guide/tables-clustering-micropartitions
- Access Control: https://docs.snowflake.com/en/user-guide/security-access-control-overview
- Dynamic Data Masking: https://docs.snowflake.com/en/user-guide/security-column-ddm-intro
- Row Access Policies: https://docs.snowflake.com/en/user-guide/security-row-intro
- Account Usage: https://docs.snowflake.com/en/sql-reference/account-usage

### Dataiku DSS

- Dataiku DSS Documentation: https://doc.dataiku.com/dss/latest/
- The Flow: https://doc.dataiku.com/dss/latest/flow/index.html
- Visual Recipes: https://doc.dataiku.com/dss/latest/other_recipes/index.html
- Automation Scenarios: https://doc.dataiku.com/dss/latest/scenarios/index.html
- Scenario Steps: https://doc.dataiku.com/dss/latest/scenarios/steps.html
- Scenario Triggers: https://doc.dataiku.com/dss/latest/scenarios/triggers.html
- Scenario Reporters: https://doc.dataiku.com/dss/latest/scenarios/reporters.html
- Metrics, Checks and Data Quality: https://doc.dataiku.com/dss/latest/metrics-check-data-quality/index.html
- Data Quality Rules: https://doc.dataiku.com/dss/latest/metrics-check-data-quality/data-quality-rules.html
- Production Deployments and Bundles: https://doc.dataiku.com/dss/latest/deployment/index.html
- Project Deployer: https://doc.dataiku.com/dss/latest/deployment/deploying-bundles.html

### Microsoft Power BI / Power Query

- Power BI Documentation: https://learn.microsoft.com/power-bi/
- Power Query Folding Guidance: https://learn.microsoft.com/power-bi/guidance/power-query-folding
- Query Folding Examples: https://learn.microsoft.com/power-query/query-folding-examples
- Incremental Refresh Overview: https://learn.microsoft.com/power-bi/connect-data/incremental-refresh-overview
- Configure Incremental Refresh: https://learn.microsoft.com/power-bi/connect-data/incremental-refresh-configure
- Star Schema Guidance: https://learn.microsoft.com/power-bi/guidance/star-schema
- Power BI Semantic Models: https://learn.microsoft.com/fabric/data-warehouse/semantic-models
- Dataflows Refresh Optimization: https://learn.microsoft.com/power-bi/transform-model/dataflows/dataflows-understand-optimize-refresh

---

## 1.6 Définitions clés

| Terme | Définition |
|---|---|
| **Snowflake** | Plateforme data cloud utilisée pour stockage, calcul SQL, transformation ELT, gouvernance, partage et exposition de datasets certifiés. |
| **Dataiku DSS** | Plateforme de conception, orchestration, préparation, data quality, data science et déploiement de pipelines data. |
| **Power Query** | Moteur de préparation de données intégré à Power BI Desktop, Power BI dataflows et autres produits Microsoft. |
| **Power BI semantic model** | Modèle analytique Power BI contenant tables, relations, mesures DAX, sécurité, hiérarchies et logique de consommation. |
| **Query folding** | Capacité de Power Query à traduire ses transformations en requête envoyée à la source, afin d’éviter un traitement local coûteux. |
| **Pushdown** | Exécution d’une transformation au plus près de la source ou du moteur de calcul, par exemple dans Snowflake. |
| **Data contract** | Contrat décrivant schéma, types, nullabilité, grain, qualité, SLA, owner, règles d’évolution et sécurité d’un dataset. |
| **Certified dataset** | Table, vue ou semantic model validé comme source fiable et réutilisable. |
| **Last-mile transformation** | Transformation légère spécifique au rapport ou à l’expérience utilisateur finale. |
| **Business-critical logic** | Règle métier qui impacte un KPI, une décision, un reporting officiel ou une réconciliation. |
| **Presentation logic** | Renommage, formatage, typage final ou ajustement d’affichage qui ne modifie pas le sens métier des données. |
| **Quality gate** | Point de contrôle bloquant ou alertant avant propagation des données. |
| **Serving layer** | Couche de données préparée pour consommation BI, API, exports ou downstream analytics. |
| **Source of truth** | Niveau de vérité officiel d’une donnée ou définition KPI. |
| **Semantic drift** | Écart progressif de définition métier entre couches ou rapports. |
| **Schema drift** | Évolution non contrôlée de structure : ajout, suppression, renommage ou changement de type de colonne. |
| **Grain** | Niveau de détail d’une table ou dataset : une ligne par contrat, client-mois, transaction, produit-jour, etc. |
| **Incremental refresh** | Actualisation ne rechargeant qu’une fenêtre de données définie, généralement par date ou partition. |
| **Certified Power BI-ready dataset** | Dataset amont préparé avec grain, clés, types, dates, qualité et documentation adaptés à la modélisation Power BI. |

---

## 2. Objectif et problème adressé

## 2.1 Objectif

Comparer et standardiser trois lieux possibles de préparation des données avant la construction d’un rapport Power BI :

1. **Snowflake**, côté plateforme data et base de données.
2. **Dataiku DSS**, côté pipeline analytique, orchestration et qualité.
3. **Power Query**, côté dernière préparation dans Power BI.

Le but est de définir une répartition claire des traitements pour maximiser :

- performance ;
- fiabilité ;
- gouvernance ;
- maintenabilité ;
- sécurité ;
- réutilisation ;
- cohérence des KPI ;
- rapidité de delivery ;
- qualité du modèle Power BI.

---

## 2.2 Problème courant

Dans de nombreuses organisations, les transformations sont dispersées entre :

- SQL Snowflake ;
- recipes Dataiku ;
- scripts Python ;
- Power Query M ;
- mesures DAX ;
- transformations manuelles dans fichiers Excel ;
- filtres cachés dans rapports ;
- datasets intermédiaires non documentés.

Cette dispersion crée :

- incohérences entre rapports ;
- duplication de logique ;
- refresh Power BI lents ;
- rupture de query folding ;
- baisse de performance ;
- explosion des coûts ;
- difficulté de debug ;
- perte de confiance métier ;
- dette technique ;
- problèmes de sécurité ;
- difficultés de certification.

---

## 2.3 Principe cible

Le principe directeur est :

```text
Pushdown-first, orchestration-second, presentation-last
```

Cela signifie :

1. **Snowflake** traite les transformations lourdes, critiques, réutilisables et gouvernées.
2. **Dataiku** orchestre, contrôle, documente, monitorise et industrialise les flux.
3. **Power Query** réalise uniquement les ajustements légers de consommation spécifiques au rapport.

---

## 3. Vision cible

## 3.1 Vision

La préparation des données avant Power BI doit être organisée comme une chaîne de valeur industrielle :

```text
Sources opérationnelles
        ↓
Ingestion / Raw
        ↓
Snowflake — transformations structurantes, historisation, tables certifiées
        ↓
Dataiku — orchestration, contrôles qualité, publication, monitoring
        ↓
Power Query — last-mile léger, compatible query folding
        ↓
Power BI semantic model — modèle en étoile, mesures DAX, sécurité
        ↓
Rapports Power BI — visualisation et décision
```

---

## 3.2 Résultat attendu

Chaque rapport Power BI corporate doit consommer des données :

- au bon grain ;
- avec types normalisés ;
- avec clés stables ;
- avec définitions métier validées ;
- avec qualité contrôlée ;
- avec fraîcheur connue ;
- avec logique critique centralisée ;
- avec transformation locale limitée ;
- avec documentation accessible ;
- avec sécurité appliquée ;
- avec performance de refresh maîtrisée.

---

## 3.3 Indicateurs de succès

| Indicateur | Cible recommandée |
|---|---|
| Transformations lourdes en Power Query | 0 sur datasets volumineux |
| KPI critiques définis dans plusieurs couches | 0 |
| Datasets BI sans grain documenté | 0 |
| Datasets Power BI sans source certifiée disponible | 0 pour reporting corporate |
| Ruptures de query folding non documentées | 0 sur rapports critiques |
| Refresh full alors qu’incrémental possible | 0 hors exception |
| Dataiku flows critiques sans quality gates | 0 |
| Tables Snowflake de reporting sans tests SQL | 0 |
| Rapports Power BI avec logique métier majeure en M | 0 |
| Duplications de règles entre rapports | Réduction continue |
| Temps de refresh Power BI | Conforme SLA |
| Incidents liés à divergence KPI | 0 critique |

---

## 4. Analyse détaillée par outil

# 4.1 Snowflake — préparation en base

## 4.1.1 Rôle principal

Snowflake doit être le lieu prioritaire pour :

- transformations lourdes ;
- jointures volumineuses ;
- déduplication ;
- normalisation ;
- historisation ;
- snapshots ;
- agrégations structurantes ;
- calculs métier réutilisables ;
- production de tables ou vues certifiées ;
- gouvernance des données sensibles ;
- exposition de datasets Power BI-ready.

Snowflake est la couche de **robustesse, performance, gouvernance et réutilisation**.

---

## 4.1.2 Points forts

| Point fort | Explication |
|---|---|
| Scalabilité compute/stockage | Adaptée aux grands volumes et transformations lourdes. |
| Exécution SQL massivement parallèle | Pertinente pour joins, agrégations, MERGE, transformations ELT. |
| Gouvernance centralisée | RBAC, masking policies, row access policies, tags, classification. |
| Réutilisation inter-équipes | Tables/vues certifiées consommables par plusieurs rapports et domaines. |
| Optimisation incrémentale | MERGE, streams/tasks, dynamic tables, partitions logiques, fenêtres de recalcul. |
| Observabilité | Query history, warehouse metering, access history, monitoring de coûts. |
| Sécurité | Contrôle fin des rôles, accès, masquage et partage. |
| Data sharing | Partage sécurisé de données sans copie si besoin. |
| CI/CD SQL | Scripts versionnés et testables. |
| Performance BI | Réduit le volume et la complexité importés dans Power BI. |

---

## 4.1.3 Limites

| Limite | Implication |
|---|---|
| Moins adapté aux itérations no-code rapides | Les profils non SQL peuvent avoir besoin de Dataiku ou Power Query pour prototyper. |
| Gouvernance plus formelle | Les changements nécessitent revue, tests, promotion. |
| Risque de sur-engineering | Tout ne doit pas être industrialisé en base. |
| Coût compute à maîtriser | Warehouses, requêtes, materialization et optimisations doivent être monitorés. |
| Logique trop présentationnelle non pertinente | Renommages finaux, libellés report-only et formats visuels doivent rester proches de Power BI. |
| Forte dépendance aux standards SQL | Sans conventions, Snowflake peut devenir une couche difficile à maintenir. |

---

## 4.1.4 Contraintes

Snowflake nécessite :

- standards de nommage ;
- grain documenté ;
- clés stables ;
- tests SQL ;
- data contracts ;
- CI/CD ;
- ownership ;
- monitoring coûts ;
- gestion des warehouses ;
- stratégie incrémentale ;
- gestion schema drift ;
- documentation métier ;
- politiques de sécurité ;
- vues contractuelles stables.

---

## 4.1.5 Risques fréquents

| Risque | Effet |
|---|---|
| Sur-transformation | Trop d’objets et logique inutile en amont. |
| Multiplication de tables intermédiaires | Dette technique et coûts storage. |
| Mauvais grain | Cardinalités fausses dans Power BI. |
| KPI non testés | Régressions silencieuses. |
| MERGE non idempotent | Doublons ou pertes de données. |
| Absence de tests de cardinalité | Joins N-N non détectés. |
| Refresh Power BI non aligné avec partitions | Temps de refresh excessif. |
| Sécurité appliquée trop tard | Exposition de données sensibles. |
| Requêtes coûteuses non monitorées | Dérapage crédits. |
| Views instables | Ruptures de rapports aval. |

---

## 4.1.6 Cas d’usage cibles

Snowflake est prioritaire pour :

- consolidation multi-sources ;
- typage robuste ;
- déduplication volumineuse ;
- normalisation de référentiels ;
- historisation SCD ;
- snapshots ;
- calculs de KPI réutilisables ;
- tables de faits et dimensions ;
- agrégations lourdes ;
- incremental loads ;
- MERGE / UPSERT ;
- tables de reporting certifiées ;
- vues sécurisées ;
- data masking ;
- row-level access ;
- datasets multi-rapports ;
- préparation d’incremental refresh Power BI ;
- optimisation de refresh ;
- logs et audit techniques.

---

## 4.1.7 Règles obligatoires Snowflake pour Power BI

Snowflake MUST fournir aux rapports corporate :

- un grain explicite ;
- des clés stables ;
- des types normalisés ;
- des dates compatibles incremental refresh ;
- des colonnes métier documentées ;
- des règles KPI critiques centralisées si réutilisables ;
- des contrôles qualité ;
- des tests de non-régression ;
- un owner ;
- un SLA ;
- des vues/tables stables ;
- des politiques sécurité appliquées ;
- une stratégie de changement de schéma ;
- des performances compatibles avec Power BI.

---

# 4.2 Dataiku DSS — préparation dans le pipeline analytique

## 4.2.1 Rôle principal

Dataiku doit être utilisé pour :

- orchestrer les flux ;
- rendre les pipelines lisibles ;
- coordonner plusieurs sources et moteurs ;
- appliquer des quality gates ;
- industrialiser des traitements visuels et code ;
- gérer scenarios, triggers et alertes ;
- publier des outputs contrôlés ;
- faciliter la collaboration entre data engineers, analysts et data scientists ;
- opérer la chaîne de préparation jusqu’à la couche serving.

Dataiku est la couche de **productivité, orchestration, qualité, traçabilité et opérationnalisation**.

---

## 4.2.2 Points forts

| Point fort | Explication |
|---|---|
| Flow visuel | Permet de comprendre dépendances et lineage. |
| Productivité no-code/code | Adapté aux équipes mixtes. |
| Scenarios | Orchestration, triggers, retries, notifications. |
| Metrics/checks | Contrôles qualité intégrés aux objets du flow. |
| Data Quality Rules | Expression et reporting de règles de qualité. |
| Bundles | Promotion Design → Automation Node. |
| Collaboration | Documentation, partage, ownership. |
| Pushdown | Peut déléguer transformations lourdes à Snowflake. |
| Monitoring | Historique des jobs/scenarios, alertes et runs. |
| ML/AI integration | Utile pour feature engineering, scoring et workflows ML. |

---

## 4.2.3 Limites

| Limite | Implication |
|---|---|
| Performance dépend du moteur | Mauvais choix engine = lenteur/coût. |
| Couche supplémentaire à opérer | Permissions, connexions, code env, scenarios, bundles. |
| Risque de dispersion logique | Règles métier cachées dans recipes visuelles ou scripts. |
| Flows illisibles si non structurés | Dette visuelle et opérationnelle. |
| Datasets intermédiaires nombreux | Coût stockage et complexité. |
| Promotion à gouverner | Bundles, variables, connexions et Automation Node doivent être maîtrisés. |

---

## 4.2.4 Contraintes

Dataiku nécessite :

- conventions projet ;
- conventions flow ;
- nommage datasets/recipes/scenarios ;
- séparation exploration/production ;
- data contracts ;
- checks qualité ;
- scenarios idempotents ;
- reporters ;
- runbooks ;
- promotion par bundles ;
- mapping connexions par environnement ;
- gestion des credentials ;
- monitoring de production ;
- revues périodiques ;
- nettoyage des objets orphelins.

---

## 4.2.5 Risques fréquents

| Risque | Effet |
|---|---|
| Traitements lourds exécutés hors Snowflake | Dégradation performance et coût. |
| Duplication logique Snowflake/Dataiku | Divergence de KPI. |
| Recipes dupliquées | Dette technique. |
| Flow trop grand | Debug difficile. |
| Scenarios sans quality gates | Incidents tardifs dans Power BI. |
| Triggers concurrents | Builds incohérents. |
| Variables non gouvernées | Comportements différents entre environnements. |
| Bundles non versionnés | Production non reproductible. |
| Credentials dans code | Risque sécurité. |
| Outputs publiés sans validation métier | Perte de confiance. |

---

## 4.2.6 Cas d’usage cibles

Dataiku est pertinent pour :

- orchestration de pipelines ;
- coordination multi-sources ;
- exécution de quality gates ;
- publication de datasets certifiés ;
- monitoring de fraîcheur ;
- alerting ;
- préparation collaborative ;
- prototypage industrialisable ;
- enrichissements intermédiaires ;
- workflows combinant SQL et Python ;
- reprocessing contrôlé ;
- validation métier ;
- pipelines ML/scoring ;
- exports contrôlés ;
- automatisation de tests avant Power BI.

---

## 4.2.7 Règles obligatoires Dataiku pour Power BI

Dataiku MUST :

- privilégier pushdown Snowflake pour traitements lourds ;
- documenter le rôle de chaque flow ;
- structurer raw/staging/curated/serving ;
- ajouter quality gates avant publication Power BI ;
- historiser résultats de checks critiques ;
- déclencher alertes sur échec et retard SLA ;
- promouvoir via bundles versionnés ;
- séparer DEV/TEST/PROD ;
- sécuriser connexions et credentials ;
- éviter duplication de logique métier Snowflake ;
- documenter variables et triggers ;
- fournir runbook pour pipelines critiques.

---

# 4.3 Power Query — préparation dans Power BI

## 4.3.1 Rôle principal

Power Query doit être réservé à la dernière couche de préparation, proche du rapport et du semantic model Power BI.

Power Query est la couche de **last-mile BI preparation**, c’est-à-dire :

- renommage final ;
- typage final léger ;
- sélection de colonnes ;
- filtres de consommation ;
- paramètres d’import ;
- harmonisation de libellés pour le modèle ;
- petites colonnes dérivées non critiques ;
- adaptation spécifique au rapport.

Power Query ne doit pas devenir la couche principale de transformation métier corporate.

---

## 4.3.2 Points forts

| Point fort | Explication |
|---|---|
| Rapidité de prototypage | Très adapté aux ajustements rapides. |
| Ergonomie analyste | Accessible aux BI Developers et Data Analysts. |
| Intégration Power BI | Directement lié au semantic model. |
| Paramètres RangeStart/RangeEnd | Support incremental refresh. |
| Query folding | Peut déléguer transformations à la source si compatible. |
| Ajustements locaux | Utile pour rapport spécifique. |
| Typage final | Permet d’assurer cohérence modèle. |
| Renommage business-friendly | Améliore lisibilité utilisateur. |

---

## 4.3.3 Limites

| Limite | Implication |
|---|---|
| Mauvais choix pour gros volumes | Refresh lent, mémoire, risque timeout. |
| Maintenabilité faible si logique complexe | M difficile à relire et réutiliser. |
| Réutilisation limitée | Logique souvent dupliquée entre PBIX. |
| Debug moins industriel | Moins adapté aux contrôles de production. |
| Risque rupture query folding | Transformation exécutée localement. |
| Sécurité tardive | Données sensibles peuvent arriver trop loin. |
| Divergence inter-rapports | Règles répliquées différemment. |

---

## 4.3.4 Contraintes

Power Query nécessite :

- discipline forte ;
- query folding vérifié ;
- transformations limitées ;
- documentation des étapes locales ;
- alignement avec datasets certifiés ;
- paramètres incremental refresh corrects ;
- absence de logique métier critique ;
- contrôle des sources ;
- limitation des colonnes ;
- revue avant publication ;
- mise à jour en cas schema drift.

---

## 4.3.5 Risques fréquents

| Risque | Effet |
|---|---|
| Logique métier critique en M | Divergence de KPI. |
| Transformations non foldables | Refresh lent. |
| Joins volumineux dans Desktop | Mémoire et performance dégradées. |
| Duplications entre PBIX | Maintenance coûteuse. |
| Schema drift non contrôlé | Refresh cassé. |
| Filtres cachés | Mauvaise interprétation métier. |
| Remplacement de NULL arbitraire | KPI faux. |
| Merge/append lourds locaux | Dégradation forte. |
| Connexion directe à raw | Modèle fragile. |
| Absence de documentation | Dette BI. |

---

## 4.3.6 Cas d’usage cibles

Power Query est adapté à :

- renommage final de colonnes ;
- suppression de colonnes non utilisées ;
- typage final ;
- filtres de période paramétrés ;
- paramètres RangeStart/RangeEnd ;
- petites colonnes de présentation ;
- harmonisation de libellés ;
- sélection de tables/vues certifiées ;
- transformations légères propres à un rapport ;
- gestion de paramètres d’environnement ;
- désactivation chargement de tables intermédiaires ;
- vérification locale avant modélisation.

---

## 4.3.7 Règles obligatoires Power Query

Power Query MUST :

- rester léger ;
- préserver query folding lorsque source compatible ;
- documenter toute transformation locale ;
- exclure transformations volumineuses ;
- éviter logique KPI critique ;
- s’appuyer sur données certifiées ;
- limiter les colonnes importées ;
- appliquer typage final explicite ;
- utiliser RangeStart/RangeEnd pour incremental refresh ;
- être revu en cas de rupture de folding ;
- ne pas dupliquer une logique déjà disponible en Snowflake/Dataiku.

---

## 5. Synthèse comparative

## 5.1 Comparaison générale

| Critère | Snowflake | Dataiku | Power Query |
|---|---|---|---|
| Volume de données | Excellent | Bon à très bon si pushdown | Limité à moyen |
| Transformation lourde | Excellent | Bon si moteur adapté | Non recommandé |
| Jointures volumineuses | Excellent | Possible si pushdown | Non recommandé |
| Déduplication massive | Excellent | Possible | Non recommandé |
| Historisation | Excellent | Orchestration possible | Non recommandé |
| Gouvernance enterprise | Excellent | Bon à très bon | Moyen |
| Sécurité données sensibles | Excellent | Bon si permissions/connexions maîtrisées | Limité |
| Observabilité | Excellent via Account Usage | Bon via scenarios/metrics/logs | Faible à moyen |
| Data quality | SQL tests forts | Metrics/checks forts | Limité |
| Réutilisation inter-équipes | Excellent | Bon | Faible à moyen |
| Prototypage rapide | Moyen | Excellent | Excellent |
| Collaboration no-code/code | Moyen | Excellent | Bon pour BI |
| CI/CD | Fort si outillage | Fort via bundles | Limité à PBIP/Git selon contexte |
| Maintenabilité long terme | Excellent si standards | Bon si flows gouvernés | Faible si usage excessif |
| Pertinence KPI critiques | Très forte | Forte si gouvernée | Limitée |
| Last-mile reporting | Faible | Faible à moyen | Excellent |
| Risque de dette si mal utilisé | Objets SQL inutiles | Flows illisibles | M complexe et dupliqué |

---

## 5.2 Matrice de décision rapide

| Transformation | Outil cible | Raison |
|---|---|---|
| Nettoyage volumineux | Snowflake | Performance, pushdown, gouvernance |
| Déduplication massive | Snowflake | SQL scalable, tests de cardinalité |
| Historisation SCD | Snowflake | Stabilité, audit, réutilisation |
| Snapshots périodiques | Snowflake | Stockage et calcul adaptés |
| Quality gates pipeline | Dataiku + Snowflake | Contrôles techniques et opérationnels |
| Orchestration multi-étapes | Dataiku | Scenarios, triggers, reporters |
| Publication dataset validé | Dataiku + Snowflake | Pipeline + couche serving |
| Enrichissement léger rapport | Power Query | Last-mile |
| Renommage final | Power Query | Lisibilité modèle |
| Typage final | Power Query | Intégration Power BI |
| Mesure dynamique selon filtres | DAX | Contexte du semantic model |
| KPI réutilisable de base | Snowflake ou semantic model certifié | Source of truth |
| KPI interactif spécifique rapport | DAX | Dépend du contexte visuel |
| Row-level security source | Snowflake/Power BI selon design | Gouvernance accès |
| Masquage données sensibles | Snowflake en priorité | Security by design |
| Filtre RangeStart/RangeEnd | Power Query | Incremental refresh |
| Transformations non foldables lourdes | Snowflake/Dataiku | À déplacer en amont |

---

## 6. Principe directeur : pushdown-first, orchestration-second, presentation-last

## 6.1 Définition

```text
Pushdown-first
→ Exécuter les transformations lourdes au plus près du moteur data.

Orchestration-second
→ Utiliser Dataiku pour organiser, contrôler, déclencher, monitorer et publier.

Presentation-last
→ Limiter Power Query aux ajustements légers propres au rapport.
```

---

## 6.2 Règles MUST

- Les transformations lourdes MUST être réalisées dans Snowflake ou via pushdown Snowflake.
- Les règles métier critiques réutilisables MUST être centralisées hors Power Query.
- Les quality gates MUST exister avant exposition Power BI.
- Power Query MUST rester une couche légère.
- Le query folding MUST être vérifié pour les rapports critiques.
- Les changements de grain MUST être documentés.
- Les KPI critiques MUST avoir une source de vérité unique.
- Les datasets Power BI corporate SHOULD consommer des vues/tables certifiées.
- Les transformations dupliquées entre rapports MUST être remontées en amont.
- Les datasets sensibles MUST être sécurisés avant Power BI lorsque possible.

---

## 6.3 Décision en 5 questions

Avant d’implémenter une transformation, poser :

1. **Est-elle volumineuse ?**  
   Si oui, Snowflake ou pushdown.

2. **Est-elle réutilisable par plusieurs rapports ?**  
   Si oui, Snowflake ou Dataiku serving layer.

3. **Impacte-t-elle un KPI critique ?**  
   Si oui, éviter Power Query ; privilégier source certifiée ou semantic model.

4. **Est-elle spécifique à un seul rapport ?**  
   Si oui, Power Query peut être acceptable si légère.

5. **Casse-t-elle le query folding ?**  
   Si oui, déplacer en amont ou justifier.

---

## 7. Répartition cible par étape du flux

| Étape du flux | Snowflake | Dataiku | Power Query |
|---|---|---|---|
| Ingestion initiale | Oui | Oui si orchestrateur/connecteur | Non |
| Consolidation multi-sources | Oui | Oui pour orchestration | Non |
| Typage robuste | Oui | Oui | Léger seulement |
| Nettoyage lourd | Oui prioritaire | Possible si pushdown | Non recommandé |
| Déduplication | Oui prioritaire | Possible | Non recommandé |
| Normalisation référentiels | Oui | Oui si workflow collaboratif | Non recommandé |
| Historisation | Oui prioritaire | Orchestration | Non |
| Snapshots | Oui | Orchestration | Non |
| Partitionnement / incrémentalité | Oui | Orchestration | Paramètres RangeStart/RangeEnd |
| Règles métier critiques | Oui prioritaire | Oui si gouverné | Non recommandé |
| KPI de base réutilisables | Oui ou semantic model certifié | Possible pour pipeline | Non |
| Contrôles qualité SQL | Oui | Oui en complément | Limité |
| Quality gates opérationnels | Possible | Oui prioritaire | Non |
| Publication dataset certifié | Oui | Oui | Non |
| Renommage final | Limité | Limité | Oui |
| Typage final rapport | Limité | Limité | Oui |
| Colonnes de présentation | Non | Limité | Oui si légères |
| Mise en forme visuelle | Non | Non | Oui / Power BI model |
| Mesures interactives | Non | Non | DAX |
| Sécurité source | Oui | Oui accès projet | Power BI RLS selon design |
| Monitoring pipeline | Oui queries/cost | Oui scenarios/checks | Refresh history limité |

---

## 8. Architecture cible pour un projet BI corporate

## 8.1 Pattern général

```text
1. Snowflake
   - raw
   - staging
   - curated
   - mart / serving
   - tests SQL
   - masking / row access
   - vues contractuelles

2. Dataiku
   - flow orchestration
   - quality gates
   - scenarios
   - triggers
   - reporters
   - bundles
   - monitoring
   - publication contrôlée

3. Power BI / Power Query
   - connexion à table/vue certifiée
   - filtre incremental refresh
   - sélection colonnes
   - typage final
   - renommage final
   - modèle en étoile
   - DAX
   - rapport
```

---

## 8.2 Pattern recommandé détaillé

```text
SOURCE SYSTEMS
    ↓
SNOWFLAKE RAW
    - ingestion brute
    - metadata source
    - contrôle technique minimal
    ↓
SNOWFLAKE STAGING
    - typage
    - normalisation
    - détection erreurs
    - déduplication technique
    ↓
SNOWFLAKE CURATED
    - entités métier
    - historisation
    - référentiels
    - règles métier stables
    ↓
SNOWFLAKE MART / SERVING
    - faits/dimensions
    - vues contractuelles
    - tables Power BI-ready
    - security policies
    ↓
DATAIKU SCENARIOS
    - build
    - checks
    - quality gates
    - publication
    - alerting
    ↓
POWER QUERY
    - last-mile
    - RangeStart/RangeEnd
    - folding preserved
    ↓
POWER BI SEMANTIC MODEL
    - star schema
    - relationships
    - DAX measures
    - RLS if applicable
    ↓
POWER BI REPORT
    - visualisation
    - storytelling
    - decision support
```

---

## 8.3 Variante avec Dataiku comme orchestrateur principal

```text
Dataiku Scenario
    ├── Pre-check sources
    ├── Execute Snowflake SQL / pushdown recipes
    ├── Run Dataiku metrics & checks
    ├── Publish Snowflake serving tables
    ├── Notify BI refresh readiness
    └── Monitor SLA
```

Dans cette variante, Snowflake reste le moteur principal pour transformations lourdes, tandis que Dataiku pilote l’ordre, les contrôles et les alertes.

---

## 8.4 Variante avec Power BI direct Snowflake

Cette variante est acceptable si :

- la table/vue Snowflake est déjà certifiée ;
- le modèle Power BI est propre ;
- Power Query reste minimal ;
- incremental refresh est aligné avec une colonne date ;
- les volumes sont compatibles ;
- la sécurité est maîtrisée ;
- les refresh sont monitorés.

Pattern :

```text
Snowflake certified view/table
        ↓
Power Query minimal folding-preserved
        ↓
Power BI semantic model
        ↓
Report
```

---

## 9. Snowflake — responsabilités détaillées

## 9.1 Snowflake doit produire des objets Power BI-ready

Un objet Power BI-ready doit avoir :

- grain documenté ;
- clés stables ;
- types normalisés ;
- colonnes utiles ;
- dates explicites ;
- colonne de partition si incremental refresh ;
- aucune PII non nécessaire ;
- noms techniques stables ;
- définitions colonnes ;
- owner ;
- SLA ;
- tests qualité ;
- stratégie schema evolution ;
- performance validée ;
- sécurité appliquée.

---

## 9.2 Couches recommandées

| Couche Snowflake | Rôle |
|---|---|
| `RAW` | Données brutes ou quasi brutes |
| `STAGING` | Typage, nettoyage technique, standardisation |
| `CURATED` | Entités métier harmonisées |
| `MART` | Faits, dimensions, agrégats BI |
| `SECURITY` | Mapping accès, policies |
| `GOVERNANCE` | Tags, data contracts, métadonnées |
| `OPS` | Logs, audit, contrôles |
| `QUARANTINE` | Rejets |

---

## 9.3 Tables et vues certifiées

### Règles

- Les rapports corporate SHOULD consommer des tables/vues certifiées.
- Les vues contractuelles doivent éviter `SELECT *`.
- Les changements breaking doivent être versionnés.
- Les tables mart doivent avoir un grain stable.
- Les colonnes doivent être typées.
- Les champs inutiles doivent être exclus.
- Les clés doivent permettre relations Power BI 1-*.
- Les dates doivent permettre incremental refresh.
- Les agrégations doivent être validées.

---

## 9.4 Exemple de vue contractuelle

```sql
CREATE OR REPLACE SECURE VIEW SALES_PRD.MART.SVW_POWERBI_SALES_DAILY_V1 AS
SELECT
    sales_date,
    product_key,
    customer_segment_key,
    region_key,
    sales_amount_eur,
    margin_amount_eur,
    transaction_count,
    updated_at
FROM SALES_PRD.MART.FACT_SALES_DAILY
WHERE is_valid = TRUE;
```

---

## 9.5 Tests SQL obligatoires

| Test | Exemple |
|---|---|
| Unicité clé | `COUNT(*) GROUP BY key HAVING COUNT(*) > 1` |
| Nullabilité | champs obligatoires non null |
| Bornes métier | montants >= 0 |
| Fraîcheur | max date >= attendu |
| Réconciliation | total source vs total mart |
| Cardinalité | dimension unique avant join |
| Volume | variation vs baseline |
| Referential integrity | foreign keys présentes |
| Schema drift | colonnes attendues |
| KPI regression | avant/après |

---

## 9.6 Incremental readiness pour Power BI

Snowflake doit livrer :

- colonne date stable ;
- typage date/time correct ;
- granularité compatible ;
- historique complet ;
- late arriving data gérée ;
- partitions ou fenêtres logiques ;
- stratégie de recalcul ;
- vues compatibles RangeStart/RangeEnd ;
- éviter fonctions empêchant folding côté Power Query.

---

## 9.7 Sécurité Snowflake

Snowflake doit gérer autant que possible :

- masking policies ;
- row access policies ;
- secure views ;
- tags de classification ;
- RBAC ;
- vues limitées aux colonnes nécessaires ;
- séparation environnements ;
- data sharing gouverné.

Power BI ne doit pas être le premier et unique niveau de protection des données sensibles si celles-ci peuvent être sécurisées en amont.

---

## 10. Dataiku — responsabilités détaillées

## 10.1 Rôle d’orchestration

Dataiku doit orchestrer :

- disponibilité sources ;
- lancement traitements Snowflake ;
- recettes Dataiku ;
- contrôles qualité ;
- publication outputs ;
- alerting ;
- reprocessing ;
- monitoring ;
- promotion bundles.

---

## 10.2 Flow structuré

Un flow Dataiku destiné à Power BI doit être structuré :

```text
Sources
  ↓
Raw datasets
  ↓
Staging datasets
  ↓
Curated datasets
  ↓
Serving Power BI datasets
  ↓
Quality / Audit / Quarantine
```

---

## 10.3 Quality gates Dataiku

### Pre-checks

Avant build :

- source disponible ;
- connexion valide ;
- schéma conforme ;
- volume minimal ;
- fraîcheur source ;
- variables présentes ;
- code env valide.

### Post-checks

Après build :

- row count ;
- nulls ;
- duplicates ;
- referential integrity ;
- KPI reconciliation ;
- freshness ;
- rejected rows ;
- output availability.

---

## 10.4 Scenarios de production

Un scenario Dataiku de production doit inclure :

1. initialisation run context ;
2. pre-checks ;
3. build raw/staging ;
4. checks staging ;
5. build curated ;
6. checks curated ;
7. build serving ;
8. checks serving ;
9. publication ;
10. notification ;
11. audit log.

---

## 10.5 Bundles et promotion

### Règles

- Tout projet production doit être promu par bundle.
- Le bundle doit être versionné.
- Les variables PROD doivent être validées.
- Les connexions PROD doivent être mappées.
- Les scenarios PROD doivent être testés.
- Les reporters doivent être configurés.
- Le rollback doit être possible.
- Les changements doivent être documentés.

---

## 10.6 Dataiku ne doit pas remplacer Snowflake pour tout

Dataiku peut orchestrer Snowflake, mais ne doit pas systématiquement extraire les données hors base.

### Règle

Si une transformation relationnelle lourde peut être exécutée efficacement dans Snowflake, elle SHOULD être poussée vers Snowflake.

---

## 11. Power Query — responsabilités détaillées

## 11.1 Power Query doit rester last-mile

Power Query peut faire :

- choisir la vue/table certifiée ;
- filtrer via paramètres ;
- sélectionner colonnes utiles ;
- appliquer typage final ;
- renommer colonnes pour le modèle ;
- désactiver chargement de requêtes intermédiaires ;
- créer petites colonnes de présentation ;
- gérer paramètres RangeStart/RangeEnd ;
- préserver query folding.

---

## 11.2 Power Query ne doit pas faire

Power Query ne doit pas porter :

- déduplication massive ;
- historisation ;
- joins volumineux ;
- KPI critiques réutilisables ;
- règles métier corporate ;
- nettoyage source complexe ;
- masquage de sécurité ;
- consolidation multi-sources lourde ;
- traitement de fichiers massifs ;
- correction de qualité de données ;
- transformations qui cassent folding sans justification.

---

## 11.3 Query folding

### Règles

- Vérifier folding pour étapes majeures.
- Placer filtres tôt.
- Appliquer RangeStart/RangeEnd correctement.
- Éviter étapes non foldables avant filtres.
- Déplacer en amont toute transformation non foldable lourde.
- Documenter toute rupture de folding.
- Tester refresh dans Power BI Service, pas seulement Desktop.

---

## 11.4 Incremental refresh

Power Query doit :

- créer paramètres `RangeStart` et `RangeEnd` ;
- filtrer la colonne date appropriée ;
- préserver folding ;
- aligner période avec stratégie Snowflake ;
- tester initial refresh ;
- gérer données tardives selon politique ;
- documenter fenêtre historique et fenêtre incrémentale.

---

## 11.5 Exemple de transformation locale acceptable

```text
Source Snowflake certifiée:
SALES_PRD.MART.SVW_POWERBI_SALES_DAILY_V1

Power Query:
- filtrer sales_date avec RangeStart/RangeEnd
- sélectionner colonnes utilisées
- renommer sales_amount_eur -> Sales Amount
- typer Sales Amount en Decimal Number
- désactiver chargement des requêtes intermédiaires
```

---

## 12. Frontière entre SQL, Dataiku, Power Query et DAX

## 12.1 Règle fondamentale

La localisation d’une logique dépend de quatre critères :

1. volume ;
2. réutilisation ;
3. criticité métier ;
4. dépendance au contexte utilisateur.

---

## 12.2 Où placer la logique ?

| Type de logique | Snowflake | Dataiku | Power Query | DAX |
|---|---:|---:|---:|---:|
| Nettoyage source massif | Oui | Orchestration | Non | Non |
| Déduplication | Oui | Possible | Non | Non |
| Historisation | Oui | Orchestration | Non | Non |
| KPI de base réutilisable | Oui | Possible | Non | Possible si semantic model certifié |
| KPI dépendant filtres utilisateur | Non | Non | Non | Oui |
| Renommage final | Non | Limité | Oui | Non |
| Formatage visuel | Non | Non | Limité | Oui / rapport |
| Quality checks | Oui | Oui | Limité | Non |
| RLS source | Oui | Accès projet | Non | Oui selon modèle |
| Calcul row-level simple spécifique rapport | Possible | Possible | Oui si léger | Non |
| Calcul agrégé dynamique | Non | Non | Non | Oui |
| Mapping référentiel stable | Oui | Possible | Non | Non |
| Prototypage | Moyen | Oui | Oui | Oui |
| Industrialisation | Oui | Oui | Non seul | Oui pour semantic model |

---

## 12.3 KPI critiques

### Règle

Un KPI critique doit avoir une seule définition officielle.

### Options acceptables

| Emplacement | Usage |
|---|---|
| Snowflake | KPI de base réutilisable, calcul stable |
| Power BI semantic model / DAX | KPI interactif dépendant du contexte de filtre |
| Dataiku | Pré-calcul ou contrôle de KPI dans pipeline |
| Power Query | Non recommandé pour KPI critique |

### Exemple

```text
Sales Amount:
- agrégation de base validée dans Snowflake
- mesure DAX [Sales Amount] = SUM(FactSales[SalesAmount])
- pas de recalcul métier en Power Query
```

---

## 13. Patterns recommandés

## 13.1 Pattern Corporate BI standard

```text
Snowflake:
- FACT_SALES_DAILY
- DIM_CUSTOMER
- DIM_PRODUCT
- DIM_DATE

Dataiku:
- sc_daily_sales_build
- sc_daily_sales_quality
- quality gates
- alerts

Power Query:
- import FACT/DIM certifiées
- incremental refresh
- typage/renommage final

Power BI:
- star schema
- measures DAX
- RLS if needed
```

---

## 13.2 Pattern prototypage vers industrialisation

```text
Phase 1 — Prototype
Power Query / Dataiku visual recipes

Phase 2 — Validation métier
Dataiku flow + small sample + checks

Phase 3 — Industrialisation
Snowflake SQL + Dataiku orchestration

Phase 4 — Certification
Power BI semantic model + documentation + monitoring
```

### Règle

Un prototype Power Query qui devient récurrent doit être migré vers Dataiku/Snowflake.

---

## 13.3 Pattern qualité renforcée

```text
Snowflake SQL tests
    ↓
Dataiku metrics/checks
    ↓
Power BI refresh only if quality OK
    ↓
Power BI report shows data freshness/status
```

---

## 13.4 Pattern incremental refresh sécurisé

```text
Snowflake:
- table avec date_partition
- stratégie MERGE/reprocessing
- late data handled

Dataiku:
- build partitions
- freshness checks
- SLA alerts

Power Query:
- RangeStart/RangeEnd
- folding preserved

Power BI:
- incremental refresh policy
```

---

## 13.5 Pattern vues contractuelles

```text
Snowflake physical tables
        ↓
Snowflake secure contract views
        ↓
Power Query minimal
        ↓
Semantic model
```

Avantages :

- stabilité schema ;
- sécurité ;
- masquage ;
- limitation colonnes ;
- indépendance backend ;
- réduction casse aval.

---

## 13.6 Pattern Dataiku orchestration + Snowflake compute

```text
Dataiku scenario
    ↓
SQL recipe pushdown Snowflake
    ↓
Dataiku checks
    ↓
Snowflake serving table
    ↓
Power BI refresh
```

---

## 14. Anti-patterns majeurs

## 14.1 Anti-pattern : Power Query comme ETL corporate

### Symptômes

- nombreux merges Power Query ;
- étapes M complexes ;
- refresh très long ;
- logique métier dans PBIX ;
- duplication entre rapports.

### Correction

Migrer la logique vers Snowflake ou Dataiku, puis limiter Power Query au last-mile.

---

## 14.2 Anti-pattern : KPI calculé dans trois couches

### Symptômes

- KPI calculé dans Snowflake ;
- modifié dans Dataiku ;
- recalculé dans Power Query ;
- redéfini en DAX.

### Risque

Aucune équipe ne sait quelle valeur est officielle.

### Correction

Définir source of truth et dictionnaire KPI.

---

## 14.3 Anti-pattern : Dataiku flow spaghetti

### Symptômes

- trop de branches ;
- recipes dupliquées ;
- datasets orphelins ;
- objets expérimentaux connectés à PROD.

### Correction

Refactoriser par couches et scenarios séparés.

---

## 14.4 Anti-pattern : Snowflake sur-transformé

### Symptômes

- dizaines d’objets intermédiaires ;
- transformations spécifiques à un seul visuel ;
- tables non documentées ;
- coûts élevés.

### Correction

Garder Snowflake pour logique structurante et réutilisable.

---

## 14.5 Anti-pattern : query folding cassé trop tôt

### Symptômes

- filtre incremental appliqué après étape non foldable ;
- refresh complet ;
- timeout Power BI Service.

### Correction

Déplacer transformation en amont ou réordonner étapes Power Query.

---

## 14.6 Anti-pattern : grain non documenté

### Symptômes

- relations Power BI ambiguës ;
- many-to-many ;
- montants dupliqués ;
- totaux faux.

### Correction

Définir grain dans Snowflake et semantic model.

---

## 15. Quality gates minimaux

## 15.1 Avant exposition Power BI

Tout dataset destiné à Power BI corporate doit passer au minimum :

- contrôle schéma ;
- contrôle nulls champs obligatoires ;
- contrôle unicité clé ;
- contrôle duplicats ;
- contrôle volume ;
- contrôle fraîcheur ;
- contrôle bornes métier ;
- contrôle référentiel ;
- contrôle réconciliation si applicable ;
- contrôle sécurité colonnes sensibles ;
- validation grain.

---

## 15.2 Répartition quality gates

| Contrôle | Snowflake | Dataiku | Power Query |
|---|---|---|---|
| Schéma | Oui | Oui | Détection limitée |
| Nulls | Oui | Oui | Limité |
| Duplicats | Oui | Oui | Non recommandé |
| Volumes | Oui | Oui | Non |
| Freshness | Oui | Oui | Affichage possible |
| Réconciliation | Oui | Oui | Non |
| Cardinalité joins | Oui | Oui | Non |
| Schema drift | Oui | Oui | Refresh error seulement |
| PII detection | Oui | Oui | Non recommandé |
| Query folding | Non | Non | Oui |
| Refresh duration | Non | Indirect | Oui/Service |

---

## 15.3 Exemple de quality gate

```yaml
dataset: sales_prd.mart.fact_sales_daily
consumer: Power BI Sales Executive Dashboard
quality_gates:
  - name: sales_date_freshness
    layer: Snowflake/Dataiku
    rule: "max(sales_date) >= current_date - 1"
    severity: critical
    action: block_powerbi_refresh
  - name: sales_amount_not_negative
    layer: Snowflake
    rule: "sales_amount_eur >= 0"
    severity: critical
    action: quarantine_and_alert
  - name: row_count_variation
    layer: Dataiku
    rule: "variation <= 20%"
    severity: warning
    action: alert_owner
```

---

## 16. Gouvernance des changements

## 16.1 Types de changements

| Type | Exemple | Gouvernance |
|---|---|---|
| Non breaking | ajout colonne nullable | notification |
| Minor | renommage label Power BI | review BI |
| Major | nouvelle règle métier | validation PO/Data Steward |
| Breaking | suppression colonne | plan migration |
| Critical | changement grain | validation architecture + métier |
| Security | ajout PII | validation RSSI/Data Governance |
| Performance | changement refresh | test performance |

---

## 16.2 Changements de schéma

### Règles

- Les suppressions/renommages doivent être considérés breaking.
- Les ajouts de colonnes nullable peuvent être compatibles.
- Les changements de type doivent être testés.
- Les changements de grain doivent être revalidés.
- Les consommateurs Power BI doivent être notifiés.
- Les vues contractuelles doivent maintenir compatibilité autant que possible.
- Une version V2 de vue peut être créée si rupture nécessaire.

---

## 16.3 Changelog obligatoire

```markdown
# Changelog — BI Data Preparation Chain

## 2026-06-12 — v2.0.0

### Snowflake
- Added SALES_PRD.MART.SVW_POWERBI_SALES_DAILY_V2.
- Added tests for customer_key uniqueness.
- Changed incremental window from 7 to 10 days.

### Dataiku
- Added quality gate for freshness.
- Added reporter to data-platform-alerts.
- Updated bundle to v2.0.0.

### Power BI / Power Query
- Updated source view from V1 to V2.
- Preserved incremental refresh policy.
- Removed local transformation for customer segment.

### Breaking changes
- Column revenue_amount replaced by sales_amount_eur.

### Validation
- UAT completed by Sales Operations.
- KPI regression passed.
```

---

## 17. RACI de la chaîne complète

| Activité | Data Engineer | Analytics Engineer | BI Developer | Data Steward | Data Architect | Platform Admin | PO Métier |
|---|---:|---:|---:|---:|---:|---:|---:|
| Design couche Snowflake | A/R | C | I | C | A/R | C | C |
| Tables/vues certifiées | A/R | R | C | A | C | I | C |
| Orchestration Dataiku | A/R | R | C | C | C | C | I |
| Quality gates | R | R | C | A | C | I | C |
| Power Query last-mile | C | R | A/R | C | I | I | C |
| Semantic model Power BI | C | A/R | A/R | C | C | I | C |
| Validation KPI | C | C | C | A | C | I | A/R |
| Sécurité données | R | C | C | C | C | A/R | I |
| Performance refresh | C | C | A/R | I | C | C | I |
| Monitoring production | A/R | C | C | C | I | C | I |
| Changement breaking | C | C | C | A | A/R | C | A/R |

---

## 18. Checklist production readiness

## 18.1 Snowflake readiness

- [ ] Table/vue certifiée disponible.
- [ ] Grain documenté.
- [ ] Clés stables.
- [ ] Types normalisés.
- [ ] Dates compatibles incremental refresh.
- [ ] Tests SQL exécutés.
- [ ] KPI réconciliés.
- [ ] Sécurité appliquée.
- [ ] Colonnes sensibles exclues ou masquées.
- [ ] Query performance validée.
- [ ] Owner technique défini.
- [ ] Owner métier défini.
- [ ] Changelog mis à jour.
- [ ] Schema evolution plan disponible.

---

## 18.2 Dataiku readiness

- [ ] Flow structuré par couches.
- [ ] Recipes nommées.
- [ ] Scenarios de build configurés.
- [ ] Pre-checks configurés.
- [ ] Post-checks configurés.
- [ ] Quality gates critiques bloquants.
- [ ] Reporters configurés.
- [ ] Runbook disponible.
- [ ] Bundle versionné.
- [ ] Variables par environnement validées.
- [ ] Connexions sécurisées.
- [ ] Monitoring activé.
- [ ] Rollback identifié.

---

## 18.3 Power Query readiness

- [ ] Source certifiée utilisée.
- [ ] Transformations locales limitées.
- [ ] Query folding vérifié.
- [ ] RangeStart/RangeEnd configurés si incremental refresh.
- [ ] Colonnes inutiles supprimées.
- [ ] Typage final appliqué.
- [ ] Renommage final documenté.
- [ ] Aucune logique métier critique en M.
- [ ] Aucune PII inutile importée.
- [ ] Refresh testé dans Power BI Service.
- [ ] Documentation locale disponible.

---

## 18.4 Power BI semantic model readiness

- [ ] Schéma en étoile.
- [ ] Relations 1-* explicites.
- [ ] Table calendrier.
- [ ] Mesures DAX centralisées.
- [ ] Pas de mesures implicites critiques.
- [ ] RLS testée si applicable.
- [ ] KPI validés.
- [ ] Performance rapport testée.
- [ ] Documentation modèle.
- [ ] Data freshness visible.

---

## 19. SOP — choix du lieu de transformation

## 19.1 Procédure

1. Décrire la transformation.
2. Identifier volume.
3. Identifier réutilisation.
4. Identifier criticité métier.
5. Identifier dépendance au contexte utilisateur.
6. Vérifier sécurité.
7. Vérifier besoin de monitoring.
8. Vérifier impact refresh.
9. Vérifier query folding si Power Query envisagé.
10. Choisir Snowflake/Dataiku/Power Query/DAX.
11. Documenter décision.
12. Implémenter.
13. Tester.
14. Monitorer.

---

## 19.2 Arbre de décision

```text
La transformation impacte-t-elle un KPI critique ?
    Oui → Snowflake ou Semantic Model DAX certifié
    Non → continuer

La transformation est-elle volumineuse ?
    Oui → Snowflake / Dataiku pushdown
    Non → continuer

La transformation est-elle réutilisable ?
    Oui → Snowflake / Dataiku serving
    Non → continuer

La transformation est-elle spécifique à un rapport ?
    Oui → Power Query si légère et foldable
    Non → continuer

La transformation dépend-elle des filtres utilisateur ?
    Oui → DAX
    Non → Snowflake/Dataiku selon gouvernance
```

---

## 20. SOP — migration d’une logique Power Query vers Snowflake/Dataiku

1. Identifier transformations M concernées.
2. Classer : présentation, métier, qualité, performance.
3. Mesurer impact refresh.
4. Vérifier query folding.
5. Identifier logique réutilisable.
6. Traduire logique en SQL Snowflake ou recipe Dataiku.
7. Créer tests de non-régression.
8. Publier vue/table certifiée.
9. Simplifier Power Query.
10. Comparer résultats PBIX avant/après.
11. Tester refresh Desktop et Service.
12. Documenter changement.
13. Déployer.
14. Monitorer.

---

## 21. SOP — création d’un dataset Power BI-ready

1. Cadrer besoin métier.
2. Définir KPI et questions.
3. Définir grain.
4. Définir faits/dimensions.
5. Identifier sources.
6. Créer data contract.
7. Implémenter transformations Snowflake.
8. Ajouter tests SQL.
9. Orchestrer via Dataiku si applicable.
10. Ajouter quality gates.
11. Publier table/vue serving.
12. Documenter colonnes.
13. Valider sécurité.
14. Connecter Power BI.
15. Limiter Power Query.
16. Configurer incremental refresh.
17. Créer semantic model.
18. Valider KPI.
19. Tester performance.
20. Publier rapport.
21. Monitorer usage et refresh.

---

## 22. SOP — incident de refresh Power BI lié à la préparation

1. Vérifier erreur Power BI Service.
2. Identifier étape Power Query en échec.
3. Vérifier query folding.
4. Vérifier schema drift.
5. Vérifier source Snowflake.
6. Vérifier scenario Dataiku.
7. Vérifier fraîcheur dataset.
8. Vérifier volumes.
9. Vérifier credentials/permissions.
10. Vérifier changement récent.
11. Contenir impact.
12. Corriger couche responsable.
13. Relancer pipeline Dataiku si nécessaire.
14. Relancer refresh Power BI.
15. Documenter incident.
16. Ajouter test ou alerte.

---

## 23. Lignes rouges

Les pratiques suivantes sont interdites pour les projets BI corporate :

- implémenter une logique métier critique uniquement dans Power Query ;
- dupliquer un KPI entre Snowflake, Dataiku, Power Query et DAX sans source de vérité ;
- charger des données raw directement dans un rapport corporate ;
- faire des joins volumineux en Power Query Desktop ;
- publier un rapport sans grain documenté des tables sources ;
- casser query folding sur un dataset volumineux sans justification ;
- exposer des colonnes sensibles à Power BI sans besoin ;
- utiliser Dataiku comme simple copie manuelle sans scenario/quality gates ;
- ignorer les failures Dataiku avant refresh Power BI ;
- publier des vues Snowflake contractuelles avec `SELECT *`;
- changer le schéma Snowflake sans analyser impacts Power BI ;
- mélanger datasets exploratoires et certifiés ;
- publier un PBIX avec transformations locales non documentées ;
- faire un refresh full quotidien si incremental refresh est possible ;
- laisser des datasets Dataiku intermédiaires orphelins ;
- maintenir plusieurs versions divergentes d’un même dataset BI ;
- créer une relation many-to-many dans Power BI pour compenser un mauvais grain amont ;
- masquer une erreur qualité par un filtre Power Query ;
- remplacer des NULL par zéro sans validation métier ;
- promouvoir en production sans tests de non-régression KPI.

---

## 24. Astuces expertes et gains rapides

## 24.1 Rule of thumb

```text
Si c’est lourd → Snowflake.
Si c’est orchestré → Dataiku.
Si c’est local au rapport → Power Query.
Si c’est interactif → DAX.
```

---

## 24.2 Déplacer tôt les filtres

Dans Power Query, appliquer les filtres foldables le plus tôt possible, surtout pour incremental refresh.

---

## 24.3 Créer des vues contractuelles

Une vue Snowflake stable entre la table physique et Power BI réduit les ruptures.

---

## 24.4 Utiliser Dataiku comme quality gate

Ne pas seulement orchestrer le build : bloquer la publication si la qualité est mauvaise.

---

## 24.5 Afficher la fraîcheur dans Power BI

Le rapport doit montrer :

- date dernière mise à jour source ;
- date dernier refresh Power BI ;
- statut qualité ;
- limites connues.

---

## 24.6 Mesurer les transformations Power Query

Si une transformation Power Query devient lente, elle est probablement candidate à migration.

---

## 24.7 Conserver un dictionnaire transformationnel

Documenter où chaque règle est appliquée :

| Règle | Snowflake | Dataiku | Power Query | DAX |
|---|---|---|---|---|
| Filtre commandes annulées | Oui | Non | Non | Non |
| Renommage libellés | Non | Non | Oui | Non |
| Marge brute | Oui | Non | Non | Mesure SUM |
| Variation YoY | Non | Non | Non | Oui |

---

## 25. Annexes

## 25.1 Template de matrice de répartition des transformations

```markdown
# Transformation Responsibility Matrix

| Transformation | Description | Couche cible | Justification | Owner | Tests | Statut |
|---|---|---|---|---|---|---|
| Deduplicate customers | Keep latest customer record by customer_id | Snowflake | Volumineux et réutilisable | Data Engineering | uniqueness customer_id | Approved |
| Check freshness | Verify max transaction date | Dataiku | Quality gate orchestration | Data Engineering | freshness check | Approved |
| Rename columns | Business-friendly names for report | Power Query | Last-mile report only | BI Developer | visual check | Approved |
| YoY growth | Dynamic filter context calculation | DAX | Depends on slicers | BI Developer | KPI validation | Approved |
```

---

## 25.2 Template de data contract Power BI-ready

```yaml
dataset_name: FACT_SALES_DAILY
technical_object: SALES_PRD.MART.SVW_POWERBI_SALES_DAILY_V1
consumer: Power BI Sales Executive Dashboard
owner_business: Sales Operations
owner_technical: Data Engineering
data_steward: Sales Data Steward

grain: "1 row = 1 sales_date x product_key x region_key"

refresh:
  snowflake_update: "daily 06:00 CET"
  dataiku_quality_gate: "daily 06:30 CET"
  powerbi_refresh: "daily 07:00 CET"
  incremental_column: sales_date

keys:
  primary:
    - sales_date
    - product_key
    - region_key
  foreign:
    - product_key
    - region_key

columns:
  - name: sales_date
    type: date
    nullable: false
    description: "Date de vente"
  - name: product_key
    type: integer
    nullable: false
    description: "Clé produit"
  - name: sales_amount_eur
    type: decimal
    nullable: false
    description: "Montant des ventes validées en EUR"

quality_gates:
  - name: freshness
    rule: "max(sales_date) >= current_date - 1"
    severity: critical
  - name: no_negative_sales
    rule: "sales_amount_eur >= 0"
    severity: critical
  - name: uniqueness
    rule: "primary key unique"
    severity: critical

security:
  classification: internal
  pii: false
  masking_required: false

schema_evolution:
  add_nullable_column: allowed_with_notice
  drop_column: breaking_change
  rename_column: breaking_change
  change_grain: critical_breaking_change
```

---

## 25.3 Template de checklist de revue Power Query

```markdown
# Power Query Review Checklist

## Source

- [ ] Source certifiée utilisée
- [ ] Objet source documenté
- [ ] Pas de connexion directe à raw
- [ ] Permissions validées

## Transformations

- [ ] Transformations locales limitées
- [ ] Pas de logique métier critique
- [ ] Pas de jointures volumineuses
- [ ] Colonnes inutiles supprimées
- [ ] Typage final explicite
- [ ] Renommages documentés

## Query folding

- [ ] Folding vérifié sur étapes clés
- [ ] Filtres appliqués tôt
- [ ] RangeStart/RangeEnd foldables
- [ ] Ruptures documentées
- [ ] Transformations non foldables lourdes déplacées en amont

## Incremental refresh

- [ ] Colonne date correcte
- [ ] RangeStart/RangeEnd configurés
- [ ] Historique défini
- [ ] Fenêtre incrémentale définie
- [ ] Test Service effectué

## Documentation

- [ ] Transformations locales décrites
- [ ] Justification disponible
- [ ] Owner BI identifié
```

---

## 25.4 Template de readiness review

```markdown
# BI Data Preparation Readiness Review

## Rapport

Nom du rapport:
Workspace:
Owner BI:
Owner métier:

## Snowflake

- [ ] Table/vue certifiée:
- [ ] Grain:
- [ ] Tests SQL:
- [ ] Security:
- [ ] SLA:

## Dataiku

- [ ] Projet:
- [ ] Scenario:
- [ ] Quality gates:
- [ ] Bundle:
- [ ] Reporter:

## Power Query

- [ ] Transformations locales:
- [ ] Folding:
- [ ] Incremental refresh:
- [ ] Documentation:

## Power BI Model

- [ ] Star schema:
- [ ] Measures:
- [ ] RLS:
- [ ] Performance:

## Décision

- [ ] Ready for PROD
- [ ] Ready with minor fixes
- [ ] Not ready
```

---

## 25.5 Template d’incident

```markdown
# Incident — BI Data Preparation Chain

## Résumé

Date:
Rapport:
Dataset:
Sévérité:

## Symptôme

- Refresh failed
- KPI incorrect
- Data stale
- Performance degradation
- Security issue
- Other:

## Couche responsable probable

- Snowflake
- Dataiku
- Power Query
- Power BI semantic model
- Source system

## Diagnostic

Snowflake checks:
Dataiku scenario:
Power Query folding:
Power BI refresh:
Schema drift:
Data quality:

## Impact

Utilisateurs:
KPI:
Décisions:
SLA:

## Correction

Action immédiate:
Correction durable:

## Prévention

Test ajouté:
Quality gate ajouté:
Documentation mise à jour:
Owner:
Deadline:
```

---

## 25.6 Template de documentation des transformations

```markdown
# Transformation Catalog

| Règle | Description métier | Implémentée dans | Objet | Owner | Test | Réutilisable |
|---|---|---|---|---|---|---|
| Exclure commandes annulées | Les commandes annulées ne comptent pas dans le CA | Snowflake | MART.FACT_SALES | Finance/Data Eng | KPI reconciliation | Oui |
| Normaliser pays | Mapper codes pays vers référentiel ISO | Snowflake | REF.COUNTRY | Data Steward | accepted values | Oui |
| Vérifier fraîcheur | Bloquer si données > J-1 | Dataiku | sc_daily_quality | Data Eng | freshness check | Oui |
| Renommer Sales Amount | Libellé utilisateur | Power Query | PBIX Sales | BI Dev | visual review | Non |
| Calcul YoY % | Variation selon contexte filtre | DAX | Semantic Model | BI Dev | KPI UAT | Oui dans modèle |
```

---

## 25.7 Exemple de répartition idéale

```text
Besoin:
Rapport Power BI de performance commerciale quotidienne.

Snowflake:
- Ingestion ventes
- Déduplication transactions
- Historisation clients
- Construction FACT_SALES_DAILY
- Construction DIM_CUSTOMER, DIM_PRODUCT, DIM_DATE
- Tests SQL
- Secure views

Dataiku:
- Orchestration daily
- Pre-check source
- Build Snowflake transformations
- Checks qualité
- Alertes si retard
- Publication dataset certifié

Power Query:
- Connexion aux vues certifiées
- RangeStart/RangeEnd
- Typage final
- Renommage final
- Suppression colonnes inutiles

Power BI:
- Star schema
- Mesures DAX
- RLS si nécessaire
- Rapport exécutif
```

---

## 25.8 Confidentialité

Certaines données, exemples, noms d’objets, noms de projets, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, aux règles de gouvernance, aux politiques de sécurité, à l’architecture Snowflake, à l’instance Dataiku et aux contraintes Power BI de l’organisation.

---

## 25.9 Résumé exécutif

La meilleure préparation des données avant visualisation Power BI repose sur une combinaison spécialisée :

- **Snowflake** pour la robustesse, la performance, la gouvernance, l’historisation, les transformations lourdes et les datasets certifiés.
- **Dataiku** pour l’orchestration, la reproductibilité, les contrôles qualité, les scenarios, les alertes, les bundles et l’exploitation opérationnelle.
- **Power Query** pour le dernier ajustement de consommation, strictement limité aux besoins du rapport et en préservant le query folding.
- **Power BI semantic model / DAX** pour les calculs interactifs dépendant du contexte utilisateur.

Cette répartition permet un compromis optimal entre qualité de données, vitesse de livraison, coût de maintenance, sécurité, performance de rafraîchissement et confiance métier.

Le principe à retenir :

```text
Snowflake prépare et certifie.
Dataiku orchestre et contrôle.
Power Query ajuste légèrement.
Power BI modélise et raconte.
```
