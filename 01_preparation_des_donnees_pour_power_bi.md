# Standard Corporate Power BI — Préparation des Données comme Source Optimale

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Préparation des données pour une exploitation optimale dans Power BI |
| **Version** | 2.0 |
| **Date** | 2026-06-10 |
| **Auteur / Rôle** | Équipe BI & Data Governance |
| **Validation / Approbation** | Responsable BI, Data Architect, Lead Data Engineer, Lead Analytics Engineer, RSSI si applicable |
| **Public cible** | Data Engineers, BI Developers, Analytics Engineers, Data Analysts, Product Owners Data, Data Stewards, Administrateurs Power BI / Fabric |
| **Commanditaire** | Direction Data & Performance / Direction Métier |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Tous les jeux de données utilisés dans Power BI : Import, DirectQuery, Direct Lake, Dataflows, Dataflows Gen2, Lakehouse, Warehouse, SQL views, fichiers certifiés, APIs, pipelines et sources partagées |
| **Objectif du document** | Définir les règles, standards, contrôles qualité et bonnes pratiques permettant de préparer des données robustes, cohérentes, maintenables, sécurisées et performantes pour Power BI. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les exigences sont applicables par défaut à tout projet BI, tout semantic model Power BI, tout dataflow ou toute source destinée à une consommation analytique dans Power BI.

### Interprétation des termes

| Terme | Signification |
|---|---|
| **MUST** | Exigence obligatoire, non négociable hors dérogation formelle. |
| **SHOULD** | Exigence fortement recommandée ; toute exception doit être justifiée et documentée. |
| **MAY** | Option autorisée selon le contexte, sans contradiction avec les exigences MUST/SHOULD. |
| **MUST NOT** | Interdiction explicite. |
| **Exception documentée** | Dérogation validée, limitée dans le temps, avec motif, impact, approbateur nommé et plan de remédiation. |

### Mode de rédaction

Les règles sont formulées de manière courte, contrôlable et orientée exécution. Le standard vise à être utilisable par les équipes Data Engineering, BI, Data Governance, sécurité et métiers.

### Gestion des exceptions

Toute dérogation doit inclure :

- le périmètre concerné ;
- le motif métier ou technique ;
- l’impact attendu sur qualité, sécurité, performance, coût ou maintenabilité ;
- la durée de validité ;
- le plan de remédiation ;
- l’approbateur nommé ;
- la date de revue.

### Précedence

En cas de conflit, l’ordre de priorité est le suivant :

1. obligations légales, réglementaires et contractuelles ;
2. politiques de sécurité, confidentialité et conformité ;
3. standards d’architecture Data / Cloud / Fabric ;
4. standards Data Governance ;
5. présent standard Power BI Data Preparation ;
6. pratiques locales d’équipe.

---

## 1.2 Définitions clés

| Terme | Définition |
|---|---|
| **BI-ready data** | Données prêtes pour la consommation analytique : typées, documentées, contrôlées, historisées si nécessaire, sécurisées et alignées avec les définitions métier. |
| **Data contract** | Accord versionné décrivant le schéma, les règles qualité, les SLA, l’ownership, la sécurité et les attentes de consommation d’un dataset. |
| **Data product** | Actif de données publié, documenté, gouverné et maintenu comme un produit réutilisable. |
| **Semantic model** | Modèle Power BI contenant tables, relations, mesures, hiérarchies, rôles de sécurité et métadonnées métier. |
| **Query folding** | Capacité de Power Query à pousser les transformations vers la source pour optimiser le traitement. |
| **Schema drift** | Évolution non maîtrisée du schéma source : ajout, suppression, renommage ou changement de type de colonne. |
| **Data quality gate** | Point de contrôle bloquant ou non bloquant permettant de valider la qualité avant exposition aux consommateurs. |
| **Quarantine table** | Table contenant les enregistrements rejetés ou suspects, avec raison de rejet et métadonnées de traitement. |
| **Incremental load** | Chargement uniquement des données nouvelles ou modifiées depuis le dernier traitement. |
| **Idempotence** | Capacité d’un pipeline à être relancé sans créer de doublons ni modifier incorrectement le résultat final. |
| **Lineage** | Traçabilité du parcours d’une donnée depuis la source jusqu’au rapport ou KPI consommé. |
| **SLA** | Service Level Agreement : niveau de service attendu, par exemple disponibilité des données à une heure donnée. |
| **SLO** | Service Level Objective : objectif opérationnel mesurable lié à la qualité ou disponibilité du service. |
| **PII** | Personally Identifiable Information : données personnelles permettant d’identifier directement ou indirectement une personne. |
| **RLS** | Row-Level Security : sécurité au niveau des lignes. |
| **OLS** | Object-Level Security : sécurité au niveau des tables ou colonnes. |
| **SCD** | Slowly Changing Dimension : dimension dont les changements d’attributs peuvent être historisés. |

---

## 2. Vision, principes directeurs et résultats attendus

## 2.1 Vision

Produire des données **BI-ready dès l’amont**, avec une logique métier explicite, une qualité vérifiable, une sécurité native et une structure compatible avec les contraintes de modélisation, de performance et de gouvernance Power BI.

La préparation des données pour Power BI ne doit pas être considérée comme une suite de corrections locales dans chaque rapport. Elle doit être traitée comme une chaîne industrielle : contrats de données, pipelines contrôlés, tests qualité, documentation, monitoring, ownership et publication de sources fiables.

---

## 2.2 Principes directeurs

1. **Data Quality by Design**  
   La qualité se construit dans les sources, pipelines et couches préparées, pas uniquement dans les rapports.

2. **Single Source of Truth**  
   Chaque indicateur critique doit avoir une définition unique, validée et traçable.

3. **Semantic Readiness**  
   Les données préparées doivent être directement exploitables pour la modélisation sémantique : grain clair, clés fiables, dimensions stabilisées, types corrects.

4. **Minimiser la transformation tardive**  
   Les transformations lourdes doivent être centralisées dans l’ETL/ELT, le Lakehouse, le Warehouse ou les dataflows partagés, pas dupliquées dans chaque rapport.

5. **Shift-left Data Governance**  
   Les règles de qualité, sécurité, conformité et documentation doivent être intégrées dès la conception des pipelines.

6. **Traçabilité et auditabilité**  
   Chaque champ critique doit pouvoir être expliqué : origine, transformation, règle de calcul, owner, fréquence de mise à jour.

7. **Security & Privacy by Design**  
   Les données sensibles doivent être minimisées, classifiées, masquées, pseudonymisées ou protégées selon les exigences.

8. **Performance pragmatique**  
   La granularité, la cardinalité, les modes de stockage, les partitions et la fraîcheur doivent être optimisés selon les usages réels.

9. **Reusable by Default**  
   Les transformations communes doivent être mutualisées et versionnées.

10. **Observable and Operable**  
   Les flux de données doivent être monitorés : volumes, fraîcheur, erreurs, rejets, temps de traitement, dérives de schéma et disponibilité.

---

## 2.3 Résultats attendus

L’application de ce standard doit permettre de :

