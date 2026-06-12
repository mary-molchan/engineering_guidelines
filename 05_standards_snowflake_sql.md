# Standard Corporate — Snowflake SQL

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Règles, patterns et meilleures pratiques Snowflake SQL |
| **Version** | 2.0 |
| **Date** | 2026-06-12 |
| **Auteur / Rôle** | Équipe Data Engineering & Analytics Engineering |
| **Validation / Approbation** | Lead Data Engineer, Data Architect, Head of BI, Data Governance Officer |
| **Public cible** | Data Engineers, Analytics Engineers, BI Developers, Data Analysts avancés, Platform Engineers, Data Stewards |
| **Commanditaire** | Direction Data & Performance / DSI |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Code SQL Snowflake utilisé pour ingestion, transformation, data marts, contrôles qualité, pipelines incrémentaux, vues contractuelles, tests, monitoring et support de production |
| **Objectif du document** | Définir les standards d’écriture SQL Snowflake afin d’assurer lisibilité, performance, fiabilité, sécurité, testabilité, auditabilité, maintenabilité et évolutivité du code data en entreprise. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les exigences s’appliquent par défaut à tout code SQL Snowflake développé, revu, exécuté ou maintenu dans l’organisation.

Sont concernés :

- scripts DDL ;
- scripts DML ;
- transformations ELT ;
- modèles dbt ou équivalents ;
- vues métier ;
- vues sécurisées ;
- dynamic tables ;
- streams/tasks ;
- procédures SQL ;
- contrôles qualité ;
- scripts de monitoring ;
- scripts de migration ;
- scripts de correction production ;
- requêtes analytiques critiques.

### Interprétation des termes

| Terme | Signification |
|---|---|
| **MUST** | Exigence obligatoire, non négociable hors dérogation formelle. |
| **SHOULD** | Exigence fortement recommandée ; toute exception doit être justifiée et documentée. |
| **MAY** | Option autorisée selon le contexte, sans contradiction avec les exigences MUST/SHOULD. |
| **MUST NOT** | Interdiction explicite. |
| **Exception documentée** | Écart temporaire validé par un approbateur nommé, avec justification, durée de validité et plan de remédiation. |

### Mode de rédaction

Les règles sont orientées :

- qualité de code ;
- performance Snowflake ;
- déterminisme ;
- sécurité ;
- auditabilité ;
- data quality ;
- CI/CD ;
- production readiness ;
- maintenabilité long terme.

### Gestion des exceptions

Toute dérogation doit inclure :

- le contexte métier ;
- le script ou objet concerné ;
- le motif de l’écart ;
- l’impact performance ;
- l’impact sécurité ;
- l’impact qualité des données ;
- l’impact coût ;
- l’impact maintenabilité ;
- la durée de validité ;
- le plan de remédiation ;
- l’approbateur nommé ;
- la date de révision.

### Précedence

En cas de conflit, l’ordre de priorité est :

1. obligations légales, réglementaires et contractuelles ;
2. politiques sécurité / RSSI ;
3. standards Data Governance ;
4. standards Data Architecture ;
5. standard plateforme Snowflake corporate ;
6. présent standard Snowflake SQL ;
7. pratiques locales d’équipe.

---

## 1.2 Sources officielles Snowflake utilisées

Ce standard s’appuie sur la documentation officielle Snowflake, notamment :

