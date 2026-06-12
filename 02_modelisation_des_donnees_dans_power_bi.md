# Standard Corporate Power BI : Modélisation des Données

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Modélisation des données dans Power BI |
| **Date** | N / A |
| **Auteur / Rôle** | Maryna MOLCHAN / Data Analytics Engineer |
| **Validation / Approbation** | Responsable BI, Data Architect, Lead Analytics Engineer, Data Governance Officer |
| **Public cible** | BI Developers, Data Analysts, Analytics Engineers, Data Engineers, Data Stewards, Product Owners Data |
| **Commanditaire** | Direction Data & Performance / Direction Métier |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Tous les semantic models Power BI, datasets partagés, modèles tabulaires, modèles composites et modèles certifiés |
| **Objectif du document** | Définir les règles de modélisation permettant de garantir cohérence métier, performance, maintenabilité, sécurité, réutilisabilité et gouvernance des modèles Power BI. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les exigences s’appliquent par défaut à tout semantic model Power BI développé, maintenu, migré ou certifié dans l’organisation.

### Interprétation des termes

| Terme | Signification |
|---|---|
| **MUST** | Exigence obligatoire, non négociable hors dérogation formelle. |
| **SHOULD** | Exigence fortement recommandée ; toute exception doit être justifiée et documentée. |
| **MAY** | Option autorisée selon le contexte, à condition de ne pas contredire les règles MUST/SHOULD. |
| **MUST NOT** | Interdiction explicite. |
| **Exception documentée** | Écart temporaire validé par un approbateur nommé, avec justification, durée de validité et plan de remédiation. |

### Mode de rédaction

Les règles sont rédigées selon une logique de gouvernance, robustesse sémantique, performance, sécurité et industrialisation.

### Gestion des exceptions

Toute dérogation doit inclure :

- le contexte métier ;
- le motif de l’écart ;
- l’impact attendu sur la performance, la sécurité, la dette technique ou la maintenabilité ;
- la durée de validité ;
- le plan de remédiation ;
- l’approbateur nommé ;
- la date de révision.

### Précedence

En cas de conflit, l’ordre de priorité est le suivant :

1. obligations légales, réglementaires et contractuelles ;
2. politiques de sécurité et confidentialité de l’entreprise ;
3. standards Data Governance / Data Architecture ;
4. présent standard Power BI ;
5. pratiques locales d’équipe.

---

## 1.2 Définitions clés

| Terme | Définition |
|---|---|
| **Semantic model** | Modèle Power BI contenant tables, relations, mesures, hiérarchies, rôles de sécurité et métadonnées métier. Anciennement souvent appelé dataset. |
| **Fact table** | Table représentant des événements mesurables : ventes, transactions, écritures, tickets, commandes, stocks, mouvements. |
| **Dimension table** | Table descriptive permettant d’analyser les faits : client, produit, date, géographie, organisation, contrat, canal. |
| **Grain** | Niveau de détail d’une table de faits : une ligne par transaction, par jour-produit-magasin, par client-mois, etc. |
| **Conformed dimension** | Dimension partagée et cohérente entre plusieurs domaines ou modèles. |
| **Measure** | Calcul DAX explicite représentant une métrique métier. |
| **Calculated column** | Colonne calculée dans le modèle Power BI, matérialisée au refresh et stockée en mémoire. |
| **RLS** | Row-Level Security : sécurité au niveau des lignes. |
| **OLS** | Object-Level Security : sécurité au niveau des tables ou colonnes. |
| **Certified semantic model** | Modèle validé comme source fiable et réutilisable dans l’organisation. |
| **Calculation group** | Objet tabulaire permettant de factoriser des logiques de calcul répétitives, notamment time intelligence, scénarios ou formats dynamiques. |
| **TMDL** | Tabular Model Definition Language, représentation textuelle des métadonnées d’un modèle tabulaire. |

---

## 2. Vision, principes directeurs et résultats attendus

## 2.1 Vision

Construire des modèles analytiques standardisés, performants et compréhensibles, capables de servir à la fois :

- les usages corporate ;
- les tableaux de bord exécutifs ;
- l’analyse opérationnelle ;
- le self-service BI contrôlé ;
- la réutilisation inter-rapports ;
- l’industrialisation des KPI ;
- la gouvernance des données analytiques.

Un semantic model Power BI ne doit pas être considéré comme un simple support technique de rapport. Il constitue une couche sémantique d’entreprise qui traduit les données brutes en langage métier fiable, sécurisé et réutilisable.

---

## 2.2 Principes directeurs

1. **Star Schema First**  
   La modélisation en étoile est le standard par défaut pour les modèles analytiques Power BI.

2. **Business Semantics First**  
   Les objets du modèle doivent refléter les concepts métier, pas uniquement la structure technique des sources.

3. **One KPI, One Definition**  
   Une métrique stratégique doit avoir une définition officielle, documentée, validée et réutilisable.

4. **Measures over Implicit Logic**  
   Les KPI doivent être implémentés en mesures DAX explicites, jamais uniquement dans les visuels.

5. **Simple Model, Strong Semantics**  
   Le modèle doit rester lisible, navigable et compréhensible pour les utilisateurs avancés.

6. **Performance by Design**  
   La performance doit être intégrée dès la conception : cardinalité, relations, storage mode, DAX, agrégations, refresh.

7. **Security by Design**  
   Les exigences RLS, OLS, confidentialité et accès doivent être pensées dès le design du modèle.

8. **Reusable by Default**  
   Tout modèle à vocation corporate doit être conçu pour être réutilisé par plusieurs rapports et équipes.

9. **Governed Self-Service**  
   Le self-service doit s’appuyer sur des modèles certifiés, documentés et contrôlés.

10. **Lifecycle Managed**  
    Les modèles critiques doivent être versionnés, testés, déployés et monitorés comme des actifs logiciels.

---

## 2.3 Résultats attendus

L’application de ce standard doit permettre de :

- réduire les écarts de KPI entre rapports ;
- diminuer les temps de réponse des visuels ;
- limiter la charge capacitaire ;
- améliorer la qualité du self-service BI ;
- accélérer l’onboarding des nouveaux développeurs ;
- faciliter les audits et revues de modèles ;
- réduire la dette DAX ;
- fiabiliser la sécurité d’accès aux données ;
- industrialiser les modèles réutilisables ;
- améliorer la confiance des métiers dans les rapports Power BI.

---

## 2.4 Indicateurs de succès du standard

| Indicateur | Cible recommandée |
|---|---|
| Taux de modèles corporate utilisant un schéma en étoile | ≥ 90 % |
| Taux de KPI critiques documentés | 100 % |
| Taux de modèles critiques avec RLS/OLS testés | 100 % |
| Temps de réponse des pages exécutives | Généralement < 5 secondes hors contraintes spécifiques |
| Nombre de modèles quasi-dupliqués | Réduction continue |
| Taux de réutilisation des semantic models certifiés | Augmentation trimestrielle |
| Taux d’incidents liés à divergence de KPI | Réduction continue |
| Taux de modèles avec ownership défini | 100 % |

---

## 3. Rôles et responsabilités

## 3.1 RACI simplifié

