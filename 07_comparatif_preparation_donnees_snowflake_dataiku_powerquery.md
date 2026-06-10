# Comparatif des approches de preparation des donnees avant reporting Power BI

## Objectif
Comparer, sur une base de bonnes pratiques editeur, trois lieux de preparation des donnees avant la construction d'un rapport dans Power BI Desktop :
- Snowflake (cote base de donnees)
- Dataiku DSS
- Power Query (Power BI Desktop)

Le but est de definir une repartition claire des traitements pour maximiser performance, fiabilite, gouvernance et maintenabilite.

## Sources de reference officielles
- Snowflake Documentation: https://docs.snowflake.com/
- Dataiku DSS Documentation: https://doc.dataiku.com/dss/latest/
- Microsoft Learn - Power BI / Power Query: https://learn.microsoft.com/power-bi/

---

## Partie 1 - Analyse detaillee par outil

### 1) Snowflake (preparation en base)

| Axe | Analyse professionnelle |
|---|---|
| Role principal | Industrialiser les transformations lourdes et stabiliser des jeux de donnees certifies pour la consommation BI. |
| Points forts | 1) Scalabilite compute/stockage elevee pour gros volumes. 2) Execution SQL massivement parallele adaptee aux jointures et agregations lourdes. 3) Gouvernance centralisee (roles, masking, policies) favorable aux contextes corporate. 4) Forte reutilisation des datasets certifies entre plusieurs rapports Power BI. 5) Bon support des modeles incrementaux (partitions, micro-batch, merge) pour reduire les temps de refresh. 6) Traçabilite des executions et observabilite des requetes utiles pour le pilotage de performance BI. |
| Limites | 1) Moins adapte aux iterations tres rapides de type no-code. 2) Courbe d'apprentissage SQL/architecture plus forte pour les profils purement analytiques. 3) Changement de logique souvent plus formalise (revue, deploiement). 4) Certaines transformations tres "presentation" n'ont pas de valeur a etre industrialisees en base. |
| Contraintes | 1) Necessite standards stricts de modelisation (naming, grain, cles, historisation). 2) Versioning et CI/CD SQL indispensables pour eviter les regressions. 3) Gestion active du cout compute (warehouses, fenetres d'execution, auto-suspend). 4) Definition explicite des SLA de fraicheur pour aligner data prep et refresh Power BI. 5) Documentation des regles metier obligatoire pour fiabiliser la couche certifiee. |
| Risques frequents | 1) Sur-transformation: logique trop complexe en amont sans gain BI reel. 2) Multiplication d'objets intermediaires peu maintenables. 3) Derive de grain (mauvais niveau de detail) impactant cardinalites du modele Power BI. 4) Absence de tests de non-regression sur KPI critiques. 5) Mauvaise strategie incrementale provoquant refresh longs ou donnees manquantes. |
| Cas d'usage cibles | Cleansings volumineux, normalisation, dedoublonnage, historisation, agregations lourdes, tables de reporting partagees. |

### 2) Dataiku DSS (preparation dans le pipeline analytique)

| Axe | Analyse professionnelle |
|---|---|
| Role principal | Orchestrer des flux de preparation reproductibles (visuels et code) entre sources et cibles, avec suivi de lineage et scenarios. |
| Points forts | 1) Forte productivite pour concevoir des pipelines lisibles et collaboratifs. 2) Equilibre no-code/code utile pour equipes mixtes (analystes, data engineers). 3) Lineage et traçabilite des recettes facilitant audit et maintenance. 4) Scenarios d'orchestration, supervision et alerting bien adaptes a la production. 5) Capacite de pushdown vers Snowflake pour conserver la performance base. 6) Bon cadre pour controles qualite de flux avant exposition au reporting. |
| Limites | 1) Performance variable selon le moteur d'execution choisi (pushdown ou non). 2) Couche supplementaire a operer et administrer (droits, projets, runtime). 3) Risque de dispersion de logique metier entre recettes visuelles et scripts SQL. 4) Peut devenir moins lisible si le flux grossit sans architecture claire par domaine. |
| Contraintes | 1) Gouvernance de projet indispensable (naming, dossiers, ownership des recipes). 2) Regles claires sur ce qui doit rester en Snowflake vs Dataiku. 3) Politique de promotion dev/test/prod necessaire pour fiabiliser les livraisons. 4) Maitrise des jeux intermediaires pour limiter cout stockage et dette technique. 5) Documentation des dependances et prerequis de scenarios pour robustesse operationnelle. |
| Risques frequents | 1) Execution involontaire hors base de traitements lourds (degradation forte des performances). 2) Duplication de transformations deja gerees dans Snowflake. 3) Multiplication de datasets intermediaires non purges. 4) Incoherence entre definitions KPI Dataiku et modele Power BI final. 5) Couverture de tests insuffisante sur les transitions de pipeline. |
| Cas d'usage cibles | Enrichissements intermediaires, prototypage industrialisable, controle qualite de pipeline, orchestration de chaines multi-sources. |