- [SQL command reference](https://docs.snowflake.com/en/sql-reference/sql-all)
- [Query syntax](https://docs.snowflake.com/en/sql-reference/constructs)
- [SELECT](https://docs.snowflake.com/en/sql-reference/sql/select)
- [QUALIFY](https://docs.snowflake.com/en/sql-reference/constructs/qualify)
- [MERGE](https://docs.snowflake.com/en/sql-reference/sql/merge)
- [Transactions](https://docs.snowflake.com/en/sql-reference/transactions)
- [BEGIN](https://docs.snowflake.com/en/sql-reference/sql/begin)
- [COMMIT](https://docs.snowflake.com/en/sql-reference/sql/commit)
- [ROLLBACK](https://docs.snowflake.com/en/sql-reference/sql/rollback)
- [Window function syntax and usage](https://docs.snowflake.com/en/sql-reference/functions-window-syntax)
- [Analyzing data with window functions](https://docs.snowflake.com/en/user-guide/functions-window-using)
- [Working with joins](https://docs.snowflake.com/en/user-guide/querying-joins)
- [Semi-structured data types](https://docs.snowflake.com/en/sql-reference/data-types-semistructured)
- [Querying semi-structured data](https://docs.snowflake.com/en/user-guide/querying-semistructured)
- [FLATTEN](https://docs.snowflake.com/en/sql-reference/functions/flatten)
- [Conversion functions](https://docs.snowflake.com/en/sql-reference/functions-conversion)
- [TRY_CAST](https://docs.snowflake.com/en/sql-reference/functions/try_cast)
- [TRY_TO_DATE](https://docs.snowflake.com/en/sql-reference/functions/try_to_date)
- [Constraints overview](https://docs.snowflake.com/en/sql-reference/constraints-overview)
- [Micro-partitions and data clustering](https://docs.snowflake.com/en/user-guide/tables-clustering-micropartitions)
- [Clustering keys](https://docs.snowflake.com/en/user-guide/tables-clustering-keys)
- [Optimizing query performance](https://docs.snowflake.com/en/user-guide/performance-query-options)
- [Optimizing storage for performance](https://docs.snowflake.com/en/user-guide/performance-query-storage)
- [Search Optimization Service](https://docs.snowflake.com/en/user-guide/search-optimization-service)
- [Query Acceleration Service](https://docs.snowflake.com/en/user-guide/query-acceleration-service)
- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro)
- [Streams and tasks](https://docs.snowflake.com/en/user-guide/data-pipelines-intro)
- [Dynamic tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/overview)
- [Decision guide for dynamic tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/decision-guide)
- [Account Usage](https://docs.snowflake.com/en/sql-reference/account-usage)
- [QUERY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/query_history)

---

## 1.3 Définitions clés

| Terme | Définition |
|---|---|
| **Production SQL** | SQL exécuté dans un pipeline, un modèle, une vue ou un contexte utilisé par des consommateurs métier. |
| **Ad hoc SQL** | Requête exploratoire non destinée à être déployée telle quelle. |
| **Deterministic SQL** | SQL produisant un résultat stable et explicite pour un même état de données. |
| **Idempotent SQL** | SQL pouvant être rejoué sans générer de doublons ou effets secondaires indésirables. |
| **CTE** | Common Table Expression, bloc `WITH` structurant une requête. |
| **Predicate pruning** | Capacité à réduire les données scannées via filtres efficaces et micro-partitions. |
| **Micro-partition** | Unité interne de stockage Snowflake utilisée pour optimiser le scan et le pruning. |
| **Clustering key** | Clé permettant d’améliorer l’organisation physique de grandes tables selon certains accès. |
| **SARGable predicate** | Predicate permettant à l’optimiseur d’exploiter efficacement les données et métadonnées. |
| **QUALIFY** | Clause Snowflake permettant de filtrer directement sur le résultat de fonctions analytiques/window functions. |
| **MERGE** | Instruction permettant d’insérer, mettre à jour ou supprimer des lignes selon correspondance entre source et cible. |
| **Watermark** | Marqueur de progression incrémentale : timestamp, ID monotone, batch ID ou version. |
| **Late arriving data** | Données arrivant après la fenêtre de traitement attendue. |
| **Schema drift** | Évolution non prévue du schéma source : ajout, suppression, renommage ou changement de type. |
| **Data contract** | Contrat définissant schéma, qualité, SLA, règles d’évolution et ownership d’un dataset. |
| **Quarantine table** | Table isolant les lignes invalides ou rejetées. |
| **Query Profile** | Vue Snowflake permettant d’analyser les opérateurs, scans, joins, spill, temps d’exécution et goulots d’étranglement. |
| **Query tag** | Métadonnée de session/requête permettant d’identifier application, pipeline, owner, environnement ou run. |

---

## 2. Vision, principes directeurs et résultats attendus

## 2.1 Vision

Produire un SQL Snowflake robuste, standardisé et industriel, facilement relisible par les équipes, performant à grande échelle, sécurisé par design, auditable et vérifiable en CI/CD.

Le SQL Snowflake d’entreprise ne doit pas être considéré comme une simple suite de requêtes. Il constitue une couche d’ingénierie critique qui porte :

- la logique de transformation ;
- les règles métier ;
- la qualité des données ;
- l’incrémentalité ;
- la performance ;
- la traçabilité ;
- la sécurité ;
- la maintenabilité ;
- la confiance analytique.

---

## 2.2 Principes directeurs

1. **Readable SQL is maintainable SQL**  
   Un SQL lisible est plus facile à relire, corriger, optimiser et transmettre.

2. **Explicit over Implicit**  
   Les colonnes, types, filtres, ordres, règles métier et hypothèses doivent être explicites.

3. **Determinism by Default**  
   Le résultat doit être stable et non dépendant d’un ordre implicite ou de comportements ambigus.

4. **Performance by Intent**  
   L’optimisation doit être liée aux patterns d’accès réels, mesurée et documentée.

5. **Cost-Aware SQL**  
   Un SQL inefficace augmente les coûts compute. Toute requête critique doit être pensée avec conscience du scan, warehouse, fréquence et concurrence.

6. **Small, Composable Steps**  
   Préférer des transformations atomiques, testables et composables aux méga-requêtes opaques.

7. **Test-First Mindset**  
   Les contrôles qualité, cardinalité, schéma, volumes et réconciliation doivent accompagner le code.

8. **Idempotence and Replayability**  
   Les pipelines doivent pouvoir être rejoués sans corruption ni duplication.

9. **Security by Query Design**  
   Les colonnes sensibles doivent être exclues, masquées ou exposées via objets sécurisés.

10. **Governed Evolution**  
    Les changements doivent être versionnés, revus, testés et promus via pipeline contrôlé.

11. **Observe Everything Critical**  
    Les scripts critiques doivent être monitorables via query tags, audit tables et historiques Snowflake.

---

## 2.3 Résultats attendus

L’application de ce standard doit permettre de :

- réduire les régressions en production ;
- améliorer les temps d’exécution des requêtes critiques ;
- réduire les coûts liés aux scans et traitements inutiles ;
- accélérer les code reviews ;
- standardiser les pratiques entre squads data ;
- fiabiliser les pipelines incrémentaux ;
- réduire les duplications silencieuses ;
- améliorer la traçabilité des transformations ;
- sécuriser les outputs contenant des données sensibles ;
- faciliter le debug et le support de production ;
- améliorer l’onboarding des nouveaux data engineers.

---

## 2.4 Indicateurs de succès

| Indicateur | Cible recommandée |
|---|---|
| Scripts PROD avec `SELECT *` | 0 |
| Transformations critiques avec tests qualité | 100 % |
| Pipelines critiques idempotents | 100 % |
| Requêtes critiques avec `QUERY_TAG` | 100 % |
| Code SQL critique passé en review | 100 % |
| Incidents dus à duplication de join | 0 critique |
| Scripts avec owner documenté | 100 % |
| Tables cibles avec contrôle de volume | 100 % sur périmètre critique |
| Requêtes top coût revues mensuellement | Top 20 minimum |
| Changements breaking non annoncés | 0 |
| DML PROD sans stratégie rollback | 0 |

---

## 3. Rôles et responsabilités

## 3.1 RACI simplifié

| Activité | Data Engineer | Analytics Engineer | Reviewer Tech Lead | Data Architect | Data Steward | Métier / PO | Platform Engineer |
|---|---:|---:|---:|---:|---:|---:|---:|
| Écriture SQL ingestion | A/R | C | C | C | C | I | C |
| Écriture SQL transformation | A/R | A/R | C | C | C | I | I |
| Modélisation marts | C | A/R | C | C | C | A/R | I |
| Revue qualité code | C | C | A/R | C | I | I | I |
| Revue performance | R | C | A/R | C | I | I | C |
| Revue sécurité SQL | R | C | C | C | C | I | A/R |
| Tests data quality | R | A/R | C | C | C | I | I |
| Validation sémantique métier | C | C | I | C | C | A/R | I |
| Déploiement CI/CD | R | C | A | C | I | I | C |
| Monitoring post-déploiement | R | C | A | I | I | I | C |
| Documentation SQL critique | R | R | C | C | A | C | I |

---

## 3.2 Ownership du code SQL

Chaque script SQL critique doit avoir :

- un owner technique ;
- un owner métier si la logique impacte un KPI ;
- un domaine de données ;
- un environnement cible ;
- un niveau de criticité ;
- une fréquence d’exécution ;
- un warehouse attendu ;
- un statut lifecycle ;
- une stratégie de test ;
- une stratégie rollback si production ;
- une documentation ou lien vers data contract.

---

## 4. Structure standard du code SQL

## 4.1 Structure recommandée d’un script

```sql
/*
  Script: build_sales_mart.sql
  Domain: SALES
  Environment: PROD
  Owner: Data Engineering
  Purpose: Build daily sales mart used by certified Power BI reports.
  Grain: 1 row = 1 sales_date x product_id x region_code
  Source tables:
    - SALES_PRD.CURATED.SALES_TRANSACTION
    - SALES_PRD.CURATED.PRODUCT
  Target table:
    - SALES_PRD.MART.FACT_SALES_DAILY
  Incremental: Yes
  Watermark: transaction_date
  Sensitive data: No direct PII in output
  Last reviewed: 2026-06-12
*/

ALTER SESSION SET QUERY_TAG = 'app=sql_pipeline;domain=sales;env=prod;pipeline=sales_mart;step=build_fact_sales_daily';

WITH source_sales AS (
    SELECT
        transaction_id,
        transaction_date,
        product_id,
        region_code,
        sales_amount_eur,
        updated_at
    FROM SALES_PRD.CURATED.SALES_TRANSACTION
    WHERE transaction_date >= :start_date
      AND transaction_date < :end_date
),

valid_sales AS (
    SELECT
        transaction_date::DATE AS sales_date,
        product_id,
        region_code,
        sales_amount_eur
    FROM source_sales
    WHERE sales_amount_eur >= 0
),

aggregated_sales AS (
    SELECT
        sales_date,
        product_id,
        region_code,
        SUM(sales_amount_eur) AS sales_amount_eur,
        COUNT(*) AS transaction_count
    FROM valid_sales
    GROUP BY
        sales_date,
        product_id,
        region_code
)

SELECT
    sales_date,
    product_id,
    region_code,
    sales_amount_eur,
    transaction_count,
    CURRENT_TIMESTAMP() AS processed_at
FROM aggregated_sales;
```

---

## 4.2 Sections obligatoires pour SQL critique

Tout SQL critique doit préciser :

- objectif ;
- domaine ;
- owner ;
- sources ;
- cible ;
- granularité ;
- logique incrémentale ;
- règles qualité ;
- données sensibles ;
- paramètres ;
- hypothèses ;
- risques ;
- date de revue.

Ces éléments peuvent être dans l’en-tête du script, dans un fichier README ou dans la documentation du pipeline.

---

## 5. Style et formatage

## 5.1 Règles générales

### Obligations

- Utiliser des mots-clés SQL en majuscules.
- Utiliser une indentation cohérente.
- Mettre une colonne par ligne dans les `SELECT` de production.
- Aligner la logique pour faciliter la lecture.
- Utiliser des alias explicites.
- Éviter les abréviations obscures.
- Commenter les hypothèses métier.
- Éviter les commentaires inutiles décrivant l’évidence.
- Ne pas mélanger plusieurs styles dans un même repository.

---

## 5.2 Format recommandé

```sql
SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.order_date,
    o.order_amount_eur
FROM CURATED.CUSTOMER AS c
INNER JOIN CURATED.ORDER AS o
    ON o.customer_id = c.customer_id
WHERE o.order_date >= DATE '2026-01-01'
  AND o.order_status = 'VALIDATED';
```

---

## 5.3 Alias

### Règles

- Tout alias de table doit être explicite ou standard.
- Les alias à une lettre sont autorisés uniquement pour requêtes simples ou conventions évidentes.
- Les alias doivent rester stables dans une requête.
- Les colonnes ambiguës doivent toujours être préfixées.
- Ne pas réutiliser le même alias pour des entités différentes.

### Exemples

| Table | Alias recommandé |
|---|---|
| `CUSTOMER` | `cust` ou `c` |
| `ORDER` | `ord` |
| `PRODUCT` | `prod` |
| `SALES_TRANSACTION` | `sales` |
| `CALENDAR` | `cal` |
| `REGION` | `reg` |

---

## 5.4 Commentaires

### Bon commentaire

```sql
-- Exclude cancelled transactions because Finance validates revenue only after order confirmation.
WHERE order_status = 'VALIDATED'
```

### Mauvais commentaire

```sql
-- Filter order status.
WHERE order_status = 'VALIDATED'
```

### Règle

Les commentaires doivent expliquer le **pourquoi**, pas seulement le **quoi**.

---

## 5.5 Nommage des CTE

Les CTE doivent représenter des étapes logiques.

### Bonnes pratiques

| Préfixe | Usage |
|---|---|
| `source_` | Extraction de tables sources |
| `filtered_` | Filtrage initial |
| `validated_` | Application règles qualité |
| `deduped_` | Déduplication |
| `enriched_` | Jointures d’enrichissement |
| `aggregated_` | Agrégation |
| `ranked_` | Fenêtres analytiques |
| `final_` | Projection finale |

### Exemple

```sql
WITH source_orders AS (...),
filtered_orders AS (...),
deduped_orders AS (...),
enriched_orders AS (...),
final_orders AS (...)
SELECT
    ...
FROM final_orders;
```

---

## 6. Sélection de colonnes et déterminisme

## 6.1 Interdiction de `SELECT *`

### Règle

`SELECT *` est interdit dans tout SQL de production, sauf exception technique très limitée et documentée.

### Risques

- rupture silencieuse en cas de schema drift ;
- exposition de colonnes sensibles ;
- scan inutile ;
- coût plus élevé ;
- outputs instables ;
- dépendances non maîtrisées ;
- incompatibilité avec vues contractuelles.

### Acceptable uniquement pour

- exploration ad hoc ;
- debug local temporaire ;
- profilage ponctuel ;
- table de quarantine contenant payload brut ;
- procédure générique validée.

---

## 6.2 Colonnes explicites

### Recommandé

```sql
SELECT
    customer_id,
    customer_name,
    customer_segment,
    created_at
FROM CURATED.CUSTOMER;
```

### Interdit en production

```sql
SELECT *
FROM CURATED.CUSTOMER;
```

---

## 6.3 Ordre déterministe

### Règles

- Ne jamais supposer un ordre de lignes sans `ORDER BY`.
- Toute logique `ROW_NUMBER()` doit avoir un `ORDER BY` déterministe.
- Les ex æquo doivent être traités explicitement.
- Les déduplications doivent utiliser un ordre stable.
- Les `LIMIT` sans `ORDER BY` sont interdits en logique métier.

### Mauvais exemple

```sql
SELECT
    customer_id,
    order_id
FROM CURATED.ORDER
LIMIT 100;
```

### Bon exemple

```sql
SELECT
    customer_id,
    order_id
FROM CURATED.ORDER
ORDER BY order_date DESC, order_id DESC
LIMIT 100;
```

---

## 6.4 Déduplication déterministe

```sql
WITH ranked_orders AS (
    SELECT
        order_id,
        customer_id,
        order_status,
        updated_at,
        load_id,
        ROW_NUMBER() OVER (
            PARTITION BY order_id
            ORDER BY updated_at DESC, load_id DESC
        ) AS row_num
    FROM STAGING.ORDER_EVENTS
)

SELECT
    order_id,
    customer_id,
    order_status,
    updated_at
FROM ranked_orders
WHERE row_num = 1;
```

Ou avec `QUALIFY` :

```sql
SELECT
    order_id,
    customer_id,
    order_status,
    updated_at
FROM STAGING.ORDER_EVENTS
QUALIFY ROW_NUMBER() OVER (
    PARTITION BY order_id
    ORDER BY updated_at DESC, load_id DESC
) = 1;
```

---

## 7. Types, casts et conversions

## 7.1 Règles générales

- Les types doivent être explicites.
- Les casts implicites doivent être évités.
- Les conversions de dates, nombres et booléens doivent être contrôlées.
- Les formats d’entrée doivent être documentés.
- Les conversions risquées doivent utiliser des fonctions `TRY_` lorsque l’échec doit devenir un rejet qualité.
- Les conversions critiques ne doivent pas masquer silencieusement les erreurs.

---

## 7.2 Conversion stricte vs conversion tolérante

### Conversion stricte

À utiliser lorsque toute erreur doit bloquer.

```sql
SELECT
    CAST(amount AS NUMBER(18, 2)) AS amount_eur
FROM source_table;
```

### Conversion tolérante

À utiliser lorsque les lignes invalides doivent être isolées.

```sql
SELECT
    TRY_TO_NUMBER(amount) AS amount_eur,
    amount AS raw_amount
FROM source_table;
```

### Contrôle qualité associé

```sql
SELECT
    COUNT(*) AS invalid_amount_count
FROM source_table
WHERE amount IS NOT NULL
  AND TRY_TO_NUMBER(amount) IS NULL;
```

---

## 7.3 Dates et timestamps

### Règles

- Toujours préciser le format ou la stratégie de parsing si la source est texte.
- Documenter le fuseau horaire.
- Ne pas mélanger timestamps locaux et UTC sans conversion explicite.
- Séparer date métier, date source, date ingestion et date traitement.
- Utiliser des noms explicites : `order_date`, `created_at_utc`, `ingested_at`, `processed_at`.

### Exemple

```sql
SELECT
    order_id,
    TRY_TO_TIMESTAMP_NTZ(order_timestamp_raw, 'YYYY-MM-DD HH24:MI:SS') AS order_timestamp_ntz,
    CONVERT_TIMEZONE('Europe/Paris', 'UTC', order_timestamp_raw::TIMESTAMP_NTZ) AS order_timestamp_utc
FROM RAW.ORDER_EVENTS;
```

---

## 7.4 Montants et précision

### Règles

- Utiliser `NUMBER(precision, scale)` pour montants financiers.
- Éviter `FLOAT` pour calculs financiers.
- Documenter devise et échelle.
- Ne pas mélanger EUR, kEUR, cents et montants bruts.
- Convertir les montants dans une couche contrôlée.

### Exemple

```sql
SELECT
    invoice_id,
    TRY_TO_DECIMAL(amount_raw, 18, 2) AS amount_eur
FROM RAW.INVOICE;
```

---

## 7.5 Booléens et flags

### Règles

- Les flags doivent avoir une signification métier claire.
- Éviter les chaînes ambiguës : `Y`, `N`, `1`, `0`, `true`, `false` mélangés.
- Normaliser dans la couche staging.
- Documenter les valeurs inconnues.

```sql
CASE
    WHEN UPPER(is_active_raw) IN ('Y', 'YES', 'TRUE', '1') THEN TRUE
    WHEN UPPER(is_active_raw) IN ('N', 'NO', 'FALSE', '0') THEN FALSE
    ELSE NULL
END AS is_active
```

---

## 8. Gestion des NULL

## 8.1 Principes

`NULL` signifie absence de valeur. Il ne doit pas être confondu avec :

- zéro ;
- chaîne vide ;
- inconnu ;
- non applicable ;
- non renseigné ;
- non disponible ;
- erreur de parsing.

---

## 8.2 Règles

- Documenter la signification de `NULL` pour colonnes critiques.
- Ne pas remplacer systématiquement `NULL` par `0`.
- Utiliser `COALESCE` uniquement si la substitution a un sens métier.
- Préserver les `NULL` techniques jusqu’à la couche où une règle métier est validée.
- Mesurer les taux de nullité.
- Isoler les nulls interdits dans une table de rejet ou audit.

---

## 8.3 Mauvaise pratique

```sql
SELECT
    COALESCE(discount_amount, 0) AS discount_amount
FROM SALES;
```

Si `NULL` signifie “non calculé”, ce remplacement peut produire une analyse fausse.

---

## 8.4 Bonne pratique

```sql
SELECT
    discount_amount,
    CASE
        WHEN discount_amount IS NULL THEN 'MISSING_DISCOUNT'
        ELSE 'VALID'
    END AS discount_quality_status
FROM SALES;
```

---

## 8.5 Null-safe comparisons

### Attention

Les comparaisons avec `NULL` doivent être explicites.

```sql
WHERE customer_email IS NULL
```

et non :

```sql
WHERE customer_email = NULL
```

---

## 9. CTE, modularité et lisibilité

## 9.1 Règles

- Les requêtes complexes doivent être structurées en CTE.
- Chaque CTE doit avoir un rôle clair.
- Les CTE doivent être nommés sémantiquement.
- Les CTE inutilisés doivent être supprimés.
- La profondeur excessive doit être évitée.
- Les CTE ne doivent pas masquer une transformation trop complexe.
- Les étapes critiques peuvent être matérialisées pour audit ou performance.

---

## 9.2 Pattern recommandé

```sql
WITH source_data AS (
    SELECT
        ...
    FROM ...
),

validated_data AS (
    SELECT
        ...
    FROM source_data
    WHERE ...
),

deduped_data AS (
    SELECT
        ...
    FROM validated_data
    QUALIFY ROW_NUMBER() OVER (
        PARTITION BY business_key
        ORDER BY updated_at DESC, load_id DESC
    ) = 1
),

final AS (
    SELECT
        ...
    FROM deduped_data
)

SELECT
    ...
FROM final;
```

---

## 9.3 Quand matérialiser

Matérialiser une étape peut être justifié si :

- elle est réutilisée ;
- elle facilite le debug ;
- elle réduit fortement le volume ;
- elle évite un recalcul coûteux ;
- elle constitue un checkpoint de qualité ;
- elle isole un traitement instable ;
- elle permet une reprise après incident.

---

## 9.4 Méga-requêtes

Une requête trop longue doit être refactorisée si :

- elle dépasse la compréhension en code review ;
- elle mélange ingestion, nettoyage, business logic et agrégation ;
- elle n’a aucun checkpoint ;
- elle est difficile à tester ;
- elle est lente sans diagnostic clair ;
- elle est critique pour la production.

---

## 10. Joins et cardinalité

## 10.1 Règles générales

- Toute clé de jointure doit être explicite.
- La cardinalité attendue doit être connue.
- Les jointures doivent être testées contre duplications.
- Les jointures many-to-many doivent être explicitement justifiées.
- Les filtres doivent être placés de manière lisible.
- Les colonnes utilisées pour joindre doivent avoir types compatibles.
- Les clés texte longues doivent être évitées si une clé substitut existe.
- Les jointures sur champs normalisés doivent être faites après normalisation.

---

## 10.2 Cardinalités attendues

| Cardinalité | Exemple | Risque |
|---|---|---|
| 1-1 | client actuel ↔ profil client | doublon inattendu |
| 1-N | client ↔ commandes | duplication normale côté commande |
| N-1 | transaction ↔ produit | produit manquant |
| N-N | clients ↔ segments multiples | explosion de lignes |
| Optional | commande ↔ promotion | valeurs NULL attendues |

---

## 10.3 Tests de cardinalité

### Duplicats côté dimension

```sql
SELECT
    product_id,
    COUNT(*) AS row_count
FROM CURATED.PRODUCT
GROUP BY product_id
HAVING COUNT(*) > 1;
```

### Explosion de lignes après join

```sql
WITH before_join AS (
    SELECT COUNT(*) AS row_count
    FROM STAGING.SALES
),

after_join AS (
    SELECT COUNT(*) AS row_count
    FROM STAGING.SALES AS sales
    LEFT JOIN CURATED.PRODUCT AS prod
        ON prod.product_id = sales.product_id
)

SELECT
    before_join.row_count AS before_count,
    after_join.row_count AS after_count,
    after_join.row_count - before_join.row_count AS row_delta
FROM before_join
CROSS JOIN after_join;
```

---

## 10.4 Joins explicites

### Recommandé

```sql
SELECT
    sales.transaction_id,
    sales.product_id,
    prod.product_category
FROM CURATED.SALES AS sales
INNER JOIN CURATED.PRODUCT AS prod
    ON prod.product_id = sales.product_id;
```

### Interdit

```sql
SELECT
    *
FROM CURATED.SALES, CURATED.PRODUCT
WHERE PRODUCT.product_id = SALES.product_id;
```

---

## 10.5 LEFT JOIN

### Règle

Un `LEFT JOIN` ne doit pas être transformé accidentellement en `INNER JOIN` par un filtre dans `WHERE`.

### Mauvais exemple

```sql
SELECT
    sales.transaction_id,
    promo.promotion_name
FROM SALES AS sales
LEFT JOIN PROMOTION AS promo
    ON promo.promotion_id = sales.promotion_id
WHERE promo.is_active = TRUE;
```

### Correction

```sql
SELECT
    sales.transaction_id,
    promo.promotion_name
FROM SALES AS sales
LEFT JOIN PROMOTION AS promo
    ON promo.promotion_id = sales.promotion_id
   AND promo.is_active = TRUE;
```

---

## 10.6 Many-to-many

Les jointures many-to-many sont interdites sans :

- justification ;
- estimation d’impact volumétrique ;
- test de duplication ;
- règle d’allocation si métrique additive ;
- validation métier ;
- documentation.

---

## 11. Window functions et QUALIFY

## 11.1 Principes

Les fonctions analytiques sont puissantes mais doivent être explicites.

### Règles

- Toujours définir `PARTITION BY` selon la logique métier.
- Toujours définir `ORDER BY` lorsque l’ordre influence le résultat.
- Gérer les ex æquo explicitement.
- Utiliser `QUALIFY` pour filtrer sur des fonctions analytiques lorsque cela améliore la lisibilité.
- Éviter les fenêtres sur très grands volumes sans filtre ou partition pertinente.
- Tester les résultats sur cas limites.

---

## 11.2 ROW_NUMBER vs RANK vs DENSE_RANK

| Fonction | Usage | Gestion des ex æquo |
|---|---|---|
| `ROW_NUMBER()` | choisir une ligne unique | casse les ex æquo selon ordre |
| `RANK()` | classement avec trous | mêmes rangs, rangs sautés |
| `DENSE_RANK()` | classement sans trous | mêmes rangs, pas de saut |

---

## 11.3 Exemple avec QUALIFY

```sql
SELECT
    customer_id,
    order_id,
    order_date,
    order_amount_eur
FROM CURATED.ORDER
QUALIFY ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY order_date DESC, order_id DESC
) = 1;
```

---

## 11.4 Window frames

### Règle

Pour les agrégats analytiques cumulés ou glissants, le frame doit être explicite si le comportement par défaut peut être ambigu.

```sql
SELECT
    sales_date,
    product_id,
    sales_amount_eur,
    SUM(sales_amount_eur) OVER (
        PARTITION BY product_id
        ORDER BY sales_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS cumulative_sales_amount_eur
FROM MART.FACT_SALES_DAILY;
```

---

## 11.5 Anti-patterns

- `ROW_NUMBER()` sans ordre stable.
- Déduplication sans tie-breaker.
- Fenêtre globale sur table massive.
- `ORDER BY updated_at` alors que plusieurs lignes ont le même timestamp.
- Utilisation de `RANK()` alors qu’une seule ligne doit être conservée.
- Utilisation d’une fenêtre pour remplacer une agrégation simple.

---

## 12. Agrégations

## 12.1 Règles

- La granularité de sortie doit être documentée.
- Les colonnes non agrégées doivent correspondre au grain.
- Les métriques additives, semi-additives et non additives doivent être distinguées.
- Les ratios doivent être calculés à partir de numérateur/dénominateur agrégés, pas comme moyenne de ratios, sauf justification.
- Les périodes incomplètes doivent être gérées.
- Les dimensions optionnelles doivent être traitées explicitement.

---

## 12.2 Ratio correct

### Mauvaise pratique

```sql
SELECT
    region_code,
    AVG(margin_amount / sales_amount) AS margin_rate
FROM SALES
GROUP BY region_code;
```

### Bonne pratique

```sql
SELECT
    region_code,
    SUM(margin_amount) / NULLIF(SUM(sales_amount), 0) AS margin_rate
FROM SALES
GROUP BY region_code;
```

---

## 12.3 GROUP BY

### Recommandation

Préférer les noms de colonnes ou expressions claires selon convention d’équipe. Les positions numériques peuvent être concises mais moins lisibles en code critique.

```sql
SELECT
    sales_date,
    product_id,
    SUM(sales_amount_eur) AS sales_amount_eur
FROM MART.FACT_SALES
GROUP BY
    sales_date,
    product_id;
```

---

## 13. Temporalité et logique de dates

## 13.1 Types de dates

Les colonnes temporelles doivent distinguer :

| Colonne | Signification |
|---|---|
| `event_at` | moment de l’événement métier |
| `created_at` | création dans la source |
| `updated_at` | dernière modification source |
| `ingested_at` | ingestion dans Snowflake |
| `processed_at` | transformation/pipeline |
| `valid_from` | début validité métier ou SCD |
| `valid_to` | fin validité métier ou SCD |
| `deleted_at` | suppression logique |

---

## 13.2 Règles

- Toujours expliciter le fuseau horaire lorsque pertinent.
- Ne pas comparer des timestamps de fuseaux différents sans conversion.
- Utiliser des bornes temporelles demi-ouvertes : `>= start` et `< end`.
- Documenter les calendriers fiscaux.
- Éviter `DATE_TRUNC` sur colonne filtrée si un filtre range est possible.
- Prévoir les late arriving data.
- Ne pas mélanger date d’événement et date d’ingestion.

---

## 13.3 Filtre temporel recommandé

```sql
WHERE transaction_date >= DATE '2026-06-01'
  AND transaction_date < DATE '2026-07-01'
```

Plutôt que :

```sql
WHERE DATE_TRUNC('month', transaction_date) = DATE '2026-06-01'
```

---

## 13.4 Watermarks

### Règles

- Le watermark doit être persistant.
- Le watermark doit être audité.
- Les late arriving data doivent être couvertes par une fenêtre de recouvrement si nécessaire.
- Les pipelines doivent enregistrer la période traitée.
- Les reprocess doivent être possibles.

### Exemple

```sql
SELECT
    MAX(updated_at) AS new_watermark
FROM STAGING.ORDER_EVENTS
WHERE updated_at > :previous_watermark;
```

---

## 14. Semi-structured data

## 14.1 Principes

Snowflake permet de stocker et requêter des données semi-structurées avec `VARIANT`, `OBJECT` et `ARRAY`. Cette puissance doit être gouvernée.

### Règles

- Les données semi-structurées doivent être normalisées lorsque les champs sont fréquemment utilisés.
- Les chemins JSON critiques doivent être documentés.
- Les types extraits doivent être castés explicitement.
- Les erreurs de parsing doivent être mesurées.
- Les changements de structure doivent être détectés.
- Les colonnes `VARIANT` ne doivent pas devenir un substitut permanent à une modélisation claire.
- Les performances doivent être testées entre requête directe `VARIANT` et extraction relationnelle.

---

## 14.2 Extraction simple

```sql
SELECT
    payload:customer.id::STRING AS customer_id,
    payload:customer.email::STRING AS customer_email,
    TRY_TO_TIMESTAMP_NTZ(payload:event_time::STRING) AS event_time
FROM RAW.EVENTS;
```

---

## 14.3 FLATTEN

`FLATTEN` est une table function permettant d’exploser `VARIANT`, `OBJECT` ou `ARRAY`.

```sql
SELECT
    event.event_id,
    item.value:product_id::STRING AS product_id,
    item.value:quantity::NUMBER AS quantity
FROM RAW.ORDER_EVENTS AS event,
LATERAL FLATTEN(input => event.payload:items) AS item;
```

---

## 14.4 Règles FLATTEN

- Toujours vérifier la cardinalité générée.
- Toujours conserver une clé de rattachement à l’événement parent.
- Caster explicitement les champs extraits.
- Gérer les arrays vides.
- Tester les payloads invalides.
- Éviter les `FLATTEN` multiples non maîtrisés.
- Préférer une table relationnelle normalisée pour champs critiques fréquents.

---

## 14.5 Contrôle de dérive JSON

```sql
SELECT
    OBJECT_KEYS(payload) AS top_level_keys,
    COUNT(*) AS row_count
FROM RAW.EVENTS
GROUP BY top_level_keys
ORDER BY row_count DESC;
```

---

## 15. Incrémentalité, MERGE et idempotence

## 15.1 Principes

Les traitements incrémentaux doivent être :

- déterministes ;
- rejouables ;
- auditables ;
- compatibles late arriving data ;
- protégés contre doublons ;
- monitorés ;
- testés.

---

## 15.2 MERGE standard

```sql
MERGE INTO CURATED.CUSTOMER AS tgt
USING (
    SELECT
        customer_id,
        customer_name,
        customer_segment,
        updated_at,
        record_hash
    FROM STAGING.CUSTOMER_DEDUPED
) AS src
    ON tgt.customer_id = src.customer_id
WHEN MATCHED
     AND tgt.record_hash <> src.record_hash
THEN UPDATE SET
    customer_name = src.customer_name,
    customer_segment = src.customer_segment,
    updated_at = src.updated_at,
    record_hash = src.record_hash,
    processed_at = CURRENT_TIMESTAMP()
WHEN NOT MATCHED
THEN INSERT (
    customer_id,
    customer_name,
    customer_segment,
    updated_at,
    record_hash,
    processed_at
) VALUES (
    src.customer_id,
    src.customer_name,
    src.customer_segment,
    src.updated_at,
    src.record_hash,
    CURRENT_TIMESTAMP()
);
```

---

## 15.3 Règles MERGE

- La source du `MERGE` doit être dédupliquée.
- La clé de merge doit être stable.
- Les updates doivent être conditionnels si possible.
- Les suppressions doivent être explicites.
- Les soft deletes doivent être préférés si audit requis.
- Les volumes impactés doivent être journalisés.
- Les `MERGE` critiques doivent être testés sur cas :
  - insert ;
  - update ;
  - unchanged ;
  - delete ;
  - duplicate source ;
  - late arriving data.

---

## 15.4 Déduplication avant MERGE

```sql
WITH deduped_source AS (
    SELECT
        customer_id,
        customer_name,
        updated_at,
        load_id,
        record_hash
    FROM STAGING.CUSTOMER_EVENTS
    QUALIFY ROW_NUMBER() OVER (
        PARTITION BY customer_id
        ORDER BY updated_at DESC, load_id DESC
    ) = 1
)

SELECT
    *
FROM deduped_source;
```

---

## 15.5 Hash de changement

```sql
MD5(
    CONCAT_WS(
        '|',
        COALESCE(customer_name, ''),
        COALESCE(customer_segment, ''),
        COALESCE(customer_status, '')
    )
) AS record_hash
```

### Règles

- Documenter les colonnes incluses.
- Stabiliser l’ordre.
- Normaliser les valeurs si nécessaire.
- Attention aux collisions théoriques.
- Ne pas inclure `processed_at` ou champs techniques variables.

---

## 15.6 Soft delete

```sql
WHEN MATCHED AND src.is_deleted = TRUE
THEN UPDATE SET
    is_deleted = TRUE,
    deleted_at = CURRENT_TIMESTAMP(),
    processed_at = CURRENT_TIMESTAMP()
```

---

## 15.7 Hard delete

Les hard deletes sont autorisés uniquement si :

- besoin légal ou technique validé ;
- audit réalisé ;
- rollback possible ;
- périmètre contrôlé ;
- approbation obtenue.

---

## 16. Transactions, DDL et DML

## 16.1 Principes

Les opérations critiques doivent être contrôlées, atomiques lorsque possible et vérifiables avant/après.

### Règles

- Séparer scripts DDL et DML.
- Encadrer les DML critiques par contrôles.
- Utiliser transactions explicites lorsque plusieurs DML doivent réussir ensemble.
- Comprendre les comportements DDL dans les transactions.
- Prévoir rollback ou stratégie de restauration.
- Éviter les changements destructifs non réversibles.
- Journaliser les volumes impactés.

---

## 16.2 Transaction DML

```sql
BEGIN;

DELETE FROM MART.FACT_SALES_DAILY
WHERE sales_date >= :start_date
  AND sales_date < :end_date;

INSERT INTO MART.FACT_SALES_DAILY (
    sales_date,
    product_id,
    region_code,
    sales_amount_eur,
    transaction_count,
    processed_at
)
SELECT
    sales_date,
    product_id,
    region_code,
    sales_amount_eur,
    transaction_count,
    CURRENT_TIMESTAMP()
FROM TEMP_FACT_SALES_DAILY_REBUILD;

COMMIT;
```

---

## 16.3 Contrôle avant/après

```sql
SELECT COUNT(*) AS rows_before
FROM MART.FACT_SALES_DAILY
WHERE sales_date >= :start_date
  AND sales_date < :end_date;

-- DML here

SELECT COUNT(*) AS rows_after
FROM MART.FACT_SALES_DAILY
WHERE sales_date >= :start_date
  AND sales_date < :end_date;
```

---

## 16.4 Changements DDL

### Backward-compatible

- ajout colonne nullable ;
- ajout vue ;
- ajout table ;
- ajout tag ;
- ajout commentaire ;
- ajout policy non bloquante après test.

### Breaking changes

- suppression colonne ;
- renommage colonne ;
- changement type incompatible ;
- changement de granularité ;
- suppression vue contractuelle ;
- modification sémantique KPI ;
- changement de policy exposant/masquant différemment.

---

## 16.5 DDL évolutif

### Recommandé

```sql
ALTER TABLE CURATED.CUSTOMER
ADD COLUMN IF NOT EXISTS customer_lifecycle_stage STRING;
```

### À éviter sans plan

```sql
DROP COLUMN customer_segment;
```

---

## 17. Sécurité et conformité dans le SQL

## 17.1 Règles

- Ne pas exposer de colonnes sensibles non nécessaires.
- Utiliser vues sécurisées, masking policies et row access policies prévues.
- Ne pas hardcoder secrets, tokens, emails personnels ou identifiants sensibles.
- Ne pas contourner les policies avec copies non protégées.
- Ne pas exporter de données sensibles via requêtes ad hoc non approuvées.
- Ne pas utiliser des rôles admin dans scripts métier.
- Éviter de dupliquer des colonnes PII dans les marts.
- Documenter toute exposition de données sensibles.

---

## 17.2 Projection sécurisée

### Mauvais exemple

```sql
CREATE OR REPLACE VIEW MART.VW_CUSTOMER_ANALYSIS AS
SELECT
    customer_id,
    full_name,
    email,
    phone,
    customer_segment
FROM CURATED.CUSTOMER;
```

### Meilleur exemple

```sql
CREATE OR REPLACE SECURE VIEW MART.SVW_CUSTOMER_ANALYSIS AS
SELECT
    customer_id,
    customer_segment,
    customer_lifecycle_stage
FROM CURATED.CUSTOMER;
```

---

## 17.3 Filtrage par rôle

Ne pas coder des règles de sécurité dispersées dans de multiples requêtes si une row access policy est disponible et gouvernée.

### À éviter

```sql
WHERE region_code IN ('FR', 'BE')
```

dans plusieurs vues pour simuler une sécurité.

### Préférer

- row access policy ;
- table de mapping des droits ;
- secure view ;
- RBAC ;
- tag-based masking si pertinent.

---

## 18. Performance Snowflake SQL

## 18.1 Principes

La performance Snowflake dépend de :

- volume scanné ;
- micro-partition pruning ;
- filtres ;
- projection de colonnes ;
- jointures ;
- cardinalité ;
- warehouse ;
- concurrence ;
- spill ;
- clustering ;
- search optimization ;
- query acceleration ;
- matérialisation ;
- cache ;
- fréquence d’exécution.

---

## 18.2 Règles fondamentales

- Limiter les colonnes sélectionnées.
- Filtrer tôt.
- Utiliser des prédicats simples et efficaces.
- Éviter les fonctions sur colonnes filtrées critiques.
- Réduire les données avant joins lourds.
- Agréger au bon grain.
- Éviter les `DISTINCT` pour masquer un problème de join.
- Éviter les cross joins accidentels.
- Tester avec volumes réalistes.
- Utiliser Query Profile pour requêtes critiques.
- Optimiser la logique avant d’augmenter le warehouse.

---

## 18.3 Predicate pruning

### Moins efficace

```sql
WHERE DATE_TRUNC('month', transaction_date) = DATE '2026-06-01'
```

### Plus efficace

```sql
WHERE transaction_date >= DATE '2026-06-01'
  AND transaction_date < DATE '2026-07-01'
```

---

## 18.4 Projection

### Mauvais exemple

```sql
SELECT
    *
FROM CURATED.SALES
WHERE transaction_date >= DATE '2026-06-01';
```

### Bon exemple

```sql
SELECT
    transaction_id,
    transaction_date,
    product_id,
    region_code,
    sales_amount_eur
FROM CURATED.SALES
WHERE transaction_date >= DATE '2026-06-01';
```

---

## 18.5 DISTINCT

### Règle

`DISTINCT` ne doit pas être utilisé pour masquer un problème de duplication.

### Mauvais usage

```sql
SELECT DISTINCT
    sales.transaction_id,
    prod.product_category
FROM SALES AS sales
JOIN PRODUCT AS prod
    ON prod.product_id = sales.product_id;
```

### Correction

Tester et corriger la cardinalité de `PRODUCT`.

---

## 18.6 Query Profile

Pour toute requête critique lente, analyser :

- bytes scanned ;
- partitions scanned ;
- pruning ;
- join order ;
- join type ;
- explosion de lignes ;
- spills disque ;
- opérateurs les plus coûteux ;
- temps de compilation ;
- temps d’exécution ;
- remote/local disk I/O ;
- skew ;
- queue time ;
- warehouse size.

---

## 18.7 Clustering

### À envisager si

- table très volumineuse ;
- filtres fréquents sur colonnes spécifiques ;
- pruning insuffisant ;
- requêtes critiques répétées ;
- bénéfice mesuré supérieur au coût.

### À éviter si

- table petite ;
- patterns de requête instables ;
- coût de maintenance supérieur au gain ;
- clustering choisi “par habitude” ;
- aucune mesure avant/après.

---

## 18.8 Search Optimization Service

À envisager pour :

- point lookups ;
- filtres très sélectifs ;
- recherches sur colonnes spécifiques ;
- certaines recherches dans données semi-structurées.

### Règle

Doit être activé uniquement après mesure et justification.

---

## 18.9 Query Acceleration Service

À envisager pour certaines requêtes qui bénéficient d’offload sur ressources partagées Snowflake.

### Règle

Doit être piloté par mesure, coût et bénéfice réel.

---

## 19. Dynamic Tables, Streams et Tasks dans le SQL

## 19.1 Dynamic Tables

### Cas d’usage

- logique exprimable en `SELECT` ;
- pipeline déclaratif ;
- fraîcheur contrôlée ;
- transformations multi-étapes ;
- agrégations maintenues.

### Règles SQL

- La requête doit être lisible et testable.
- Le grain de sortie doit être documenté.
- `TARGET_LAG` doit correspondre au SLA.
- Les coûts doivent être monitorés.
- Les dépendances doivent être documentées.
- Les requêtes doivent éviter la complexité excessive.

---

## 19.2 Streams + Tasks

### Cas d’usage

- CDC ;
- traitement incrémental ;
- logique impérative ;
- MERGE contrôlé ;
- side effects ;
- audit spécifique.

### Règles

- Le stream doit être consommé régulièrement.
- Le task doit être monitoré.
- La logique doit être idempotente.
- Les erreurs doivent être auditables.
- Les DAGs de tasks doivent être documentés.

---

## 19.3 Choix recommandé

| Besoin | Pattern |
|---|---|
| Transformation déclarative maintenue fraîche | Dynamic Table |
| Upsert contrôlé avec audit | Stream + Task + MERGE |
| Chargement fichiers | COPY / Snowpipe |
| Vue contractuelle sans matérialisation | View / Secure View |
| Agrégat très répété | Dynamic Table ou Materialized View |
| Procédure complexe | Stored Procedure + Task |

---

## 20. Data quality SQL

## 20.1 Tests minimaux

Chaque table critique doit avoir des tests SQL couvrant :

- schéma ;
- nullabilité ;
- unicité ;
- duplicats ;
- bornes ;
- référentiels ;
- fraîcheur ;
- volumes ;
- réconciliation ;
- dérive de distribution ;
- valeurs invalides ;
- orphan keys.

---

## 20.2 Tests nullabilité

```sql
SELECT
    COUNT(*) AS invalid_count
FROM CURATED.CUSTOMER
WHERE customer_id IS NULL;
```

---

## 20.3 Tests unicité

```sql
SELECT
    customer_id,
    COUNT(*) AS row_count
FROM CURATED.CUSTOMER
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

---

## 20.4 Tests bornes métier

```sql
SELECT
    COUNT(*) AS invalid_count
FROM CURATED.SALES
WHERE sales_amount_eur < 0;
```

---

## 20.5 Tests fraîcheur

```sql
SELECT
    MAX(ingested_at) AS latest_ingestion_time,
    DATEDIFF('minute', MAX(ingested_at), CURRENT_TIMESTAMP()) AS minutes_since_latest_ingestion
FROM RAW.CRM_CUSTOMER;
```

---

## 20.6 Tests réconciliation

```sql
WITH source_total AS (
    SELECT
        SUM(amount_eur) AS amount_eur
    FROM STAGING.SALES_SOURCE
),

target_total AS (
    SELECT
        SUM(sales_amount_eur) AS amount_eur
    FROM CURATED.SALES
)

SELECT
    source_total.amount_eur AS source_amount,
    target_total.amount_eur AS target_amount,
    target_total.amount_eur - source_total.amount_eur AS delta_amount
FROM source_total
CROSS JOIN target_total;
```

---

## 20.7 Table de résultats DQ

```sql
CREATE TABLE IF NOT EXISTS OPS.DATA_QUALITY_RESULT (
    test_run_id STRING,
    test_name STRING,
    domain STRING,
    object_name STRING,
    severity STRING,
    status STRING,
    checked_at TIMESTAMP_LTZ,
    invalid_count NUMBER,
    sample_query STRING,
    owner STRING
);
```

---

## 21. Observabilité SQL

## 21.1 Query tags

Tout SQL exécuté par pipeline doit définir `QUERY_TAG`.

```sql
ALTER SESSION SET QUERY_TAG = 'app=dbt;domain=sales;env=prod;model=fact_sales_daily;run_id=20260612_070000';
```

### Règles

- `QUERY_TAG` obligatoire en PROD pour pipelines.
- Le format doit être standardisé.
- Les valeurs doivent permettre de suivre coûts, erreurs et durée.
- Les ad hoc critiques doivent aussi être taggés.

---

## 21.2 Audit tables

Chaque pipeline critique doit enregistrer :

- run_id ;
- pipeline ;
- étape ;
- start time ;
- end time ;
- statut ;
- query_id ;
- rows read ;
- rows inserted ;
- rows updated ;
- rows deleted ;
- rows rejected ;
- watermark start ;
- watermark end ;
- error message.

---

## 21.3 Requête monitoring des erreurs

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

## 21.4 Requête top coûts/durées

```sql
SELECT
    query_id,
    user_name,
    role_name,
    warehouse_name,
    query_tag,
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

## 21.5 SLA SQL

Pour les pipelines critiques, monitorer :

- durée ;
- fraîcheur ;
- volumes ;
- erreurs ;
- rejets ;
- coût ;
- rows affected ;
- watermark ;
- nombre de partitions/scans si pertinent ;
- dérive par rapport à baseline.

---

## 22. CI/CD et code review

## 22.1 Règles

- Tout SQL de production doit passer par code review.
- Les scripts doivent être versionnés.
- Les changements doivent être testés en DEV/UAT.
- Les tests qualité doivent être exécutés avant merge.
- Les changements breaking doivent être identifiés.
- Les rollbacks doivent être prévus.
- Les secrets sont interdits dans le code.
- Les migrations doivent être idempotentes lorsque possible.

---

## 22.2 Pull Request checklist

```markdown
# Pull Request Checklist — Snowflake SQL

## Code quality

- [ ] Style SQL respecté
- [ ] CTE nommés clairement
- [ ] Alias explicites
- [ ] Pas de `SELECT *`
- [ ] Colonnes explicitement listées
- [ ] Commentaires d’intention ajoutés si nécessaire

## Determinism

- [ ] `ROW_NUMBER` avec `ORDER BY` stable
- [ ] `LIMIT` avec `ORDER BY` si logique métier
- [ ] Déduplication déterministe
- [ ] Casts explicites
- [ ] Fuseau horaire traité

## Joins

- [ ] Clés de jointure explicites
- [ ] Cardinalité attendue documentée
- [ ] Tests duplicats effectués
- [ ] Pas de many-to-many non justifié

## Data quality

- [ ] Tests nulls
- [ ] Tests duplicats
- [ ] Tests volumes
- [ ] Tests bornes métier
- [ ] Réconciliation source/cible si applicable

## Performance

- [ ] Colonnes limitées
- [ ] Filtres efficaces
- [ ] Query Profile analysé si requête critique
- [ ] Warehouse adapté
- [ ] Impact coût estimé

## Security

- [ ] Pas de données sensibles inutiles
- [ ] Policies/vues sécurisées respectées
- [ ] Pas de secrets hardcodés
- [ ] Rôle d’exécution conforme

## Deployment

- [ ] Script rejouable/idempotent si pipeline
- [ ] Rollback documenté
- [ ] Changelog mis à jour
- [ ] Documentation/data contract mis à jour
```

---

## 22.3 Tests automatisés

### Catégories

- schema tests ;
- data tests ;
- uniqueness tests ;
- relationship tests ;
- accepted values ;
- freshness tests ;
- regression tests ;
- performance smoke tests ;
- security tests.

---

## 22.4 Non-regression KPI

```sql
WITH previous_result AS (
    SELECT
        SUM(sales_amount_eur) AS sales_amount_eur
    FROM VALIDATION.PREVIOUS_FACT_SALES_DAILY
),

new_result AS (
    SELECT
        SUM(sales_amount_eur) AS sales_amount_eur
    FROM MART.FACT_SALES_DAILY
)

SELECT
    previous_result.sales_amount_eur AS previous_amount,
    new_result.sales_amount_eur AS new_amount,
    new_result.sales_amount_eur - previous_result.sales_amount_eur AS delta_amount
FROM previous_result
CROSS JOIN new_result;
```

---

## 23. Documentation SQL

## 23.1 Documentation minimale par script critique

```markdown
# SQL Script Documentation

## Script

Nom:

## Objectif

Description:

## Owner

Technique:
Métier:

## Sources

- Source 1
- Source 2

## Cible

Table/vue:

## Grain

1 ligne = ...

## Incrémentalité

- Watermark:
- Fenêtre:
- Late arriving data:

## Règles métier

- Règle 1
- Règle 2

## Règles qualité

- Test 1
- Test 2

## Données sensibles

- Oui/Non
- Protection:

## Performance

- Warehouse:
- Fréquence:
- Volume attendu:

## Rollback

Procédure:

## Dernière validation

Date:
```

---

## 23.2 Commentaires d’objets

### Tables

```sql
COMMENT ON TABLE MART.FACT_SALES_DAILY IS
'Daily sales fact table. Grain: 1 row per sales_date, product_id, region_code. Used by certified Sales Power BI reports.';
```

### Colonnes

```sql
COMMENT ON COLUMN MART.FACT_SALES_DAILY.sales_amount_eur IS
'Validated sales amount in EUR, excluding cancelled transactions and VAT.';
```

---

## 24. Lignes rouges

Les pratiques suivantes sont interdites :

- déployer du SQL non relu en production ;
- utiliser `SELECT *` dans transformations critiques ;
- utiliser `DISTINCT` pour masquer des duplications de join ;
- ignorer les duplications issues de joins many-to-many ;
- dédupliquer avec `ROW_NUMBER()` sans ordre stable ;
- utiliser `LIMIT` sans `ORDER BY` pour une logique métier ;
- modifier un schéma critique sans plan de compatibilité ;
- hardcoder des secrets ou identifiants sensibles ;
- exposer des données sensibles non nécessaires ;
- faire du développement exploratoire en PROD ;
- exécuter des DML destructifs sans rollback ;
- ignorer les alertes de coût/performance ;
- fusionner un `MERGE` avec source non dédupliquée ;
- mélanger dates locales et UTC sans conversion ;
- remplacer des NULL par zéro sans validation métier ;
- créer une vue contractuelle avec `SELECT *` ;
- ajouter une logique métier implicite non documentée ;
- augmenter le warehouse sans analyse Query Profile ;
- créer des tables mart sans grain documenté ;
- ignorer les tests de réconciliation sur pipelines critiques.

---

## 25. Astuces expertes et gains rapides

## 25.1 Nommer les CTE comme des étapes de pipeline

```sql
WITH source_ AS (...),
validated_ AS (...),
deduped_ AS (...),
enriched_ AS (...),
final_ AS (...)
```

Cela rend le debug et la review plus rapides.

---

## 25.2 Mesurer avant optimiser

Prioriser :

- top requêtes par durée ;
- top requêtes par bytes scanned ;
- top requêtes par coût warehouse ;
- top requêtes par fréquence ;
- top requêtes par impact métier.

---

## 25.3 Limiter les colonnes tôt

Réduire la projection dans la première CTE diminue souvent scan, mémoire et complexité.

---

## 25.4 Tester cardinalité en CI

Un test de duplication évite des semaines de KPI faux.

---

## 25.5 MERGE avec clé stable

Toujours merger sur une clé technique ou métier fiable, jamais sur un libellé instable.

---

## 25.6 Commentaires d’intention

Documenter pourquoi une règle existe :

```sql
-- Keep a 3-day overlap to catch late arriving transactions from ERP weekend batches.
WHERE updated_at >= DATEADD('day', -3, :watermark)
```

---

## 25.7 Refactoring incrémental

Préférer petits changements testables à une réécriture massive non contrôlée.

---

## 25.8 QUALIFY pour lisibilité

Utiliser `QUALIFY` pour réduire les sous-requêtes inutiles lors des filtrages window functions.

---

## 25.9 Query tags exploitables

Un bon query tag réduit fortement le temps d’investigation en incident.

---

## 25.10 Quarantine first

Ne pas jeter silencieusement les lignes invalides. Les isoler, les mesurer, les corriger.

---

## 26. Mode opératoire standard

## 26.1 SOP — Développement d’une transformation SQL

1. Lire le besoin métier.
2. Identifier sources et data contract.
3. Définir grain cible.
4. Identifier données sensibles.
5. Écrire requête en CTE lisibles.
6. Lister colonnes explicitement.
7. Ajouter règles de validation.
8. Tester cardinalité des joins.
9. Tester nulls, duplicats, bornes.
10. Tester volumes.
11. Tester performance sur échantillon représentatif.
12. Ajouter query tag.
13. Documenter hypothèses.
14. Soumettre en code review.
15. Corriger retours.
16. Déployer en DEV.
17. Valider en UAT.
18. Déployer en PROD.
19. Monitorer post-déploiement.

---

## 26.2 SOP — Optimisation d’une requête lente

1. Confirmer impact métier.
2. Récupérer query_id.
3. Analyser Query Profile.
4. Identifier bytes scanned.
5. Identifier partitions scannées.
6. Identifier joins coûteux.
7. Identifier spills.
8. Vérifier filtres.
9. Vérifier projection de colonnes.
10. Vérifier cardinalité.
11. Tester refactoring SQL.
12. Tester pré-agrégation si pertinent.
13. Évaluer clustering/search optimization si besoin.
14. Comparer avant/après.
15. Documenter gain et coût.
16. Déployer changement.
17. Monitorer.

---

## 26.3 SOP — Mise en production MERGE incrémental

1. Définir clé de merge.
2. Définir watermark.
3. Gérer late arriving data.
4. Dédupliquer source.
5. Créer hash de changement si nécessaire.
6. Tester insert.
7. Tester update.
8. Tester unchanged.
9. Tester duplicate source.
10. Tester delete/soft delete.
11. Tester re-run.
12. Journaliser rows affected.
13. Ajouter audit table.
14. Ajouter monitoring.
15. Déployer via CI/CD.
16. Surveiller première exécution PROD.

---

## 26.4 SOP — Correction de données en production

1. Créer ticket.
2. Qualifier impact.
3. Identifier tables concernées.
4. Extraire échantillon avant correction.
5. Créer clone ou backup logique si nécessaire.
6. Préparer script transactionnel.
7. Préparer rollback.
8. Faire peer review.
9. Obtenir approbation.
10. Exécuter en fenêtre contrôlée.
11. Vérifier volumes.
12. Vérifier KPI.
13. Documenter correction.
14. Mettre à jour post-mortem si incident.

---

## 27. Checklist qualité avant merge/déploiement

## 27.1 Qualité du code

- [ ] Style SQL respecté.
- [ ] Mots-clés en majuscules.
- [ ] Indentation cohérente.
- [ ] CTE nommés sémantiquement.
- [ ] Alias explicites.
- [ ] Colonnes listées explicitement.
- [ ] Absence de `SELECT *`.
- [ ] Commentaires d’intention présents.
- [ ] Pas de logique morte.
- [ ] Pas de duplication inutile.

---

## 27.2 Déterminisme

- [ ] Déduplication déterministe.
- [ ] `ROW_NUMBER()` avec ordre stable.
- [ ] Ex æquo traités.
- [ ] `LIMIT` accompagné d’un `ORDER BY` si métier.
- [ ] Casts explicites.
- [ ] Fuseaux horaires gérés.
- [ ] NULL semantics documentées.
- [ ] Ratio calculé correctement.

---

## 27.3 Joins

- [ ] Clés de jointure explicites.
- [ ] Types compatibles.
- [ ] Cardinalité documentée.
- [ ] Tests duplicats exécutés.
- [ ] Many-to-many justifié.
- [ ] LEFT JOIN non transformé accidentellement.
- [ ] Volume avant/après contrôlé.

---

## 27.4 Qualité des données

- [ ] Tests nulls.
- [ ] Tests unicité.
- [ ] Tests bornes.
- [ ] Tests accepted values.
- [ ] Tests fraîcheur.
- [ ] Tests réconciliation.
- [ ] Tests orphan keys.
- [ ] Quarantine prévue si nécessaire.
- [ ] Résultats de tests documentés.

---

## 27.5 Performance

- [ ] Filtres efficaces.
- [ ] Colonnes limitées.
- [ ] Query Profile analysé si critique.
- [ ] Bytes scanned acceptables.
- [ ] Warehouse adapté.
- [ ] Pas de cross join accidentel.
- [ ] Pas de `DISTINCT` cache-misère.
- [ ] Clustering/search optimization justifié si utilisé.
- [ ] Impact coût évalué.

---

## 27.6 Fiabilité

- [ ] Transformation idempotente.
- [ ] Reprocessing possible.
- [ ] Watermark documenté.
- [ ] Late arriving data gérées.
- [ ] MERGE source dédupliquée.
- [ ] Audit run prévu.
- [ ] Rollback documenté.
- [ ] Monitoring configuré.

---

## 27.7 Sécurité

- [ ] Données sensibles exclues ou protégées.
- [ ] Masking/row policies respectées.
- [ ] Pas de secrets hardcodés.
- [ ] Rôle d’exécution conforme.
- [ ] Pas de contournement de sécurité.
- [ ] Objets cibles conformes RBAC.
- [ ] Vues sécurisées si nécessaire.

---

## 27.8 Documentation

- [ ] Objectif documenté.
- [ ] Grain documenté.
- [ ] Sources documentées.
- [ ] Cible documentée.
- [ ] Règles métier documentées.
- [ ] Data contract mis à jour.
- [ ] Changelog mis à jour.
- [ ] Owner défini.

---

## 28. Gouvernance documentaire

## 28.1 Révision du standard

Ce standard doit être revu :

- trimestriellement ;
- après incident SQL majeur ;
- après régression performance importante ;
- après incident de qualité de données ;
- après incident de sécurité ;
- après évolution importante Snowflake ;
- après adoption d’un nouveau framework SQL/dbt ;
- après feedback récurrent de code review.

---

## 28.2 Registre des dérogations

```markdown
# Dérogation Snowflake SQL

## Script ou objet concerné

Nom:

## Règle concernée

Règle du standard:

## Motif

Pourquoi l’écart est nécessaire:

## Impact

- Performance:
- Coût:
- Sécurité:
- Qualité:
- Maintenabilité:

## Durée de validité

Date de fin:

## Plan de remédiation

Actions prévues:

## Approbateur

Nom / rôle:

## Date de revue

Date:
```

---

## 28.3 Incident SQL

```markdown
# Incident SQL

## Résumé

Date:
Domaine:
Pipeline:
Sévérité:

## Description

Que s’est-il passé ?

## Cause probable

- Join incorrect
- Déduplication non déterministe
- Schema drift
- Cast incorrect
- NULL mal interprété
- Requête trop coûteuse
- DML destructif
- Autre:

## Impact

- Données:
- KPI:
- Utilisateurs:
- Coût:
- Sécurité:

## Correction immédiate

Actions:

## Prévention

- Test ajouté:
- Règle standard mise à jour:
- Monitoring ajouté:
- Owner:
- Deadline:
```

---

## 29. Annexes

## 29.1 Template SQL de transformation

```sql
/*
  Script:
  Domain:
  Owner:
  Purpose:
  Grain:
  Sources:
  Target:
  Incremental:
  Watermark:
  Sensitive data:
*/

ALTER SESSION SET QUERY_TAG = 'app=sql_pipeline;domain=<domain>;env=<env>;pipeline=<pipeline>;step=<step>';

WITH source_data AS (
    SELECT
        source_id,
        source_updated_at,
        business_key,
        metric_amount
    FROM <database>.<schema>.<source_table>
    WHERE source_updated_at >= :watermark_start
      AND source_updated_at < :watermark_end
),

validated_data AS (
    SELECT
        source_id,
        source_updated_at,
        business_key,
        TRY_TO_DECIMAL(metric_amount, 18, 2) AS metric_amount,
        CASE
            WHEN business_key IS NULL THEN 'MISSING_BUSINESS_KEY'
            WHEN TRY_TO_DECIMAL(metric_amount, 18, 2) IS NULL THEN 'INVALID_AMOUNT'
            ELSE 'VALID'
        END AS quality_status
    FROM source_data
),

valid_rows AS (
    SELECT
        source_id,
        source_updated_at,
        business_key,
        metric_amount
    FROM validated_data
    WHERE quality_status = 'VALID'
),

deduped_rows AS (
    SELECT
        source_id,
        source_updated_at,
        business_key,
        metric_amount
    FROM valid_rows
    QUALIFY ROW_NUMBER() OVER (
        PARTITION BY business_key
        ORDER BY source_updated_at DESC, source_id DESC
    ) = 1
),

final AS (
    SELECT
        business_key,
        metric_amount,
        source_updated_at,
        CURRENT_TIMESTAMP() AS processed_at
    FROM deduped_rows
)

SELECT
    business_key,
    metric_amount,
    source_updated_at,
    processed_at
FROM final;
```

---

## 29.2 Template MERGE idempotent

```sql
MERGE INTO <target_table> AS tgt
USING (
    SELECT
        business_key,
        attribute_1,
        attribute_2,
        record_hash,
        source_updated_at
    FROM <staging_table>
    QUALIFY ROW_NUMBER() OVER (
        PARTITION BY business_key
        ORDER BY source_updated_at DESC
    ) = 1
) AS src
    ON tgt.business_key = src.business_key
WHEN MATCHED
     AND tgt.record_hash <> src.record_hash
THEN UPDATE SET
    attribute_1 = src.attribute_1,
    attribute_2 = src.attribute_2,
    record_hash = src.record_hash,
    source_updated_at = src.source_updated_at,
    processed_at = CURRENT_TIMESTAMP()
WHEN NOT MATCHED
THEN INSERT (
    business_key,
    attribute_1,
    attribute_2,
    record_hash,
    source_updated_at,
    processed_at
) VALUES (
    src.business_key,
    src.attribute_1,
    src.attribute_2,
    src.record_hash,
    src.source_updated_at,
    CURRENT_TIMESTAMP()
);
```

---

## 29.3 Template contrôle cardinalité join

```sql
WITH left_table AS (
    SELECT
        business_key,
        COUNT(*) AS left_count
    FROM <left_table>
    GROUP BY business_key
),

right_table AS (
    SELECT
        business_key,
        COUNT(*) AS right_count
    FROM <right_table>
    GROUP BY business_key
),

cardinality_check AS (
    SELECT
        left_table.business_key,
        left_table.left_count,
        right_table.right_count
    FROM left_table
    LEFT JOIN right_table
        ON right_table.business_key = left_table.business_key
    WHERE COALESCE(right_table.right_count, 0) > 1
)

SELECT
    *
FROM cardinality_check;
```

---

## 29.4 Template DQ suite minimale

```sql
-- Null check
SELECT 'customer_id_not_null' AS test_name, COUNT(*) AS invalid_count
FROM CURATED.CUSTOMER
WHERE customer_id IS NULL

UNION ALL

-- Unique check
SELECT 'customer_id_unique' AS test_name, COUNT(*) AS invalid_count
FROM (
    SELECT customer_id
    FROM CURATED.CUSTOMER
    GROUP BY customer_id
    HAVING COUNT(*) > 1
)

UNION ALL

-- Accepted values
SELECT 'status_accepted_values' AS test_name, COUNT(*) AS invalid_count
FROM CURATED.CUSTOMER
WHERE customer_status NOT IN ('ACTIVE', 'INACTIVE', 'PROSPECT');
```

---

## 29.5 Template Query History par query tag

```sql
SELECT
    query_id,
    query_tag,
    user_name,
    role_name,
    warehouse_name,
    start_time,
    total_elapsed_time / 1000 AS elapsed_seconds,
    bytes_scanned,
    execution_status,
    error_message
FROM SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY
WHERE query_tag ILIKE '%pipeline=sales_mart%'
  AND start_time >= DATEADD('day', -7, CURRENT_TIMESTAMP())
ORDER BY start_time DESC;
```

---

## 29.6 Template de correction transactionnelle

```sql
BEGIN;

CREATE OR REPLACE TEMP TABLE TMP_ROWS_TO_FIX AS
SELECT
    business_key,
    old_value,
    new_value
FROM SUPPORT.FIX_REQUEST_20260612;

UPDATE MART.TARGET_TABLE AS tgt
SET
    corrected_column = fix.new_value,
    corrected_at = CURRENT_TIMESTAMP()
FROM TMP_ROWS_TO_FIX AS fix
WHERE fix.business_key = tgt.business_key;

-- Validation before COMMIT
SELECT
    COUNT(*) AS corrected_rows
FROM MART.TARGET_TABLE AS tgt
JOIN TMP_ROWS_TO_FIX AS fix
    ON fix.business_key = tgt.business_key
WHERE tgt.corrected_column = fix.new_value;

COMMIT;
```

Rollback pattern à prévoir selon contexte :

- Time Travel ;
- clone préalable ;
- table backup ;
- script inverse ;
- reprocessing pipeline.

---

## 29.7 Template README pour dossier SQL

```markdown
# SQL Module — <Domain / Pipeline>

## Objectif

Décrire le but du module SQL.

## Périmètre

- Sources:
- Cibles:
- Environnement:

## Grain

1 ligne = ...

## Scripts

| Script | Description | Fréquence | Owner |
|---|---|---|---|
| 01_create_tables.sql | Création objets | À la demande | Data Engineering |
| 02_load_staging.sql | Chargement staging | Daily | Data Engineering |
| 03_build_mart.sql | Construction mart | Daily | Analytics Engineering |

## Tests

- Null checks
- Unique checks
- Volume checks
- Reconciliation checks

## Déploiement

Décrire pipeline CI/CD.

## Rollback

Décrire procédure.

## Monitoring

- Query tag:
- Dashboard:
- Alertes:
```

---

## 29.8 Confidentialité

Certaines données, exemples, noms de tables, requêtes, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, aux règles de gouvernance, aux politiques de sécurité, à l’architecture Snowflake, au framework de transformation utilisé et aux besoins métier de l’organisation.

---

## 29.9 Résumé exécutif du standard

Un SQL Snowflake corporate doit être :

- lisible ;
- explicite ;
- déterministe ;
- idempotent ;
- performant ;
- sécurisé ;
- testé ;
- documenté ;
- monitoré ;
- versionné ;
- relu ;
- gouverné.

La qualité d’un SQL Snowflake ne se mesure pas uniquement au fait qu’il “fonctionne”. Elle se mesure à sa capacité à produire durablement un résultat fiable, compréhensible, contrôlable, performant et sécurisé en production.