| Activité | BI Developer | Analytics Engineer | Data Architect | Data Steward | Métier / PO | Admin Power BI | Security Officer |
|---|---:|---:|---:|---:|---:|---:|---:|
| Cadrage besoins analytiques | C | R | C | C | A/R | I | I |
| Définition du grain | R | R | A | C | C | I | I |
| Design facts/dimensions | R | R | A | C | C | I | I |
| Modélisation sémantique | R | R | C | C | A | I | I |
| Définition KPI | R | R | C | C | A/R | I | I |
| Validation fonctionnelle | C | C | I | C | A/R | I | I |
| Sécurité RLS/OLS | C | C | C | C | I | A/R | A/R |
| Performance tuning | R | R | C | I | C | C | I |
| Documentation modèle | R | R | C | A | C | I | I |
| Certification semantic model | C | C | C | A | C | A/R | C |
| Déploiement DEV/TEST/PROD | R | R | C | I | I | A/R | I |
| Monitoring usage/performance | R | R | C | I | C | A/R | I |
| Gestion des dérogations | C | C | A | A | C | C | C |

---

## 3.2 Ownership obligatoire

Chaque semantic model publié doit avoir :

- un **Business Owner** ;
- un **Technical Owner** ;
- un **Data Steward** si le modèle contient des données critiques ;
- un **Support Contact** ;
- une **classification de criticité** ;
- un **niveau de confidentialité** ;
- une **date de dernière revue** ;
- une **fréquence de refresh validée** ;
- une **documentation accessible**.

Aucun modèle corporate ne doit être publié sans ownership explicite.

---

## 4. Architecture de référence

## 4.1 Positionnement du semantic model

Le semantic model Power BI doit être positionné comme une couche analytique au-dessus des données préparées.

Architecture cible recommandée :

```text
Sources opérationnelles
        ↓
Ingestion / ELT / ETL
        ↓
Data Lake / Data Warehouse / Lakehouse
        ↓
Couches préparées et gouvernées
        ↓
Semantic Model Power BI
        ↓
Rapports Power BI / Excel / Analyze in Excel / Self-service BI
```

Le semantic model ne doit pas compenser durablement l’absence de modélisation, de nettoyage ou de gouvernance en amont.

---

## 4.2 Responsabilité du modèle Power BI

Le modèle Power BI est responsable de :

- la couche sémantique ;
- la définition des KPI ;
- les relations analytiques ;
- les mesures DAX ;
- la sécurité RLS/OLS ;
- les hiérarchies métier ;
- les formats et descriptions utilisateur ;
- l’optimisation analytique ;
- la consommation par rapports et self-service.

Le modèle Power BI ne doit pas devenir le lieu principal de :

- nettoyage lourd des données ;
- correction structurelle des sources ;
- déduplication complexe ;
- règles métier opérationnelles critiques ;
- transformation historique massive ;
- orchestration de pipelines ;
- substitution à un Data Warehouse ou Lakehouse.

---

## 4.3 Modèle cible par défaut

Le modèle cible par défaut est un schéma en étoile :

```text
                 DimDate
                    |
DimCustomer — FactSales — DimProduct
                    |
               DimGeography
                    |
                DimChannel
```

Règle générale :

- les tables de faits contiennent les métriques et clés étrangères ;
- les dimensions contiennent les attributs descriptifs ;
- les relations vont des dimensions vers les faits ;
- les mesures DAX calculent les indicateurs ;
- les colonnes techniques inutiles sont masquées ;
- les dimensions conformes sont réutilisées entre modèles.

---

## 5. Standard de modélisation

## 5.1 Schéma en étoile

### Règles obligatoires

- Le schéma en étoile est obligatoire par défaut.
- Les faits et dimensions doivent être clairement séparés.
- Les dimensions doivent filtrer les faits.
- Les faits ne doivent pas filtrer les dimensions, sauf cas exceptionnel documenté.
- Les tables techniques doivent être masquées à l’utilisateur final.
- Les dimensions conformes doivent être utilisées lorsqu’elles existent.
- Les modèles en table unique sont interdits pour les usages analytiques multi-axes.

### Exceptions possibles

Une exception au schéma en étoile peut être acceptée uniquement pour :

- un modèle très simple et temporaire ;
- un prototype non certifié ;
- une contrainte source documentée ;
- un use case DirectQuery spécifique ;
- une stratégie composite model validée ;
- une agrégation ou table de performance.

Toute exception doit être documentée.

---

## 5.2 Grain des tables de faits

Le grain d’une table de faits doit être défini avant toute modélisation.

### Règles obligatoires

Chaque table de faits doit avoir une phrase de grain documentée.

Exemples :

```text
FactSales:
Une ligne par transaction de vente, par produit, par client, par date de commande.

FactInventorySnapshot:
Une ligne par produit, par entrepôt, par date de snapshot.

FactTicket:
Une ligne par ticket support créé dans le système source.

FactBudget:
Une ligne par compte analytique, par mois, par département, par scénario budgétaire.
```

### Pourquoi le grain est critique

Un grain mal défini entraîne :

- des doubles comptages ;
- des KPI incohérents ;
- des relations ambiguës ;
- des mesures DAX complexes ;
- des problèmes de performance ;
- une perte de confiance métier.

### Règle de contrôle

Avant publication, le développeur doit pouvoir répondre aux questions suivantes :

- Quelle entité représente une ligne ?
- La table est-elle transactionnelle, snapshot ou cumulative ?
- Les montants sont-ils additifs, semi-additifs ou non additifs ?
- Les dimensions liées correspondent-elles au même niveau de détail ?
- Les doublons sont-ils attendus ou anormaux ?

---

## 5.3 Typologie des tables de faits

| Type de fait | Description | Exemples | Point d’attention |
|---|---|---|---|
| **Transaction fact** | Événement ponctuel | Vente, commande, paiement, ticket | Généralement additif |
| **Periodic snapshot** | État à intervalle fixe | Stock journalier, solde mensuel | Attention aux mesures semi-additives |
| **Accumulating snapshot** | Processus avec étapes | Pipeline commercial, cycle commande-livraison | Gestion des dates multiples |
| **Factless fact** | Événement sans mesure numérique | Présence, inscription, interaction | Utile pour compter des occurrences |
| **Budget / Forecast fact** | Données prévisionnelles | Budget, forecast, objectif | Doit être séparé du réalisé |

### Règle

Les faits de natures différentes doivent être séparés, sauf justification documentée.

---

## 5.4 Dimensions

### Règles obligatoires

- Les dimensions doivent contenir les attributs descriptifs.
- Les dimensions doivent avoir une clé unique.
- Les attributs hiérarchiques doivent être placés dans les dimensions.
- Les colonnes techniques inutiles doivent être masquées.
- Les libellés métier doivent être compréhensibles.
- Les dimensions partagées doivent être conformes lorsque possible.

### Dimensions standards recommandées

| Dimension | Usage |
|---|---|
| **DimDate** | Analyse temporelle |
| **DimCustomer** | Analyse client |
| **DimProduct** | Analyse produit |
| **DimGeography** | Analyse géographique |
| **DimOrganization** | Entités, business units, départements |
| **DimEmployee** | Collaborateurs, managers, équipes |
| **DimSupplier** | Fournisseurs |
| **DimScenario** | Réel, budget, forecast |
| **DimCurrency** | Devise et conversion |
| **DimChannel** | Canal de vente ou d’interaction |

