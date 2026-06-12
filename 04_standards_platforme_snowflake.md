# Standard Corporate — Plateforme Snowflake

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Gouvernance, sécurité, performance et exploitation de la plateforme Snowflake |
| **Version** | 2.0 |
| **Date** | 2026-06-12 |
| **Auteur / Rôle** | Équipe Data Platform & Data Governance |
| **Validation / Approbation** | Head of Data Platform, Lead Data Engineer, Data Architect, RSSI, Data Governance Officer |
| **Public cible** | Data Engineers, Platform Engineers, Analytics Engineers, Admins Snowflake, Data Architects, Data Stewards, Security Engineers |
| **Commanditaire** | Direction Data & Performance / DSI |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Organisation Snowflake, comptes Snowflake, environnements DEV/UAT/PROD, bases, schémas, rôles, warehouses, pipelines d’ingestion, transformations, gouvernance, data sharing, observabilité et optimisation des coûts |
| **Objectif du document** | Définir les règles, standards, contrôles et bonnes pratiques pour exploiter Snowflake comme plateforme data industrielle : sécurisée, performante, gouvernée, observable, scalable, fiable et économiquement maîtrisée. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les exigences s’appliquent par défaut à tout usage de Snowflake dans l’organisation : ingestion, transformation, analytics, data sharing, gouvernance, administration, sécurité, monitoring et exploitation.

### Interprétation des termes

| Terme | Signification |
|---|---|
| **MUST** | Exigence obligatoire, non négociable hors dérogation formelle. |
| **SHOULD** | Exigence fortement recommandée ; toute exception doit être justifiée et documentée. |
| **MAY** | Option autorisée selon le contexte, sans contradiction avec les exigences MUST/SHOULD. |
| **MUST NOT** | Interdiction explicite. |
| **Exception documentée** | Écart temporaire validé par un approbateur nommé, avec justification, durée de validité et plan de remédiation. |

### Mode de rédaction

Les règles sont rédigées selon une logique :

- audit ;
- sécurité ;
- least privilege ;
- fiabilité ;
- performance ;
- maîtrise des coûts ;
- automatisation ;
- observabilité ;
- gouvernance ;
- maintenabilité.

### Gestion des exceptions

Toute dérogation doit inclure :

- le contexte métier ;
- le périmètre concerné ;
- le motif de l’écart ;
- l’impact sécurité ;
- l’impact coût ;
- l’impact performance ;
- l’impact conformité ;
- l’impact exploitation ;
- la durée de validité ;
- le plan de remédiation ;
- l’approbateur nommé ;
- la date de révision.

### Précedence

En cas de conflit, l’ordre de priorité est le suivant :

1. obligations légales, réglementaires et contractuelles ;
2. politiques de sécurité de l’entreprise ;
3. exigences RSSI / conformité / RGPD ;
4. standards Data Governance / Data Architecture ;
5. présent standard Snowflake ;
6. pratiques locales d’équipe.

---

## 1.2 Sources officielles Snowflake utilisées

Ce standard s’appuie sur la documentation officielle Snowflake, notamment :