- réduire les anomalies de reporting ;
- diminuer le temps de préparation des semantic models ;
- accélérer les refresh Power BI ;
- réduire les échecs de chargement ;
- améliorer la cohérence des KPI entre équipes ;
- réduire la duplication de transformations ;
- améliorer la traçabilité des données ;
- améliorer la conformité RGPD et sécurité ;
- fiabiliser les sources certifiées ;
- faciliter l’onboarding des équipes BI/Data ;
- réduire la dette technique dans Power Query, DAX et les rapports ;
- augmenter la confiance métier dans les données publiées.

---

## 2.4 Indicateurs de succès du standard

| Indicateur | Cible recommandée |
|---|---:|
| Datasets critiques avec data contract versionné | 100 % |
| Colonnes critiques documentées | 100 % |
| Sources certifiées avec owner métier et technique | 100 % |
| Flux critiques monitorés | 100 % |
| Échecs de refresh non traités | 0 |
| Schema drift non approuvé en production | 0 |
| Taux de duplication critique dans dimensions de référence | 0 % ou seuil approuvé |
| Taux de rejets non expliqués | 0 % |
| KPI critiques avec définition validée | 100 % |
| Données PII exposées sans justification | 0 |
| Transformations dupliquées entre rapports | Réduction continue |
| Temps moyen de diagnostic incident data | Réduction continue |

---

## 3. Rôles et responsabilités

## 3.1 RACI simplifié

| Activité | Data Engineer | Analytics Engineer | BI Developer | Data Steward | Métier / PO | Data Architect | Admin Power BI / Fabric | RSSI / Security Officer |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Contrat de données | R | C | C | A | C | C | I | C |
| Définition du grain | R | R | C | C | A/R | C | I | I |
| Règles de qualité | R | C | C | A | C | C | I | C |
| Définitions métier | C | C | C | C | A/R | C | I | I |
| Design pipeline | R | C | I | C | C | A | C | C |
| Transformation SQL/ELT | R | R | C | C | I | C | I | I |
| Dataflows / Power Query partagé | C | R | R | C | C | C | C | I |
| Publication source certifiée | R | R | C | A | C | C | A/R | C |
| Documentation / glossaire | C | C | C | A/R | C | I | I | I |
| Classification des données | C | C | I | R | C | C | I | A |
| Sécurité & conformité | C | I | I | C | I | C | C | A/R |
| Monitoring et alerting | R | C | C | C | I | C | A/R | I |
| Gestion des incidents data | R | C | C | C | C | C | C | C |

**Légende** :

- **R** : Responsable exécution.
- **A** : Accountable, responsable de la validation finale.
- **C** : Consulted, consulté avant décision.
- **I** : Informed, informé.

---

## 3.2 Ownership obligatoire

Chaque source destinée à Power BI doit avoir :

- un **Business Owner** ;
- un **Technical Owner** ;
- un **Data Steward** pour les données critiques ;
- un **Support Contact** ;
- une **classification de confidentialité** ;
- une **fréquence de mise à jour** ;
- un **SLA de disponibilité** ;
- une **politique de rétention** ;
- un **niveau de certification** ;
- une **documentation accessible** ;
- un **processus de gestion des changements**.

Aucun dataset critique ne doit être exposé à Power BI sans ownership explicite.

---

## 4. Architecture cible de préparation des données

## 4.1 Positionnement dans la chaîne analytique

Architecture de référence :

```text
Sources opérationnelles / APIs / fichiers / événements
        ↓
Ingestion contrôlée
        ↓
Landing / Raw / Bronze
        ↓
Standardisation / Nettoyage / Silver
        ↓
Business Rules / Conformed Data / Gold
        ↓
Sources BI-ready / Data Products certifiés
        ↓
Power BI Semantic Models
        ↓
Rapports, Apps, Excel, Self-service BI
```

Le modèle Power BI doit consommer prioritairement des données issues d’une couche **Gold**, **curated**, **serving**, **warehouse** ou **data product** documentée.

---

## 4.2 Responsabilité des couches

| Couche | Responsabilité principale | Exemples de traitements |
|---|---|---|
| **Raw / Bronze** | Conservation fidèle des données sources | ingestion brute, horodatage, metadata source |
| **Silver** | Nettoyage, typage, normalisation technique | parsing dates, dédoublonnage, standardisation codes |
| **Gold** | Règles métier et données BI-ready | KPI pré-calculés si nécessaire, dimensions conformes, faits analytiques |
| **Semantic Model** | Modélisation analytique et mesures | relations, DAX, hiérarchies, RLS/OLS, formats |
| **Report** | Visualisation et interaction utilisateur | filtres, navigation, storytelling, UX |

### Règle

Les transformations doivent être positionnées au niveau le plus durable et réutilisable possible.

---

## 4.3 Ce qui doit être fait en amont de Power BI

Les traitements suivants doivent être réalisés en amont lorsque possible :

- nettoyage lourd ;
- dédoublonnage complexe ;
- harmonisation de référentiels ;
- historisation SCD ;
- règles de survivorship ;
- masquage ou pseudonymisation ;
- jointures volumineuses ;
- pré-agrégations lourdes ;
- contrôles d’intégrité ;
- parsing de fichiers complexes ;
- mapping de valeurs libres ;
- préparation des clés ;
- transformation multi-sources ;
- calculs métier partagés entre plusieurs domaines.

Power BI ne doit pas devenir une couche de correction durable de la qualité source.

---

## 4.4 Ce qui peut rester dans Power Query

Power Query peut être utilisé pour :

- sélectionner les colonnes utiles ;
- renommer les colonnes pour usage local ;
- appliquer des filtres simples ;
- typer les colonnes si le volume est raisonnable ;
- fusionner de petites tables de référence ;
- préparer des prototypes ;
- appeler un dataflow partagé ;
- réaliser des transformations légères proches de la consommation.

### Règle

Toute transformation Power Query réutilisée dans plusieurs rapports doit être candidate à une centralisation dans un dataflow, une vue SQL, un Lakehouse, un Warehouse ou un pipeline partagé.

---

## 5. Standard de préparation des données

## 5.1 Contrat de données obligatoire

Chaque source doit disposer d’un **data contract** versionné.

### Contenu minimal obligatoire

- nom fonctionnel du dataset ;
- nom technique ;
- description métier ;
- owner métier ;
- owner technique ;
- support contact ;
- schéma attendu ;
- colonnes, types, nullabilité ;
- clés primaires et étrangères ;
- contraintes métier ;
- grain ;
- fréquence de mise à jour ;
- SLA de disponibilité ;
- politique de rétention ;
- règles d’historisation ;
- règles de sécurité ;
- classification des données ;
- règles de qualité ;
- seuils d’alerte ;
- processus de changement ;
- consommateurs connus.

### Règles

- Le data contract doit être stocké dans un repository versionné.
- Toute évolution de schéma doit passer par une revue d’impact.
- Les breaking changes doivent être annoncés avant déploiement.
- Les consommateurs critiques doivent être identifiés.
- Les changements non compatibles doivent être bloqués ou versionnés.

### Anti-pattern

Modifier une colonne critique en production sans prévenir les consommateurs Power BI.

---

## 5.2 Nommage et conventions

### Règles générales

- Les noms doivent être explicites, stables et compréhensibles.
- Les noms techniques doivent éviter les espaces et caractères spéciaux.
- Les abréviations obscures doivent être interdites.
- Les conventions doivent être homogènes entre sources.
- Les noms métier doivent être alignés avec le glossaire corporate.

### Conventions techniques recommandées