---

## 5.5 Slowly Changing Dimensions

Les dimensions historisées doivent être traitées explicitement.

### SCD Type 1

Utiliser SCD Type 1 lorsque l’historique des changements n’est pas requis.

Exemple :

```text
Correction d’un libellé produit mal orthographié.
```

### SCD Type 2

Utiliser SCD Type 2 lorsque l’analyse historique doit refléter la valeur valable à la date de l’événement.

Exemple :

```text
Un client change de segment.
Les ventes historiques doivent rester rattachées à l’ancien segment.
Les nouvelles ventes doivent être rattachées au nouveau segment.
```

### Règles

- Les dimensions SCD2 doivent avoir une clé substitut.
- Les colonnes `ValidFrom`, `ValidTo`, `IsCurrent` doivent être disponibles si pertinent.
- Le mapping entre faits et dimension historisée doit être fait en amont si possible.
- Les règles d’historisation doivent être documentées.
- Les dimensions SCD2 volumineuses doivent être surveillées pour la cardinalité.

---

## 5.6 Clés et cardinalité

### Règles obligatoires

- Les relations doivent utiliser des clés stables.
- Les clés numériques substituts sont préférées.
- Les clés texte longues dans les relations doivent être évitées.
- Les colonnes de haute cardinalité doivent être limitées dans le modèle.
- Les identifiants transactionnels doivent être conservés uniquement si nécessaires au drillthrough ou à l’audit.
- Les colonnes inutilisées doivent être supprimées avant chargement.

### Bonnes pratiques

| Situation | Recommandation |
|---|---|
| ID technique source long | Remplacer par clé substitut numérique si possible |
| Numéro de facture | Garder comme degenerate dimension uniquement si nécessaire |
| Adresse email | Éviter dans le modèle sauf besoin validé et sécurisé |
| Timestamp précis | Remplacer par date ou tranches horaires si le détail n’est pas nécessaire |
| Texte libre | Éviter dans le modèle analytique sauf cas d’usage explicite |

---

## 5.7 Relations

### Règles obligatoires

- Les relations `1-*` de dimension vers fait sont le standard.
- Le sens de filtrage doit être **single direction** par défaut.
- Les relations bidirectionnelles doivent être exceptionnelles.
- Les relations many-to-many doivent être évitées ou modélisées explicitement.
- Les relations inactives doivent être documentées.
- Les relations inactives doivent être activées explicitement avec `USERELATIONSHIP`.
- Les relations ambiguës sont interdites dans les modèles certifiés.

### Exemple de relation inactive maîtrisée

```dax
Sales Amount by Delivery Date =
CALCULATE(
    [Sales Amount],
    USERELATIONSHIP(FactSales[DeliveryDateKey], DimDate[DateKey])
)
```

### Relations bidirectionnelles

Les relations bidirectionnelles peuvent être utilisées uniquement si :

- le besoin métier est clairement justifié ;
- l’impact performance a été testé ;
- l’ambiguïté du modèle est maîtrisée ;
- la solution alternative a été évaluée ;
- la décision est documentée.

---

## 5.8 Many-to-many et bridge tables

Les relations many-to-many directes doivent être évitées lorsque possible.

### Approche recommandée

Utiliser une bridge table explicite.

```text
DimCustomer
     |
BridgeCustomerSegment
     |
DimSegment
```

### Règles

- La bridge table doit avoir un grain clair.
- Les pondérations doivent être définies si une allocation est nécessaire.
- Les mesures impactées doivent être testées contre des cas de référence.
- Le comportement des filtres doit être documenté.
- Les relations bidirectionnelles automatiques doivent être évitées sauf justification.

---

## 5.9 Dimensions dégénérées

Une dimension dégénérée correspond à un attribut de fait conservé dans la table de faits, par exemple :

- numéro de facture ;
- numéro de commande ;
- numéro de ticket ;
- référence transactionnelle.

### Règles

Les dimensions dégénérées sont autorisées si :

- elles servent au drillthrough ;
- elles servent à l’audit ;
- elles ne sont pas utilisées comme axe analytique majeur ;
- leur cardinalité est maîtrisée ;
- elles ne dégradent pas significativement la taille du modèle.

---

## 5.10 Gestion du temps

### Table calendrier

Une table calendrier enterprise est obligatoire pour tout modèle contenant une analyse temporelle.

### Colonnes minimales

La table calendrier doit contenir au minimum :

- Date ;
- DateKey ;
- Année ;
- Trimestre ;
- Mois ;
- Numéro du mois ;
- Nom du mois ;
- Semaine ;
- Jour ;
- Année fiscale ;
- Période fiscale ;
- Indicateur jour ouvré ;
- Indicateur fin de mois ;
- Indicateur période courante si pertinent.

### Règles obligatoires

- La table calendrier doit être marquée comme Date Table.
- Les time intelligence automatiques non contrôlées doivent être désactivées.
- Les colonnes temporelles doivent être triées correctement.
- Les calendriers fiscaux doivent être documentés.
- Les modèles multi-calendriers doivent justifier clairement chaque calendrier.
- Les dates multiples doivent être gérées par relations inactives ou dimensions de rôle.

### Dimensions de rôle

Exemple :

```text
FactSales[OrderDateKey]    → DimDate[DateKey]
FactSales[DeliveryDateKey] → DimDeliveryDate[DateKey]
FactSales[InvoiceDateKey]  → DimInvoiceDate[DateKey]
```

Approches possibles :

- une seule `DimDate` avec relations inactives ;
- plusieurs dimensions de rôle ;
- choix à documenter selon complexité, lisibilité et performance.

---

## 5.11 Mesures DAX

### Règles obligatoires

- Toute métrique métier doit être une mesure explicite.
- Les mesures implicites sont interdites pour les KPI stratégiques.
- Les calculs ne doivent pas être dupliqués dans plusieurs visuels.
- Les mesures doivent être nommées selon une convention stable.
- Les mesures complexes doivent être commentées.
- Les mesures doivent être organisées en dossiers d’affichage.
- Les mesures doivent réutiliser des mesures atomiques lorsque possible.

### Structure recommandée des mesures

```text
00 - Base Measures
01 - Financial KPIs
02 - Sales KPIs
03 - Operational KPIs
04 - Time Intelligence
05 - Ratios
06 - Forecast / Budget
07 - Technical / QA
```

### Exemple de mesure atomique

```dax
Sales Amount =
SUM(FactSales[SalesAmount])
```

### Exemple de mesure dérivée

```dax
Sales Amount YTD =
TOTALYTD(
    [Sales Amount],
    DimDate[Date]
)
```

### Exemple de ratio robuste

```dax
Gross Margin % =
DIVIDE(
    [Gross Margin],
    [Sales Amount]
)
```

### Règle de robustesse

Utiliser `DIVIDE()` plutôt que l’opérateur `/` pour éviter les erreurs de division par zéro.

---

## 5.12 Branching de mesures

Le branching de mesures est obligatoire pour les KPI complexes.

### Mauvaise pratique

```dax
Margin % =
DIVIDE(
    SUM(FactSales[SalesAmount]) - SUM(FactSales[CostAmount]),
    SUM(FactSales[SalesAmount])
)
```