### 3) Power Query (Power BI Desktop)

| Axe | Analyse professionnelle |
|---|---|
| Role principal | Derniere mile de preparation specifique au rapport/dataset avant modelisation semantique dans Power BI. |
| Points forts | 1) Vitesse elevee pour ajustements de derniere mile avant visuels. 2) Forte ergonomie pour analystes BI (profil fonctionnel). 3) Integration native avec modele Power BI (types, relations, chargement). 4) Capacite a parametrer des filtres de consommation par dataset. 5) Bon levier pour harmoniser rapidement naming et lisibilite des champs exposes aux utilisateurs. |
| Limites | 1) Non adapte aux transformations volumineuses ou complexes a grande echelle. 2) Maintenabilite degradee si trop de logique metier est implementee en M. 3) Reutilisation limitee entre rapports sans mutualisation explicite. 4) Debug/diagnostic moins robuste qu'une couche de preparation industrialisee. |
| Contraintes | 1) Preservation du query folding critique pour performance de refresh. 2) Discipline de conception necessaire pour garder un ETL leger cote rapport. 3) Alignement obligatoire avec les datasets certifies amont (Snowflake/Dataiku). 4) Documentation des transformations locales pour eviter derive fonctionnelle. 5) Limitation du nombre d'etapes couteuses dans Power Query pour conserver UX de developpement. |
| Risques frequents | 1) Logique metier critique placee trop tard dans le flux (risque d'incoherence inter-rapports). 2) Temps de refresh excessifs dus a transformations non foldables. 3) Duplication de regles dans plusieurs fichiers Power BI. 4) Ecart progressif entre version analyste locale et standard enterprise. 5) Fragilite lors des evolutions de schema source. |
| Cas d'usage cibles | Renommage final de colonnes, typage final, petites colonnes derivees de presentation, parametrage de filtres de consommation. |

---

## Synthese comparative

| Critere | Snowflake | Dataiku | Power Query |
|---|---|---|---|
| Volume de donnees | Excellent | Bon a tres bon (selon pushdown) | Limite a moyen |
| Performance de transformation | Excellent | Bon (excellent si pushdown) | Moyen |
| Gouvernance enterprise | Excellent | Bon a tres bon | Moyen |
| Reutilisation inter-equipes | Excellent | Bon | Faible a moyen |
| Vitesse de prototypage | Moyen | Excellent | Excellent |
| Maintenabilite long terme | Excellent si standards | Bon si cadre de recipes | Faible a moyen si usage excessif |
| Pertinence pour logique metier critique | Tres forte | Forte | Limitee |

---

## Partie 2 - Recommandation cible: combinaison des 3 outils

## Principe directeur
Approche "pushdown-first, orchestration-second, presentation-last" :
1. traiter au plus pres de la donnee (Snowflake),
2. orchestrer et controler les flux (Dataiku),
3. limiter Power Query aux ajustements de consommation BI.

### Repartition recommandee par etape

| Etape du flux | Snowflake | Dataiku | Power Query |
|---|---|---|---|
| Ingestion / consolidation | Oui (staging, vues, tables intermediaires) | Oui (connecteurs, orchestration) | Non |
| Nettoyage lourd / dedoublonnage | Oui (prioritaire) | Possible si pushdown gere | Non recommande |
| Regles metier critiques | Oui (SQL versionne, testable) | Possible pour orchestration des controles | Non recommande |
| Historisation / snapshots / partitionnement | Oui (prioritaire) | Pilotage des jobs | Non |
| Qualite des donnees (tests) | Oui (tests SQL) | Oui (checks pipeline, scenarios) | Limite |
| Preparation de datasets metiers certifies | Oui | Oui (publication/automation) | Non |
| Ajustements specifiques rapport | Limite | Limite | Oui (leger) |
| Mise en forme finale pour visuels | Non | Non | Oui |

### Allocation pratique des responsabilites

| Type de transformation | Outil recommande | Justification |
|---|---|---|
| Jointures volumineuses multi-sources | Snowflake | Moteur SQL scalable et gouverne |
| Calculs metiers reutilisables (KPI de base) | Snowflake | Coherence inter-rapports |
| Enchainement de flux, dependances, scheduling | Dataiku | Orchestration et observabilite |
| Controles qualite de pipeline | Dataiku + Snowflake | Double niveau: technique et metier |
| Parametrage local d'un dataset Power BI | Power Query | Flexibilite analyste sur le dernier kilometre |
| Renommage/typage final orientee usage | Power Query | Lisibilite du modele de rapport |

---

## Pattern cible pour un projet BI corporate

1. Snowflake
- Construire les couches de donnees (raw -> trusted -> reporting).
- Implementer les regles metier critiques et l'historisation.
- Exposer une table/vue certifiee pour la BI.
- Definir explicitement le grain des tables de reporting (ex: 1 ligne = 1 contrat x mois) pour eviter les erreurs de cardinalite dans Power BI.
- Livrer des objets "prets BI" avec cles stables, types normalises, dates conformes et colonnes de partition pour refresh incremental.
- Mettre en place des tests SQL automatisees avant publication: unicite de cle metier, controle des NULL sur champs obligatoires, coherence des KPI de base.
- Publier des metadonnees minimales par objet: definition des colonnes, regles de calcul, frequence de rafraichissement, proprietaire metier/technique.
- Stabiliser les performances via modeles incrementaux (MERGE/INSERT par partition) et fenetres de recalcul limitees.

2. Dataiku
- Orchestrer les traitements, checks et dependances.
- Industrialiser l'execution (scenarios, monitoring, alerting).
- Publier les datasets valides vers la couche reporting.
- Configurer les recipes pour privilegier le pushdown Snowflake sur toutes les transformations lourdes.
- Structurer les flows par zones fonctionnelles (ingestion, preparation, controles, publication) pour faciliter maintenance et audit.
- Ajouter des quality gates obligatoires avant publication: volume attendu, taux de nulls, seuil de doublons, fraicheur des partitions.
- Parametrer des scenarios avec reprise sur incident (retry), alertes ciblees et journalisation des erreurs exploitables.
- Versionner les changements de pipeline (regles, schema, mapping) avec procedure de promotion dev -> test -> prod.

3. Power BI (Power Query)
- Appliquer seulement des transformations legeres et specifiques au rapport.
- Preserver le query folding autant que possible.
- Limiter les transformations a la consommation: renommage lisible, typage final, colonnes derivees simples non critiques.
- Verifier explicitement le query folding sur les etapes majeures; en cas de rupture, deplacer la logique en amont (Snowflake/Dataiku).
- Construire un modele semantique propre: schema en etoile, relations 1-* explicites, table calendrier standard, mesures DAX centralisees.
- Aligner le parametrage incremental refresh avec la colonne de partition livree par Snowflake.
- Documenter dans le fichier PBIX les transformations locales restantes pour eviter duplication et derive inter-rapports.

---

## Limites et points de vigilance

- Reduire la duplication de logique metier entre Snowflake, Dataiku et Power Query.
- Limiter les transformations lourdes en Power Query Desktop pour des datasets volumineux.
- Garantir une source of truth unique pour les indicateurs critiques (idealement Snowflake).
- Formaliser clairement la frontiere entre "donnee certifiee" et "ajustement de presentation".
- Encadrer strictement les changements de grain entre couches (ex: passage ligne contrat -> ligne client) pour proteger mesures et relations.
- Maitriser les jointures (many-to-many implicites) avant Power BI; verifier systematiquement les cardinalites attendues.
- Imposer des cles techniques stables (surrogate/business keys) pour securiser le suivi incremental.
- Ne pas publier de dataset sans controles minimaux: unicite, nullite, volumetrie attendue, fraicheur des partitions.
- Centraliser les KPI critiques a un seul niveau de verite (SQL ou M ou DAX) pour supprimer les calculs en double.
- Detecter les ruptures de query folding et controler les etapes M couteuses avant mise en production.
- Tracer les renommages de colonnes pour conserver l'alignement entre dictionnaire source et modele BI.
- Privilegier l'incremental refresh plutot qu'un refresh "full" quand une partition/date est disponible.
- Purger les datasets intermediaires orphelins dans Dataiku (sans usage aval) pour limiter cout et complexite.
- Attribuer un ownership explicite: chaque table/vue/pipeline doit avoir un responsable metier et un responsable technique.
- Interdire les promotions directes en production sans environnement test et jeu de donnees representatif.
- Encadrer les evolutions de schema source via un processus d'alerte en cas d'ajout/suppression/renommage de colonnes.
- Separer strictement donnees certifiees et donnees exploratoires dans les datasets exposes au rapport final.
- Completer la documentation technique par les definitions metier des champs et KPI utilises en visuel.

---

## Conclusion executive
La meilleure preparation des donnees avant visualisation Power BI repose sur une combinaison specialisee :
- Snowflake pour la robustesse, la performance et la gouvernance des transformations structurantes.
- Dataiku pour l'orchestration, la reproductibilite et le pilotage operationnel des pipelines.
- Power Query pour le dernier ajustement de consommation, strictement limite aux besoins du rapport.

Cette repartition permet un compromis optimal entre qualite de donnees, vitesse de livraison, cout de maintenance et performance de rafraichissement dans Power BI.
