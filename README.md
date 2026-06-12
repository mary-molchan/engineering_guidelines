# 📘 Engineering Guidelines — Data & Analytics Engineering Standards

![Documentation](https://img.shields.io/badge/Documentation-Engineering%20Guidelines-blue)
![Power BI](https://img.shields.io/badge/Power%20BI-Analytics%20%26%20Semantic%20Modeling-yellow)
![Snowflake](https://img.shields.io/badge/Snowflake-Cloud%20Data%20Platform-29B5E8)
![Dataiku](https://img.shields.io/badge/Dataiku-Data%20Pipelines-2AB1AC)
![SQL](https://img.shields.io/badge/SQL-Production%20Standards-336791)
![Data Governance](https://img.shields.io/badge/Data%20Governance-Quality%20%7C%20Security%20%7C%20Lineage-purple)
![Markdown](https://img.shields.io/badge/Format-Markdown-black)
![Status](https://img.shields.io/badge/Status-Portfolio%20%2F%20Knowledge%20Base-success)

---

## 🎯 Executive Summary

Ce repository regroupe un ensemble de guides techniques et méthodologiques consacrés à l’analytics engineering, à la préparation des données, à la gouvernance BI, à Snowflake, à Dataiku, à Power BI et aux standards SQL de production.

L’objectif n’est pas uniquement de documenter des bonnes pratiques isolées, mais de formaliser une approche structurée et industrialisable de la chaîne data : depuis la préparation des données en amont jusqu’à la modélisation sémantique, la visualisation, l’orchestration des pipelines, la qualité, la sécurité, la performance et la maintenabilité.

Ces guides sont conçus comme des standards corporate réutilisables dans des contextes professionnels où les enjeux sont :

- fiabilité des données ;
- cohérence des indicateurs ;
- performance des traitements ;
- maîtrise des coûts ;
- gouvernance des accès ;
- qualité documentaire ;
- industrialisation des pipelines ;
- réduction de la dette technique ;
- adoption métier des solutions BI ;
- passage du prototype vers la production.

---

## 🧭 Positionnement du repository

Ce repository se positionne comme une base de référence personnelle et professionnelle pour structurer des projets data selon une logique de **senior analytics engineering / data engineering**.

Il couvre plusieurs niveaux de la chaîne analytique :

```text
Source Systems
    ↓
Data Preparation & Data Quality
    ↓
Snowflake / Data Platform
    ↓
Dataiku / Pipeline Orchestration
    ↓
Power BI / Semantic Modeling
    ↓
Visual Analytics & Decision Support
```

Les documents présents dans ce repository formalisent des standards applicables à des projets data corporate, notamment pour :

- concevoir des datasets fiables et réutilisables ;
- préparer des données optimisées pour Power BI ;
- définir les responsabilités entre Snowflake, Dataiku et Power Query ;
- écrire du SQL Snowflake maintenable et performant ;
- organiser une plateforme Snowflake gouvernée ;
- structurer des pipelines Dataiku industrialisables ;
- concevoir des rapports Power BI lisibles, performants et orientés décision.

---

## 📚 Liste des guides

| Guide | Domaine | Objectif |
|---|---|---|
| [Préparation des données pour Power BI](./01_preparation_des_donnees_pour_power_bi.md) | Power BI / Data Preparation | Définir les standards de préparation des données avant reporting Power BI : data contracts, qualité, typage, granularité, sécurité, refresh, gouvernance et performance. |
| [Modélisation des données dans Power BI](./02_modelisation_des_donnees_dans_power_bi.md) | Power BI / Semantic Modeling | Formaliser les bonnes pratiques de modélisation Power BI : schéma en étoile, tables de faits, dimensions, relations, DAX, incremental refresh, sécurité et certification des modèles. |
| [Analyse visuelle et data storytelling dans Power BI](./03_analyse_visuelle_dans_power_bi.md) | Power BI / Data Visualization | Structurer les principes de conception visuelle, storytelling analytique, accessibilité, UX, performance perçue et adoption des dashboards. |
| [Standards de plateforme Snowflake](./04_standards_plateforme_snowflake.md) | Snowflake / Data Platform | Définir les règles d’architecture, sécurité, RBAC, warehouses, coûts, gouvernance, ingestion, observabilité, Time Travel, data sharing et exploitation Snowflake. |
| [Standards Snowflake SQL](./05_standards_snowflake_sql.md) | Snowflake / SQL Engineering | Définir les conventions SQL de production : style, CTE, joins, cardinalité, MERGE, idempotence, performance, semi-structured data, tests et CI/CD. |
| [Standards Dataiku Pipelines](./06_standards_dataiku_pipelines.md) | Dataiku / Pipeline Engineering | Formaliser les bonnes pratiques de conception, orchestration, quality gates, scenarios, bundles, permissions, monitoring et exploitation des pipelines Dataiku. |
| [Comparatif : Snowflake, Dataiku, Power Query](./07_comparatif_preparation_donnees_snowflake_dataiku_powerquery.md) | Data Architecture / Tooling Strategy | Définir un framework de décision pour répartir les transformations entre Snowflake, Dataiku, Power Query et Power BI selon volume, gouvernance, criticité et maintenabilité. |

---

## 🧱 Architecture conceptuelle des standards

Ces guides s’inscrivent dans une architecture analytique cible où chaque outil a un rôle clair.

```text
Snowflake
- transformations lourdes
- logique métier réutilisable
- données certifiées
- sécurité
- performance
- historisation
- coûts et observabilité

Dataiku
- orchestration
- quality gates
- scenarios
- monitoring
- promotion par bundles
- collaboration no-code / code
- industrialisation des flux

Power Query
- last-mile preparation
- typage final
- renommage final
- filtres de consommation
- incremental refresh
- transformations légères

Power BI
- semantic model
- DAX
- relations
- visual analytics
- storytelling
- decision support
```

---

## 🧩 Principes directeurs

Les guides reposent sur plusieurs principes structurants.

### 1. Pushdown-first

Les transformations lourdes doivent être réalisées au plus près du moteur de données, principalement dans Snowflake ou via pushdown depuis Dataiku.

### 2. Orchestration-second

Les pipelines doivent être orchestrés, monitorés et contrôlés via une couche dédiée comme Dataiku, avec scenarios, checks, alertes et runbooks.

### 3. Presentation-last

Power Query doit rester une couche de dernier kilomètre, limitée aux ajustements légers spécifiques au rapport.

### 4. One KPI, One Definition

Un indicateur critique doit avoir une définition officielle, documentée et stable, afin d’éviter les divergences entre rapports.

### 5. Quality by Design

La qualité des données doit être intégrée dans les pipelines via tests, checks, contrôles de volume, fraîcheur, duplicats, nulls et réconciliation.

### 6. Security by Design

Les accès, données sensibles, masquages, rôles, connexions et exports doivent être gouvernés dès la conception.

### 7. Production Readiness

Un artefact data n’est pas considéré comme prêt pour la production sans owner, documentation, tests, monitoring, stratégie de rollback et processus de support.

---

## 🏗️ Domaines couverts

Ce repository couvre les domaines suivants :

| Domaine | Couverture |
|---|---|
| **Data Preparation** | Typage, nettoyage, déduplication, contrats de données, granularité, qualité, refresh, schema drift. |
| **Power BI Engineering** | Préparation des données, semantic modeling, DAX, visual analytics, storytelling, accessibilité, performance. |
| **Snowflake Engineering** | Plateforme, RBAC, warehouses, coûts, SQL, MERGE, Time Travel, policies, observabilité, CI/CD. |
| **Dataiku Pipelines** | Flows, recipes, scenarios, triggers, reporters, metrics, checks, bundles, automation nodes. |
| **Data Governance** | Ownership, data contracts, lineage, documentation, sécurité, classification, qualité et certification. |
| **Analytics Engineering** | Modèles analytiques, tables certifiées, tests, documentation, KPI, source of truth. |
| **Production Operations** | Monitoring, runbooks, incident management, post-mortems, SLA, MTTD, MTTR. |

---

## 🧠 Compétences démontrées

Ce repository met en évidence des compétences clés en data engineering, analytics engineering et BI engineering.

### Data Engineering

- conception de pipelines fiables ;
- structuration raw / staging / curated / serving ;
- gestion de la qualité des données ;
- industrialisation des traitements ;
- optimisation des performances ;
- gestion des coûts compute ;
- observabilité et monitoring ;
- documentation technique.

### Analytics Engineering

- modélisation analytique ;
- préparation de datasets certifiés ;
- définition de métriques fiables ;
- tests de non-régression ;
- gouvernance des KPI ;
- transformation SQL maintenable ;
- alignement entre données et besoins métier.

### Business Intelligence Engineering

- préparation des données pour Power BI ;
- modélisation sémantique ;
- conception de dashboards orientés décision ;
- data storytelling ;
- accessibilité ;
- performance de refresh ;
- adoption utilisateur.

### Data Governance

- data contracts ;
- ownership ;
- classification ;
- règles de sécurité ;
- lineage ;
- documentation ;
- certification ;
- gestion du cycle de vie des artefacts data.

---

## 🔍 Comment utiliser ce repository

Ce repository peut être utilisé comme :

- base de standards pour des projets data ;
- support de documentation technique ;
- référentiel de bonnes pratiques ;
- checklist de production readiness ;
- support de revue technique ;
- base de formation interne ;
- démonstrateur de posture senior / lead data engineer ;
- ressource portfolio pour présenter une approche structurée de l’analytics engineering.

---

## ✅ Exemples d’usage

### Pour un projet Power BI

Utiliser les guides suivants :

1. [Préparation des données pour Power BI](./01_preparation_des_donnees_pour_power_bi.md)
2. [Modélisation des données dans Power BI](./02_modelisation_des_donnees_dans_power_bi.md)
3. [Analyse visuelle et data storytelling dans Power BI](./03_analyse_visuelle_dans_power_bi.md)
4. [Comparatif : Snowflake, Dataiku, Power Query](./07_comparatif_preparation_donnees_snowflake_dataiku_powerquery.md)

### Pour un projet Snowflake

Utiliser les guides suivants :

1. [Standards de plateforme Snowflake](./04_standards_plateforme_snowflake.md)
2. [Standards Snowflake SQL](./05_standards_snowflake_sql.md)
3. [Comparatif : Snowflake, Dataiku, Power Query](./07_comparatif_preparation_donnees_snowflake_dataiku_powerquery.md)

### Pour un projet Dataiku

Utiliser les guides suivants :

1. [Standards Dataiku Pipelines](./06_standards_dataiku_pipelines.md)
2. [Préparation des données pour Power BI](./01_preparation_des_donnees_pour_power_bi.md)
3. [Comparatif : Snowflake, Dataiku, Power Query](./07_comparatif_preparation_donnees_snowflake_dataiku_powerquery.md)

---

## 🧪 Production Readiness Mindset

Les guides de ce repository sont construits autour d’une logique de passage en production.

Un pipeline, un dataset ou un rapport ne doit pas être considéré comme prêt uniquement parce qu’il fonctionne techniquement. Il doit aussi être :

- documenté ;
- testé ;
- sécurisé ;
- monitoré ;
- maintenable ;
- compréhensible ;
- relançable ;
- gouverné ;
- aligné avec les besoins métier ;
- compatible avec les standards d’entreprise.

---

## 📐 Standards transverses appliqués

Les documents suivent une structure corporate cohérente :

- vision et principes directeurs ;
- rôles et responsabilités ;
- standards obligatoires ;
- bonnes pratiques ;
- lignes rouges ;
- checklists ;
- SOP ;
- gouvernance documentaire ;
- templates ;
- annexes ;
- logique de production readiness.

Cette structure permet de passer d’une simple documentation technique à un véritable cadre méthodologique réutilisable.

---

## 🚫 Anti-patterns ciblés

Ces guides visent notamment à éviter :

- logique métier critique dispersée entre plusieurs outils ;
- transformations lourdes dans Power Query ;
- SQL non déterministe ;
- absence de tests qualité ;
- modèles Power BI non gouvernés ;
- pipelines Dataiku non monitorés ;
- Snowflake utilisé sans maîtrise des coûts ;
- duplication des KPI ;
- absence d’ownership ;
- absence de documentation ;
- passage en production sans validation ;
- dashboards difficiles à interpréter ;
- dette technique invisible.

---

## 🗂️ Organisation recommandée du repository

```text
engineering-guidelines/
│
├── README.md
│
├── 01_preparation_des_donnees_pour_power_bi.md
├── 02_modelisation_des_donnees_dans_power_bi.md
├── 03_analyse_visuelle_dans_power_bi.md
├── 04_standards_plateforme_snowflake.md
├── 05_standards_snowflake_sql.md
├── 06_standards_dataiku_pipelines.md
├── 07_comparatif_preparation_donnees_snowflake_dataiku_powerquery.md
│
└── assets/
    └── images/
```

---

## 🧭 Roadmap possible

Les évolutions futures possibles de ce repository peuvent inclure :

- guide sur la gouvernance des semantic models Power BI ;
- guide sur les tests automatisés en analytics engineering ;
- guide sur CI/CD pour projets BI et data ;
- guide sur la documentation des KPI ;
- guide sur l’observabilité data ;
- guide sur les data contracts ;
- guide sur l’usage de GenAI pour accélérer la documentation et la revue technique ;
- templates de pull request ;
- templates de runbook ;
- templates de data contract ;
- matrices RACI réutilisables.

---

## 👩‍💻 À propos

Ce repository fait partie de mon portfolio professionnel autour de la data, de la BI et de l’analytics engineering.

Il reflète ma capacité à :

- structurer une démarche data de bout en bout ;
- formaliser des standards techniques ;
- documenter des pratiques réutilisables ;
- relier outils, architecture et besoins métier ;
- appliquer une logique de production readiness ;
- construire une documentation claire, exploitable et orientée entreprise.

---

## 📌 Résumé

Ce repository n’est pas seulement une collection de notes techniques.  
Il constitue une base structurée de standards data engineering et analytics engineering, conçue pour démontrer une approche professionnelle, gouvernée et industrialisable de la chaîne analytique moderne.

```text
Reliable data.
Governed pipelines.
Certified models.
Actionable dashboards.
```