| Type d’objet | Convention | Exemple |
|---|---|---|
| Table de faits | `fact_<process>` | `fact_sales` |
| Dimension | `dim_<entity>` | `dim_customer` |
| Bridge table | `bridge_<entity_a>_<entity_b>` | `bridge_customer_segment` |
| Snapshot | `fact_<entity>_snapshot` | `fact_inventory_snapshot` |
| Référence | `ref_<domain>` | `ref_currency` |
| Staging | `stg_<source>_<entity>` | `stg_crm_account` |
| Colonne ID | `<entity>_id` | `customer_id` |
| Clé substitut | `<entity>_key` | `customer_key` |
| Code | `<entity>_code` | `country_code` |
| Date | `<event>_date` | `order_date` |
| Timestamp | `<event>_timestamp` | `created_timestamp` |
| Montant | `<metric>_amount` | `sales_amount` |
| Devise | `<metric>_currency` | `sales_currency` |
| Booléen | `is_<condition>` | `is_active` |
| Flag | `<condition>_flag` | `quality_issue_flag` |

### Anti-patterns

- `col1`, `valeur2`, `date_x`, `flag_final_new` ;
- mélanger français et anglais sans règle ;
- changer fréquemment des noms de colonnes exposées ;
- utiliser des noms métier différents pour la même notion ;
- utiliser des suffixes incohérents pour les clés.

---

## 5.3 Typage des données

### Règles obligatoires

- Les dates doivent être stockées comme dates, pas comme texte.
- Les montants doivent être numériques, pas texte.
- Les booléens doivent être normalisés.
- Les timestamps doivent avoir une règle de timezone explicite.
- Les types numériques doivent être adaptés : entier, décimal, devise.
- Les colonnes de relation doivent avoir des types cohérents entre tables.
- Les colonnes utilisées pour incremental refresh doivent être compatibles avec les filtres temporels.

### Exemple de typage recommandé

| Donnée | Type recommandé |
|---|---|
| Identifiant technique | bigint / int64 |
| Clé substitut | bigint / int64 |
| Code court | string contrôlé |
| Montant financier | decimal avec précision documentée |
| Quantité entière | integer |
| Quantité mesurée | decimal |
| Date analytique | date |
| Timestamp d’événement | datetime avec timezone documentée |
| Flag | boolean |
| Pourcentage | decimal, pas texte avec `%` |

### Point Power BI

Un type incorrect peut entraîner :

- conversions coûteuses au refresh ;
- rupture du query folding ;
- mauvaise compression VertiPaq ;
- erreurs DAX ;
- tris incorrects ;
- relations impossibles ou instables.

---

## 5.4 Gestion des valeurs manquantes et invalides

### Règles obligatoires

- Distinguer `NULL` technique, `inconnu`, `non applicable`, `non renseigné` et `erreur source`.
- Documenter le traitement de chaque colonne critique.
- Définir un seuil acceptable de valeurs nulles.
- Isoler les rejets dans une table de quarantaine.
- Ne jamais remplacer silencieusement une valeur critique sans traçabilité.

### Classification recommandée

| Cas | Signification | Traitement possible |
|---|---|---|
| `NULL` technique | Valeur absente dans la source | Contrôle DQ, rejet si obligatoire |
| `Unknown` | Valeur inconnue mais attendue | Mapper vers membre `Unknown` dans dimension |
| `Not Applicable` | Valeur non pertinente | Conserver si métier validé |
| `Not Provided` | Valeur non renseignée par utilisateur/process | Monitorer taux de complétude |
| `Invalid` | Valeur non conforme | Rejet, correction ou quarantine |

### Exemple de membre inconnu dans une dimension

```text
customer_key = -1
customer_id = 'UNKNOWN'
customer_name = 'Unknown Customer'
```

Cette approche évite les pertes de lignes dans les faits lorsque la dimension n’est pas encore disponible.

---

## 5.5 Dédoublonnage et unicité

### Règles obligatoires

- Définir les clés métier et clés techniques.
- Mesurer le taux de duplication.
- Garantir l’unicité des dimensions de référence.
- Définir des règles de survivorship.
- Tracer les doublons supprimés ou fusionnés.
- Alerter en cas de duplication critique.

### Règles de survivorship possibles

| Règle | Exemple |
|---|---|
| Source prioritaire | CRM prioritaire sur fichier manuel |
| Date la plus récente | Dernière mise à jour conservée |
| Complétude maximale | Enregistrement avec plus de champs renseignés |
| Statut actif | Client actif prioritaire sur client inactif |
| Score qualité | Enregistrement avec meilleur score DQ |

### Contrôle minimal

```sql
SELECT customer_id, COUNT(*) AS row_count
FROM dim_customer
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

Tout doublon sur une clé supposée unique doit être traité avant exposition à Power BI.

---

## 5.6 Harmonisation sémantique

### Règles obligatoires

- Les unités doivent être unifiées.
- Les devises doivent être standardisées.
- Les pays doivent être normalisés selon un référentiel.
- Les segments, canaux, statuts et catégories doivent être contrôlés.
- Les valeurs libres doivent être mappées vers des listes de valeurs maîtrisées.
- Les définitions métier doivent être alignées avec le glossaire.

### Exemples

| Problème | Correction attendue |
|---|---|
| `France`, `FR`, `FRA`, `france` | Référentiel pays unique |
| `EUR`, `€`, `Euro` | Code devise standard |
| `K EUR` mélangé avec `EUR` | Unité unique ou colonne d’unité explicite |
| `B2B`, `Business`, `Corporate` | Mapping vers canal officiel |
| Statuts libres | Table de référence contrôlée |

### Règle

Un même concept métier ne doit pas être représenté par plusieurs codifications concurrentes dans les sources Power BI.

---

## 5.7 Historisation et temporalité

### Règles obligatoires

- Distinguer date de fait, date de saisie, date de modification et date de traitement.
- Définir les règles d’historisation des dimensions évolutives.
- Utiliser SCD Type 2 lorsque l’analyse historique l’exige.
- Documenter les colonnes temporelles critiques.
- Standardiser les fuseaux horaires.
- Prévoir la gestion des données tardives.

### Dates à distinguer

| Type de date | Description |
|---|---|
| `event_date` | Date de l’événement métier |
| `created_timestamp` | Date de création dans le système source |
| `updated_timestamp` | Date de dernière modification source |
| `processed_timestamp` | Date de traitement pipeline |
| `loaded_timestamp` | Date de chargement dans la couche analytique |
| `valid_from` | Début de validité d’une version historisée |
| `valid_to` | Fin de validité d’une version historisée |

### Exemple SCD Type 2

```text
dim_customer
- customer_key
- customer_id
- customer_segment
- valid_from
- valid_to
- is_current
```

### Anti-pattern

Écraser l’historique d’un segment client alors que les ventes historiques doivent rester attachées au segment valable au moment de la transaction.

---

## 5.8 Granularité

### Règles obligatoires

- La granularité cible doit être définie avant préparation.
- La granularité doit être compatible avec les KPI.
- Les pré-agrégations doivent être documentées.
- Une perte de détail doit être explicitement validée.
- Les données transactionnelles ne doivent pas être chargées si seul un agrégat est nécessaire.

### Exemples de grains

```text
fact_sales:
1 ligne = 1 transaction, 1 produit, 1 client, 1 date de commande.