### Bonne pratique

```dax
Sales Amount =
SUM(FactSales[SalesAmount])

Cost Amount =
SUM(FactSales[CostAmount])

Gross Margin =
[Sales Amount] - [Cost Amount]

Gross Margin % =
DIVIDE([Gross Margin], [Sales Amount])
```

### Avantages

- meilleure lisibilité ;
- réduction de la duplication ;
- maintenance simplifiée ;
- tests unitaires plus faciles ;
- réutilisation dans d’autres KPI.

---

## 5.13 Variables DAX

Les variables `VAR` doivent être utilisées pour :

- améliorer la lisibilité ;
- éviter la répétition de sous-calculs ;
- expliciter les étapes métier ;
- faciliter le debugging.

Exemple :

```dax
Customer Retention Rate =
VAR CustomersPreviousPeriod =
    CALCULATE(
        [Distinct Customers],
        DATEADD(DimDate[Date], -1, YEAR)
    )
VAR CustomersCurrentPeriod =
    [Distinct Customers]
RETURN
    DIVIDE(CustomersCurrentPeriod, CustomersPreviousPeriod)
```

---

## 5.14 Colonnes calculées

### Règles

- Les colonnes calculées doivent être limitées aux besoins stricts.
- Les transformations doivent être faites en amont lorsque possible.
- Les colonnes calculées sur tables volumineuses doivent être évitées.
- L’impact mémoire doit être évalué avant ajout.
- Les colonnes calculées ne doivent pas remplacer une mesure lorsque le calcul dépend du contexte de filtre.

### Quand utiliser une colonne calculée

Une colonne calculée est acceptable pour :

- une classification stable ;
- un attribut de tri ;
- une concaténation métier faible volumétrie ;
- une règle non dépendante du contexte de filtre ;
- une optimisation documentée.

### Quand éviter une colonne calculée

Éviter si :

- la table est volumineuse ;
- le calcul peut être fait dans l’ETL ;
- le calcul dépend des slicers ;
- le résultat peut être une mesure ;
- la cardinalité créée est élevée.

---

## 5.15 Tables calculées

Les tables calculées doivent rester exceptionnelles.

### Cas d’usage acceptables

- table de mesures ;
- table de scénarios ;
- table de paramètres ;
- table de segmentation contrôlée ;
- table technique pour calculation groups ou reporting avancé.

### Risques

- augmentation de la taille du modèle ;
- refresh plus lourd ;
- logique métier dispersée ;
- difficulté de gouvernance ;
- dépendances cachées.

---

## 5.16 Hiérarchies et ergonomie du semantic model

### Règles

- Les hiérarchies naturelles doivent être créées dans les dimensions.
- Les colonnes techniques doivent être masquées.
- Les noms visibles doivent être métier, clairs et stables.
- Les mesures doivent être regroupées par dossiers.
- Les descriptions doivent être renseignées pour les KPI critiques.
- Les colonnes de tri doivent être configurées.
- Les champs inutiles au reporting doivent être masqués ou exclus.

### Exemples de hiérarchies

```text
Temps:
Année > Trimestre > Mois > Date

Géographie:
Pays > Région > Ville > Site

Produit:
Catégorie > Sous-catégorie > Produit

Organisation:
Groupe > Business Unit > Département > Équipe
```

---

## 5.17 Naming conventions

## Tables

| Type | Convention | Exemple |
|---|---|---|
| Dimension | `Dim<Entity>` | `DimCustomer` |
| Fact | `Fact<Process>` | `FactSales` |
| Bridge | `Bridge<EntityAEntityB>` | `BridgeCustomerSegment` |
| Parameter | `Param<Usage>` | `ParamScenario` |
| Measures table | `_Measures` ou `Measures` | `_Measures` |

## Colonnes

| Type | Convention | Exemple |
|---|---|---|
| Clé substitut | `<Entity>Key` | `CustomerKey` |
| Clé source | `<Entity>SourceId` | `CustomerSourceId` |
| Libellé | `<Entity>Name` | `CustomerName` |
| Code | `<Entity>Code` | `ProductCode` |
| Date | `<Event>Date` | `OrderDate` |
| Montant | `<Metric>Amount` | `SalesAmount` |

## Mesures

| Type | Convention | Exemple |
|---|---|---|
| Montant | `<Business Concept> Amount` | `Sales Amount` |
| Nombre | `<Business Concept> Count` | `Order Count` |
| Ratio | `<Business Concept> %` | `Gross Margin %` |
| Écart | `<Business Concept> Variance` | `Budget Variance` |
| Time intelligence | `<Measure> YTD` | `Sales Amount YTD` |

### Règles

- Éviter les abréviations obscures.
- Éviter les noms purement techniques dans les objets visibles.
- Ne pas mélanger plusieurs langues dans les noms métier.
- Stabiliser les noms avant certification.
- Documenter tout renommage impactant les rapports existants.

---

## 6. Modes de stockage et architecture de performance

## 6.1 Choix du storage mode

Le choix du mode de stockage doit être explicite et documenté.

| Mode | Usage recommandé | Risques |
|---|---|---|
| **Import** | Performance, modèles analytiques standards, données historisées | Taille mémoire, refresh |
| **DirectQuery** | Données quasi temps réel, contraintes de volumétrie ou sécurité source | Latence, dépendance source, limitations DAX |
| **Composite** | Combinaison Import + DirectQuery ou plusieurs sources | Complexité, gouvernance, performance |
| **Direct Lake** | Scénarios Fabric/Lakehouse avec forte volumétrie | Dépendance à l’architecture Fabric et aux contraintes de sécurité |
| **Live Connection** | Consommation d’un modèle centralisé existant | Moins de flexibilité locale |

### Règle

Le mode Import est le choix par défaut pour les modèles analytiques corporate, sauf besoin documenté de DirectQuery, Composite ou Direct Lake.

---

## 6.2 DirectQuery

DirectQuery doit être utilisé avec prudence.

### Cas d’usage acceptables

- besoin de données très fraîches ;
- volumétrie incompatible avec Import ;
- sécurité appliquée à la source ;
- contraintes réglementaires empêchant l’import ;
- modèle léger sur source optimisée.

### Exigences

- La source doit être optimisée pour les requêtes analytiques.
- Les agrégations doivent être évaluées.
- Les requêtes fréquentes doivent être testées.
- Les relations complexes doivent être limitées.
- Les transformations Power Query doivent être minimales.
- Les visuels doivent être optimisés.
- Les limites fonctionnelles doivent être communiquées au métier.

### Interdits

- Utiliser DirectQuery pour compenser un mauvais modèle source.
- Construire un modèle DirectQuery sur une source transactionnelle non optimisée.
- Multiplier les visuels lourds sur une même page.
- Ignorer les temps de réponse source.
- Publier sans test de charge minimal.

---

## 6.3 Composite models

Les composite models doivent être réservés à des besoins clairement identifiés.

### Cas d’usage

- combiner données agrégées Import et détail DirectQuery ;
- enrichir un semantic model existant ;
- connecter plusieurs modèles ou sources ;
- permettre un équilibre entre performance et fraîcheur.

### Règles