- [Overview of Access Control](https://docs.snowflake.com/en/user-guide/security-access-control-overview)
- [Access Control Best Practices](https://docs.snowflake.com/en/user-guide/security-access-control-considerations)
- [Access Control Privileges](https://docs.snowflake.com/en/user-guide/security-access-control-privileges)
- [Securing Snowflake](https://docs.snowflake.com/en/guides-overview-secure)
- [Authentication Policies](https://docs.snowflake.com/en/user-guide/authentication-policies)
- [Multi-Factor Authentication](https://docs.snowflake.com/en/user-guide/security-mfa)
- [Key-Pair Authentication and Rotation](https://docs.snowflake.com/en/user-guide/key-pair-auth)
- [Network Policies](https://docs.snowflake.com/en/user-guide/network-policies)
- [Overview of Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-overview)
- [Working with Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-tasks)
- [Warehouse Considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations)
- [Resource Monitors](https://docs.snowflake.com/en/user-guide/resource-monitors)
- [Cost Controls for Warehouses](https://docs.snowflake.com/en/user-guide/cost-controlling-controls)
- [Data Governance in Snowflake](https://docs.snowflake.com/en/guides-overview-govern)
- [Dynamic Data Masking](https://docs.snowflake.com/en/user-guide/security-column-ddm-intro)
- [Row Access Policies](https://docs.snowflake.com/en/user-guide/security-row-intro)
- [Object Tagging](https://docs.snowflake.com/en/user-guide/object-tagging/introduction)
- [Tag-Based Masking Policies](https://docs.snowflake.com/en/user-guide/tag-based-masking-policies)
- [Sensitive Data Classification](https://docs.snowflake.com/en/user-guide/classify-intro)
- [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro)
- [Snowpipe Streaming](https://docs.snowflake.com/en/user-guide/snowpipe-streaming/data-load-snowpipe-streaming-overview)
- [Streams and Tasks](https://docs.snowflake.com/en/user-guide/data-pipelines-intro)
- [Introduction to Streams](https://docs.snowflake.com/en/user-guide/streams-intro)
- [Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/overview)
- [Decision Guide for Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/decision-guide)
- [Time Travel and Fail-safe](https://docs.snowflake.com/en/user-guide/data-availability)
- [Understanding Time Travel](https://docs.snowflake.com/en/user-guide/data-time-travel)
- [Replication and Failover](https://docs.snowflake.com/en/user-guide/account-replication-intro)
- [Cloning Considerations](https://docs.snowflake.com/en/user-guide/object-clone)
- [Secure Views](https://docs.snowflake.com/en/user-guide/views-secure)
- [Optimizing Query Performance](https://docs.snowflake.com/en/user-guide/performance-query-options)
- [Search Optimization Service](https://docs.snowflake.com/en/user-guide/search-optimization-service)
- [Query Acceleration Service](https://docs.snowflake.com/en/user-guide/query-acceleration-service)
- [Account Usage](https://docs.snowflake.com/en/sql-reference/account-usage)
- [QUERY_HISTORY View](https://docs.snowflake.com/en/sql-reference/account-usage/query_history)
- [WAREHOUSE_METERING_HISTORY View](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_metering_history)
- [WAREHOUSE_LOAD_HISTORY View](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_load_history)
- [ACCESS_HISTORY View](https://docs.snowflake.com/en/sql-reference/organization-usage/access_history)

---

## 1.3 Définitions clés

| Terme | Définition |
|---|---|
| **Organization** | Niveau d’administration regroupant plusieurs comptes Snowflake. |
| **Account** | Instance Snowflake logique, rattachée à une région et un cloud provider. |
| **Database** | Conteneur logique de schémas et objets data. |
| **Schema** | Conteneur d’objets : tables, vues, stages, pipes, streams, tasks, policies, procedures. |
| **Warehouse** | Ressource compute virtuelle utilisée pour exécuter requêtes, chargements et transformations. |
| **RBAC** | Role-Based Access Control : privilèges accordés à des rôles, rôles attribués aux utilisateurs. |
| **UBAC** | User-Based Access Control : privilèges accordés directement à des utilisateurs ; à limiter fortement en entreprise. |
| **DAC** | Discretionary Access Control : chaque objet a un propriétaire capable de déléguer l’accès. |
| **System Role** | Rôle système Snowflake, par exemple `ACCOUNTADMIN`, `SECURITYADMIN`, `SYSADMIN`, `USERADMIN`. |
| **Functional Role** | Rôle aligné sur un usage métier ou technique. |
| **Access Role** | Rôle donnant des privilèges sur des objets précis. |
| **Warehouse Role** | Rôle permettant l’usage d’un warehouse. |
| **Masking Policy** | Politique masquant dynamiquement les valeurs d’une colonne selon rôle ou contexte. |
| **Row Access Policy** | Politique filtrant les lignes visibles selon rôle ou contexte. |
| **Tag** | Métadonnée gouvernée applicable aux objets Snowflake. |
| **Data Classification** | Processus d’identification et de catégorisation des données sensibles. |
| **Stage** | Emplacement interne ou externe utilisé pour charger/décharger des fichiers. |
| **Pipe** | Objet Snowpipe permettant le chargement automatique ou semi-automatique de fichiers. |
| **Stream** | Objet suivant les changements d’une table pour des traitements CDC. |
| **Task** | Objet d’orchestration Snowflake exécutant du SQL selon un planning ou une dépendance. |
| **Dynamic Table** | Table matérialisée déclarative maintenue par Snowflake selon une cible de fraîcheur. |
| **Time Travel** | Fonction permettant d’accéder à des versions historiques de données pendant une période de rétention. |
| **Fail-safe** | Mécanisme Snowflake de récupération après Time Travel, géré par Snowflake, non destiné aux opérations utilisateur courantes. |
| **Zero-copy Clone** | Clone logique rapide d’un objet sans copie physique immédiate des données. |
| **Resource Monitor** | Objet permettant de suivre et contrôler la consommation de crédits. |
| **Account Usage** | Schémas de métadonnées et historique d’usage exposés dans la base `SNOWFLAKE`. |
| **Secure View** | Vue sécurisée limitant l’exposition de la logique et renforçant certains scénarios de partage/gouvernance. |
| **Data Sharing** | Mécanisme Snowflake de partage de données sans copie entre comptes. |

---

## 2. Vision, principes directeurs et résultats attendus

## 2.1 Vision

Opérer Snowflake comme une plateforme data industrielle, gouvernée et mesurable, capable de supporter :

- ingestion batch et near-real-time ;
- transformations SQL industrialisées ;
- analytics et BI ;
- data sharing interne et externe ;
- data products ;
- workloads data engineering ;
- workloads analytics engineering ;
- workloads data science contrôlés ;
- gouvernance fine des accès ;
- protection des données sensibles ;
- observabilité ;
- optimisation des coûts ;
- reprise sur incident.

Snowflake ne doit pas être utilisé comme un simple entrepôt de tables. Il doit être géré comme une plateforme critique d’entreprise.

---

## 2.2 Principes directeurs

1. **Security by Design**  
   Les identités, rôles, policies, accès réseau et objets sensibles sont sécurisés dès la conception.

2. **Least Privilege**  
   Les utilisateurs, services et workloads disposent uniquement des privilèges strictement nécessaires.

3. **Separation of Duties**  
   Les responsabilités administration, sécurité, ingestion, transformation, consommation et audit sont séparées.

4. **Environment Isolation**  
   DEV, UAT et PROD sont isolés. Les écritures cross-environment sont interdites sauf procédure approuvée.

5. **Cost Awareness**  
   Chaque warehouse, pipeline et requête doit être conçu avec conscience du coût compute, storage et services.

6. **Data Reliability**  
   Les pipelines doivent être idempotents, rejouables, monitorés et conformes aux SLA.

7. **Observability First**  
   Les métriques, logs, historiques de requêtes, coûts, erreurs, volumes et incidents doivent être exploitables.

8. **Governed Automation**  
   Les objets critiques doivent être provisionnés, versionnés et promus via processus contrôlé.

9. **Data Contracts**  
   Les datasets critiques doivent avoir un contrat de données : schéma, SLA, owner, règles qualité, classification.

10. **Secure Data Sharing**  
    Tout partage de données doit être contractuel, gouverné, auditable et limité au strict nécessaire.

11. **Performance by Design**  
    Les performances se construisent par le bon design : modélisation, clustering, warehouse, requêtes, données, optimisation.

12. **Recovery Preparedness**  
    Time Travel, Fail-safe, clones, réplication et runbooks doivent être intégrés à la stratégie de continuité.

---

## 2.3 Résultats attendus

L’application de ce standard doit permettre de :

- réduire les incidents d’accès et de conformité ;
- réduire les privilèges excessifs ;
- améliorer la stabilité des pipelines ;
- améliorer la qualité des données exposées ;
- réduire les coûts non maîtrisés ;
- améliorer la prévisibilité des dépenses Snowflake ;
- accélérer les livraisons data avec moins de régressions ;
- renforcer l’auditabilité ;
- réduire le MTTR lors d’incidents ;
- standardiser les environnements et objets ;
- faciliter l’onboarding des équipes data ;
- augmenter la confiance des métiers dans les produits data.

---

## 2.4 Indicateurs de succès

| Indicateur | Cible recommandée |
|---|---|
| Comptes avec SSO/MFA/authentication policies conformes | 100 % |
| Service users sans mot de passe humain | 100 % |
| Warehouses avec auto-suspend actif | 100 % hors exception |
| Warehouses avec owner documenté | 100 % |
| Objets sensibles taggés/classifiés | 100 % sur périmètre critique |
| Colonnes sensibles protégées par masking policy | 100 % si exposition non privilégiée |
| Tables critiques avec owner et SLA | 100 % |
| Pipelines critiques avec monitoring et alerting | 100 % |
| Resource monitors configurés | 100 % sur comptes/warehouses critiques |
| Revues d’accès trimestrielles | 100 % des rôles sensibles |
| Requêtes coûteuses revues mensuellement | Top 20 minimum |
| Incidents majeurs avec post-mortem | 100 % |
| Déploiements PROD via processus contrôlé | 100 % |

---

## 3. Rôles et responsabilités

## 3.1 RACI simplifié

| Activité | Platform Engineer | Data Engineer | Analytics Engineer | Data Architect | Data Steward | RSSI / Security | Métier / PO | FinOps |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Architecture organization/accounts | A/R | C | I | A/R | I | C | I | C |
| Design environnements DEV/UAT/PROD | A/R | C | C | A/R | I | C | I | C |
| Provisioning roles/warehouses/policies | A/R | C | I | C | I | C | I | C |
| RBAC et revues d’accès | R | C | I | C | C | A/R | I | I |
| Ingestion pipelines | C | A/R | C | C | C | I | C | I |
| Transformations SQL/dbt/tasks | C | A/R | A/R | C | C | I | C | I |
| Data contracts | C | R | R | C | A | C | A/R | I |
| Gouvernance metadata/tags | C | C | C | C | A/R | C | C | I |
| Masking/row access policies | R | C | C | C | A | A/R | C | I |
| Data sharing externe | C | C | C | A/R | A/R | A/R | C | I |
| Resource monitors | A/R | C | I | C | I | I | C | A/R |
| Optimisation coûts | R | R | C | C | I | I | C | A/R |
| Observabilité | A/R | R | C | C | C | C | I | C |
| Incident management | A/R | R | C | C | C | C | I | C |
| Post-mortem | A/R | R | C | C | C | C | I | C |
| Documentation | R | R | R | C | A | C | C | I |

---

## 3.2 Ownership obligatoire

Chaque objet critique doit avoir :

- un owner métier ;
- un owner technique ;
- un domaine de données ;
- un niveau de criticité ;
- une classification de confidentialité ;
- un SLA de disponibilité ;
- une politique de rétention ;
- une stratégie de support ;
- une fréquence de revue ;
- un référentiel de documentation.

Objets concernés :

- databases ;
- schemas ;
- tables critiques ;
- vues contractuelles ;
- stages ;
- pipes ;
- streams ;
- tasks ;
- dynamic tables ;
- warehouses ;
- resource monitors ;
- masking policies ;
- row access policies ;
- shares ;
- integrations ;
- service users.

---

## 4. Architecture des comptes et environnements

## 4.1 Architecture cible

Architecture recommandée :

```text
Snowflake Organization
│
├── Account: COMPANY_DEV
│   ├── Databases DEV
│   ├── Warehouses DEV
│   └── Roles DEV
│
├── Account: COMPANY_UAT
│   ├── Databases UAT
│   ├── Warehouses UAT
│   └── Roles UAT
│
├── Account: COMPANY_PRD
│   ├── Databases PROD
│   ├── Warehouses PROD
│   └── Roles PROD
│
└── Account: COMPANY_SECOPS / GOVERNANCE / SHARED_SERVICES
    ├── Monitoring
    ├── Governance metadata
    └── Cross-account reporting
```

### Règles

- DEV, UAT et PROD doivent être isolés.
- Les écritures cross-environment sont interdites sans procédure approuvée.
- Les rôles PROD ne doivent pas être utilisés pour du développement quotidien.
- Les données PROD ne doivent pas être copiées en DEV sans anonymisation, masquage ou autorisation explicite.
- Les conventions de nommage doivent être alignées entre environnements.
- Les paramètres sensibles doivent être contrôlés au niveau compte.
- Les comptes critiques doivent être monitorés au niveau organization si disponible.

---

## 4.2 Stratégies d’isolation

| Stratégie | Description | Usage recommandé |
|---|---|---|
| **Account per environment** | Un compte Snowflake par environnement | Recommandé pour isolation forte |
| **Database per environment** | Environnements séparés par databases dans un compte | Acceptable pour petites organisations, moins isolé |
| **Schema per environment** | Environnements séparés par schémas | À éviter pour production critique |
| **Branch/schema temporaire** | Environnement temporaire de feature | Développement contrôlé |

### Standard corporate

Le standard recommandé est **un compte Snowflake distinct par environnement majeur** lorsque les contraintes sécurité, coût et gouvernance le justifient.

---

## 4.3 Conventions d’environnements

| Environnement | Usage | Données | Accès |
|---|---|---|---|
| **DEV** | Développement, expérimentation contrôlée | Données synthétiques, anonymisées ou échantillonnées | Équipes data |
| **UAT** | Recette métier et tests d’intégration | Données proches PROD, sécurisées | Testeurs autorisés |
| **PROD** | Exécution officielle | Données réelles | Accès strict et audité |
| **SANDBOX** | Exploration limitée | Données non sensibles ou contrôlées | Accès temporaire |
| **SECOPS/GOV** | Monitoring, gouvernance, audit | Métadonnées, logs, vues d’usage | Platform/Security |

---

## 4.4 Paramètres critiques de compte

Les paramètres suivants doivent être évalués et documentés :

- authentication policies ;
- network policies ;
- password policies si usage autorisé ;
- session policies ;
- default warehouse ;
- timezone ;
- data retention defaults ;
- statement timeout ;
- lock timeout ;
- query tag usage ;
- client telemetry ;
- allowed integrations ;
- OAuth / SSO ;
- MFA enforcement ;
- user provisioning via SCIM ;
- failover/replication configuration.

---

## 4.5 Règles cross-environment

### Interdits

- Écrire directement de DEV vers PROD.
- Utiliser un warehouse PROD pour des tests DEV.
- Lire des données sensibles PROD depuis DEV sans contrôle.
- Déployer manuellement en PROD sans validation.
- Copier un schéma PROD complet en DEV sans classification préalable.
- Utiliser `ACCOUNTADMIN` pour exécuter des pipelines métiers.

### Exceptions possibles

- restauration contrôlée ;
- migration approuvée ;
- clone sécurisé pour incident ;
- test de performance encadré ;
- opération de support urgente avec traçabilité.

---

## 5. Gouvernance des identités et des rôles

## 5.1 Principes RBAC

Snowflake repose principalement sur un modèle de contrôle d’accès par rôles. Les privilèges doivent être accordés aux rôles, puis les rôles aux utilisateurs ou rôles supérieurs.

### Règles obligatoires

- Utiliser RBAC comme modèle standard.
- Interdire les privilèges directs aux utilisateurs en PROD, hors exception formelle.
- Séparer rôles d’administration, rôles techniques, rôles d’accès et rôles métier.
- Appliquer le principe du moindre privilège.
- Documenter les hiérarchies de rôles.
- Réviser les rôles sensibles au moins trimestriellement.
- Supprimer les privilèges dormants.
- Utiliser des groupes IdP/SCIM lorsque possible.
- Limiter strictement `ACCOUNTADMIN`, `SECURITYADMIN`, `SYSADMIN`.
- Interdire l’usage quotidien des rôles admin pour développer.

---

## 5.2 Modèle de rôles recommandé

```text
System Roles
│
├── ACCOUNTADMIN
├── SECURITYADMIN
├── SYSADMIN
├── USERADMIN
│
Custom Governance Roles
│
├── DATA_GOVERNANCE_ADMIN
├── DATA_SECURITY_ADMIN
├── FINOPS_ADMIN
│
Functional Roles
│
├── FR_DATA_ENGINEER
├── FR_ANALYTICS_ENGINEER
├── FR_DATA_ANALYST
├── FR_DATA_STEWARD
├── FR_BI_CONSUMER
│
Access Roles
│
├── AR_SALES_RAW_RO
├── AR_SALES_CURATED_RW
├── AR_SALES_MART_RO
├── AR_FINANCE_MART_RO
│
Warehouse Roles
│
├── WH_ETL_SALES_USE
├── WH_BI_STANDARD_USE
└── WH_DS_EXPERIMENT_USE
```

---

## 5.3 Séparation des rôles

### Rôles d’administration

Utilisés pour administrer la plateforme.

Exemples :

- gestion utilisateurs ;
- gestion rôles ;
- gestion network policies ;
- gestion integrations ;
- gestion resource monitors ;
- revue sécurité.

### Rôles fonctionnels

Alignés sur les profils utilisateurs.

Exemples :

- data engineer ;
- analytics engineer ;
- data analyst ;
- data steward ;
- BI consumer.

### Rôles d’accès

Donnent accès aux objets data.

Exemples :

- lecture mart sales ;
- écriture curated finance ;
- lecture raw logs.

### Rôles warehouse

Donnent le droit d’utiliser du compute.

Exemples :

- usage warehouse ETL ;
- usage warehouse BI ;
- usage warehouse DS.

---

## 5.4 Pattern recommandé de grants

```sql
-- Access role: lecture sur un schéma MART
CREATE ROLE IF NOT EXISTS AR_SALES_MART_RO;

GRANT USAGE ON DATABASE SALES_PRD TO ROLE AR_SALES_MART_RO;
GRANT USAGE ON SCHEMA SALES_PRD.MART TO ROLE AR_SALES_MART_RO;
GRANT SELECT ON ALL TABLES IN SCHEMA SALES_PRD.MART TO ROLE AR_SALES_MART_RO;
GRANT SELECT ON FUTURE TABLES IN SCHEMA SALES_PRD.MART TO ROLE AR_SALES_MART_RO;
GRANT SELECT ON ALL VIEWS IN SCHEMA SALES_PRD.MART TO ROLE AR_SALES_MART_RO;
GRANT SELECT ON FUTURE VIEWS IN SCHEMA SALES_PRD.MART TO ROLE AR_SALES_MART_RO;

-- Functional role
CREATE ROLE IF NOT EXISTS FR_SALES_ANALYST;

GRANT ROLE AR_SALES_MART_RO TO ROLE FR_SALES_ANALYST;

-- Warehouse usage
CREATE ROLE IF NOT EXISTS WH_BI_STANDARD_USE;

GRANT USAGE ON WAREHOUSE BI_STANDARD_WH TO ROLE WH_BI_STANDARD_USE;
GRANT ROLE WH_BI_STANDARD_USE TO ROLE FR_SALES_ANALYST;

-- User assignment
GRANT ROLE FR_SALES_ANALYST TO USER user_name;
```

---

## 5.5 Ownership

### Règles

- Chaque objet doit avoir un owner technique explicite.
- Le privilège `OWNERSHIP` doit être limité.
- Le transfert d’ownership doit être tracé.
- Les objets de production ne doivent pas être possédés par des rôles personnels.
- Les objets critiques doivent être possédés par des rôles de service ou rôles d’équipe.
- Le owner d’un objet ne doit pas être confondu avec le consommateur métier.

---

## 5.6 Secondary roles

### Règles

- L’usage des secondary roles doit être explicite et auditable.
- Les requêtes de production automatisées doivent utiliser un rôle principal contrôlé.
- Les équipes doivent éviter de dépendre d’un comportement implicite des rôles secondaires.
- Les politiques de sécurité doivent être testées avec les rôles réellement utilisés.
- Toute activation large de secondary roles doit être validée par Security/Data Platform.

---

## 5.7 Comptes utilisateurs et service users

### Utilisateurs humains

- SSO recommandé.
- MFA obligatoire selon politique entreprise.
- Accès via groupes IdP/SCIM si possible.
- Pas de partage de compte.
- Pas de credentials dans code.
- Revue des utilisateurs inactifs.

### Service users

- Réservés aux applications, pipelines et outils d’orchestration.
- Authentification forte obligatoire.
- Key-pair authentication recommandée.
- Pas de mot de passe humain.
- Rotation des clés documentée.
- Rôles minimaux.
- Ownership clair.
- Contact support.
- Journalisation des usages.
- Désactivation automatique à la fin du besoin.

---

## 5.8 Revue d’accès

### Fréquence minimale

| Type d’accès | Fréquence |
|---|---|
| Rôles admin | Mensuelle |
| Rôles PROD RW | Mensuelle ou trimestrielle |
| Rôles sensibles PII/Finance/RH | Trimestrielle |
| Rôles lecture standard | Semestrielle |
| Service users | Trimestrielle |
| Shares externes | Trimestrielle ou selon contrat |

### Points de contrôle

- utilisateurs inactifs ;
- rôles sans owner ;
- rôles avec privilèges excessifs ;
- grants directs utilisateurs ;
- rôles admin trop nombreux ;
- service users non documentés ;
- accès à données sensibles ;
- accès cross-domain ;
- privilèges `OWNERSHIP`, `MANAGE GRANTS`, `CREATE INTEGRATION`.

---

## 6. Authentification, réseau et sécurité d’accès

## 6.1 Authentification

### Règles

- SSO doit être utilisé pour les utilisateurs humains lorsque disponible.
- MFA doit être appliquée aux utilisateurs humains selon la politique entreprise.
- Les service users doivent utiliser une authentification forte non interactive.
- Key-pair authentication doit être privilégiée pour les automatisations.
- Les secrets ne doivent jamais être stockés dans du code source.
- Les clés privées doivent être stockées dans un coffre sécurisé.
- Les clés doivent être rotées selon politique sécurité.
- Les utilisateurs sans activité doivent être désactivés selon processus IAM.

---

## 6.2 Authentication policies

Les authentication policies doivent être utilisées pour contrôler :

- méthodes d’authentification autorisées ;
- obligation MFA ;
- clients autorisés ;
- règles spécifiques par utilisateur ou compte ;
- restrictions pour service users.

### Règle

Toute exception à la politique d’authentification standard doit être validée par RSSI/Security.

---

## 6.3 Network policies

Les network policies permettent de contrôler l’accès réseau entrant à Snowflake.

### Règles

- Les comptes PROD doivent avoir une stratégie réseau documentée.
- Les plages IP autorisées doivent être minimisées.
- Les exceptions doivent être temporaires.
- Les changements doivent être testés pour éviter un lock-out.
- Les intégrations SCIM ou services externes peuvent nécessiter des politiques dédiées.
- Les accès depuis outils tiers doivent être répertoriés.
- Les accès depuis réseaux personnels doivent être interdits sauf exception approuvée.

---

## 6.4 Secrets et credentials

### Interdits

- Credentials dans Git.
- Credentials dans notebooks partagés.
- Credentials dans scripts locaux non chiffrés.
- Partage de clés privées par email/chat.
- Utilisation de comptes personnels pour pipelines.
- Réutilisation de clés entre environnements.

### Obligations

- Utiliser un secret manager approuvé.
- Rotations planifiées.
- Révocation immédiate en cas de suspicion.
- Journalisation des accès.
- Inventaire des intégrations.
- Ownership technique.

---

## 6.5 Break-glass access

Un accès d’urgence doit être prévu pour incident majeur.

### Règles

- Accès limité à un petit nombre d’administrateurs.
- Usage uniquement en situation exceptionnelle.
- MFA obligatoire.
- Journalisation renforcée.
- Notification Security.
- Post-review obligatoire.
- Suppression ou retour au niveau standard après incident.

---

## 7. Sécurité des données et gouvernance

## 7.1 Classification des données

Chaque colonne critique doit être classifiée.

### Niveaux recommandés

| Niveau | Description | Exemples |
|---|---|---|
| **PUBLIC** | Données publiables | Documentation publique |
| **INTERNAL** | Données internes non sensibles | Référentiels non confidentiels |
| **CONFIDENTIAL** | Données métier sensibles | CA, marge, contrats |
| **SENSITIVE** | Données personnelles ou réglementées | Email, téléphone, ID client |
| **RESTRICTED** | Données très sensibles | Santé, RH, rémunération, secrets |

---

## 7.2 Tags

### Règles

- Les tags doivent être définis au niveau gouvernance.
- Les tags critiques doivent avoir un owner.
- Les valeurs de tags doivent être contrôlées.
- Les tags doivent être appliqués aux databases, schemas, tables, vues et colonnes selon besoin.
- Les tags doivent être utilisés pour la classification, ownership, domaine, criticité, rétention.
- Les tags doivent être monitorés.
- Les tags ne doivent pas être décoratifs.

### Tags recommandés

| Tag | Exemple |
|---|---|
| `data_domain` | SALES, FINANCE, HR |
| `data_classification` | INTERNAL, SENSITIVE, RESTRICTED |
| `data_owner` | finance_controlling |
| `technical_owner` | data_platform |
| `retention_policy` | 7Y, 3Y, 90D |
| `sla_tier` | GOLD, SILVER, BRONZE |
| `pii_type` | EMAIL, PHONE, NAME |
| `criticality` | HIGH, MEDIUM, LOW |
| `source_system` | CRM, ERP, APP |
| `environment` | DEV, UAT, PROD |

---

## 7.3 Dynamic Data Masking

Dynamic Data Masking doit être utilisé pour masquer les colonnes sensibles à l’exécution selon rôle ou contexte.

### Règles

- Toute colonne sensible exposée à des rôles non privilégiés doit être masquée.
- Les policies doivent être centralisées dans un schéma gouvernance.
- Les roles autorisés doivent être explicites.
- Les policies doivent être testées avec utilisateurs représentatifs.
- Les policies doivent être documentées.
- Les changements de policies doivent suivre CI/CD ou change management.
- Les exceptions doivent être validées par Data Governance et Security.

### Exemple

```sql
CREATE OR REPLACE MASKING POLICY GOV_SECURITY.MASK_EMAIL
AS (val STRING) RETURNS STRING ->
  CASE
    WHEN IS_ROLE_IN_SESSION('FR_PII_AUTHORIZED') THEN val
    WHEN val IS NULL THEN NULL
    ELSE REGEXP_REPLACE(val, '^(.).+(@.+)$', '\\1***\\2')
  END;

ALTER TABLE CUSTOMER_PRD.CURATED.CUSTOMER
MODIFY COLUMN EMAIL SET MASKING POLICY GOV_SECURITY.MASK_EMAIL;
```

---

## 7.4 Tag-based masking

Le tag-based masking doit être privilégié pour industrialiser la protection des colonnes sensibles.

### Avantages

- application cohérente ;
- réduction des oublis ;
- gouvernance centralisée ;
- maintenance simplifiée ;
- automatisation par classification.

### Règles

- Définir des tags de classification.
- Associer policies aux tags lorsque pertinent.
- Tester les précédences entre policy directe et policy via tag.
- Monitorer les colonnes taggées sans policy.
- Monitorer les policies non utilisées.
- Documenter les rôles autorisés.

---

## 7.5 Row Access Policies

Les row access policies doivent être utilisées lorsque l’accès doit dépendre :

- d’une région ;
- d’une business unit ;
- d’un pays ;
- d’un portefeuille client ;
- d’une entité légale ;
- d’un rôle hiérarchique ;
- d’un périmètre contractuel.

### Exemple conceptuel

```sql
CREATE OR REPLACE ROW ACCESS POLICY GOV_SECURITY.RAP_REGION
AS (region_code STRING) RETURNS BOOLEAN ->
  EXISTS (
    SELECT 1
    FROM GOV_SECURITY.USER_REGION_ACCESS a
    WHERE a.user_name = CURRENT_USER()
      AND a.region_code = region_code
  );

ALTER TABLE SALES_PRD.MART.SALES_BY_REGION
ADD ROW ACCESS POLICY GOV_SECURITY.RAP_REGION ON (REGION_CODE);
```

### Règles

- La table de mapping des droits doit être gouvernée.
- Les cas multi-périmètres doivent être testés.
- Les utilisateurs sans mapping doivent avoir accès à zéro ligne par défaut.
- Les policies doivent être testées pour performance.
- Les rôles bypass doivent être strictement limités.
- Les policies doivent être documentées.

---

## 7.6 Secure views

Les secure views doivent être utilisées lorsque :

- la logique de la vue ne doit pas être exposée ;
- les données sont partagées ;
- la vue sert de contrat externe ;
- le périmètre contient des données sensibles ;
- la gouvernance impose une couche de protection.

### Règles

- Les secure views critiques doivent avoir un owner.
- Les définitions doivent être versionnées.
- Les performances doivent être testées.
- Les limitations doivent être documentées.
- Les secure views ne remplacent pas une stratégie RBAC/policies.

---

## 7.7 Données personnelles et RGPD

### Règles

- Appliquer minimisation des données.
- Éviter l’ingestion de PII non nécessaire.
- Pseudonymiser ou tokeniser en amont si possible.
- Masquer les données sensibles à la consommation.
- Définir des politiques de rétention.
- Documenter la finalité.
- Journaliser les accès aux données sensibles.
- Éviter les exports non contrôlés.
- Prévoir suppression/anonymisation si applicable.
- Impliquer RSSI/DPO selon criticité.

---

## 8. Structuration des objets et conventions de nommage

## 8.1 Architecture de données recommandée

```text
DATABASE_DOMAIN_ENV
│
├── RAW
│   ├── données brutes ou quasi brutes
│   └── ingestion auditée
│
├── STAGING
│   ├── typage
│   ├── normalisation
│   └── nettoyage technique
│
├── CURATED
│   ├── données harmonisées
│   ├── qualité contrôlée
│   └── entités métier
│
├── MART
│   ├── modèles analytiques
│   ├── agrégats métier
│   └── vues de consommation
│
├── GOVERNANCE
│   ├── tags
│   ├── policies
│   └── mapping sécurité
│
├── OPS
│   ├── logs
│   ├── contrôles
│   └── tables de monitoring
│
└── QUARANTINE
    ├── lignes rejetées
    └── fichiers invalides
```

---

## 8.2 Conventions databases

Format recommandé :

```text
<DOMAIN>_<ENV>
```

Exemples :

```text
SALES_PRD
SALES_UAT
SALES_DEV
FINANCE_PRD
CUSTOMER_PRD
GOVERNANCE_PRD
OPS_PRD
```

---

## 8.3 Conventions schemas

| Schéma | Usage |
|---|---|
| `RAW` | Données chargées depuis sources, sans logique métier lourde |
| `STAGING` | Typage, nettoyage technique, standardisation |
| `CURATED` | Données harmonisées et validées |
| `MART` | Consommation BI, analytics, data products |
| `REF` | Référentiels stables |
| `SECURITY` | Tables de mapping d’accès |
| `GOVERNANCE` | Tags, policies, objets gouvernance |
| `OPS` | Logs, monitoring, audit technique |
| `QUARANTINE` | Rejets, anomalies, données invalides |
| `SANDBOX` | Exploration contrôlée et temporaire |

---

## 8.4 Conventions objets

| Objet | Convention | Exemple |
|---|---|---|
| Table raw | `RAW_<SOURCE>_<ENTITY>` | `RAW_CRM_CUSTOMER` |
| Table staging | `STG_<ENTITY>` | `STG_CUSTOMER` |
| Table curated | `<ENTITY>` | `CUSTOMER` |
| Table fact | `FACT_<PROCESS>` | `FACT_SALES` |
| Table dimension | `DIM_<ENTITY>` | `DIM_CUSTOMER` |
| Vue mart | `VW_<SUBJECT>` | `VW_SALES_PERFORMANCE` |
| Secure view | `SVW_<SUBJECT>` | `SVW_CUSTOMER_SECURED` |
| Stage | `STG_<SOURCE>_<TYPE>` | `STG_S3_CRM_JSON` |
| File format | `FF_<FORMAT>_<OPTIONS>` | `FF_CSV_PIPE_GZIP` |
| Pipe | `PIPE_<SOURCE>_<TARGET>` | `PIPE_CRM_CUSTOMER` |
| Stream | `STRM_<TABLE>` | `STRM_CUSTOMER_CHANGES` |
| Task | `TSK_<PROCESS>_<STEP>` | `TSK_SALES_LOAD_CURATED` |
| Dynamic table | `DT_<ENTITY>` | `DT_CUSTOMER_DAILY` |
| Procedure | `SP_<ACTION>` | `SP_REPROCESS_PIPELINE` |
| Function | `FN_<PURPOSE>` | `FN_NORMALIZE_EMAIL` |
| Masking policy | `MP_<DATA_TYPE>_<PURPOSE>` | `MP_STRING_EMAIL` |
| Row access policy | `RAP_<DOMAIN>_<PURPOSE>` | `RAP_SALES_REGION` |
| Tag | `TAG_<PURPOSE>` | `TAG_DATA_CLASSIFICATION` |
| Warehouse | `<WORKLOAD>_<SIZE>_WH` | `ETL_M_WH` |

---

## 8.5 Table types

### Permanent tables

À utiliser pour :

- données de production ;
- données critiques ;
- tables historisées ;
- données nécessitant Time Travel/Fail-safe selon politique.

### Transient tables

À utiliser pour :

- données intermédiaires ;
- staging non critique ;
- tables reconstructibles ;
- réduction de coûts de rétention.

### Temporary tables

À utiliser pour :

- session temporaire ;
- traitements ponctuels ;
- tests locaux ;
- étapes non persistantes.

### Règles

- Ne pas utiliser transient tables pour données critiques sans validation.
- Ne pas utiliser temporary tables dans des pipelines nécessitant audit/reprise.
- Documenter le type choisi pour tables critiques.
- Aligner retention avec criticité et coût.

---

## 9. Warehouses et compute

## 9.1 Principes

Les warehouses doivent être spécialisés, monitorés et dimensionnés selon la charge réelle.

### Règles obligatoires

- Chaque warehouse doit avoir un owner.
- Chaque warehouse doit avoir un usage défini.
- Auto-suspend doit être activé sauf exception documentée.
- Auto-resume doit être activé lorsque compatible avec l’usage.
- Les workloads incompatibles doivent être isolés.
- Les warehouses doivent être monitorés.
- Les tailles doivent être revues régulièrement.
- Les warehouses inutilisés doivent être suspendus ou supprimés.
- Les warehouses ne doivent pas être partagés entre charges critiques et expérimentales.
- Les warehouses doivent être rattachés à resource monitors si pertinent.

---

## 9.2 Typologie de warehouses

| Warehouse | Usage | Recommandation |
|---|---|---|
| `ETL_<SIZE>_WH` | Chargements et transformations batch | Isoler par domaine ou criticité |
| `BI_<SIZE>_WH` | Requêtes interactives BI | Optimiser concurrence et latence |
| `DS_<SIZE>_WH` | Data science / exploration | Limiter quotas et plages horaires |
| `ADMIN_XS_WH` | Administration légère | Très petit, contrôlé |
| `MONITORING_XS_WH` | Jobs de monitoring | Petit, auto-suspend court |
| `DBT_<SIZE>_WH` | Transformations dbt | Séparer DEV/PROD |
| `ADHOC_<SIZE>_WH` | Analyses ponctuelles | Quotas stricts |
| `CRITICAL_ETL_WH` | Pipelines critiques | Monitoring renforcé |

---

## 9.3 Auto-suspend et auto-resume

### Règles

- Auto-suspend doit être activé.
- La valeur doit être adaptée au workload.
- Les warehouses interactifs peuvent avoir un délai plus long si justifié.
- Les warehouses batch doivent avoir un délai court.
- Les exceptions doivent être approuvées.
- Les paramètres doivent être revus après incidents de coût.

### Recommandations indicatives

| Usage | Auto-suspend indicatif |
|---|---:|
| Admin / monitoring | 60 à 120 secondes |
| Développement | 60 à 300 secondes |
| ETL batch | 60 à 300 secondes |
| BI interactive | 300 à 600 secondes selon usage |
| Data science | 300 secondes ou quota strict |
| Warehouse critique haute disponibilité | Exception documentée |

---

## 9.4 Dimensionnement

### Règles

- Commencer petit, mesurer, puis ajuster.
- Ne pas surdimensionner par défaut.
- Tester avec données réelles.
- Séparer le problème de latence du problème de concurrence.
- Utiliser multi-cluster pour concurrence, pas pour accélérer une seule requête.
- Optimiser SQL et données avant augmentation permanente de taille.
- Documenter tout passage à une taille supérieure.
- Revoir mensuellement les warehouses coûteux.

---

## 9.5 Multi-cluster warehouses

À utiliser pour :

- forte concurrence ;
- workloads BI avec pics ;
- plusieurs utilisateurs simultanés ;
- files d’attente récurrentes.

### Règles

- Définir `MIN_CLUSTER_COUNT` et `MAX_CLUSTER_COUNT`.
- Choisir scaling policy selon besoin.
- Monitorer queue time.
- Monitorer crédits.
- Ne pas utiliser multi-cluster pour compenser une mauvaise modélisation.
- Documenter le seuil d’activation.

---

## 9.6 Resource monitors

### Règles

- Les comptes PROD doivent avoir un resource monitor account-level.
- Les warehouses critiques ou coûteux doivent avoir des monitors dédiés.
- Les seuils doivent déclencher notifications et actions.
- Les contacts d’alerte doivent être maintenus.
- Les alertes doivent être suivies.
- Ignorer une alerte crédit sans action est interdit.
- Les seuils doivent être révisés avec FinOps.

### Exemple

```sql
CREATE OR REPLACE RESOURCE MONITOR RM_BI_STANDARD_MONTHLY
  WITH CREDIT_QUOTA = 1000
  FREQUENCY = MONTHLY
  START_TIMESTAMP = IMMEDIATELY
  TRIGGERS
    ON 50 PERCENT DO NOTIFY
    ON 80 PERCENT DO NOTIFY
    ON 95 PERCENT DO NOTIFY
    ON 100 PERCENT DO SUSPEND;

ALTER WAREHOUSE BI_STANDARD_WH
  SET RESOURCE_MONITOR = RM_BI_STANDARD_MONTHLY;
```

---

## 9.7 Query Acceleration Service

Query Acceleration Service peut être envisagé pour réduire l’impact de certaines requêtes lourdes ou atypiques.

### Règles

- Activer seulement après analyse du workload.
- Mesurer avant/après.
- Vérifier coût additionnel.
- Prioriser optimisation SQL/données avant activation globale.
- Documenter les warehouses concernés.
- Monitorer usage et bénéfice.

---

## 9.8 Search Optimization Service

Search Optimization Service peut être utile pour :

- point lookups ;
- filtres sélectifs ;
- recherches fréquentes sur colonnes spécifiques ;
- tables volumineuses avec accès ciblés.

### Règles

- Utiliser uniquement sur cas d’usage validé.
- Mesurer le gain.
- Surveiller coût de maintenance.
- Éviter activation large non ciblée.
- Documenter les colonnes et requêtes concernées.

---

## 9.9 Clustering et materialized views

### Clustering

À envisager pour grandes tables avec filtres récurrents et micro-partitions mal alignées.

Règles :

- analyser query patterns ;
- éviter clustering prématuré ;
- surveiller coût de reclustering ;
- définir clés simples ;
- revoir périodiquement.

### Materialized views

À envisager pour :

- requêtes répétitives coûteuses ;
- agrégations stables ;
- sous-ensembles fréquents.

Règles :

- mesurer coût de maintenance ;
- documenter dépendances ;
- tester fraîcheur ;
- éviter prolifération.

---

## 10. Ingestion et chargement

## 10.1 Principes

Les pipelines d’ingestion doivent être :

- idempotents ;
- rejouables ;
- auditables ;
- monitorés ;
- compatibles avec schema drift contrôlé ;
- capables d’isoler les erreurs ;
- alignés sur un data contract ;
- sécurisés de bout en bout.

---

## 10.2 Stages

### Règles

- Les stages doivent être nommés selon convention.
- Les stages externes doivent utiliser des integrations sécurisées.
- Les accès aux stages doivent être limités.
- Les credentials statiques sont interdits si une integration sécurisée est disponible.
- Les fichiers doivent suivre une convention.
- Les stages doivent avoir un owner.
- Les stages PROD doivent être monitorés.

### Convention fichiers

```text
/source=<source_system>/entity=<entity>/load_date=YYYY-MM-DD/file_timestamp=YYYYMMDDHHMMSS/
```

Exemple :

```text
/source=crm/entity=customer/load_date=2026-06-12/customer_20260612070000_001.json.gz
```

---

## 10.3 File formats

### Règles

- Les file formats doivent être déclarés explicitement.
- Les paramètres doivent être documentés.
- Les valeurs nulles doivent être normalisées.
- L’encodage doit être défini.
- La compression doit être standardisée.
- Les changements de format doivent être versionnés.

### Exemple

```sql
CREATE OR REPLACE FILE FORMAT RAW.FF_CSV_PIPE_GZIP
  TYPE = CSV
  FIELD_DELIMITER = '|'
  SKIP_HEADER = 1
  NULL_IF = ('', 'NULL', 'null')
  EMPTY_FIELD_AS_NULL = TRUE
  COMPRESSION = GZIP
  FIELD_OPTIONALLY_ENCLOSED_BY = '"'
  ENCODING = 'UTF8';
```

---

## 10.4 COPY INTO

### Règles

- Utiliser `COPY INTO` avec file format explicite.
- Capturer l’historique de chargement.
- Utiliser validation avant chargement pour fichiers critiques.
- Gérer les erreurs selon criticité.
- Stocker metadata de fichier si nécessaire.
- Éviter chargement non déterministe.
- Prévoir table de rejets/quarantaine.

### Exemple

```sql
COPY INTO RAW.RAW_CRM_CUSTOMER
FROM @RAW.STG_S3_CRM_JSON/customer/
FILE_FORMAT = (FORMAT_NAME = RAW.FF_JSON_GZIP)
MATCH_BY_COLUMN_NAME = CASE_INSENSITIVE
ON_ERROR = 'CONTINUE';
```

---

## 10.5 Snowpipe

Snowpipe permet de charger des fichiers depuis un stage en micro-batch lorsqu’ils deviennent disponibles.

### Cas d’usage

- ingestion quasi continue de fichiers ;
- événements cloud storage ;
- latence en minutes ;
- découplage source/warehouse manuel.

### Règles

- Documenter notification mechanism.
- Monitorer pipes.
- Capturer erreurs.
- Prévoir reprise.
- Ne pas utiliser Snowpipe sans stratégie de schema drift.
- Monitorer coûts.
- Tester les fichiers invalides.
- Prévoir table de chargement audit.

---

## 10.6 Snowpipe Streaming

Snowpipe Streaming permet de charger des données directement en lignes sans staging fichiers intermédiaires.

### Cas d’usage

- CDC near-real-time ;
- IoT ;
- logs applicatifs ;
- fraude ;
- monitoring opérationnel ;
- événements haute fréquence.

### Règles

- Valider besoin de latence.
- Éviter si batch suffit.
- Définir contrat de schéma.
- Gérer schema evolution selon politique.
- Monitorer débit, erreurs, coûts.
- Sécuriser service users.
- Tester reprise et doublons.
- Documenter ordering et late events.

---

## 10.7 Schema drift

### Règles

- Toute dérive de schéma doit être détectée.
- Les ajouts de colonnes peuvent être tolérés selon data contract.
- Les suppressions/renommages sont breaking changes.
- Les changements de type doivent être bloquants sauf compatibilité documentée.
- Les pipelines doivent alerter.
- Les données invalides doivent être isolées.
- Les consommateurs doivent être informés en cas de changement impactant.

### Matrice de compatibilité

| Changement | Statut par défaut |
|---|---|
| Ajout colonne nullable | Compatible sous condition |
| Ajout colonne non nullable | Breaking change |
| Suppression colonne | Breaking change |
| Renommage colonne | Breaking change |
| Changement type compatible | À valider |
| Changement type incompatible | Bloquant |
| Changement signification métier | Bloquant |
| Changement de granularité | Bloquant majeur |

---

## 10.8 Idempotence

Un pipeline est idempotent s’il peut être rejoué sans créer de doublons ni incohérences.

### Patterns recommandés

- load_id unique ;
- file_name + row_number ;
- checksum ;
- staging puis merge ;
- tables audit ;
- déduplication déterministe ;
- transactions ;
- quarantine ;
- upsert contrôlé.

### Exemple de clé technique

```sql
MD5(CONCAT_WS('|', source_system, file_name, row_number, business_key))
```

---

## 10.9 Quarantaine

### Règles

Les lignes ou fichiers invalides doivent être isolés dans un schéma `QUARANTINE`.

Informations minimales :

- source ;
- fichier ;
- ligne ;
- payload brut ;
- raison du rejet ;
- date de rejet ;
- pipeline ;
- run_id ;
- sévérité ;
- statut de résolution.

---

## 11. Transformations, orchestration et pipelines

## 11.1 Positionnement des transformations

### Règles

- Les transformations critiques doivent être versionnées.
- Les transformations doivent être testables.
- La logique métier doit être documentée.
- Les transformations doivent être observables.
- Les transformations doivent être idempotentes.
- Les dépendances doivent être explicites.
- Les coûts doivent être monitorés.
- Les transformations ne doivent pas être cachées dans des scripts locaux non versionnés.

---

## 11.2 Streams

Les streams suivent les changements d’une table et permettent des traitements CDC.

### Cas d’usage

- détection des inserts/updates/deletes ;
- pipelines incrémentaux ;
- synchronisation de tables ;
- traitement d’événements ;
- propagation de changements.

### Règles

- Chaque stream doit avoir un consumer identifié.
- Le délai de consommation doit être compatible avec la rétention.
- Les streams non consommés doivent être alertés.
- Les changements doivent être consommés transactionnellement.
- Les streams doivent être documentés.
- Les streams doivent être monitorés.

---

## 11.3 Tasks

Les tasks exécutent du SQL selon un planning ou une dépendance.

### Règles

- Les tasks doivent avoir un owner.
- Les schedules doivent être documentés.
- Les dépendances doivent former un DAG clair.
- Les retries doivent être gérés.
- Les erreurs doivent alerter.
- Les tasks critiques doivent être monitorées.
- Les tasks suspendues doivent être détectées.
- Les changements doivent passer par CI/CD.
- Les tasks doivent utiliser un warehouse adapté ou serverless si validé.

---

## 11.4 Streams + Tasks

Pattern recommandé pour traitement incrémental :

```text
Source Table
    ↓
Stream
    ↓
Task
    ↓
MERGE into Target Table
    ↓
Audit + Monitoring
```

Exemple conceptuel :

```sql
CREATE OR REPLACE STREAM STRM_CUSTOMER_CHANGES
ON TABLE RAW.RAW_CRM_CUSTOMER;

CREATE OR REPLACE TASK TSK_CUSTOMER_MERGE_CURATED
  WAREHOUSE = ETL_M_WH
  SCHEDULE = '5 MINUTE'
  WHEN SYSTEM$STREAM_HAS_DATA('STRM_CUSTOMER_CHANGES')
AS
MERGE INTO CURATED.CUSTOMER tgt
USING (
  SELECT *
  FROM STRM_CUSTOMER_CHANGES
) src
ON tgt.customer_id = src.customer_id
WHEN MATCHED THEN UPDATE SET
  tgt.customer_name = src.customer_name,
  tgt.updated_at = CURRENT_TIMESTAMP()
WHEN NOT MATCHED THEN INSERT (
  customer_id,
  customer_name,
  created_at,
  updated_at
) VALUES (
  src.customer_id,
  src.customer_name,
  CURRENT_TIMESTAMP(),
  CURRENT_TIMESTAMP()
);
```

---

## 11.5 Dynamic Tables

Dynamic tables permettent de définir des pipelines déclaratifs en SQL avec une cible de fraîcheur.

### Cas d’usage

- transformations SQL exprimables en `SELECT` ;
- pipelines multi-étapes ;
- agrégations ;
- tables maintenues automatiquement ;
- besoin de fraîcheur contrôlée par `TARGET_LAG`.

### Règles

- Utiliser dynamic tables lorsque le besoin correspond au modèle déclaratif.
- Définir `TARGET_LAG` selon SLA réel.
- Monitorer la fraîcheur.
- Documenter dépendances.
- Tester coût et performance.
- Ne pas remplacer toute orchestration par dynamic tables sans architecture.
- Évaluer streams/tasks ou materialized views lorsque plus adaptés.

### Exemple

```sql
CREATE OR REPLACE DYNAMIC TABLE MART.DT_DAILY_SALES
  TARGET_LAG = '1 hour'
  WAREHOUSE = ETL_M_WH
AS
SELECT
  order_date::DATE AS sales_date,
  product_id,
  region_code,
  SUM(amount_eur) AS sales_amount
FROM CURATED.SALES
GROUP BY 1, 2, 3;
```

---

## 11.6 Choisir entre Dynamic Tables, Streams/Tasks et Materialized Views

| Besoin | Option recommandée |
|---|---|
| Pipeline SQL déclaratif multi-étapes | Dynamic Tables |
| CDC avec logique transactionnelle contrôlée | Streams + Tasks |
| Agrégation répétitive pour accélérer requêtes | Materialized View |
| Chargement fichiers near-real-time | Snowpipe |
| Streaming rows direct | Snowpipe Streaming |
| Orchestration externe complexe | Airflow/Dagster/dbt Cloud/etc. + Snowflake |
| Traitement très spécifique procedural | Stored procedure + task |

---

## 11.7 Orchestration externe

Lorsque l’orchestration est gérée hors Snowflake :

- Airflow ;
- Dagster ;
- dbt Cloud ;
- Azure Data Factory ;
- AWS Step Functions ;
- GitHub Actions ;
- GitLab CI ;
- outil interne.

### Règles

- Les identités doivent être service users.
- Les rôles doivent être minimaux.
- Les credentials doivent être dans secret manager.
- Les exécutions doivent être taggées avec `QUERY_TAG`.
- Les erreurs doivent être centralisées.
- Les dépendances doivent être documentées.
- Les retries doivent être configurés.
- Les SLA doivent être monitorés.

---

## 11.8 Query tags

Les pipelines et outils doivent utiliser `QUERY_TAG`.

### Format recommandé

```json
{
  "app": "dbt",
  "domain": "sales",
  "env": "prod",
  "pipeline": "sales_mart",
  "job": "daily_build",
  "owner": "data_engineering",
  "run_id": "20260612_070000"
}
```

Exemple :

```sql
ALTER SESSION SET QUERY_TAG = 'app=dbt;domain=sales;env=prod;pipeline=sales_mart;run_id=20260612_070000';
```

### Règles

- `QUERY_TAG` obligatoire pour pipelines PROD.
- `QUERY_TAG` recommandé pour requêtes ad hoc avancées.
- Les tags doivent être exploitables dans `QUERY_HISTORY`.
- Les valeurs doivent être standardisées.

---

## 12. Data modeling, performance et qualité SQL

## 12.1 Principes de conception

### Règles

- Les tables doivent avoir une granularité claire.
- Les types doivent être explicites.
- Les champs techniques doivent être documentés.
- Les clés métier doivent être identifiées.
- Les tables critiques doivent avoir des contrôles qualité.
- Les tables mart doivent être optimisées pour consommation.
- Les tables volumineuses doivent être analysées pour clustering/search optimization.
- Les requêtes critiques doivent être benchmarkées.

---

## 12.2 Table design

### Colonnes techniques recommandées

| Colonne | Usage |
|---|---|
| `source_system` | Origine |
| `source_file_name` | Traçabilité fichier |
| `source_row_number` | Traçabilité ligne |
| `ingested_at` | Date d’ingestion |
| `loaded_at` | Date de chargement |
| `updated_at` | Date de mise à jour |
| `load_id` | Identifiant run |
| `record_hash` | Détection changement |
| `is_deleted` | Soft delete si applicable |
| `valid_from` | Début validité SCD |
| `valid_to` | Fin validité SCD |
| `is_current` | Version courante |

---

## 12.3 Requêtes SQL

### Bonnes pratiques

- Sélectionner uniquement les colonnes nécessaires.
- Éviter `SELECT *` dans objets de production.
- Filtrer tôt.
- Éviter les cross joins non maîtrisés.
- Vérifier cardinalités avant joins.
- Utiliser CTEs pour lisibilité, sans surcomplexifier.
- Éviter les fonctions sur colonnes filtrées si elles empêchent pruning.
- Pré-agréger si nécessaire.
- Tester avec volumes réalistes.
- Lire les profils de requêtes.

### Anti-patterns

- `SELECT *` dans une vue contractuelle.
- Join sur colonnes texte longues.
- Transformations lourdes dans requêtes BI répétées.
- Tables mart avec granularité incohérente.
- Requêtes ad hoc massives sur warehouse BI partagé.
- Recalcul permanent d’agrégats stables.
- Absence de filtres temporels sur tables volumineuses.
- Utilisation de PROD comme sandbox.

---

## 12.4 Optimisation

Options à considérer selon cas :

- taille warehouse ;
- multi-cluster ;
- Query Acceleration Service ;
- Search Optimization Service ;
- clustering ;
- materialized views ;
- dynamic tables ;
- pré-agrégations ;
- refactoring SQL ;
- réduction de cardinalité ;
- meilleure modélisation ;
- séparation workloads ;
- cache via patterns répétitifs.

### Règle

Augmenter la taille du warehouse doit rarement être la première réponse. La première réponse doit être l’analyse du query profile, de la modélisation et du pattern d’accès.

---

## 13. Time Travel, Fail-safe, cloning et continuité

## 13.1 Time Travel

Time Travel permet d’interroger ou restaurer des objets à un état antérieur pendant la période de rétention configurée.

### Règles

- La rétention doit être définie par criticité.
- Les coûts de stockage historique doivent être pris en compte.
- Les tables non critiques peuvent avoir une rétention réduite.
- Les tables critiques peuvent nécessiter une rétention plus longue selon édition et politique.
- Les procédures de restauration doivent être testées.
- Les utilisateurs doivent comprendre que Time Travel n’est pas un backup complet d’entreprise.

---

## 13.2 Fail-safe

Fail-safe est un mécanisme de récupération géré par Snowflake après la période Time Travel.

### Règles

- Ne pas considérer Fail-safe comme un outil opérationnel standard.
- Ne pas promettre des restaurations utilisateur basées sur Fail-safe.
- Documenter la différence Time Travel / Fail-safe.
- Définir des backups/réplication si RTO/RPO l’exigent.

---

## 13.3 Zero-copy cloning

Zero-copy cloning permet de créer rapidement des copies logiques d’objets.

### Cas d’usage

- tests ;
- UAT ;
- debugging ;
- développement ;
- rollback ;
- investigations ;
- simulation de migration.

### Règles

- Les clones de données sensibles doivent respecter les politiques de sécurité.
- Les clones PROD vers non-PROD doivent être contrôlés.
- Les coûts liés à la rétention et divergence doivent être monitorés.
- Les clones temporaires doivent avoir date d’expiration.
- Les clones doivent être taggés.
- Les clones ne doivent pas devenir des sources permanentes non gouvernées.

---

## 13.4 Replication et failover

### Cas d’usage

- disaster recovery ;
- haute disponibilité ;
- migration région/cloud ;
- continuité d’activité ;
- partage contrôlé multi-région.

### Règles

- Définir RTO/RPO par domaine.
- Configurer replication/failover groups selon criticité.
- Tester failover.
- Documenter les objets inclus/exclus.
- Comprendre les limitations de réplication.
- Monitorer statut de réplication.
- Prévoir runbook de bascule.
- Tester accès et rôles après bascule.

---

## 13.5 RTO/RPO

| Niveau | RTO | RPO | Exemple |
|---|---:|---:|---|
| Critical | < 4h | < 1h | Finance closing, opérations critiques |
| High | < 24h | < 4h | Reporting exécutif |
| Medium | < 48h | < 24h | Analytics périodique |
| Low | Best effort | Best effort | Sandbox, exploration |

---

## 14. Data sharing et consommation externe

## 14.1 Principes

Tout partage de données doit être :

- contractuel ;
- limité ;
- sécurisé ;
- auditable ;
- validé juridiquement ;
- validé sécurité ;
- documenté ;
- réversible.

---

## 14.2 Data sharing interne

### Règles

- Utiliser des vues contractuelles.
- Éviter partage direct de tables brutes.
- Documenter owner et SLA.
- Protéger données sensibles.
- Versionner les vues exposées.
- Notifier les breaking changes.
- Monitorer consommation.

---

## 14.3 Data sharing externe

### Obligations

- validation juridique ;
- validation RSSI ;
- validation Data Governance ;
- classification des données ;
- contrat de partage ;
- durée de validité ;
- périmètre des objets ;
- restrictions d’usage ;
- procédure de révocation ;
- monitoring ;
- point de contact.

### Interdits

- exposer données sensibles sans base légale et protection ;
- partager RAW directement ;
- partager objets non documentés ;
- accorder accès illimité ;
- oublier révocation après fin contrat ;
- créer un share sans owner.

---

## 14.4 Vues contractuelles

Les vues contractuelles doivent :

- masquer la complexité backend ;
- stabiliser les noms colonnes ;
- exposer uniquement le nécessaire ;
- intégrer règles de sécurité ;
- être versionnées ;
- documenter schéma et SLA ;
- éviter `SELECT *`.

Exemple :

```sql
CREATE OR REPLACE SECURE VIEW SALES_PRD.MART.SVW_SALES_CONTRACT_V1 AS
SELECT
  sales_date,
  region_code,
  product_category,
  sales_amount_eur
FROM SALES_PRD.MART.FACT_SALES_DAILY
WHERE is_valid = TRUE;
```

---

## 15. Observabilité, audit et monitoring

## 15.1 Principes

Snowflake doit être monitoré comme une plateforme de production.

### Dimensions à monitorer

- usage compute ;
- usage storage ;
- coûts crédits ;
- query history ;
- warehouse load ;
- erreurs de pipelines ;
- durée jobs ;
- freshness ;
- volumes traités ;
- rejected rows ;
- schema drift ;
- accès données sensibles ;
- grants ;
- users inactifs ;
- roles excessifs ;
- shares ;
- tasks ;
- pipes ;
- dynamic tables ;
- replication ;
- incidents.

---

## 15.2 Account Usage

Le schéma `SNOWFLAKE.ACCOUNT_USAGE` doit être utilisé pour le monitoring account-level.

Vues utiles :

| Vue | Usage |
|---|---|
| `QUERY_HISTORY` | Requêtes, durées, erreurs, users, warehouses |
| `WAREHOUSE_METERING_HISTORY` | Crédits par warehouse |
| `WAREHOUSE_LOAD_HISTORY` | Charge et queueing des warehouses |
| `LOGIN_HISTORY` | Connexions utilisateurs |
| `GRANTS_TO_USERS` | Rôles accordés |
| `GRANTS_TO_ROLES` | Privilèges accordés |
| `TABLES` | Inventaire tables |
| `VIEWS` | Inventaire vues |
| `TASK_HISTORY` | Exécution tasks |
| `COPY_HISTORY` | Historique COPY |
| `ACCESS_HISTORY` | Accès objets et colonnes selon disponibilité/édition |
| `TAG_REFERENCES` | Tags appliqués |
| `POLICY_REFERENCES` | Policies appliquées |

---

## 15.3 Monitoring des coûts

### Requête exemple — crédits par warehouse

```sql
SELECT
  warehouse_name,
  DATE_TRUNC('day', start_time) AS usage_day,
  SUM(credits_used) AS credits_used
FROM SNOWFLAKE.ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY
WHERE start_time >= DATEADD('day', -30, CURRENT_TIMESTAMP())
GROUP BY 1, 2
ORDER BY usage_day DESC, credits_used DESC;
```

---

## 15.4 Monitoring des requêtes coûteuses

```sql
SELECT
  query_id,
  user_name,
  role_name,
  warehouse_name,
  start_time,
  total_elapsed_time / 1000 AS elapsed_seconds,
  bytes_scanned,
  rows_produced,
  query_text
FROM SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY
WHERE start_time >= DATEADD('day', -7, CURRENT_TIMESTAMP())
  AND execution_status = 'SUCCESS'
ORDER BY total_elapsed_time DESC
LIMIT 50;
```

---

## 15.5 Monitoring des erreurs

```sql
SELECT
  query_id,
  user_name,
  role_name,
  warehouse_name,
  start_time,
  error_code,
  error_message,
  query_text
FROM SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY
WHERE start_time >= DATEADD('day', -7, CURRENT_TIMESTAMP())
  AND execution_status = 'FAIL'
ORDER BY start_time DESC;
```

---

## 15.6 Monitoring des warehouses

Points à suivre :

- crédits utilisés ;
- temps idle ;
- queue time ;
- fréquence auto-resume ;
- durée moyenne requêtes ;
- pics d’usage ;
- utilisateurs principaux ;
- top queries ;
- workloads par query tag ;
- sous-utilisation ;
- surutilisation ;
- warehouses jamais utilisés.

---

## 15.7 Monitoring des tasks

Points à suivre :

- tasks échouées ;
- tasks suspendues ;
- durée d’exécution ;
- retard vs schedule ;
- DAG bloqué ;
- retries ;
- consommation warehouse ;
- changements de définition ;
- absence de données dans streams.

---

## 15.8 Alerting

Alertes minimales :

- dépassement seuil crédits ;
- échec pipeline critique ;
- task suspendue ;
- pipe en erreur ;
- warehouse sans auto-suspend ;
- warehouse coûteux anormal ;
- table critique non rafraîchie ;
- schema drift ;
- accès à données sensibles inhabituel ;
- rôle admin utilisé hors plage normale ;
- login suspect ;
- réplication en retard ;
- failover group en erreur.

---

## 15.9 Tableaux de bord Platform Ops

Un dashboard Snowflake Platform Ops doit inclure :

- consommation crédits par compte ;
- consommation crédits par warehouse ;
- coûts par domaine ;
- tendances quotidiennes/mensuelles ;
- top 20 requêtes coûteuses ;
- top 20 users/service users ;
- erreurs pipelines ;
- SLA freshness ;
- statut tasks/pipes ;
- ressources inutilisées ;
- accès admin ;
- accès données sensibles ;
- coverage tags/policies ;
- incidents ouverts ;
- actions FinOps.

---

## 16. Cost governance et FinOps

## 16.1 Principes

La maîtrise des coûts Snowflake repose sur :

- visibilité ;
- ownership ;
- budgets ;
- alertes ;
- isolation des workloads ;
- optimisation SQL ;
- dimensionnement adapté ;
- suppression ressources inutiles ;
- revue régulière ;
- responsabilisation des domaines.

---

## 16.2 Allocation des coûts

Chaque coût doit pouvoir être rattaché à :

- compte ;
- warehouse ;
- domaine ;
- application ;
- pipeline ;
- owner ;
- environnement ;
- query tag ;
- cost center si applicable.

### Règle

Tout warehouse PROD doit avoir un tag ou une métadonnée de cost ownership.

---

## 16.3 Revue FinOps mensuelle

### Agenda recommandé

1. Consommation mensuelle par compte.
2. Consommation par warehouse.
3. Évolution vs mois précédent.
4. Dépassements resource monitors.
5. Top requêtes coûteuses.
6. Warehouses idle ou sous-utilisés.
7. Snowpipe/Tasks/Dynamic Tables coûteux.
8. Storage growth.
9. Time Travel/Fail-safe storage.
10. Clones coûteux.
11. Search Optimization / QAS / clustering costs.
12. Actions d’optimisation.
13. Arbitrages performance vs coût.
14. Mise à jour budgets.

---

## 16.4 Actions d’optimisation

| Signal | Action |
|---|---|
| Warehouse idle coûteux | Réduire auto-suspend |
| Queue time élevé | Multi-cluster ou séparation workload |
| Requêtes lentes et coûteuses | Optimiser SQL, données, clustering |
| BI lent | Mart dédié, agrégats, warehouse BI |
| ETL coûteux | Incrémental, dynamic tables, refactoring |
| Storage en croissance | Revue rétention, clones, transient |
| Top user coûteux | Analyse usages, formation, quota |
| Sandbox coûteuse | Auto-suspend court, budget strict |
| Clustering coûteux | Revoir clés ou désactiver |
| Search optimization coûteux | Limiter aux colonnes utiles |

---

## 17. CI/CD, Infrastructure as Code et change management

## 17.1 Principes

Les objets Snowflake critiques doivent être gérés comme du code.

### À versionner

- rôles ;
- grants ;
- warehouses ;
- databases ;
- schemas ;
- tables ;
- vues ;
- dynamic tables ;
- streams ;
- tasks ;
- pipes ;
- file formats ;
- stages ;
- procedures ;
- functions ;
- masking policies ;
- row access policies ;
- tags ;
- resource monitors ;
- documentation ;
- tests ;
- runbooks.

---

## 17.2 Repositories

Structure recommandée :

```text
snowflake-platform/
│
├── README.md
├── CHANGELOG.md
├── environments/
│   ├── dev/
│   ├── uat/
│   └── prod/
│
├── roles/
│   ├── functional_roles.sql
│   ├── access_roles.sql
│   └── warehouse_roles.sql
│
├── databases/
│   ├── sales/
│   ├── finance/
│   └── customer/
│
├── governance/
│   ├── tags.sql
│   ├── masking_policies.sql
│   ├── row_access_policies.sql
│   └── classification_rules.md
│
├── warehouses/
│   ├── warehouses.sql
│   └── resource_monitors.sql
│
├── pipelines/
│   ├── stages/
│   ├── pipes/
│   ├── streams/
│   ├── tasks/
│   └── dynamic_tables/
│
├── tests/
│   ├── security_tests.sql
│   ├── data_quality_tests.sql
│   └── performance_tests.md
│
└── docs/
    ├── architecture.md
    ├── runbook.md
    ├── rbac_matrix.md
    └── cost_governance.md
```

---

## 17.3 Promotion DEV → UAT → PROD

### Règles

- Aucun changement PROD manuel non urgent.
- Toute migration doit être revue.
- Les scripts doivent être idempotents si possible.
- Les changements breaking doivent être annoncés.
- Les rollbacks doivent être prévus.
- Les tests sécurité doivent passer.
- Les tests qualité doivent passer.
- Les tests performance doivent être exécutés pour objets critiques.
- Les approbations doivent être tracées.

---

## 17.4 Pull Request checklist

```markdown
# Pull Request Checklist — Snowflake

## Sécurité

- [ ] Aucun secret dans le code
- [ ] RBAC conforme
- [ ] Pas de grant direct utilisateur
- [ ] Données sensibles protégées
- [ ] Rôles admin non utilisés pour pipeline

## Qualité

- [ ] Scripts idempotents ou procédure de rollback
- [ ] Naming convention respectée
- [ ] Owner documenté
- [ ] Tags appliqués
- [ ] Data contract mis à jour si nécessaire

## Performance

- [ ] Requêtes critiques testées
- [ ] Warehouse adapté
- [ ] Pas de scan massif non justifié
- [ ] Impact coût évalué

## Exploitation

- [ ] Monitoring prévu
- [ ] Alerting prévu
- [ ] Runbook mis à jour
- [ ] Changelog mis à jour
```

---

## 18. Qualité des données et data contracts

## 18.1 Data contract obligatoire

Chaque dataset critique doit avoir un data contract versionné.

### Contenu minimal

- nom fonctionnel ;
- nom technique ;
- owner métier ;
- owner technique ;
- domaine ;
- source ;
- schéma ;
- types ;
- nullabilité ;
- contraintes ;
- granularité ;
- règles qualité ;
- SLA ;
- fréquence ;
- rétention ;
- classification ;
- règles d’accès ;
- stratégie de schema evolution ;
- contacts support ;
- consommateurs.

---

## 18.2 Contrôles qualité

### Contrôles minimaux

| Type | Exemples |
|---|---|
| Schema | colonnes attendues, types, nullabilité |
| Unicité | clé primaire, clé métier |
| Complétude | taux de nulles |
| Validité | bornes, formats, domaines de valeurs |
| Référentiel | codes pays, devises, statuts |
| Fraîcheur | date max, retard |
| Volume | variation anormale |
| Distribution | outliers, dérive |
| Réconciliation | totaux source vs cible |
| Duplicats | doublons techniques/métier |

---

## 18.3 Tables d’audit pipeline

Table minimale :

```sql
CREATE TABLE IF NOT EXISTS OPS.PIPELINE_RUN_AUDIT (
  run_id STRING,
  pipeline_name STRING,
  environment STRING,
  started_at TIMESTAMP_LTZ,
  ended_at TIMESTAMP_LTZ,
  status STRING,
  source_system STRING,
  target_table STRING,
  rows_read NUMBER,
  rows_loaded NUMBER,
  rows_rejected NUMBER,
  error_code STRING,
  error_message STRING,
  query_id STRING,
  query_tag STRING,
  created_at TIMESTAMP_LTZ DEFAULT CURRENT_TIMESTAMP()
);
```

---

## 19. Sécurité de production et règles d’exploitation

## 19.1 Production hardening

### Obligations

- SSO/MFA/auth policies conformes.
- Network policies documentées.
- RBAC validé.
- Service users sécurisés.
- Warehouses monitorés.
- Resource monitors actifs.
- Objets sensibles taggés.
- Policies appliquées.
- Pipelines monitorés.
- Time Travel configuré.
- Runbooks disponibles.
- Alerting actif.
- Backout plan défini.
- Revues accès planifiées.

---

## 19.2 Accès PROD

### Règles

- Accès PROD par défaut en lecture uniquement pour la majorité.
- Accès écriture limité aux rôles de pipeline ou support autorisé.
- Pas de développement exploratoire en PROD.
- Pas d’objets temporaires non contrôlés en schémas PROD métier.
- Pas de manipulation manuelle de données critiques sans ticket.
- Pas de `DROP`/`TRUNCATE` en PROD sans procédure.
- Requêtes massives ad hoc à encadrer.
- Tout accès exceptionnel doit être journalisé.

---

## 19.3 Changements destructifs

Changements destructifs :

- `DROP TABLE`;
- `DROP SCHEMA`;
- `TRUNCATE`;
- suppression colonne ;
- modification type incompatible ;
- renommage colonne contractuelle ;
- changement de granularité ;
- changement de policy ;
- révocation massive de rôle ;
- désactivation task/pipe critique.

### Règles

- Approbation obligatoire.
- Analyse d’impact.
- Communication consommateurs.
- Backup/clone ou stratégie rollback.
- Fenêtre de changement.
- Validation post-changement.
- Changelog.

---

## 20. Incident management

## 20.1 Types d’incidents

| Type | Exemple |
|---|---|
| Sécurité | accès non autorisé, rôle excessif |
| Données | corruption, doublons, schéma cassé |
| Pipeline | job échoué, retard SLA |
| Performance | requêtes critiques lentes |
| Coût | explosion crédits |
| Disponibilité | warehouse suspendu à tort, dépendance source indisponible |
| Sharing | données partagées incorrectement |
| Gouvernance | données sensibles non masquées |

---

## 20.2 Runbook incident

```markdown
# Runbook Incident Snowflake

## 1. Qualification

- Incident:
- Date/heure:
- Détecté par:
- Domaine:
- Sévérité:
- Impact utilisateur:
- Impact sécurité:
- Impact coût:

## 2. Containment

- Suspendre warehouse ?
- Suspendre task ?
- Révoquer accès ?
- Stopper pipe ?
- Isoler données ?
- Informer parties prenantes ?

## 3. Diagnostic

- Query history:
- Task history:
- Pipe history:
- Warehouse usage:
- Grants:
- Recent changes:
- Source status:

## 4. Remédiation

- Action 1:
- Action 2:
- Action 3:

## 5. Validation

- Données réconciliées:
- SLA restauré:
- Sécurité validée:
- Coûts stabilisés:

## 6. Communication

- Métier:
- RSSI:
- Data Platform:
- Support:

## 7. Post-mortem

- Cause racine:
- Prévention:
- Owner:
- Deadline:
```

---

## 20.3 Sévérité

| Sévérité | Description | Exemple |
|---|---|---|
| **SEV1** | Impact critique entreprise ou sécurité | fuite données sensibles, PROD indisponible |
| **SEV2** | Impact majeur domaine ou SLA critique | pipeline finance bloqué |
| **SEV3** | Impact limité mais visible | retard reporting non critique |
| **SEV4** | Anomalie mineure | warning ou amélioration |

---

## 20.4 Post-mortem

Un post-mortem est obligatoire pour :

- SEV1 ;
- SEV2 ;
- incident sécurité ;
- dépassement coût majeur ;
- corruption de données ;
- exposition de données sensibles ;
- échec de restauration ;
- incident récurrent.

---

## 21. Checklist qualité avant mise en production

## 21.1 Sécurité

- [ ] Rôles et privilèges conformes au least privilege.
- [ ] Aucun grant direct utilisateur en PROD hors exception.
- [ ] Service users sécurisés.
- [ ] SSO/MFA/auth policies conformes.
- [ ] Network policies documentées.
- [ ] Données sensibles classifiées.
- [ ] Masking policies appliquées.
- [ ] Row access policies appliquées si nécessaire.
- [ ] Secure views utilisées si partage sensible.
- [ ] Accès admin limité.
- [ ] Revues d’accès planifiées.

---

## 21.2 Fiabilité

- [ ] Pipeline idempotent.
- [ ] Pipeline rejouable.
- [ ] Schéma validé.
- [ ] Schema drift géré.
- [ ] Contrôles qualité configurés.
- [ ] Rejets isolés.
- [ ] Audit run disponible.
- [ ] SLA documenté.
- [ ] Retry/recovery documenté.
- [ ] Runbook disponible.

---

## 21.3 Performance

- [ ] Warehouse adapté.
- [ ] Auto-suspend actif.
- [ ] Requêtes critiques benchmarkées.
- [ ] Query profile analysé.
- [ ] Tables volumineuses évaluées pour clustering/search optimization.
- [ ] BI workloads isolés.
- [ ] ETL workloads isolés.
- [ ] Multi-cluster justifié si utilisé.
- [ ] QAS/Search Optimization justifiés si utilisés.
- [ ] Pas de scan massif non justifié.

---

## 21.4 Coûts

- [ ] Resource monitor configuré.
- [ ] Estimation crédits mensuelle validée.
- [ ] Owner coût identifié.
- [ ] Query tags configurés.
- [ ] Warehouses inutilisés supprimés.
- [ ] Clones temporaires documentés.
- [ ] Retention alignée avec besoin.
- [ ] Features coûteuses monitorées.
- [ ] Alerting coût actif.

---

## 21.5 Gouvernance

- [ ] Owners documentés.
- [ ] Tags appliqués.
- [ ] Data contract disponible.
- [ ] Documentation à jour.
- [ ] RACI validé.
- [ ] Changelog disponible.
- [ ] Métadonnées gouvernance complètes.
- [ ] Support défini.
- [ ] Escalation définie.
- [ ] Dérogations enregistrées.

---

## 22. Lignes rouges

Les pratiques suivantes sont interdites :

- utiliser `ACCOUNTADMIN` pour des usages quotidiens d’engineering ;
- partager des comptes utilisateurs ;
- stocker des credentials dans Git ;
- exposer des PII sans masking ou justification ;
- accorder des privilèges directs aux utilisateurs en PROD sans exception ;
- donner `OWNERSHIP` à des rôles personnels ;
- créer des warehouses sans auto-suspend ;
- ignorer des alertes resource monitor ;
- utiliser PROD comme sandbox ;
- déployer en PROD sans revue sécurité ;
- déployer en PROD sans tests de reprise pour pipeline critique ;
- faire des `DROP` destructifs sans rollback ;
- créer des shares externes sans validation juridique/RSSI ;
- charger des données sans audit ;
- ignorer schema drift ;
- masquer des erreurs de pipeline ;
- laisser des tasks critiques suspendues sans alerte ;
- utiliser des tables RAW comme source BI directe ;
- dupliquer des données sensibles en DEV sans anonymisation ;
- utiliser un warehouse BI partagé pour des jobs lourds ;
- multiplier les features coûteuses sans mesure de valeur ;
- laisser des clones PROD sans expiration ;
- publier des vues contractuelles avec `SELECT *`.

---

## 23. Astuces expertes et gains rapides

## 23.1 Tags dès le jour 1

Mettre en place tags de domaine, owner, classification et coût dès la création des objets.

Bénéfices :

- audit facilité ;
- masking industrialisé ;
- FinOps ;
- ownership clair ;
- lineage plus exploitable.

---

## 23.2 Warehouses spécialisés

Séparer :

- ETL ;
- BI ;
- data science ;
- monitoring ;
- administration ;
- ad hoc ;
- workloads critiques.

Bénéfices :

- performance plus stable ;
- coûts mieux attribués ;
- incidents isolés ;
- tuning plus simple.

---

## 23.3 Query tags obligatoires

Les `QUERY_TAG` permettent de rattacher les coûts à un pipeline, domaine ou run.

---

## 23.4 Quarantine schema

Ne pas bloquer toute la chaîne pour quelques lignes invalides. Isoler, tracer, corriger.

---

## 23.5 Golden secure views

Exposer des vues contractuelles plutôt que les tables physiques.

Bénéfices :

- stabilité schema ;
- masquage ;
- sécurité ;
- réduction des breaking changes ;
- partage contrôlé.

---

## 23.6 Revues trimestrielles des rôles

Supprimer :

- rôles inutilisés ;
- grants dormants ;
- accès anciens prestataires ;
- service users obsolètes ;
- rôles admin excessifs.

---

## 23.7 Cost top 20

Optimiser d’abord :

- top 20 requêtes coûteuses ;
- top 20 warehouses ;
- top 20 users/service users ;
- top 20 tables en croissance ;
- top 20 tasks longues.

---

## 23.8 Clones avec date d’expiration

Tout clone temporaire doit avoir :

- owner ;
- objectif ;
- date de suppression ;
- classification ;
- ticket de référence.

---

## 24. Mode opératoire standard

## 24.1 SOP — Création d’un nouveau domaine data

1. Cadrer le domaine et les owners.
2. Définir classification des données.
3. Définir SLA et criticité.
4. Créer database/schemas selon convention.
5. Créer rôles d’accès.
6. Créer warehouses si nécessaire.
7. Appliquer tags.
8. Définir data contracts.
9. Créer pipelines ingestion.
10. Créer contrôles qualité.
11. Créer policies sécurité.
12. Créer vues contractuelles.
13. Configurer monitoring.
14. Configurer resource monitors.
15. Tester sécurité/performance/reprise.
16. Documenter.
17. Publier en UAT.
18. Valider métier.
19. Déployer en PROD.
20. Planifier revue.

---

## 24.2 SOP — Création d’un warehouse

1. Identifier workload.
2. Identifier owner.
3. Identifier environnement.
4. Choisir taille initiale.
5. Configurer auto-suspend.
6. Configurer auto-resume.
7. Définir resource monitor.
8. Créer rôle warehouse.
9. Accorder usage minimal.
10. Ajouter tags coût/domaine.
11. Tester charge.
12. Monitorer première semaine.
13. Ajuster taille ou paramètres.
14. Documenter.

---

## 24.3 SOP — Création d’un rôle

1. Définir usage.
2. Définir owner.
3. Classifier rôle : functional/access/warehouse/admin.
4. Lister privilèges nécessaires.
5. Appliquer least privilege.
6. Créer rôle.
7. Accorder privilèges.
8. Rattacher à rôle parent si nécessaire.
9. Tester avec utilisateur représentatif.
10. Documenter dans matrice RBAC.
11. Planifier revue.

---

## 24.4 SOP — Mise en production pipeline

1. Data contract validé.
2. Schéma source validé.
3. Ingestion testée.
4. Contrôles qualité testés.
5. Rejets/quarantaine testés.
6. Idempotence testée.
7. Reprocessing testé.
8. Performance testée.
9. Coût estimé.
10. Warehouse validé.
11. Sécurité validée.
12. Monitoring configuré.
13. Alerting configuré.
14. Runbook disponible.
15. UAT métier terminée.
16. Déploiement CI/CD.
17. Validation post-prod.
18. Monitoring renforcé initial.

---

## 24.5 SOP — Revue mensuelle Snowflake

1. Revue coûts.
2. Revue top warehouses.
3. Revue top queries.
4. Revue erreurs pipelines.
5. Revue SLA/freshness.
6. Revue storage growth.
7. Revue clones.
8. Revue tasks/pipes.
9. Revue accès admin.
10. Revue policies/tags.
11. Revue incidents.
12. Revue optimisations.
13. Plan d’action.

---

## 25. Gouvernance documentaire

## 25.1 Documentation obligatoire

Chaque domaine Snowflake doit maintenir :

- architecture ;
- data contracts ;
- matrice RBAC ;
- conventions objets ;
- schéma de pipelines ;
- SLA ;
- runbooks ;
- monitoring ;
- costs ownership ;
- policies ;
- changelog ;
- registre des exceptions ;
- registre incidents.

---

## 25.2 Changelog

```markdown
# Changelog — Snowflake Domain

## 2026-06-12 — Version 2.0

### Added
- Ajout table MART.FACT_SALES_DAILY.
- Ajout masking policy sur CUSTOMER.EMAIL.
- Ajout resource monitor BI_STANDARD.

### Changed
- Modification TARGET_LAG dynamic table de 4h à 1h.
- Augmentation warehouse ETL_M_WH vers ETL_L_WH pendant fenêtre batch.

### Fixed
- Correction schema drift sur champ customer_status.

### Security
- Retrait accès FR_CONSULTANT_TEMP.
- Ajout row access policy régionale.

### Cost
- Réduction auto-suspend BI_STANDARD_WH de 10 min à 5 min.

### Breaking changes
- Renommage colonne revenue_amount vers sales_amount_eur.
```

---

## 25.3 Registre des dérogations

```markdown
# Dérogation Snowflake

## Objet concerné

Nom:

## Règle concernée

Règle du standard:

## Motif

Motif détaillé:

## Impact

- Sécurité:
- Coût:
- Performance:
- Conformité:
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

## 26. Annexes

## 26.1 Template minimal de convention plateforme

```yaml
environment: PROD

accounts:
  dev: COMPANY_DEV
  uat: COMPANY_UAT
  prod: COMPANY_PRD

database_pattern: "<DOMAIN>_<ENV>"

schema_layers:
  - RAW
  - STAGING
  - CURATED
  - MART
  - REF
  - GOVERNANCE
  - SECURITY
  - OPS
  - QUARANTINE
  - SANDBOX

warehouse_classes:
  - ADMIN_XS_WH
  - MONITORING_XS_WH
  - ETL_M_WH
  - BI_M_WH
  - DS_M_WH
  - ADHOC_XS_WH

rbac_principles:
  - least_privilege
  - separation_of_duties
  - no_direct_user_grants_in_prod
  - no_admin_roles_for_daily_engineering
  - quarterly_access_review

security:
  authentication:
    - sso
    - mfa
    - key_pair_for_service_users
  network:
    - network_policies_prod
  data_protection:
    - masking_policies
    - row_access_policies
    - tag_based_governance

monitoring:
  cost_alerts:
    - 50_percent
    - 80_percent
    - 95_percent
    - 100_percent_suspend
  operational_alerts:
    - task_failure
    - pipe_failure
    - schema_drift
    - freshness_sla_breach
    - admin_role_usage
```

---

## 26.2 Template data contract

```yaml
name: sales_transactions
technical_name: SALES_PRD.CURATED.SALES_TRANSACTION
domain: SALES
owner_business: Sales Operations
owner_technical: Data Engineering
data_steward: Sales Data Steward
classification: CONFIDENTIAL
sla:
  availability: "D+0 07:00 CET"
  freshness: "daily"
  criticality: HIGH
granularity: "1 row = 1 validated sales transaction line"
source_systems:
  - CRM
  - ERP
keys:
  business:
    - transaction_id
    - line_id
  technical:
    - load_id
    - record_hash
schema:
  - name: transaction_id
    type: STRING
    nullable: false
  - name: line_id
    type: STRING
    nullable: false
  - name: transaction_date
    type: DATE
    nullable: false
  - name: customer_id
    type: STRING
    nullable: false
  - name: product_id
    type: STRING
    nullable: false
  - name: sales_amount_eur
    type: NUMBER(18,2)
    nullable: false
quality_rules:
  - "transaction_date <= CURRENT_DATE"
  - "sales_amount_eur >= 0"
  - "transaction_id is not null"
  - "duplicate rate on transaction_id,line_id = 0"
schema_evolution:
  add_nullable_column: allowed_with_notice
  drop_column: breaking_change
  rename_column: breaking_change
security:
  masking:
    - customer_id
  row_access:
    - region_code
retention:
  raw: "90 days"
  curated: "7 years"
consumers:
  - Power BI Sales Dashboard
  - Finance Revenue Reconciliation
```

---

## 26.3 Template matrice RBAC

```markdown
# RBAC Matrix

| Role | Type | Parent Role | Objects | Privileges | Owner | Review Frequency |
|---|---|---|---|---|---|---|
| FR_SALES_ANALYST | Functional | N/A | N/A | N/A | Sales Analytics | Quarterly |
| AR_SALES_MART_RO | Access | FR_SALES_ANALYST | SALES_PRD.MART | SELECT | Data Platform | Quarterly |
| WH_BI_STANDARD_USE | Warehouse | FR_SALES_ANALYST | BI_STANDARD_WH | USAGE | Data Platform | Quarterly |
```

---

## 26.4 Template warehouse definition

```sql
CREATE OR REPLACE WAREHOUSE BI_STANDARD_WH
  WAREHOUSE_SIZE = 'MEDIUM'
  AUTO_SUSPEND = 300
  AUTO_RESUME = TRUE
  INITIALLY_SUSPENDED = TRUE
  COMMENT = 'BI interactive warehouse for certified dashboards. Owner: BI Platform.';
```

---

## 26.5 Template monitoring query — warehouses sans auto-suspend

```sql
SHOW WAREHOUSES;

-- Exporter le résultat puis vérifier:
-- auto_suspend IS NULL ou valeur trop élevée
-- owner absent
-- warehouse non utilisé
```

---

## 26.6 Template monitoring query — users inactifs

```sql
SELECT
  user_name,
  MAX(event_timestamp) AS last_login
FROM SNOWFLAKE.ACCOUNT_USAGE.LOGIN_HISTORY
WHERE event_timestamp >= DATEADD('day', -180, CURRENT_TIMESTAMP())
GROUP BY user_name
HAVING MAX(event_timestamp) < DATEADD('day', -90, CURRENT_TIMESTAMP())
ORDER BY last_login;
```

---

## 26.7 Template monitoring query — grants directs utilisateurs

```sql
SELECT
  grantee_name,
  role,
  created_on
FROM SNOWFLAKE.ACCOUNT_USAGE.GRANTS_TO_USERS
WHERE deleted_on IS NULL
ORDER BY created_on DESC;
```

---

## 26.8 Template incident coût

```markdown
# Cost Incident Report

## Résumé

Date:
Compte:
Warehouse:
Montant/crédits:
Écart vs baseline:

## Détection

- Resource monitor:
- Dashboard:
- Alerte manuelle:

## Cause

- Requête coûteuse
- Warehouse laissé actif
- Explosion volume
- Feature coûteuse
- Mauvais dimensionnement
- Autre:

## Requêtes concernées

Query IDs:

## Actions immédiates

- Suspend warehouse:
- Ajuster auto-suspend:
- Stopper pipeline:
- Informer owner:

## Prévention

- Resource monitor:
- Query tag:
- Optimisation SQL:
- Formation:
- Revue architecture:
```

---

## 26.9 Confidentialité

Certaines données, exemples, noms de comptes, noms de domaines, requêtes, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, à l’édition Snowflake utilisée, aux règles de gouvernance, aux politiques de sécurité, à l’architecture cloud et aux contraintes métier de l’organisation.

---

## 26.10 Résumé exécutif du standard

Une plateforme Snowflake corporate doit être :

- sécurisée par design ;
- gouvernée par RBAC ;
- isolée par environnement ;
- monitorée ;
- optimisée en coûts ;
- documentée ;
- automatisée ;
- auditable ;
- résiliente ;
- conforme ;
- performante ;
- maintenable.

La qualité d’une plateforme Snowflake ne se mesure pas uniquement à sa capacité à stocker et traiter des données. Elle se mesure à sa capacité à fournir durablement des données fiables, sécurisées, traçables, performantes et économiquement maîtrisées à l’ensemble de l’organisation.