fact_inventory_snapshot:
1 ligne = 1 produit, 1 entrepôt, 1 date de snapshot.

fact_budget:
1 ligne = 1 mois, 1 département, 1 compte analytique, 1 scénario.
```

### Question obligatoire

Avant exposition à Power BI, l’équipe doit pouvoir répondre :

> Une ligne de cette table représente quoi exactement ?

Si la réponse n’est pas claire, le dataset n’est pas BI-ready.

---

## 5.9 Sécurité, conformité et privacy

### Règles obligatoires

- Classifier chaque colonne selon son niveau de sensibilité.
- Limiter l’exposition des PII.
- Appliquer la minimisation des données.
- Pseudonymiser ou masquer lorsque possible.
- Respecter les contraintes RGPD et locales.
- Journaliser les transformations affectant des données sensibles.
- Définir les droits d’accès par groupes.
- Éviter les exports non contrôlés.

### Classification recommandée

| Niveau | Description | Exemples |
|---|---|---|
| **Public** | Donnée publiable sans restriction | Pays, année, catégorie publique |
| **Interne** | Donnée interne non sensible | Chiffre d’affaires agrégé, région commerciale |
| **Confidentiel** | Donnée métier sensible | Marge, budget, performance fournisseur |
| **Restreint** | Donnée personnelle ou hautement sensible | Email, téléphone, salaire, données RH |
| **Critique** | Donnée réglementée ou stratégique | Données santé, données financières individuelles, secrets commerciaux |

### Règle PII

Une donnée personnelle ne doit être exposée à Power BI que si :

- le besoin métier est justifié ;
- la finalité est documentée ;
- l’accès est restreint ;
- la rétention est définie ;
- le risque d’export est évalué ;
- les alternatives d’agrégation, pseudonymisation ou masquage ont été étudiées.

---

## 5.10 Refresh, incrémentalité et fiabilité

### Règles obligatoires

- Définir une fréquence de refresh alignée avec le besoin métier.
- Définir un SLA de disponibilité.
- Implémenter un chargement incrémental pour les volumes significatifs.
- Garantir l’idempotence des pipelines.
- Prévoir la reprise sur incident.
- Alerter sur retard de flux.
- Alerter sur volume anormal.
- Alerter sur schema drift.
- Alerter sur taux de rejet anormal.

### Stratégies de chargement

| Stratégie | Usage | Risques |
|---|---|---|
| Full refresh | Petites tables, référentiels simples | Coût élevé si volume augmente |
| Incremental by date | Faits horodatés | Données tardives à gérer |
| Incremental by ID | Sources avec ID monotone | Ne capture pas toujours les updates |
| Change Data Capture | Sources transactionnelles critiques | Complexité opérationnelle |
| Snapshot | États périodiques | Volumétrie historique |
| Merge / Upsert | Dimensions ou faits modifiables | Besoin de clé fiable |

### Exemple de politique

```text
Dataset: fact_sales
Refresh: daily
SLA: available before 07:00 CET
Load strategy: incremental by updated_timestamp
Late arriving data window: 7 days
Retention: 5 years
Failure alert: immediate
Owner: Data Platform Team
```

---

## 5.11 Gestion du schema drift

### Règles obligatoires

- Le schéma attendu doit être testé automatiquement.
- Tout ajout, retrait ou changement de type doit être détecté.
- Les breaking changes doivent être bloqués en production.
- Les changements compatibles doivent être tracés.
- Les consommateurs doivent être informés des changements impactants.

### Types de changements

| Changement | Impact | Décision recommandée |
|---|---|---|
| Ajout colonne non utilisée | Faible | Autoriser, documenter |
| Suppression colonne non critique | Moyen | Revue d’impact |
| Suppression colonne critique | Élevé | Bloquer |
| Changement de type | Élevé | Bloquer ou versionner |
| Renommage colonne | Élevé | Gérer comme breaking change |
| Changement de signification métier | Critique | Validation métier obligatoire |

---

## 5.12 Gestion des rejets et tables de quarantaine

### Règles obligatoires

Tout pipeline critique doit prévoir une stratégie de gestion des rejets.

### Structure recommandée

```text
quarantine_<dataset>
- rejection_id
- source_system
- source_file_or_batch_id
- business_key
- rejection_reason
- rejected_column
- rejected_value
- severity
- detected_timestamp
- pipeline_run_id
- remediation_status
```

### Avantages

- éviter les pertes silencieuses ;
- accélérer le diagnostic ;
- fournir une preuve d’audit ;
- permettre le retraitement ;
- mesurer la qualité source.

---

## 5.13 Data quality scoring

Les datasets critiques doivent disposer d’un score de qualité.

### Dimensions de qualité recommandées

| Dimension | Description |
|---|---|
| Complétude | Champs obligatoires renseignés |
| Unicité | Absence de doublons critiques |
| Validité | Respect des formats et domaines de valeurs |
| Cohérence | Alignement entre champs liés |
| Exactitude | Conformité avec source de vérité ou règle métier |
| Fraîcheur | Données disponibles dans les délais attendus |
| Intégrité | Clés et relations valides |
| Stabilité | Absence de dérive non approuvée |

### Exemple de score

```text
Data Quality Score =
  30 % Complétude