- Les tables Import et DirectQuery doivent être clairement identifiées.
- Les relations cross-source doivent être minimisées.
- Les impacts de performance doivent être testés.
- Les limitations doivent être documentées.
- Les modèles composites critiques doivent faire l’objet d’une revue d’architecture.

---

## 6.4 Agrégations

Les agrégations doivent être envisagées pour les tables de faits volumineuses.

### Cas d’usage

- forte volumétrie ;
- requêtes fréquentes au niveau mois, produit, région ;
- DirectQuery lent sur détail ;
- dashboards exécutifs nécessitant des réponses rapides.

### Exemple

```text
FactSalesDetail:
Une ligne par transaction.

AggSalesMonthly:
Une ligne par mois, produit, région.
```

### Règles

- Le grain de l’agrégation doit être documenté.
- Les mesures compatibles doivent être identifiées.
- Le comportement détail vs agrégé doit être testé.
- Les utilisateurs doivent comprendre les limites de drilldown.
- Les agrégations doivent être réévaluées après changement majeur d’usage.

---

## 6.5 Incremental Refresh et partitionnement

### Règles obligatoires

Pour les tables de faits volumineuses :

- utiliser `RangeStart` et `RangeEnd` ;
- vérifier le query folding ;
- définir une politique de refresh adaptée ;
- documenter la fenêtre historique ;
- documenter la fenêtre incrémentale ;
- tester les données tardives ;
- vérifier le comportement après publication ;
- surveiller la durée de refresh.

### Exemple de politique

```text
Table: FactSales
Historique conservé: 5 ans
Refresh incrémental: 30 derniers jours
Détection des changements: LastModifiedDate
Fréquence: quotidienne
Responsable: BI Platform Team
```

### Points d’attention

- Les données arrivant en retard peuvent nécessiter une fenêtre de refresh plus large.
- Le query folding doit être validé.
- Les colonnes de filtre doivent être de type Date/Time compatible.
- Les changements de politique doivent être testés en environnement non productif.
- Les modèles critiques doivent avoir un plan de rollback.

---

## 7. Sécurité du modèle

## 7.1 Principes

La sécurité doit être conçue dès la modélisation.

### Règles

- Les données sensibles doivent être identifiées.
- Les rôles RLS doivent être définis selon les règles métier.
- L’OLS doit être utilisé pour masquer tables ou colonnes sensibles.
- Les rôles doivent être testés avec des comptes représentatifs.
- Les accès doivent privilégier les groupes de sécurité plutôt que les utilisateurs individuels.
- Les règles de sécurité ne doivent pas être hardcodées dans de nombreuses mesures.
- Les règles doivent être documentées.

---

## 7.2 Row-Level Security

### Approche recommandée

Utiliser une table de mapping des droits.

```text
UserSecurity
- UserPrincipalName
- BusinessUnitKey
- RegionKey
- RoleType
```

Exemple conceptuel :

```dax
UserSecurity[UserPrincipalName] = USERPRINCIPALNAME()
```

### Règles

- Préférer une RLS dynamique maintenable.
- Éviter la duplication de rôles statiques.
- Documenter le périmètre de chaque rôle.
- Tester les cas multi-périmètres.
- Tester les utilisateurs sans affectation.
- Tester les managers avec accès hiérarchique.
- Tester l’impact RLS sur les performances.

---

## 7.3 Object-Level Security

OLS doit être utilisé lorsque certaines colonnes ou tables ne doivent pas être visibles.

### Exemples

- données RH sensibles ;
- rémunération ;
- données personnelles ;
- données financières confidentielles ;
- identifiants clients ;
- colonnes réglementées.

### Règles

- Masquer une colonne dans le modèle ne suffit pas si la donnée est sensible.
- Les colonnes sensibles doivent être exclues ou protégées.
- L’OLS doit être validé avec le Security Officer ou Data Governance.
- Les impacts sur les rapports existants doivent être testés.

---

## 7.4 Confidentialité et données personnelles

### Règles

- Ne charger que les données nécessaires.
- Éviter les données personnelles directes si elles ne sont pas indispensables.
- Pseudonymiser ou agréger lorsque possible.
- Appliquer le principe du moindre privilège.
- Documenter le niveau de confidentialité.
- Éviter les exports non contrôlés.
- Vérifier les risques de ré-identification par croisement de dimensions.

### Données à risque

- nom et prénom ;
- email ;
- téléphone ;
- adresse ;
- identifiants personnels ;
- rémunération ;
- données RH ;
- données santé ;
- données financières individuelles ;
- commentaires libres ;
- données contractuelles sensibles.

---

## 8. Performance by Design

## 8.1 Optimisation du modèle

### Règles prioritaires

- Supprimer les colonnes inutilisées.
- Réduire la cardinalité.
- Préférer les entiers aux textes dans les relations.
- Réduire la précision numérique si acceptable.
- Éviter les timestamps inutiles.
- Séparer dates et heures si nécessaire.
- Utiliser des dimensions pour les attributs répétitifs.
- Pré-agréger en amont lorsque pertinent.
- Éviter les colonnes calculées volumineuses.
- Masquer les colonnes techniques.

---

## 8.2 Optimisation DAX

### Bonnes pratiques

- Utiliser des mesures atomiques.
- Réutiliser les mesures existantes.
- Utiliser `VAR` pour clarifier les calculs.
- Éviter les itérateurs inutiles sur grandes tables.
- Éviter `FILTER()` lorsque des filtres simples suffisent.
- Utiliser `DIVIDE()` pour les ratios.
- Limiter les calculs ligne à ligne.
- Tester les mesures critiques.
- Commenter les mesures complexes.

### Pattern recommandé

```dax
Revenue Growth % =
VAR CurrentRevenue =
    [Revenue]
VAR PreviousRevenue =
    CALCULATE(
        [Revenue],
        DATEADD(DimDate[Date], -1, YEAR)
    )
RETURN
    DIVIDE(
        CurrentRevenue - PreviousRevenue,
        PreviousRevenue
    )
```

---

## 8.3 Anti-patterns DAX

### Anti-pattern 1 : logique dupliquée

```dax
KPI 1 = SUM(FactSales[Amount]) - SUM(FactSales[Cost])
KPI 2 = (SUM(FactSales[Amount]) - SUM(FactSales[Cost])) / SUM(FactSales[Amount])
```

Correction :

```dax
Sales Amount = SUM(FactSales[Amount])
Cost Amount = SUM(FactSales[Cost])
Margin = [Sales Amount] - [Cost Amount]
Margin % = DIVIDE([Margin], [Sales Amount])
```

### Anti-pattern 2 : division non sécurisée

```dax
Margin % = [Margin] / [Sales Amount]
```

Correction :

```dax
Margin % = DIVIDE([Margin], [Sales Amount])
```

### Anti-pattern 3 : table de faits utilisée comme dimension

Mauvaise pratique :

```text
FactSales[CustomerName] utilisé directement dans les visuels.
```

Correction :

```text
Créer DimCustomer et relier FactSales à DimCustomer.
```

---

## 8.4 Performance des rapports consommant le modèle

Même si ce standard porte sur la modélisation, le modèle doit tenir compte de la consommation.

### Règles

- Les pages critiques doivent être testées avec données réelles.
- Les visuels lourds doivent être limités.
- Les tables détaillées doivent être filtrées par défaut.
- Les slicers sur colonnes haute cardinalité doivent être évités.
- Les mesures critiques doivent être profilées.
- Les rapports exécutifs doivent privilégier les agrégats.
- Le drillthrough doit être préféré aux pages trop détaillées.

---

## 9. Calculation groups et logique réutilisable

## 9.1 Cas d’usage

Les calculation groups sont recommandés lorsque la gouvernance DAX est mature.

### Cas d’usage typiques

- YTD, QTD, MTD ;
- année précédente ;
- variation absolue ;
- variation en pourcentage ;
- scénarios réel / budget / forecast ;
- formats dynamiques ;
- conversions d’unités ;
- vues mensuelles / cumulées.

---

## 9.2 Règles

- Les calculation groups doivent être validés par un Lead BI ou Data Architect.
- Leur impact sur les mesures existantes doit être testé.
- Les noms doivent être compréhensibles.
- Les formats dynamiques doivent être documentés.
- Les modèles critiques doivent inclure des tests de non-régression.
- Les calculation groups ne doivent pas masquer une logique métier instable.

---

## 9.3 Exemple de structure

```text
Calculation Group: Time Intelligence

Items:
- Current Period
- Previous Year
- YTD
- YTD Previous Year
- YoY
- YoY %
```

---

## 10. Documentation du semantic model

## 10.1 Documentation obligatoire

Chaque modèle corporate doit documenter :

- objectif métier ;
- périmètre fonctionnel ;
- sources utilisées ;
- fréquence de refresh ;
- owner métier ;
- owner technique ;
- niveau de confidentialité ;
- tables de faits ;
- dimensions ;
- grain de chaque table de faits ;
- définitions KPI ;
- règles de sécurité ;
- limitations connues ;
- dépendances ;
- historique des changements ;
- règles de certification.

---

## 10.2 Documentation des mesures

Chaque KPI critique doit avoir :

| Élément | Description |
|---|---|
| Nom de la mesure | Nom visible dans le modèle |
| Définition métier | Explication compréhensible par le métier |
| Formule DAX | Mesure exacte |
| Owner métier | Responsable de la définition |
| Owner technique | Responsable de l’implémentation |
| Source | Tables et champs utilisés |
| Périmètre | Population incluse/exclue |
| Filtres par défaut | Filtres structurels éventuels |
| Fréquence de mise à jour | Daily, weekly, monthly |
| Date de validation | Dernière validation métier |
| Sensibilité | Public, interne, confidentiel, restreint |

---

## 10.3 Exemple de fiche KPI

```markdown
## KPI: Gross Margin %

**Définition métier**: Part du chiffre d’affaires conservée après déduction des coûts directs.

**Formule logique**:

Gross Margin % = Gross Margin / Sales Amount

**Mesures dépendantes**:
- [Sales Amount]
- [Cost Amount]
- [Gross Margin]

**Owner métier**: Finance Controlling

**Owner technique**: BI Team

**Périmètre**:
- Inclut ventes validées
- Exclut commandes annulées
- Exclut taxes si non incluses dans Sales Amount

**Fréquence de refresh**: Quotidienne

**Niveau de confidentialité**: Interne
```

---

## 11. CI/CD, versioning et lifecycle management

## 11.1 Principes

Les semantic models critiques doivent être gérés comme des actifs logiciels.

### Règles

- Utiliser des environnements séparés : DEV, TEST/UAT, PROD.
- Versionner les métadonnées du modèle lorsque possible.
- Documenter les changements majeurs.
- Tester les KPI avant promotion.
- Tester les rôles de sécurité avant promotion.
- Utiliser des deployment pipelines lorsque disponibles.
- Éviter les publications manuelles directes en production.
- Mettre en place une stratégie de rollback.

---

## 11.2 Versioning recommandé

Éléments à versionner :

- TMDL ou metadata scripts ;
- fichiers PBIP si utilisés ;
- documentation Markdown ;
- dictionnaire de mesures ;
- mapping RLS ;
- scripts de validation ;
- checklist de publication ;
- changelog.

Structure Git recommandée :

```text
powerbi-semantic-model/
│
├── README.md
├── CHANGELOG.md
├── docs/
│   ├── model_overview.md
│   ├── kpi_dictionary.md
│   ├── security_model.md
│   └── refresh_policy.md
│
├── model/
│   ├── definition/
│   └── tmdl/
│
├── tests/
│   ├── kpi_regression_tests.md
│   └── security_tests.md
│
└── deployment/
    ├── release_checklist.md
    └── rollback_plan.md
```

---

## 11.3 Changelog obligatoire

Chaque changement significatif doit être documenté.

Exemple :

```markdown
## 2026-06-10 — Version 2.0

### Added
- Ajout de DimScenario.
- Ajout des mesures Budget Variance et Budget Variance %.

### Changed
- Refonte de la mesure Gross Margin %.
- Optimisation de FactSales via incremental refresh.

### Fixed
- Correction de la relation inactive sur DeliveryDate.

### Security
- Ajout du rôle RLS Regional Manager.

### Breaking changes
- Renommage de [Revenue] en [Sales Amount].
```

---

## 12. Certification et endorsement

## 12.1 Niveaux de statut

| Statut | Description |
|---|---|
| **Draft** | Modèle en cours de construction, non destiné à la consommation large. |
| **Validated** | Modèle validé fonctionnellement pour un périmètre limité. |
| **Promoted** | Modèle recommandé par l’équipe propriétaire. |
| **Certified** | Modèle reconnu comme source fiable corporate. |
| **Deprecated** | Modèle à ne plus utiliser, en attente de remplacement. |
| **Archived** | Modèle retiré de l’usage actif. |

---

## 12.2 Critères de certification

Un modèle peut être certifié uniquement si :

- l’ownership est défini ;
- les KPI critiques sont validés ;
- la documentation est complète ;
- la sécurité est testée ;
- les performances sont acceptables ;
- le refresh est stable ;
- le modèle respecte les naming conventions ;
- les sources sont identifiées ;
- la sensibilité des données est classifiée ;
- les dépendances sont connues ;
- le modèle ne duplique pas un modèle certifié existant.

---

## 12.3 Checklist de certification

```markdown
# Checklist Certification Semantic Model

## Ownership
- [ ] Business Owner identifié
- [ ] Technical Owner identifié
- [ ] Support Contact identifié
- [ ] Data Steward identifié si nécessaire

## Modélisation
- [ ] Schéma en étoile conforme
- [ ] Grain des faits documenté
- [ ] Dimensions conformes utilisées
- [ ] Relations sans ambiguïté
- [ ] Table calendrier validée

## KPI
- [ ] KPI critiques documentés
- [ ] Définitions validées par le métier
- [ ] Mesures DAX explicites
- [ ] Mesures complexes commentées
- [ ] Tests de non-régression effectués

## Performance
- [ ] Taille du modèle maîtrisée
- [ ] Temps de réponse acceptable
- [ ] Refresh stable
- [ ] Incremental refresh configuré si nécessaire
- [ ] Mesures critiques profilées

## Sécurité
- [ ] RLS testée
- [ ] OLS testée si applicable
- [ ] Données sensibles identifiées
- [ ] Accès via groupes de sécurité
- [ ] Risque d’export évalué

## Documentation
- [ ] README du modèle disponible
- [ ] Dictionnaire KPI disponible
- [ ] Documentation sécurité disponible
- [ ] Politique de refresh documentée
- [ ] Changelog disponible
```