+ 20 % Validité
+ 20 % Unicité
+ 15 % Fraîcheur
+ 15 % Intégrité
```

Le calcul exact doit être adapté au contexte métier.

---

## 6. Checklist qualité avant exposition à Power BI

## 6.1 Contrôle schéma

- [ ] Le dataset dispose d’un data contract versionné.
- [ ] Les colonnes sont conformes au contrat.
- [ ] Les types sont conformes.
- [ ] La nullabilité est conforme.
- [ ] Les clés sont conformes.
- [ ] Aucun schema drift non approuvé n’est présent.
- [ ] Les changements récents sont documentés.

---

## 6.2 Contrôle contenu

- [ ] Le taux de nulles est sous seuil.
- [ ] Les doublons critiques sont éliminés ou tracés.
- [ ] Les bornes de valeurs métier sont respectées.
- [ ] Les formats sont valides.
- [ ] Les valeurs libres sont mappées.
- [ ] Les référentiels sont alignés.
- [ ] Les rejets sont isolés en quarantaine.
- [ ] Les anomalies critiques sont corrigées ou acceptées formellement.

---

## 6.3 Contrôle intégrité

- [ ] Les clés primaires sont uniques.
- [ ] Les clés étrangères sont valides.
- [ ] Les dimensions de référence sont complètes.
- [ ] Les faits orphelins sont traités.
- [ ] Les membres `Unknown` sont contrôlés.
- [ ] Les relations attendues sont testées.

---

## 6.4 Contrôle temporalité

- [ ] Les timestamps sont cohérents.
- [ ] Les timezones sont documentées.
- [ ] La fraîcheur respecte le SLA.
- [ ] L’historique est correctement géré.
- [ ] Les données tardives sont prises en compte.
- [ ] Les colonnes `created`, `updated`, `processed` sont distinguées.

---

## 6.5 Contrôle performance

- [ ] La volumétrie est maîtrisée.
- [ ] Les colonnes inutiles sont supprimées.
- [ ] Les colonnes haute cardinalité sont justifiées.
- [ ] Les partitions sont définies si nécessaire.
- [ ] Les filtres nécessaires à l’incremental refresh sont disponibles.
- [ ] Les jointures lourdes sont traitées en amont.
- [ ] Les agrégations sont évaluées.

---

## 6.6 Contrôle sécurité

- [ ] Les colonnes sont classifiées.
- [ ] Les PII non nécessaires sont exclues.
- [ ] Les données sensibles sont masquées ou protégées.
- [ ] Les droits d’accès sont définis.
- [ ] Les règles de rétention sont documentées.
- [ ] Les risques d’export sont évalués.
- [ ] Les transformations sensibles sont journalisées.

---

## 6.7 Contrôle documentation

- [ ] Le dictionnaire de données est à jour.
- [ ] Les définitions KPI associées sont validées.
- [ ] Le lineage est documenté.
- [ ] Le protocole de support est défini.
- [ ] Les owners sont renseignés.
- [ ] Le changelog est mis à jour.
- [ ] Les limitations connues sont documentées.

---

## 7. Bonnes pratiques d’implémentation

## 7.1 Positionnement des transformations

### Règle générale

Les transformations doivent être placées au niveau où elles apportent le plus de valeur durable.

| Transformation | Niveau recommandé |
|---|---|
| Nettoyage structurel | ETL/ELT, SQL, Lakehouse, Warehouse |
| Dédoublonnage complexe | ETL/ELT |
| Mapping référentiel | Silver/Gold |
| Historisation SCD | Data Warehouse / Lakehouse |
| Agrégation lourde | Gold / Aggregation table |
| Typage simple | Source ou Power Query selon contexte |
| Filtre local léger | Power Query |
| Renommage pour rapport unique | Power Query |
| KPI métier partagé | Semantic model ou couche Gold selon gouvernance |

### Anti-pattern

Copier la même logique Power Query dans dix rapports différents.

---

## 7.2 Power Query

### Bonnes pratiques

- Supprimer les colonnes inutiles tôt.
- Filtrer les lignes inutiles tôt.
- Préserver le query folding autant que possible.
- Éviter les étapes non pliables avant les filtres importants.
- Renommer clairement les étapes.
- Documenter les transformations non triviales.
- Éviter les transformations ligne à ligne coûteuses sur gros volumes.
- Éviter les merges complexes dans Power Query si la source peut les faire.

### Étapes à surveiller

Certaines opérations peuvent casser le folding selon la source et le contexte :

- ajout d’index ;
- fonctions personnalisées ligne à ligne ;
- transformations texte complexes ;
- buffers ;
- merges non optimisés ;
- conversions tardives ;
- appels API non paginés ;
- logique conditionnelle complexe.

### Règle

Pour les flux critiques, le query folding doit être vérifié et documenté.

---

## 7.3 Dataflows et Dataflows Gen2

### Cas d’usage recommandés

- mutualiser des transformations entre rapports ;
- préparer des tables réutilisables ;
- centraliser une logique Power Query ;
- exposer des sources contrôlées aux équipes BI ;
- alimenter Lakehouse ou Warehouse dans Fabric ;
- réduire la duplication dans les PBIX.

### Règles

- Les dataflows critiques doivent avoir un owner.
- Les transformations doivent être documentées.
- Les refresh doivent être monitorés.
- Les dépendances entre dataflows doivent être maîtrisées.
- Les erreurs doivent être alertées.
- Les changements doivent être versionnés lorsque possible.
- Les destinations doivent être explicites.

### Anti-patterns

- Chaîner de nombreux dataflows sans documentation.
- Utiliser un dataflow comme boîte noire non monitorée.
- Mettre des règles métier critiques dans un dataflow personnel.
- Publier un dataflow partagé sans contrat de données.

---

## 7.4 SQL, Warehouse et Lakehouse

### Bonnes pratiques SQL

- Utiliser des vues ou tables curated pour Power BI.
- Éviter les requêtes ad hoc non versionnées.
- Documenter les vues exposées.
- Indexer ou optimiser les tables sources si DirectQuery est utilisé.
- Préparer les clés et types en amont.
- Éviter les fonctions scalaires coûteuses dans les vues consommées fréquemment.
- Gérer les agrégations dans la couche data lorsque pertinent.

### Lakehouse / Delta

Pour les architectures Fabric ou Lakehouse :

- utiliser des tables Delta structurées ;
- séparer les couches raw, cleaned et curated ;
- documenter les schémas ;
- gérer la rétention ;
- contrôler les permissions OneLake ;
- éviter de connecter Power BI directement à des tables raw ;
- exposer des tables Gold adaptées à Direct Lake si applicable.

---

## 7.5 DirectQuery et Direct Lake readiness

### DirectQuery readiness

Une source DirectQuery doit être :

- performante ;
- indexée ou optimisée ;
- stable ;
- gouvernée ;
- compatible avec les requêtes analytiques ;
- sécurisée ;
- monitorée.

DirectQuery ne doit pas être utilisé pour masquer un problème de refresh ou de modélisation.

### Direct Lake readiness

Une table destinée à Direct Lake doit être :

- structurée ;
- stable ;
- optimisée pour l’analyse ;
- stockée dans une couche appropriée ;
- documentée ;
- sécurisée ;
- compatible avec la stratégie de modèle sémantique.

---

## 7.6 Tests de non-régression

### Tests recommandés

- comparaison des volumes ;
- comparaison des distinct counts ;
- comparaison des sommes financières ;
- comparaison des min/max dates ;
- comparaison des KPI de référence ;
- contrôle des doublons ;
- contrôle des nulls ;
- contrôle du schéma ;
- contrôle des valeurs de référence ;
- contrôle des performances.

### Exemple

```text
Avant changement:
rows fact_sales = 15 240 188
sales_amount = 184 550 000 €

Après changement:
rows fact_sales = 15 240 188
sales_amount = 184 550 000 €

Résultat:
PASS
```

Tout écart doit être expliqué, accepté ou corrigé.

---

## 7.7 Observabilité

### Métriques obligatoires pour flux critiques

- durée du refresh ;
- statut du refresh ;
- nombre de lignes traitées ;
- nombre de lignes rejetées ;
- taux de rejet ;
- nombre de nouvelles lignes ;
- nombre de lignes modifiées ;
- fraîcheur des données ;
- taille du dataset ;
- erreurs par étape ;
- volume par partition ;
- dérives de schéma ;
- SLA respecté ou non.

### Tableau de bord d’opérabilité BI

Un dashboard d’opérabilité doit permettre de répondre :

- Quels flux sont en échec ?
- Quels datasets sont en retard ?
- Où le volume a-t-il changé anormalement ?
- Quels taux de rejet augmentent ?
- Quels modèles Power BI sont impactés ?
- Qui est owner du dataset ?
- Quel est le dernier incident connu ?

---

## 8. Data Quality Gates

## 8.1 Niveaux de contrôle

| Niveau | Description | Exemple |
|---|---|---|
| **Warning** | Anomalie non bloquante, à surveiller | Taux de nulles légèrement supérieur au seuil |
| **Blocking** | Anomalie bloquant la publication | Colonne critique absente |
| **Quarantine** | Enregistrements isolés, pipeline continue | Lignes avec montant invalide |
| **Manual review** | Validation humaine requise | Changement de signification métier |
| **Auto-remediation** | Correction automatique tracée | Mapping de code connu |

---

## 8.2 Exemples de règles qualité

| Domaine | Règle | Sévérité |
|---|---|---|
| Schéma | Colonne critique absente | Blocking |
| Type | `amount_eur` non numérique | Blocking |
| Null | `customer_id` null | Quarantine / Blocking selon contexte |
| Doublon | `transaction_id` dupliqué | Blocking |
| Valeur | `amount_eur < 0` sans type avoir | Quarantine |
| Référentiel | `country_code` inconnu | Warning / Quarantine |
| Fraîcheur | Données non disponibles à 07:00 | Blocking pour executive reporting |
| Volume | Variation > 30 % vs moyenne 30 jours | Warning ou Blocking |
| Sécurité | Colonne PII non classifiée | Blocking |

---

## 8.3 Gestion des seuils

Les seuils doivent être adaptés au contexte métier.

Exemple :

```yaml
quality_thresholds:
  null_rate:
    customer_id: 0
    customer_email: 0.05
    product_category: 0.01
  duplicate_rate:
    transaction_id: 0
  freshness:
    max_delay_minutes: 60
  volume_change:
    warning_percent: 20
    blocking_percent: 50
```

---

## 9. CI/CD et industrialisation

## 9.1 Versioning

Les éléments suivants doivent être versionnés :

- data contracts ;
- scripts SQL ;
- notebooks ;
- pipelines ;
- définitions Dataflow si possible ;
- règles DQ ;
- dictionnaires de données ;
- mapping référentiels ;
- documentation ;
- changelog ;
- scripts de tests.

---

## 9.2 Structure Git recommandée

```text
powerbi-data-preparation-standard/
│
├── README.md
├── CHANGELOG.md
├── docs/
│   ├── architecture.md
│   ├── data_contract.md
│   ├── data_quality_rules.md
│   ├── security_classification.md
│   └── runbook.md
│
├── contracts/
│   ├── fact_sales.yaml
│   └── dim_customer.yaml
│
├── sql/
│   ├── views/
│   ├── tables/
│   └── tests/
│
├── pipelines/
│   ├── ingestion/
│   ├── transformation/
│   └── monitoring/
│
├── powerquery/
│   ├── shared_functions/
│   └── dataflows/
│
└── tests/
    ├── schema_tests/
    ├── quality_tests/
    ├── regression_tests/
    └── performance_tests/
```

---

## 9.3 Pipeline de validation recommandé

```text
Pull Request
   ↓
Schema validation
   ↓
Data contract tests
   ↓
Data quality tests
   ↓
Security classification checks
   ↓
Performance smoke tests
   ↓
Documentation checks
   ↓
Approval
   ↓
Deployment
```

### Règle

Un changement de pipeline ou de source critique ne doit pas être promu sans tests minimaux.

---

## 9.4 Changelog obligatoire

Exemple :

```markdown
## 2026-06-10 — Version 2.0

### Added
- Ajout de la colonne `customer_segment` dans `dim_customer`.
- Ajout de contrôles DQ sur `sales_amount`.

### Changed
- Modification de la règle de dédoublonnage client.
- Extension de la fenêtre late arriving data de 3 à 7 jours.

### Fixed
- Correction du typage de `order_date`.

### Security
- Pseudonymisation de `customer_email`.

### Breaking changes
- Renommage de `amount` en `sales_amount`.
```

---

## 10. Publication de sources certifiées

## 10.1 Niveaux de statut

| Statut | Description |
|---|---|
| **Draft** | Source en construction, non destinée à usage large. |
| **Validated** | Source testée pour un périmètre limité. |
| **Promoted** | Source recommandée par l’équipe propriétaire. |
| **Certified** | Source reconnue comme fiable, documentée et réutilisable. |
| **Deprecated** | Source à ne plus utiliser, remplacée ou en fin de vie. |
| **Archived** | Source retirée de l’usage actif. |

---

## 10.2 Critères de certification

Une source peut être certifiée uniquement si :

- elle dispose d’un data contract ;
- elle a un owner métier ;
- elle a un owner technique ;
- son schéma est stable ;
- son grain est documenté ;
- ses règles DQ sont définies ;
- ses contrôles qualité sont opérationnels ;
- son SLA est documenté ;
- sa sécurité est validée ;
- sa documentation est disponible ;
- ses dépendances sont connues ;
- son usage Power BI est validé ;
- elle ne duplique pas une source certifiée existante.

---

## 10.3 Checklist de certification source

```markdown
# Checklist Certification Source BI-ready

## Ownership
- [ ] Business Owner identifié
- [ ] Technical Owner identifié
- [ ] Data Steward identifié si nécessaire
- [ ] Support Contact identifié

## Contrat
- [ ] Data contract versionné
- [ ] Schéma documenté
- [ ] Grain documenté
- [ ] SLA documenté
- [ ] Rétention documentée

## Qualité
- [ ] Règles DQ définies
- [ ] Seuils documentés
- [ ] Tests automatiques disponibles
- [ ] Rejets gérés
- [ ] Historique des anomalies disponible

## Sécurité
- [ ] Colonnes classifiées
- [ ] PII minimisées
- [ ] Masquage/pseudonymisation appliqué si nécessaire
- [ ] Accès par groupes
- [ ] Rétention conforme

## Performance
- [ ] Volumétrie maîtrisée
- [ ] Refresh stable
- [ ] Incremental load si nécessaire
- [ ] Query folding vérifié si Power Query
- [ ] Source compatible avec mode de consommation Power BI