---

## 13. Monitoring et run operations

## 13.1 Monitoring obligatoire

Les modèles critiques doivent être monitorés sur :

- durée de refresh ;
- échecs de refresh ;
- volume de données ;
- taille mémoire ;
- fréquence d’utilisation ;
- rapports connectés ;
- utilisateurs actifs ;
- temps de réponse ;
- consommation capacité ;
- incidents de sécurité ;
- évolution des dépendances.

---

## 13.2 Seuils d’alerte recommandés

| Indicateur | Alerte |
|---|---|
| Refresh échoué | Immédiate |
| Refresh > SLA | Investigation |
| Croissance modèle > 20 % par mois | Revue volumétrie |
| Mesure critique lente | Profiling DAX |
| Rapport connecté non utilisé > 90 jours | Revue de pertinence |
| Modèle dupliqué | Rationalisation |
| Utilisateur sans rôle RLS attendu | Revue sécurité |

---

## 13.3 Runbook minimal

Chaque modèle critique doit avoir un runbook décrivant :

- quoi faire en cas d’échec de refresh ;
- qui contacter ;
- comment relancer ;
- comment vérifier les sources ;
- comment désactiver temporairement un rapport ;
- comment communiquer un incident ;
- comment revenir à une version précédente ;
- comment documenter la résolution.

---

## 14. Checklist qualité avant publication

## 14.1 Cohérence sémantique

- [ ] Les KPI critiques sont validés par le métier.
- [ ] Les définitions sont alignées sur le glossaire corporate.
- [ ] Les noms visibles sont compréhensibles.
- [ ] Les mesures ne dupliquent pas des définitions existantes.
- [ ] Les limitations fonctionnelles sont documentées.

---

## 14.2 Intégrité du schéma

- [ ] Le modèle respecte un schéma en étoile.
- [ ] Le grain de chaque table de faits est documenté.
- [ ] Les relations sont sans ambiguïté.
- [ ] Les relations bidirectionnelles sont justifiées.
- [ ] Les relations inactives sont documentées.
- [ ] Les dimensions conformes sont réutilisées.
- [ ] Les many-to-many sont modélisés explicitement.

---

## 14.3 Performance

- [ ] Les colonnes inutilisées sont supprimées.
- [ ] Les colonnes haute cardinalité sont justifiées.
- [ ] Les mesures critiques sont testées.
- [ ] Le refresh respecte le SLA.
- [ ] L’incremental refresh est configuré si nécessaire.
- [ ] Les pages critiques sont testées.
- [ ] Les agrégations sont évaluées pour les grands volumes.

---

## 14.4 Sécurité

- [ ] Les données sensibles sont identifiées.
- [ ] RLS est configurée et testée.
- [ ] OLS est configurée si nécessaire.
- [ ] Les rôles sont assignés via groupes.
- [ ] Les cas utilisateurs représentatifs sont testés.
- [ ] Les risques d’export sont évalués.
- [ ] Les accès Build sont contrôlés.

---

## 14.5 Maintenabilité

- [ ] Naming conventions respectées.
- [ ] Dossiers d’affichage configurés.
- [ ] Mesures complexes commentées.
- [ ] Documentation disponible.
- [ ] Changelog mis à jour.
- [ ] Ownership explicite.
- [ ] Dépendances connues.
- [ ] Dette technique documentée.

---

## 15. Lignes rouges

Les pratiques suivantes sont interdites pour les modèles corporate :

- construire un modèle en table unique pour des besoins analytiques multi-axes ;
- publier un modèle sans table calendrier standard ;
- utiliser des mesures implicites pour des KPI stratégiques ;
- multiplier les relations bidirectionnelles sans justification ;
- créer des many-to-many non documentés ;
- charger des colonnes sensibles non nécessaires ;
- publier sans ownership ;
- publier sans validation métier des KPI critiques ;
- utiliser DirectQuery sans analyse de performance ;
- dupliquer un semantic model certifié existant ;
- coder la sécurité dans plusieurs mesures dispersées ;
- laisser des colonnes techniques visibles aux utilisateurs ;
- renommer des KPI sans analyse d’impact ;
- publier directement en production sans test pour un modèle critique ;
- ignorer les échecs de refresh récurrents ;
- masquer une dette technique critique sans plan de remédiation.

---

## 16. Astuces expertes et gains rapides

## 16.1 Table de mesures dédiée

Créer une table dédiée aux mesures permet de centraliser la logique métier.

```text
_Measures
- Sales Amount
- Gross Margin
- Gross Margin %
- Sales Amount YTD
- Budget Variance
```

Avantages :

- meilleure navigation ;
- expérience utilisateur plus claire ;
- maintenance simplifiée ;
- séparation entre données et logique de calcul.

---

## 16.2 Branching de mesures

Construire des mesures atomiques puis dérivées réduit la dette DAX.

```text
Base:
[Sales Amount]
[Cost Amount]

Derived:
[Gross Margin]
[Gross Margin %]

Time:
[Sales Amount YTD]
[Sales Amount PY]
[Sales Amount YoY %]
```

---

## 16.3 Dimensions dégénérées contrôlées

Garder certains identifiants transactionnels dans les faits peut être utile pour :

- drillthrough ;
- audit ;
- investigation opérationnelle ;
- rapprochement avec la source.

Mais cela doit rester contrôlé pour éviter l’explosion de cardinalité.

---

## 16.4 Format strings dynamiques

Utiliser des formats dynamiques permet d’éviter la multiplication de mesures similaires.

Exemples :

```text
Sales Amount → €#,0
Margin % → 0.0%
Volume → #,0 units
```

---

## 16.5 Calculation groups

Les calculation groups permettent d’harmoniser les calculs répétitifs.

Exemples :

- YTD ;
- Previous Year ;
- YoY ;
- YoY % ;
- Actual vs Budget ;
- Scenario selection.

À utiliser uniquement avec gouvernance mature.

---

## 16.6 Tests de non-régression KPI

Comparer automatiquement les KPI avant/après modification.

Exemple :

```text
Ancien modèle:
Sales Amount = 12 450 000 €

Nouveau modèle:
Sales Amount = 12 450 000 €

Écart:
0 %
```

Un écart doit être :

- attendu et expliqué ;
- ou bloquant avant publication.

---

## 16.7 Mapping de sécurité externalisé

Préférer une table de sécurité maintenable à une logique hardcodée.

```text
UserPrincipalName | Region | BusinessUnit | AccessLevel
user@company.com  | France | Retail       | Read
```

---

## 16.8 Modèle thin reports

Privilégier des rapports fins connectés à un semantic model certifié.

```text
Certified Semantic Model
        ↓
Report Finance
Report Sales
Report Operations
Report Executive
```

Avantages :

- KPI cohérents ;
- gouvernance centralisée ;
- maintenance réduite ;
- réutilisation accrue.

---

## 17. Mode opératoire standard

## 17.1 SOP de création d’un semantic model