## Documentation
- [ ] Dictionnaire de données disponible
- [ ] Lineage documenté
- [ ] Limitations connues documentées
- [ ] Changelog disponible
- [ ] Runbook disponible
```

---

## 11. Lignes rouges

Les pratiques suivantes sont interdites pour les sources destinées à Power BI :

- construire des KPI critiques uniquement dans des visuels sans définition centralisée ;
- utiliser des colonnes texte comme dates ou montants ;
- exposer directement des PII non nécessaires ;
- multiplier les copies locales de la même logique de transformation ;
- ignorer les dérives de schéma en production ;
- publier une source critique sans owner ;
- publier une source sans grain documenté ;
- supprimer ou renommer une colonne critique sans analyse d’impact ;
- corriger silencieusement des données sans traçabilité ;
- ignorer les doublons sur des clés supposées uniques ;
- connecter Power BI directement à une table raw pour un usage corporate ;
- charger une granularité transactionnelle inutilement fine ;
- ignorer les données tardives dans une stratégie incrémentale ;
- exposer une source non documentée comme source de vérité ;
- créer des dataflows personnels utilisés comme dépendances critiques ;
- laisser des refresh échouer sans alerte ;
- publier des données sensibles sans classification ;
- hardcoder des règles de sécurité dans des transformations dispersées ;
- dupliquer une source certifiée existante.

---

## 12. Astuces expertes et gains rapides

## 12.1 Profilage automatique quotidien

Mettre en place un rapport Data Quality quotidien :

- taux de nulles ;
- doublons ;
- cardinalités ;
- min/max ;
- outliers ;
- valeurs nouvelles ;
- fraîcheur ;
- volumes ;
- dérives de schéma.

Objectif : détecter les régressions avant les utilisateurs.

---

## 12.2 Tables de référence certifiées

Conserver dans une couche Gold versionnée :

- devises ;
- calendriers ;
- pays ;
- organisations ;
- segments ;
- produits ;
- statuts ;
- canaux ;
- mapping métiers.

Cela réduit les divergences entre rapports.

---

## 12.3 Surrogate keys numériques

Les surrogate keys numériques améliorent généralement :

- la compression ;
- les jointures ;
- la stabilité des relations ;
- la gestion des dimensions historisées.

Exemple :

```text
customer_key: 102938
customer_id: CRM-8842-AZ
```

Power BI doit utiliser prioritairement `customer_key` pour les relations.

---

## 12.4 Flag de qualité par ligne

Ajouter des flags qualité permet de filtrer ou diagnostiquer facilement.

Exemple :

```text
is_valid_record
quality_issue_flag
quality_issue_reason
quality_score
```

Ces colonnes doivent être masquées dans le semantic model si elles ne sont pas utiles aux utilisateurs.

---

## 12.5 Échantillons représentatifs

Créer des datasets de test représentatifs permet :

- d’accélérer le développement ;
- de tester les cas limites ;
- de réduire les coûts ;
- de faciliter les revues ;
- de protéger les données sensibles.

Un bon échantillon doit contenir :

- données normales ;
- valeurs nulles ;
- doublons ;
- valeurs extrêmes ;
- cas historiques ;
- données tardives ;
- cas multi-devises ou multi-pays si pertinent.

---

## 12.6 Contract tests en CI/CD

Les tests de contrat doivent bloquer le déploiement si :

- une colonne critique disparaît ;
- un type change ;
- une clé devient non unique ;
- un seuil DQ critique est dépassé ;
- une colonne sensible apparaît sans classification ;
- le SLA de fraîcheur n’est pas respecté.

---

## 12.7 Colonnes techniques cachées

Conserver des colonnes de traçabilité peut être utile :

- `source_system` ;
- `source_file_name` ;
- `batch_id` ;
- `pipeline_run_id` ;
- `loaded_timestamp` ;
- `processed_timestamp` ;
- `record_hash`.

Elles doivent être masquées dans le semantic model sauf besoin d’audit.

---

## 12.8 Hash de ligne pour détection de changement

Un hash de ligne peut aider à détecter les modifications.

```text
record_hash = hash(customer_name, customer_segment, country_code, status)
```

Usage :

- incremental load ;
- SCD ;
- audit ;
- non-régression ;
- détection de divergence source.

---

## 12.9 Late arriving data window

Pour les faits arrivant en retard, définir une fenêtre de retraitement.

Exemple :

```text
Refresh quotidien:
- charger nouvelles données du jour
- retraiter les 7 derniers jours
- conserver historique 5 ans
```

Cela évite de manquer des corrections tardives.

---

## 12.10 Tables d’agrégation contrôlées

Créer des agrégations Gold lorsque les rapports exécutifs n’ont pas besoin du détail transactionnel.

Exemple :

```text
fact_sales_daily_product_region
1 ligne = 1 date, 1 produit, 1 région
```

Avantages :

- refresh plus rapide ;
- modèle plus léger ;
- meilleurs temps de réponse ;
- réduction de la charge capacité.

---

## 13. Mode opératoire standard

## 13.1 SOP de préparation d’une nouvelle source Power BI

1. Identifier le besoin métier et les décisions à soutenir.
2. Identifier les consommateurs : rapports, semantic models, équipes, métiers.
3. Inventorier les sources disponibles.
4. Définir le grain cible.
5. Définir les KPI ou usages analytiques attendus.
6. Rédiger le data contract.
7. Valider ownership, SLA et confidentialité.
8. Profiler les données sources.
9. Définir les règles DQ.
10. Définir la stratégie de nettoyage.
11. Définir la stratégie d’historisation.
12. Définir la stratégie de chargement.
13. Construire le pipeline de préparation.
14. Implémenter les contrôles qualité.
15. Implémenter la gestion des rejets.
16. Implémenter le monitoring.
17. Tester le schéma.
18. Tester le contenu.
19. Tester la performance.
20. Tester la sécurité.
21. Valider avec le métier.
22. Publier la source BI-ready.
23. Documenter le dataset.
24. Mettre à jour le catalogue.
25. Surveiller usage et incidents.

---

## 13.2 SOP de modification d’une source existante

1. Identifier la demande de changement.
2. Classifier le changement : mineur, standard, majeur, breaking change.
3. Identifier les consommateurs impactés.
4. Analyser l’impact sur les semantic models Power BI.
5. Analyser l’impact sur les KPI.
6. Analyser l’impact sécurité.
7. Modifier le data contract.
8. Modifier le pipeline en environnement de développement.
9. Exécuter les tests de contrat.
10. Exécuter les tests DQ.
11. Exécuter les tests de non-régression.
12. Faire valider par le métier si nécessaire.
13. Mettre à jour la documentation.
14. Mettre à jour le changelog.
15. Déployer selon le processus CI/CD.
16. Monitorer après déploiement.
17. Communiquer le changement aux consommateurs.

---

## 13.3 Classification des changements

| Type | Exemple | Validation requise |
|---|---|---|
| Mineur | Ajout d’une description, correction documentation | Technical Owner |
| Standard | Ajout d’une colonne non critique | Technical Owner + Data Steward |
| Majeur | Ajout d’une nouvelle règle métier | Data Steward + PO + Data Architect |
| Sécurité | Ajout d’une colonne sensible | RSSI / Security Officer + Data Steward |
| Breaking change | Suppression ou renommage d’une colonne critique | Data Governance Board / Architecture Review |

---

## 14. Gouvernance documentaire

## 14.1 Documents obligatoires

Pour chaque source critique :

- data contract ;
- dictionnaire de données ;
- règles DQ ;
- classification sécurité ;
- lineage ;
- runbook ;
- changelog ;
- liste des consommateurs ;
- limitations connues ;
- politique de rétention ;
- procédure d’escalade.

---

## 14.2 Cycle de révision

Ce standard doit être revu :

- trimestriellement ;
- après incident majeur ;
- après audit Data Governance ;
- après changement significatif Power BI/Fabric ;
- après évolution réglementaire ;
- après dérives répétées constatées dans les projets.

---

## 14.3 Registre de dérogation

```markdown
# Dérogation

**Dataset concerné**: fact_sales
**Règle concernée**: Incremental load obligatoire pour grands volumes
**Motif**: Source ne fournit pas encore de timestamp fiable
**Impact**: Refresh plus long, coût capacité augmenté
**Durée de validité**: 3 mois
**Plan de remédiation**: Ajout d’un champ updated_timestamp côté source
**Approbateur**: Data Architect
**Date de revue**: 2026-09-10
```

---

## 14.4 Incidents alimentant l’amélioration continue

Les incidents suivants doivent déclencher une revue :

- divergence KPI ;
- refresh Power BI échoué ;
- retard SLA ;
- schema drift non approuvé ;
- exposition de donnée sensible ;
- doublons critiques ;
- source indisponible ;
- valeurs métier incohérentes ;
- changement source non communiqué ;
- requêtes DirectQuery trop lentes ;
- transformation dupliquée dans plusieurs rapports ;
- incident utilisateur lié à une définition ambiguë.

---

## 15. Annexes

## 15.1 Template minimal de data contract

```yaml
name: fact_sales
functional_name: Sales Transactions
version: 1.0
status: certified

owners:
  business_owner: Direction Commerciale
  technical_owner: Data Platform Team
  data_steward: Sales Data Steward
  support_contact: bi-support@company.com

purpose: >
  Source certifiée des transactions de vente pour les analyses Power BI
  commerciales, financières et opérationnelles.

grain: "1 ligne = 1 transaction de vente, 1 produit, 1 client, 1 date de commande"

refresh:
  frequency: daily
  sla: "J+0 07:00 CET"
  strategy: incremental
  incremental_column: updated_timestamp
  late_arriving_window_days: 7
  retention: "5 years"

keys:
  primary:
    - transaction_id
  foreign:
    - customer_key
    - product_key
    - order_date_key

schema:
  - name: transaction_id
    type: bigint
    nullable: false
    description: Identifiant unique de transaction.
    classification: internal

  - name: customer_key
    type: bigint
    nullable: false
    description: Clé substitut client.
    classification: internal

  - name: product_key
    type: bigint
    nullable: false
    description: Clé substitut produit.
    classification: internal

  - name: order_date
    type: date
    nullable: false
    description: Date de commande.
    classification: internal

  - name: sales_amount_eur
    type: decimal(18,2)
    nullable: false
    description: Montant net de vente en EUR.
    classification: confidential

  - name: updated_timestamp
    type: datetime
    nullable: false
    description: Dernière modification source.
    classification: internal

quality_rules:
  - rule: "transaction_id must be unique"
    severity: blocking
  - rule: "sales_amount_eur >= 0"
    severity: quarantine
  - rule: "order_date <= current_date"
    severity: blocking
  - rule: "customer_key must exist in dim_customer"
    severity: warning
  - rule: "product_key must exist in dim_product"
    severity: warning

security:
  classification_default: internal
  pii_present: false
  rls_required: false
  masking_required: false

consumers:
  - Sales Performance Semantic Model
  - Executive Dashboard
  - Finance Margin Report

change_management:
  breaking_change_notice_days: 15
  approval_required_from:
    - Data Steward
    - Data Architect
    - Business Owner
```

---

## 15.2 Template de dictionnaire de données

```markdown
# Data Dictionary — fact_sales

| Colonne | Type | Nullable | Description | Exemple | Classification | Règle DQ |
|---|---|---:|---|---|---|---|
| transaction_id | bigint | Non | Identifiant unique de transaction | 982134 | Internal | Unique, non null |
| customer_key | bigint | Non | Clé substitut client | 10293 | Internal | Existe dans dim_customer |
| product_key | bigint | Non | Clé substitut produit | 884 | Internal | Existe dans dim_product |
| order_date | date | Non | Date de commande | 2026-06-10 | Internal | <= current_date |
| sales_amount_eur | decimal(18,2) | Non | Montant net de vente en EUR | 120.50 | Confidential | >= 0 |
```

---

## 15.3 Template de règles Data Quality

```yaml
rules:
  - id: DQ_SCHEMA_001
    name: Required columns exist
    type: schema
    severity: blocking
    description: All mandatory columns must be present.

  - id: DQ_UNIQUE_001
    name: Transaction ID uniqueness
    type: uniqueness
    severity: blocking
    column: transaction_id
    threshold: 0

  - id: DQ_NULL_001
    name: Customer key completeness
    type: completeness
    severity: blocking
    column: customer_key
    max_null_rate: 0

  - id: DQ_VALIDITY_001
    name: Sales amount positive
    type: validity
    severity: quarantine
    column: sales_amount_eur
    condition: "sales_amount_eur >= 0"

  - id: DQ_FRESHNESS_001
    name: Data available before SLA
    type: freshness
    severity: blocking
    condition: "latest_processed_timestamp <= SLA"
```

---

## 15.4 Template de runbook incident data

```markdown
# Runbook — Incident Data Source Power BI

## 1. Identification

- Dataset concerné:
- Date/heure incident:
- Détecté par:
- Type incident:
- Sévérité:
- Rapports impactés:
- Utilisateurs impactés:

## 2. Diagnostic initial

- Dernier refresh réussi:
- Dernier refresh échoué:
- Message d’erreur:
- Changement récent connu:
- Volume attendu:
- Volume observé:
- Schema drift détecté: Oui / Non

## 3. Actions immédiates

- [ ] Vérifier source
- [ ] Vérifier pipeline
- [ ] Vérifier gateway si applicable
- [ ] Vérifier permissions
- [ ] Vérifier capacité Power BI/Fabric
- [ ] Informer owner métier
- [ ] Informer utilisateurs si impact visible

## 4. Remédiation

- Cause racine:
- Correction appliquée:
- Relance effectuée:
- Résultat:
- Données retraitées:

## 5. Post-incident

- Mesure préventive:
- Mise à jour documentation:
- Mise à jour règle DQ:
- Mise à jour monitoring:
- Date de clôture:
```

---

## 15.5 Template de contrôle avant publication

```markdown
# Pre-Publication Control — Source Power BI

## Contract
- [ ] Data contract validé
- [ ] Owners renseignés
- [ ] SLA défini
- [ ] Grain documenté

## Schema
- [ ] Colonnes conformes
- [ ] Types conformes
- [ ] Clés conformes
- [ ] Pas de schema drift non approuvé

## Content
- [ ] Nulls sous seuil
- [ ] Duplicats contrôlés
- [ ] Valeurs invalides traitées
- [ ] Référentiels alignés

## Security
- [ ] Colonnes classifiées
- [ ] PII minimisées
- [ ] Accès définis
- [ ] Rétention documentée

## Performance
- [ ] Volumétrie contrôlée
- [ ] Incremental load configuré si nécessaire
- [ ] Query folding vérifié si applicable
- [ ] Refresh testé

## Documentation
- [ ] Dictionnaire disponible
- [ ] Lineage disponible
- [ ] Runbook disponible
- [ ] Changelog mis à jour
```

---

## 15.6 Exemple de structure cible BI-ready

```text
Gold Layer / BI-ready
│
├── dim_date
├── dim_customer
├── dim_product
├── dim_geography
├── dim_organization
├── dim_currency
│
├── fact_sales
├── fact_inventory_snapshot
├── fact_budget
│
├── bridge_customer_segment
│
├── ref_country
├── ref_currency_rate
├── ref_sales_channel
│
└── quarantine_sales
```

---

## 15.7 Confidentialité

Certaines données, exemples, noms de tables, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, aux règles de gouvernance, aux politiques de sécurité et aux contraintes réglementaires de l’organisation.

---

## 15.8 Résumé exécutif du standard

Une source optimale pour Power BI doit être :

- contractualisée ;
- documentée ;
- typée correctement ;
- testée ;
- monitorée ;
- sécurisée ;
- historisée si nécessaire ;
- alignée avec le métier ;
- compatible avec le modèle sémantique ;
- performante ;
- réutilisable ;
- gouvernée ;
- maintenable.

La qualité d’un rapport Power BI dépend fortement de la qualité de la donnée préparée en amont. Une donnée mal typée, mal historisée, non documentée ou non contrôlée produit mécaniquement des modèles fragiles, des refresh instables, des KPI incohérents et une perte de confiance métier.

Ce standard doit donc être appliqué comme un cadre d’ingénierie data, et non comme une simple checklist BI.