1. Cadrer le besoin métier.
2. Identifier les décisions à soutenir.
3. Définir les KPI critiques.
4. Identifier les sources.
5. Définir le grain des faits.
6. Concevoir le schéma cible.
7. Valider facts/dimensions avec Data Architect.
8. Construire Power Query minimal et propre.
9. Configurer relations et cardinalités.
10. Créer table calendrier.
11. Implémenter mesures atomiques.
12. Implémenter mesures dérivées.
13. Organiser les mesures.
14. Masquer les colonnes techniques.
15. Configurer RLS/OLS si nécessaire.
16. Tester les KPI.
17. Tester la performance.
18. Tester le refresh.
19. Documenter le modèle.
20. Publier en environnement DEV.
21. Faire valider en UAT.
22. Promouvoir en PROD.
23. Monitorer usage, refresh et performance.
24. Préparer certification si applicable.

---

## 17.2 SOP de modification d’un modèle existant

1. Identifier le changement demandé.
2. Classifier le changement : mineur, majeur, breaking change.
3. Analyser les rapports dépendants.
4. Analyser l’impact sur les KPI.
5. Analyser l’impact sécurité.
6. Modifier en DEV.
7. Exécuter tests de non-régression.
8. Valider avec le métier.
9. Mettre à jour documentation et changelog.
10. Déployer en TEST/UAT.
11. Déployer en PROD.
12. Surveiller les refresh et usages post-déploiement.

---

## 17.3 Classification des changements

| Type | Exemple | Validation requise |
|---|---|---|
| Mineur | Correction description, masquage colonne inutile | Technical Owner |
| Standard | Ajout mesure, ajout attribut dimension | Technical Owner + PO |
| Majeur | Nouvelle table de faits, changement grain | Data Architect + PO |
| Sécurité | RLS/OLS, données sensibles | Security Officer + Admin Power BI |
| Breaking change | Renommage/suppression KPI utilisé | Comité BI / Data Governance |

---

## 18. Gouvernance documentaire

## 18.1 Cycle de révision

Ce standard doit être revu :

- trimestriellement ;
- après incident majeur ;
- après évolution significative de la plateforme Power BI/Fabric ;
- après audit Data Governance ;
- après détection de dérives répétées dans les modèles.

---

## 18.2 Gestion des dérogations

Chaque dérogation doit être enregistrée dans un registre.

Exemple :

```markdown
## Dérogation

**Modèle concerné**: Sales Operations Model  
**Règle concernée**: Relation bidirectionnelle interdite par défaut  
**Motif**: Besoin temporaire de filtrage croisé pour analyse de segmentation  
**Impact**: Risque performance modéré  
**Durée de validité**: 3 mois  
**Plan de remédiation**: Création bridge table dédiée  
**Approbateur**: Data Architect  
**Date de revue**: 2026-09-10
```

---

## 18.3 Incidents et amélioration continue

Les incidents suivants doivent alimenter l’amélioration du standard :

- divergence de KPI entre rapports ;
- problème de sécurité ;
- fuite de données ;
- refresh instable ;
- lenteur récurrente ;
- modèle dupliqué ;
- mauvaise utilisation self-service ;
- dette DAX critique ;
- mauvaise compréhension métier ;
- modèle non maintenable.

---

## 19. Annexes

## 19.1 Exemple de structure logique

```text
Dimensions:
- DimDate
- DimCustomer
- DimProduct
- DimGeography
- DimChannel
- DimOrganization
- DimScenario

Facts:
- FactSales
- FactInventorySnapshot
- FactBudget

Bridge:
- BridgeCustomerSegment

Measures:
- [Sales Amount]
- [Sales Amount YTD]
- [Gross Margin]
- [Gross Margin %]
- [Budget Amount]
- [Budget Variance]
- [Budget Variance %]
```

---

## 19.2 Exemple de README pour un semantic model

```markdown
# Semantic Model — Sales Performance

## Objectif

Ce modèle fournit une couche sémantique certifiée pour l’analyse des ventes, marges, clients, produits, régions et performance budgétaire.

## Owners

- Business Owner: Sales Direction
- Technical Owner: BI Team
- Data Steward: Sales Data Steward
- Support Contact: bi-support@company.com

## Sources

- Data Warehouse: DWH_SALES
- Tables principales:
  - fact_sales
  - dim_customer
  - dim_product
  - dim_date
  - fact_budget

## Grain

- FactSales: une ligne par transaction de vente, produit, client, date de commande.
- FactBudget: une ligne par mois, produit, région, scénario.

## Refresh

- Fréquence: quotidienne
- Incremental refresh: oui
- Historique: 5 ans
- Fenêtre incrémentale: 30 jours

## Sécurité

- RLS: oui, par région commerciale
- OLS: non
- Données sensibles: données client pseudonymisées

## KPI principaux

- Sales Amount
- Gross Margin
- Gross Margin %
- Sales Amount YTD
- Budget Variance
- Budget Variance %

## Limitations

- Les ventes annulées sont exclues.
- Les données du jour courant peuvent être incomplètes avant 09:00.
- Les données budgétaires sont mises à jour mensuellement.

## Statut

Certified
```

---

## 19.3 Template de dictionnaire KPI

```markdown
# KPI Dictionary

| KPI | Définition métier | Formule logique | Mesure DAX | Owner | Fréquence | Sensibilité |
|---|---|---|---|---|---|---|
| Sales Amount | Chiffre d’affaires validé hors annulations | Somme ventes nettes | [Sales Amount] | Sales | Daily | Internal |
| Gross Margin % | Marge brute rapportée au CA | Gross Margin / Sales Amount | [Gross Margin %] | Finance | Daily | Confidential |
```

---

## 19.4 Template de registre de sécurité

```markdown
# Security Register

| Rôle | Population | Règle RLS | Groupe de sécurité | Validé par | Date |
|---|---|---|---|---|---|
| Regional Manager | Managers régionaux | Accès aux régions affectées | SG-PBI-RegionalManagers | Security Officer | 2026-06-10 |
| Finance Controller | Finance | Accès global finance | SG-PBI-Finance | Finance Owner | 2026-06-10 |
```

---

## 19.5 Template de tests de non-régression

```markdown
# KPI Regression Test

| KPI | Ancien résultat | Nouveau résultat | Écart | Statut | Commentaire |
|---|---:|---:|---:|---|---|
| Sales Amount | 12 450 000 | 12 450 000 | 0 % | Pass | Aucun écart |
| Gross Margin % | 31.2 % | 31.1 % | -0.1 pt | Review | Écart lié à correction source |
```

---

## 19.6 Confidentialité

Certaines données, exemples, noms de tables, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, aux règles de gouvernance et aux politiques de sécurité de l’organisation.

---

## 19.7 Résumé exécutif du standard

Un semantic model Power BI corporate doit être :

- modélisé en étoile ;
- aligné sur les définitions métier ;
- documenté ;
- sécurisé ;
- performant ;
- maintenable ;
- versionné ;
- réutilisable ;
- testé ;
- monitoré ;
- gouverné.

La qualité d’un modèle Power BI ne se mesure pas uniquement à sa capacité à alimenter un rapport. Elle se mesure à sa capacité à devenir une source sémantique fiable, durable et partagée pour l’organisation.
