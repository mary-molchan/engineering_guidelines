# Standard Corporate Power BI — Analyse Visuelle et Data Storytelling

## 1. En-tête du document

| Champ | Valeur |
|---|---|
| **Titre** | Standard Corporate — Analyse visuelle et data storytelling dans Power BI |
| **Date** | N / A |
| **Auteur / Rôle** | Maryna MOLCHAN / Data Analytics Engineer |
| **Validation / Approbation** | Responsable BI, UX Lead Analytics, Lead Analytics Engineer, Direction Métier |
| **Public cible** | BI Developers, Data Analysts, Analytics Engineers, Decision Makers, Product Owners, Data Product Owners |
| **Commanditaire** | Direction Data & Performance / Direction Métier |
| **Statut** | Applicable à l’échelle de l’entreprise |
| **Périmètre** | Tous les rapports, dashboards, apps Power BI, pages de monitoring, rapports exécutifs, rapports opérationnels et produits analytiques self-service |
| **Objectif du document** | Définir les règles de conception visuelle, d’analyse, de data storytelling, d’ergonomie, d’accessibilité, de performance et de gouvernance permettant de produire des rapports Power BI clairs, fiables, actionnables, maintenables et adoptés par les utilisateurs. |

---

## 1.1 Cadre éditorial et niveau de norme

### Niveau normatif

Ce document est prescriptif. Les exigences s’appliquent par défaut à tout rapport, dashboard, app ou expérience analytique Power BI créée, maintenue, refondue ou publiée dans l’organisation.

### Interprétation des termes

| Terme | Signification |
|---|---|
| **MUST** | Exigence obligatoire, non négociable hors dérogation formelle. |
| **SHOULD** | Exigence fortement recommandée ; toute exception doit être justifiée et documentée. |
| **MAY** | Option autorisée selon le contexte, à condition de ne pas contredire les règles MUST/SHOULD. |
| **MUST NOT** | Interdiction explicite. |
| **Exception documentée** | Écart temporaire validé par un approbateur nommé, avec justification, durée de validité et plan de remédiation. |

### Mode de rédaction

Les règles sont rédigées selon une logique de décision, lisibilité, confiance analytique, accessibilité, adoption utilisateur, performance et gouvernance.

### Gestion des exceptions

Toute dérogation doit inclure :

- le contexte métier ;
- le motif de l’écart ;
- l’impact sur la compréhension utilisateur ;
- l’impact sur la performance ;
- l’impact sur l’accessibilité ;
- l’impact sur la gouvernance ;
- la durée de validité ;
- le plan de remédiation ;
- l’approbateur nommé ;
- la date de révision.

### Précedence

En cas de conflit, l’ordre de priorité est le suivant :

1. obligations légales, réglementaires et contractuelles ;
2. politiques de sécurité et confidentialité de l’entreprise ;
3. standards d’accessibilité ;
4. standards Data Governance / BI Governance ;
5. présent standard Power BI ;
6. pratiques locales d’équipe.

---

## 1.2 Définitions clés

| Terme | Définition |
|---|---|
| **Rapport Power BI** | Ensemble de pages interactives permettant d’explorer, expliquer et piloter des indicateurs métier. |
| **Dashboard** | Vue synthétique de pilotage, généralement orientée surveillance, alertes et décision rapide. |
| **Data storytelling** | Structuration narrative des données pour guider l’utilisateur d’une question métier vers un constat, une explication et une action. |
| **Insight** | Information analytique interprétée, contextualisée et utile à la décision. |
| **KPI** | Indicateur clé de performance validé, documenté et rattaché à un objectif métier. |
| **Metric** | Mesure quantitative utilisée dans l’analyse ; toutes les metrics ne sont pas nécessairement des KPI. |
| **Decision page** | Page conçue pour répondre à une décision métier précise. |
| **Analytical path** | Parcours de lecture guidant l’utilisateur de la synthèse vers le détail. |
| **Progressive disclosure** | Principe consistant à afficher d’abord l’information essentielle, puis les détails au clic ou via drillthrough. |
| **Cognitive load** | Effort mental nécessaire pour comprendre une page ou un visuel. |
| **Visual hierarchy** | Organisation visuelle qui indique implicitement ce qui est prioritaire, secondaire ou contextuel. |
| **Affordance** | Signal visuel indiquant à l’utilisateur qu’un élément est interactif ou cliquable. |
| **Accessibility** | Capacité d’un rapport à être compris et utilisé par des personnes ayant des besoins variés, y compris visuels, moteurs, cognitifs ou utilisant un clavier/screen reader. |
| **Executive summary** | Synthèse courte orientée décision, destinée aux décideurs. |
| **Operational detail** | Niveau de détail permettant l’investigation, le contrôle ou l’action terrain. |

---

## 2. Vision, principes directeurs et résultats attendus

## 2.1 Vision

Transformer les données en décisions grâce à des expériences analytiques cohérentes, fiables, accessibles et orientées action.

Un rapport Power BI corporate ne doit pas être une collection de graphiques. Il doit être un produit analytique structuré, pensé pour :

- répondre à des questions métier explicites ;
- réduire l’incertitude ;
- accélérer la prise de décision ;
- rendre les anomalies visibles ;
- guider l’utilisateur du signal vers l’explication ;
- permettre l’action ;
- préserver la confiance dans les KPI ;
- rester performant et maintenable dans le temps.

---

## 2.2 Principes directeurs

1. **Clarté avant densité**  
   Chaque page et chaque visuel doivent répondre à une question métier explicite.

2. **Decision First**  
   Le design part de la décision à prendre, non du graphique à afficher.

3. **Story First**  
   Les pages doivent suivre un fil narratif : contexte, signal, explication, action.

4. **Insight over Decoration**  
   Aucun élément visuel ne doit être ajouté uniquement pour rendre la page “jolie”.

5. **One KPI, One Meaning**  
   Un même KPI doit avoir la même définition, le même format et la même interprétation dans tout le rapport.

6. **Design System Analytique**  
   Thèmes, couleurs, typographies, composants, interactions et terminologie doivent être standardisés.

7. **Progressive Disclosure**  
   Afficher d’abord la synthèse, puis permettre l’exploration contrôlée.

8. **Accessibility by Design**  
   Lisibilité, contraste, navigation clavier, ordre de tabulation et textes alternatifs doivent être pensés dès la conception.

9. **Performance Perçue**  
   Le rapport doit être rapide à ouvrir, fluide à filtrer et facile à comprendre.

10. **Trust by Transparency**  
    Les hypothèses, limites, dates de mise à jour et définitions doivent être visibles ou facilement accessibles.

11. **Adoption by Usefulness**  
    Un rapport n’a de valeur que s’il est utilisé pour décider, piloter ou agir.

---

## 2.3 Résultats attendus

L’application de ce standard doit permettre de :

- augmenter l’adoption des dashboards ;
- réduire les mauvaises interprétations des KPI ;
- diminuer le temps de décision en comité de performance ;
- améliorer l’autonomie des utilisateurs métier ;
- réduire le nombre de demandes ad hoc répétitives ;
- améliorer la cohérence visuelle entre rapports ;
- réduire la surcharge cognitive ;
- fiabiliser la lecture des écarts ;
- améliorer la performance perçue ;
- faciliter l’onboarding des nouveaux utilisateurs ;
- renforcer la confiance dans les produits analytiques.

---

## 2.4 Indicateurs de succès

| Indicateur | Cible recommandée |
|---|---|
| Temps de compréhension de la page d’accueil | < 30 secondes |
| Temps d’identification du KPI principal | < 10 secondes |
| Taux d’utilisateurs actifs mensuels | En hausse continue |
| Taux de pages non consultées | En baisse continue |
| Nombre de questions récurrentes sur la définition des KPI | En baisse continue |
| Temps moyen de chargement des pages critiques | Conforme au SLA BI |
| Taux d’erreurs d’interprétation détectées en UAT | 0 erreur critique |
| Taux de rapports avec thème corporate appliqué | 100 % |
| Taux de rapports critiques testés en accessibilité | 100 % |
| Taux de rapports avec page méthodologie/glossaire | 100 % pour les rapports corporate |

---

## 3. Rôles et responsabilités

## 3.1 RACI simplifié

| Activité | BI Developer | Data Analyst | Analytics Engineer | UX/Data Viz Lead | Data Steward | Métier / PO | Sponsor | Admin Power BI |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Cadrage décisions à supporter | C | R | C | C | I | A/R | C | I |
| Définition personas | C | R | C | A/R | I | A/R | C | I |
| Définition KPI affichés | C | R | C | I | A | A/R | C | I |
| Design des pages | R | R | C | A/R | I | C | I | I |
| Design system / thème | C | C | I | A/R | I | C | I | I |
| Storytelling analytique | C | R | C | A/R | C | A/R | C | I |
| Performance rapport | R | C | R | C | I | I | I | C |
| Tests utilisateurs | R | R | C | A | I | C | C | I |
| Validation sémantique KPI | C | R | C | I | A | A/R | I | I |
| Validation accessibilité | R | C | I | A/R | I | C | I | I |
| Publication | R | C | C | C | I | A | I | A/R |
| Monitoring usage | R | C | C | C | I | C | C | A/R |
| Amélioration continue | R | R | C | A/R | C | A/R | C | C |

---

## 3.2 Ownership obligatoire

Chaque rapport corporate doit avoir :

- un **Business Owner** ;
- un **Technical Owner** ;
- un **Product Owner ou référent métier** ;
- un **Support Contact** ;
- un **niveau de criticité** ;
- un **public cible identifié** ;
- une **fréquence de revue** ;
- un **semantic model de référence** ;
- une **documentation accessible** ;
- un **statut de lifecycle** : Draft, UAT, Published, Certified, Deprecated, Archived.

Aucun rapport corporate ne doit être publié sans ownership explicite.

---

## 4. Cycle de vie d’un rapport Power BI

## 4.1 Étapes standard

```text
1. Cadrage décisionnel
2. Identification des personas
3. Définition des questions métier
4. Définition des KPI et seuils
5. Wireframe
6. Prototype Power BI
7. Validation métier rapide
8. Tests sémantiques
9. Tests UX
10. Tests accessibilité
11. Tests performance
12. Publication contrôlée
13. Communication et formation
14. Monitoring usage
15. Amélioration continue
16. Dépréciation ou archivage
```

---

## 4.2 Statuts du rapport

| Statut | Description | Usage autorisé |
|---|---|---|
| **Draft** | Rapport en construction | Équipe projet uniquement |
| **Prototype** | Version exploratoire validant une logique ou un design | Démonstration contrôlée |
| **UAT** | Version en recette métier | Utilisateurs pilotes |
| **Published** | Version publiée et supportée | Public cible défini |
| **Certified** | Rapport reconnu comme référence corporate | Usage corporate large |
| **Deprecated** | Rapport remplacé ou en fin de vie | Usage déconseillé |
| **Archived** | Rapport retiré de l’usage actif | Consultation historique éventuelle |

---

## 4.3 Critères de passage en production

Un rapport peut être publié en production uniquement si :

- les KPI critiques sont validés ;
- les sources sont identifiées ;
- le semantic model est approuvé ;
- les filtres par défaut sont documentés ;
- les règles de sécurité sont testées ;
- les pages principales sont testées avec utilisateurs représentatifs ;
- les performances sont acceptables ;
- le thème corporate est appliqué ;
- les règles d’accessibilité minimales sont respectées ;
- la documentation est disponible ;
- l’ownership est défini ;
- le plan de support est connu.

---

## 5. Cadrage décisionnel

## 5.1 Principe

Un rapport Power BI doit être conçu à partir des décisions qu’il doit supporter, et non à partir des données disponibles.

### Questions obligatoires avant design

- Quelle décision ce rapport doit-il aider à prendre ?
- Qui prend cette décision ?
- À quelle fréquence ?
- Avec quel niveau de détail ?
- Quelle action est attendue après lecture ?
- Quel risque existe si le KPI est mal interprété ?
- Quelle donnée est nécessaire pour agir ?
- Quel niveau de fraîcheur est requis ?
- Quelles limites doivent être visibles ?
- Quelles questions ad hoc ce rapport doit-il réduire ?

---

## 5.2 Decision brief

Chaque rapport corporate doit avoir un decision brief.

```markdown
# Decision Brief

## Objectif du rapport

Décrire en une phrase la décision ou le pilotage que le rapport doit soutenir.

## Public cible

- Persona principal:
- Personas secondaires:

## Décisions supportées

1. Décision 1
2. Décision 2
3. Décision 3

## Questions métier clés

1. Quelle est la performance actuelle ?
2. Où observe-t-on les écarts ?
3. Pourquoi ces écarts apparaissent-ils ?
4. Quelle action prioriser ?

## KPI critiques

- KPI 1:
- KPI 2:
- KPI 3:

## Fréquence d’usage

- Quotidienne / hebdomadaire / mensuelle / trimestrielle

## Niveau de détail requis

- Synthèse exécutive
- Analyse managériale
- Détail opérationnel

## Actions attendues

- Action 1
- Action 2
- Action 3

## Risques d’interprétation

- Risque 1
- Risque 2

## Limites connues

- Limite 1
- Limite 2
```

---

## 5.3 Anti-patterns de cadrage

| Anti-pattern | Risque | Correction |
|---|---|---|
| “Mettre toutes les données disponibles” | Rapport illisible | Partir des décisions et questions métier |
| “Créer un onglet par demande utilisateur” | Fragmentation et dette UX | Regrouper par parcours analytique |
| “Reproduire Excel à l’identique” | Faible valeur Power BI | Repenser l’expérience interactive |
| “Ajouter un graphique pour chaque champ” | Surcharge cognitive | Prioriser les signaux utiles |
| “Faire joli avant d’être utile” | Mauvaise adoption | Valider l’usage réel avant la décoration |

---

## 6. Personas et niveaux de lecture

## 6.1 Personas standards

| Persona | Besoin principal | Niveau de détail | Design recommandé |
|---|---|---|---|
| **Executive** | Décider vite, voir les alertes | Très synthétique | KPI, tendances, exceptions, recommandations |
| **Manager** | Piloter une équipe ou un périmètre | Synthèse + segments | Écarts, comparaisons, drilldown contrôlé |
| **Operational User** | Comprendre et agir sur les cas | Détaillé | Tables filtrées, drillthrough, causes racines |
| **Analyst** | Explorer et expliquer | Avancé | Segmentation, decomposition, export contrôlé |
| **Data Steward** | Vérifier qualité et cohérence | Technique/métier | Contrôles, anomalies, définitions, lineage |
| **Sponsor** | Suivre l’impact métier | Macro | Résultats, risques, décisions requises |

---

## 6.2 Règle de conception par persona

Un même rapport peut servir plusieurs personas, mais chaque page doit avoir un persona principal.

### Exemple

```text
Page 1 — Executive Overview
Persona principal: Executive

Page 2 — Regional Performance
Persona principal: Manager

Page 3 — Customer Drillthrough
Persona principal: Operational User

Page 4 — Data Quality & Definitions
Persona principal: Data Steward
```

---

## 6.3 Niveaux de lecture

Chaque rapport corporate devrait proposer trois niveaux :

1. **Synthèse** : que se passe-t-il ?
2. **Diagnostic** : pourquoi cela se passe-t-il ?
3. **Action** : que faut-il faire maintenant ?

---

## 7. Architecture d’un rapport

## 7.1 Structure recommandée

Un rapport corporate doit contenir :

1. **Page d’accueil / Executive Summary**
   - contexte ;
   - KPI principaux ;
   - alertes ;
   - navigation ;
   - date de mise à jour ;
   - périmètre.

2. **Pages de pilotage**
   - KPI stratégiques ;
   - tendances ;
   - écarts ;
   - seuils ;
   - statut.

3. **Pages d’analyse détaillée**
   - drilldown par axe ;
   - segmentation ;
   - causes racines ;
   - contributions.

4. **Pages opérationnelles**
   - détail actionnable ;
   - tables de contrôle ;
   - listes d’exceptions ;
   - drillthrough.

5. **Page qualité / méthode / glossaire**
   - définitions ;
   - règles de calcul ;
   - limites ;
   - fraîcheur ;
   - sources ;
   - contacts.

---

## 7.2 Navigation

### Règles

- La navigation doit être visible, stable et cohérente.
- Les boutons doivent avoir des libellés explicites.
- Le retour à la page d’accueil doit être disponible.
- Les bookmarks doivent être nommés clairement.
- Les pages masquées doivent être documentées.
- Les liens internes doivent être testés avant publication.
- Les menus ne doivent pas masquer les informations critiques.
- La navigation doit rester utilisable au clavier.

---

## 7.3 Page d’accueil

La page d’accueil doit permettre à l’utilisateur de comprendre rapidement :

- le sujet du rapport ;
- le périmètre ;
- la date de mise à jour ;
- les KPI principaux ;
- les alertes ;
- les pages disponibles ;
- les filtres actifs majeurs ;
- les limites importantes ;
- le contact support.

### Template recommandé

```text
Haut:
- Titre du rapport
- Date de mise à jour
- Périmètre
- Statut des données

Centre:
- 3 à 6 KPI stratégiques
- Variation vs référence
- Alertes critiques

Bas:
- Navigation vers pages d’analyse
- Définitions courtes
- Contact / support
```

---

## 7.4 Page glossaire et méthodologie

Tout rapport corporate doit inclure une page ou un lien vers :

- dictionnaire KPI ;
- définitions métier ;
- sources utilisées ;
- fréquence de refresh ;
- filtres structurels ;
- règles d’exclusion ;
- limites connues ;
- owner métier ;
- owner technique ;
- version du rapport ;
- changelog synthétique.

---

## 8. Hiérarchie de l’information

## 8.1 Principe

La page doit être lisible selon un parcours naturel :

1. priorité ;
2. contexte ;
3. explication ;
4. détail ;
5. action.

---

## 8.2 Structure visuelle standard

```text
Zone haute:
KPI prioritaires, statut, alertes, période, filtres principaux.

Zone centrale:
Tendances, comparaisons, décomposition, causes principales.

Zone basse:
Détail opérationnel, listes d’exception, tables de vérification.

Zone latérale ou inférieure:
Définitions, notes, aide à la lecture, date de mise à jour.
```

---

## 8.3 Règles de hiérarchie

- Les informations les plus importantes doivent être en haut à gauche ou dans la zone de lecture prioritaire.
- Les KPI principaux doivent être visuellement plus forts que les indicateurs secondaires.
- Les informations décoratives doivent être supprimées.
- Les éléments liés doivent être visuellement groupés.
- Les marges et espaces blancs doivent être utilisés pour réduire la charge cognitive.
- Les titres doivent guider l’interprétation.
- Les détails doivent être disponibles, mais non dominants.
- Les alertes doivent être visibles sans saturer la page.

---

## 8.4 Limite de densité

Règle générale :

- page exécutive : 4 à 8 éléments visuels principaux ;
- page managériale : 6 à 10 éléments visuels ;
- page opérationnelle : 6 à 12 éléments visuels, incluant tables filtrées ;
- page d’audit : densité plus élevée autorisée si le persona est expert.

Ces seuils sont indicatifs. Toute page très dense doit être justifiée par un besoin utilisateur validé.

---

## 9. Choix des visuels

## 9.1 Règle générale

Un visuel doit être choisi selon la question analytique, pas selon l’habitude ou l’esthétique.

### Question avant création

> Quelle question ce visuel permet-il de résoudre plus rapidement que du texte ou un tableau ?

---

## 9.2 Matrice de choix des visuels

| Question analytique | Visuel recommandé | À éviter |
|---|---|---|
| Évolution dans le temps | Courbe, aire simple, barres temporelles | Pie chart, carte géographique |
| Comparaison entre catégories | Barres horizontales/verticales | Jauge, radar, donut |
| Classement | Barres triées, table Top N | Nuage non trié |
| Contribution positive/négative | Waterfall | Pie chart |
| Composition d’un total | Barres empilées, 100 % stacked bar si part relative | Donut trop segmenté |
| Corrélation | Scatter plot | Courbe si pas de temporalité |
| Distribution | Histogramme, box plot si disponible | Moyenne seule |
| Géographie | Carte uniquement si la localisation est une question métier | Carte décorative |
| Écart vs objectif | KPI card, bullet chart, barres avec ligne cible | Gauge décorative |
| Détail opérationnel | Table ou matrice filtrée | Graphique saturé |
| Analyse de causes | Decomposition tree, waterfall, barres segmentées | Multiplication de slicers |
| Flux ou parcours | Sankey si réellement utile | Diagramme décoratif |

---

## 9.3 Visuels recommandés

### Courbe

À utiliser pour :

- tendance ;
- saisonnalité ;
- évolution temporelle ;
- comparaison de séries limitées.

Règles :

- l’axe X doit être temporel ;
- limiter le nombre de séries ;
- afficher la période ;
- annoter les ruptures majeures ;
- éviter les courbes trop nombreuses.

---

### Bar chart

À utiliser pour :

- comparer des catégories ;
- classer ;
- identifier des écarts ;
- comparer Top N.

Règles :

- trier selon la valeur ou la logique métier ;
- utiliser des barres horizontales pour les libellés longs ;
- démarrer l’axe à zéro sauf justification explicite ;
- éviter trop de catégories ;
- regrouper les faibles volumes si nécessaire.

---

### Waterfall

À utiliser pour :

- expliquer une variation ;
- montrer contributions positives/négatives ;
- analyser un passage de N-1 à N ;
- expliquer budget vs réel.

Règles :

- limiter le nombre de contributions ;
- clarifier le point de départ et d’arrivée ;
- utiliser des couleurs cohérentes pour positif/négatif ;
- éviter si les utilisateurs ne comprennent pas ce visuel sans légende.

---

### Scatter plot

À utiliser pour :

- corrélation ;
- segmentation ;
- détection d’outliers ;
- analyse de portefeuille.

Règles :

- nommer clairement les axes ;
- ajouter lignes de référence si utile ;
- utiliser taille/couleur avec modération ;
- prévoir tooltip explicatif ;
- éviter si trop de points sans agrégation.

---

### Carte géographique

À utiliser uniquement si la localisation est centrale dans la question métier.

Cas d’usage valides :

- performance par région ;
- couverture territoriale ;
- zones de risque ;
- logistique ;
- réseau de distribution.

À éviter si :

- la géographie n’est qu’une catégorie parmi d’autres ;
- les zones ont des tailles très différentes ;
- le niveau de précision crée un risque de confidentialité ;
- une simple barre triée serait plus claire.

---

### Table et matrice

À utiliser pour :

- vérification ;
- audit ;
- export contrôlé ;
- détail opérationnel ;
- analyse multi-attributs.

Règles :

- ne pas utiliser une table comme page principale exécutive ;
- limiter les colonnes ;
- mettre les champs actionnables en premier ;
- appliquer tri et format conditionnel ;
- prévoir filtres par défaut ;
- documenter les possibilités d’export.

---

## 9.4 Visuels à usage restreint

| Visuel | Condition d’usage |
|---|---|
| **Pie chart** | Maximum 3 à 5 segments, uniquement pour part de total simple |
| **Donut chart** | Même restriction que pie chart |
| **Gauge** | Uniquement si seuil, cible et progression sont clairs |
| **Radar chart** | À éviter sauf comparaison profilée très spécifique |
| **Treemap** | À utiliser avec prudence pour hiérarchies simples |
| **Sankey** | Seulement si les flux sont la question principale |
| **Custom visual** | Autorisé seulement si valeur analytique supérieure et performance testée |

---

## 9.5 Interdits

- Utiliser un visuel par habitude sans question analytique.
- Utiliser une carte géographique sans question spatiale.
- Utiliser une jauge décorative sans seuil ni action.
- Utiliser plusieurs couleurs sans signification.
- Créer des pie charts avec trop de segments.
- Utiliser des visuels custom non validés.
- Afficher des tableaux massifs sans filtre.
- Superposer trop de séries sur une courbe.
- Utiliser des axes tronqués sans indication claire.
- Mélanger plusieurs unités dans un même axe sans explication.

---

## 10. KPI cards et indicateurs

## 10.1 Structure d’une carte KPI

Une carte KPI corporate doit afficher, selon le contexte :

- nom du KPI ;
- valeur actuelle ;
- unité ;
- période ;
- variation absolue ;
- variation relative ;
- référence de comparaison ;
- statut ;
- seuil ;
- tendance courte ;
- date de mise à jour si critique.

### Exemple

```text
Sales Amount
€12.4M
-8.2 % vs N-1
Statut: À surveiller
Période: Jan–May 2026
```

---

## 10.2 Règles KPI

- Le KPI principal doit être visible sans interaction.
- La période doit être explicite.
- L’unité doit être visible.
- La base de comparaison doit être claire.
- Les seuils doivent être validés par le métier.
- Les statuts doivent avoir une signification documentée.
- Les KPI critiques doivent pointer vers une définition.
- Les variations relatives doivent être accompagnées d’un contexte si risque d’ambiguïté.

---

## 10.3 Statuts et seuils

| Statut | Signification | Exemple |
|---|---|---|
| **On Track** | Conforme à l’objectif | Écart < 5 % |
| **Watch** | À surveiller | Écart entre 5 % et 10 % |
| **Critical** | Action requise | Écart > 10 % |
| **No Target** | Pas de cible définie | KPI informatif |
| **Data Issue** | Donnée suspecte | Refresh incomplet ou anomalie qualité |

Les seuils doivent être métier, non arbitraires.

---

## 10.4 Éviter les KPI trompeurs

### Risques fréquents

- variation en % sur petite base ;
- comparaison avec une période incomplète ;
- moyenne masquant une distribution ;
- total masquant des segments critiques ;
- KPI affiché sans filtre actif ;
- cible non validée ;
- statut couleur sans définition.

### Correction

Ajouter :

- contexte ;
- base de calcul ;
- période ;
- définition ;
- tooltip ;
- warning ;
- comparaison pertinente.

---

## 11. Couleurs et sémantique visuelle

## 11.1 Principes

La couleur doit porter du sens, pas seulement décorer.

### Règles

- Utiliser le thème corporate.
- Réserver les couleurs d’alerte aux alertes réelles.
- Garder une signification constante.
- Limiter les couleurs simultanées.
- Vérifier le contraste.
- Vérifier la lisibilité pour les utilisateurs daltoniens.
- Ne jamais utiliser uniquement la couleur pour transmettre une information critique.

---

## 11.2 Palette sémantique recommandée

| Usage | Signification |
|---|---|
| Couleur principale | Marque, élément actif, série principale |
| Neutres | Contexte, axes, arrière-plan, éléments secondaires |
| Positif | Amélioration ou valeur favorable |
| Négatif | Dégradation ou valeur défavorable |
| Attention | Surveillance, risque modéré |
| Critique | Risque élevé, action urgente |
| Information | Note, aide, contexte |
| Sélection | Filtre actif, état sélectionné |

---

## 11.3 Règles d’alerte

- Rouge doit signifier problème réel.
- Vert doit signifier situation favorable validée.
- Orange doit signaler une attention.
- Gris doit indiquer le contexte ou l’inactif.
- Une alerte doit être liée à un seuil documenté.
- Les couleurs doivent être cohérentes sur toutes les pages.
- Les statuts doivent être accompagnés de texte ou icône.

---

## 11.4 Anti-patterns couleur

- Utiliser rouge/vert pour des catégories neutres.
- Changer la signification d’une couleur entre pages.
- Utiliser trop de couleurs saturées.
- Utiliser une couleur corporate comme alerte si elle crée une ambiguïté.
- Transmettre une alerte uniquement par couleur.
- Utiliser des dégradés non interprétables.
- Utiliser une palette incompatible avec le daltonisme.

---

## 12. Titres, labels, annotations et microcopie

## 12.1 Titres orientés insight

Les titres doivent aider à comprendre, pas seulement nommer.

### Mauvais titre

```text
Évolution du chiffre d’affaires
```

### Bon titre

```text
Le chiffre d’affaires baisse de 8 % vs N-1, principalement en région Nord
```

---

## 12.2 Règles pour les titres

- Le titre doit indiquer la question ou le constat.
- Le titre doit inclure la période si nécessaire.
- Le titre doit éviter les acronymes non expliqués.
- Le titre doit rester court.
- Le titre ne doit pas exagérer le niveau de certitude.
- Les titres insight doivent être mis à jour si les filtres changent ou rester génériques.

---

## 12.3 Labels

### Règles

- Afficher les unités.
- Éviter les labels redondants.
- Ne pas surcharger les visuels.
- Arrondir selon le niveau de décision.
- Utiliser les séparateurs numériques adaptés.
- Préciser les devises.
- Éviter les abréviations ambiguës.

---

## 12.4 Annotations

Les annotations doivent être utilisées pour expliquer :

- rupture de tendance ;
- incident ;
- changement de règle de calcul ;
- campagne commerciale ;
- migration système ;
- période incomplète ;
- anomalie connue ;
- événement externe majeur.

### Exemple

```text
Mars 2026: changement de règle de reconnaissance du revenu.
Les comparaisons avant/après doivent être interprétées avec prudence.
```

---

## 12.5 Microcopie orientée action

La microcopie doit aider l’utilisateur à agir.

| Texte faible | Texte recommandé |
|---|---|
| Filtre | Choisir la région à analyser |
| Détail | Voir les clients concernés |
| Reset | Réinitialiser les filtres |
| Export | Exporter la liste filtrée |
| Plus | Comprendre la variation |
| Info | Voir définition et limites |

---

## 13. Interactions, filtres et navigation analytique

## 13.1 Slicers

### Règles

- Limiter les slicers aux dimensions réellement utiles.
- Les slicers principaux doivent être visibles.
- Les slicers secondaires peuvent être regroupés dans un panneau.
- Les valeurs par défaut doivent être documentées.
- Les slicers haute cardinalité doivent être évités.
- Les slicers doivent avoir des libellés explicites.
- Les filtres actifs critiques doivent être visibles.

---

## 13.2 Filtres

### Types de filtres à documenter

- filtres de page ;
- filtres de rapport ;
- filtres visuels ;
- filtres RLS ;
- filtres par défaut ;
- filtres cachés ;
- filtres via bookmarks.

### Règle critique

Aucun filtre caché impactant un KPI critique ne doit exister sans documentation visible ou accessible.

---

## 13.3 Drillthrough

Le drillthrough doit répondre à une question précise.

### Exemples

- Pourquoi cette région est-elle en alerte ?
- Quels clients contribuent à la baisse ?
- Quels produits expliquent l’écart de marge ?
- Quels tickets sont hors SLA ?

### Règles

- Nommer les pages drillthrough clairement.
- Afficher le contexte reçu.
- Prévoir un bouton retour.
- Limiter les colonnes au besoin actionnable.
- Tester les cas sans donnée.
- Documenter les champs de drillthrough.

---

## 13.4 Tooltips

Les tooltips doivent enrichir la compréhension, pas répéter la valeur.

### Bon tooltip

```text
CA: €2.4M
Variation: -8 % vs N-1
Cause principale: baisse du segment PME
Périmètre: ventes validées hors annulations
```

### Règles

- Rester concis.
- Ajouter contexte, seuil ou définition.
- Éviter les tooltips surchargés.
- Ne pas masquer une information critique uniquement dans un tooltip.
- Tester la lisibilité.

---

## 13.5 Bookmarks

Les bookmarks peuvent servir à :

- vues par rôle ;
- panneaux de filtres ;
- scénarios analytiques ;
- navigation ;
- storytelling guidé.

### Règles

- Nommer clairement les bookmarks.
- Documenter leur usage.
- Tester après modification de page.
- Éviter les bookmarks complexes non maintenables.
- Ne pas utiliser les bookmarks pour masquer une logique métier instable.

---

## 13.6 Interactions croisées

### Règles

- Les interactions doivent être intentionnelles.
- Désactiver les interactions confuses.
- Tester les comportements entre visuels.
- Éviter les pages où chaque visuel filtre tous les autres sans logique.
- Préférer un parcours analytique clair.

---

## 14. Data storytelling

## 14.1 Structure narrative standard

Chaque page analytique doit suivre, lorsque pertinent :

```text
Question → Constat → Explication → Action
```

### Exemple

```text
Question:
Pourquoi la marge baisse-t-elle ce trimestre ?

Constat:
La marge brute recule de 3.2 points vs N-1.

Explication:
La baisse est concentrée sur deux familles produit et trois régions.

Action:
Prioriser revue pricing sur les familles A et B en région Nord.
```

---

## 14.2 Pyramid principle

Pour les pages exécutives :

1. commencer par la conclusion ;
2. montrer les preuves principales ;
3. donner les détails seulement si nécessaire.

### Structure

```text
Conclusion:
La performance est sous objectif.

Evidence:
- CA: -8 % vs budget
- Marge: -3.2 pts
- Région Nord: 60 % de l’écart

Action:
Revue commerciale ciblée sur Nord + segment PME.
```

---

## 14.3 Analytical storyline

Un rapport complet doit guider l’utilisateur selon un chemin logique :

```text
1. Quelle est la situation ?
2. Est-ce bon ou mauvais ?
3. Par rapport à quoi ?
4. Où sont les écarts ?
5. Pourquoi ?
6. Qui ou quoi est concerné ?
7. Quelle action prioriser ?
8. Comment suivre l’amélioration ?
```

---

## 14.4 Types de storytelling

| Type | Usage |
|---|---|
| **Performance review** | Suivi KPI, objectifs, écarts |
| **Exception management** | Détection et traitement des anomalies |
| **Root cause analysis** | Compréhension des causes |
| **Portfolio analysis** | Priorisation de segments, clients, produits |
| **Operational monitoring** | Pilotage quotidien |
| **Executive briefing** | Synthèse décisionnelle |
| **Data quality monitoring** | Surveillance de fiabilité |

---

## 14.5 Règle d’actionnabilité

Chaque page critique doit permettre au moins une des actions suivantes :

- décider ;
- prioriser ;
- escalader ;
- investiguer ;
- corriger ;
- comparer ;
- surveiller ;
- communiquer ;
- planifier ;
- valider.

Si aucune action n’est possible, la page doit être supprimée, fusionnée ou repositionnée comme contexte.

---

## 15. Accessibilité

## 15.1 Principes

L’accessibilité est une exigence de qualité, pas une option.

### Règles obligatoires

- Contraste suffisant.
- Taille de police lisible.
- Navigation clavier testée.
- Ordre de tabulation logique.
- Alt text sur les visuels essentiels.
- Titres explicites.
- Informations critiques non transmises uniquement par couleur.
- Libellés clairs et non ambigus.
- Éviter les rotations de texte.
- Éviter les textes trop petits.
- Tester les rapports avec modes d’affichage standards.

---

## 15.2 Contraste

### Règles

- Les textes doivent rester lisibles sur écran portable, écran externe et projecteur.
- Les couleurs d’arrière-plan ne doivent pas réduire la lisibilité.
- Les éléments d’alerte doivent rester compréhensibles en niveaux de gris.
- Les thèmes sombres doivent être testés séparément.
- Les captures exportées doivent rester lisibles.

---

## 15.3 Alt text

Les visuels essentiels doivent avoir un texte alternatif décrivant :

- le rôle du visuel ;
- le KPI ou la question traitée ;
- l’information principale si stable ;
- la manière d’interpréter le visuel.

### Exemple

```text
Graphique montrant l’évolution mensuelle du chiffre d’affaires et la comparaison avec l’objectif.
```

---

## 15.4 Ordre de tabulation

### Règles

- L’ordre de tabulation doit suivre le parcours de lecture.
- Les éléments décoratifs ne doivent pas être inclus inutilement.
- Les boutons doivent avoir des noms compréhensibles.
- Le retour navigation doit être accessible.
- Les slicers critiques doivent être atteignables.

---

## 15.5 Accessibilité cognitive

### Règles

- Limiter le jargon.
- Expliquer les acronymes.
- Réduire le nombre d’éléments simultanés.
- Utiliser des titres explicites.
- Regrouper les informations liées.
- Maintenir une structure stable.
- Prévoir une page “Comment lire ce rapport”.

---

## 16. Performance UX et optimisation

## 16.1 Performance perçue

Un rapport performant est un rapport :

- qui s’ouvre rapidement ;
- dont les pages principales se chargent sans attente excessive ;
- dont les filtres répondent de manière fluide ;
- dont les visuels ne se recalculent pas inutilement ;
- qui donne rapidement une réponse compréhensible.

---

## 16.2 Règles de performance rapport

- Limiter le nombre de visuels par page.
- Supprimer les visuels redondants.
- Éviter les custom visuals lourds.
- Réduire les interactions inutiles.
- Limiter les slicers haute cardinalité.
- Utiliser des pages drillthrough pour le détail.
- Éviter les tables massives ouvertes par défaut.
- Tester avec Performance Analyzer.
- Optimiser les mesures lentes.
- Utiliser des agrégations si nécessaire.
- Éviter les images lourdes.
- Compresser les ressources visuelles.
- Éviter les fonds de page complexes.
- Tester les pages avec les volumes réels.

---

## 16.3 Performance Analyzer

Les rapports critiques doivent être testés avec Performance Analyzer ou un outil équivalent.

### Mesures à observer

- temps DAX query ;
- temps visual display ;
- temps other ;
- visuels les plus lents ;
- impact des slicers ;
- impact des interactions ;
- temps de première ouverture ;
- comportement après publication.

### Règle

Tout visuel lent sur une page critique doit être :

- optimisé ;
- remplacé ;
- déplacé vers une page de détail ;
- ou justifié et documenté.

---

## 16.4 Optimisation des pages

### Bonnes pratiques

- Prioriser les pages de synthèse rapides.
- Charger les détails uniquement au drillthrough.
- Utiliser Top N plutôt que liste complète.
- Pré-filtrer les tables de détail.
- Éviter les cartes géographiques lourdes si non nécessaires.
- Limiter les calculs complexes sur visuels nombreux.
- Réduire les bookmarks inutiles.
- Tester les visuels custom avant adoption.

---

## 17. Design system Power BI

## 17.1 Principe

Un design system analytique permet d’assurer cohérence, rapidité de développement et meilleure adoption.

Il doit standardiser :

- thème JSON ;
- palette couleur ;
- typographie ;
- tailles de police ;
- styles de KPI cards ;
- styles de titres ;
- boutons ;
- menus ;
- slicers ;
- tooltips ;
- icônes ;
- règles d’alerte ;
- composants réutilisables.

---

## 17.2 Thème JSON corporate

Le thème Power BI doit définir :

- couleurs principales ;
- couleurs sémantiques ;
- arrière-plans ;
- polices ;
- tailles par défaut ;
- couleurs des axes ;
- couleurs des grilles ;
- styles de titres ;
- formats par défaut ;
- palette accessible.

### Règles

- Le thème corporate doit être versionné.
- Les changements de thème doivent être testés.
- Les rapports corporate doivent utiliser le thème standard.
- Les écarts de thème doivent être justifiés.
- Le thème doit respecter les exigences d’accessibilité.

---

## 17.3 Composants standards

Composants recommandés :

- header de page ;
- bandeau de filtres ;
- carte KPI ;
- bloc alerte ;
- bloc insight ;
- bloc définition ;
- bouton retour ;
- bouton reset filtres ;
- panneau “Aide” ;
- tooltip standard ;
- page méthode ;
- page contact support.

---

## 17.4 Layout standard

### Format 16:9

Le format 16:9 est recommandé pour les rapports corporate, sauf besoin spécifique.

### Grille

Utiliser une grille cohérente :

```text
Marge externe: stable
Espacement entre blocs: stable
Alignements: cohérents
Groupes visuels: explicites
```

### Règles

- Aligner les objets.
- Éviter les décalages visuels.
- Utiliser des espaces blancs.
- Regrouper les éléments liés.
- Maintenir les composants aux mêmes emplacements entre pages.
- Éviter les pages visuellement instables.

---

## 18. Localisation et internationalisation

## 18.1 Règles

- Les formats de dates doivent correspondre au public cible.
- Les devises doivent être explicites.
- Les séparateurs décimaux doivent être cohérents.
- Les traductions doivent être validées métier.
- Les acronymes locaux doivent être expliqués.
- Les unités doivent être standardisées.
- Les fuseaux horaires doivent être visibles si nécessaires.
- Les rapports multi-pays doivent préciser le contexte local.

---

## 18.2 Exemples de risques

| Risque | Exemple | Correction |
|---|---|---|
| Date ambiguë | 06/10/2026 | Afficher 10 Jun 2026 ou 10/06/2026 selon locale |
| Devise ambiguë | 10M | Afficher €10M, $10M ou £10M |
| Unité implicite | Volume | Afficher unités, kg, tonnes, transactions |
| Fuseau horaire absent | Dernière mise à jour 07:00 | Afficher 07:00 CET |
| Terminologie différente | Sales vs Revenue | Choisir une définition officielle |

---

## 19. Exactitude analytique et confiance

## 19.1 Règles

- Les KPI doivent provenir d’un semantic model validé ou certifié lorsque disponible.
- Les filtres cachés doivent être documentés.
- Les mesures doivent être cohérentes avec le dictionnaire KPI.
- Les totaux doivent être vérifiés.
- Les comparaisons doivent être pertinentes.
- Les périodes incomplètes doivent être signalées.
- Les données manquantes doivent être visibles si elles impactent l’analyse.
- Les anomalies connues doivent être annotées.
- Les sources doivent être indiquées.
- Les limitations doivent être accessibles.

---

## 19.2 Contextes de comparaison

Toute variation doit indiquer sa référence.

Exemples :

- vs N-1 ;
- vs mois précédent ;
- vs budget ;
- vs forecast ;
- vs objectif ;
- vs moyenne mobile ;
- vs benchmark externe ;
- vs période comparable.

### Règle

Une variation sans base de comparaison explicite est interdite pour les KPI critiques.

---

## 19.3 Gestion des périodes incomplètes

Les périodes incomplètes doivent être signalées.

Exemples :

```text
Données du mois courant partielles au 12/06/2026.
Comparaison vs mois complet non recommandée.
```

ou :

```text
Période courante exclue des tendances car données non consolidées.
```

---

## 19.4 Gestion des données absentes

Ne pas masquer silencieusement :

- données nulles ;
- absence de refresh ;
- données partielles ;
- segments non disponibles ;
- exclusions ;
- anomalies qualité ;
- rupture de source.

Prévoir un statut :

```text
No Data
Data Pending
Partial Data
Data Quality Issue
Source Unavailable
```

---

## 20. Validation utilisateur

## 20.1 Tests utilisateurs

Les rapports critiques doivent être testés avec des utilisateurs représentatifs.

### Recommandation

Tester avec 5 à 8 utilisateurs permet souvent d’identifier la majorité des problèmes d’ergonomie majeurs.

### À observer

- comprennent-ils le KPI principal sans explication ?
- trouvent-ils les filtres ?
- comprennent-ils la période ?
- interprètent-ils correctement les couleurs ?
- savent-ils quoi faire ensuite ?
- utilisent-ils les tooltips ?
- trouvent-ils les définitions ?
- se perdent-ils dans la navigation ?
- demandent-ils un export Excel immédiatement ?
- identifient-ils les limites ?

---

## 20.2 Script de test utilisateur

```markdown
# Test utilisateur — Rapport Power BI

## Contexte

Nom du rapport:
Utilisateur:
Persona:
Date:

## Tâche 1 — Compréhension rapide

Sans explication préalable, demander:
- Quel est le message principal de cette page ?
- Quel KPI semble prioritaire ?
- La situation est-elle bonne ou mauvaise ?
- Quelle action prendriez-vous ?

## Tâche 2 — Navigation

Demander:
- Trouver la performance d’une région donnée.
- Identifier le segment qui explique le plus grand écart.
- Revenir à la page d’accueil.

## Tâche 3 — Interprétation

Demander:
- Expliquer la variation principale.
- Identifier la période et la base de comparaison.
- Trouver la définition du KPI.

## Tâche 4 — Accessibilité et lisibilité

Observer:
- lisibilité des textes ;
- compréhension des couleurs ;
- taille des objets ;
- navigation clavier si applicable.

## Résultats

Points compris:
Points ambigus:
Erreurs d’interprétation:
Suggestions utilisateur:
Actions de correction:
```

---

## 20.3 Critères de validation UX

Un rapport ne doit pas passer en production si :

- le KPI principal est mal compris ;
- la couleur d’alerte est mal interprétée ;
- la période est ambiguë ;
- les filtres actifs ne sont pas identifiables ;
- l’utilisateur ne trouve pas la définition d’un KPI critique ;
- une page critique nécessite une explication orale pour être comprise ;
- la navigation principale est confuse ;
- les performances empêchent l’usage normal.

---

## 21. Adoption et conduite du changement

## 21.1 Plan d’adoption

Chaque rapport corporate doit prévoir :

- communication de lancement ;
- description de la valeur métier ;
- public cible ;
- guide de lecture ;
- formation courte ;
- canal de support ;
- collecte de feedback ;
- suivi d’usage ;
- plan d’amélioration.

---

## 21.2 Page “Comment lire ce dashboard”

Cette page doit expliquer :

- objectif du rapport ;
- structure des pages ;
- principaux KPI ;
- filtres disponibles ;
- signification des couleurs ;
- seuils ;
- fréquence de refresh ;
- définitions ;
- limites ;
- contact support.

---

## 21.3 Formation

Formats recommandés :

- démonstration de 15 minutes ;
- vidéo courte ;
- guide PDF ou Markdown ;
- session Q&A ;
- exemples de décisions ;
- FAQ ;
- mini-cas d’usage par persona.

---

## 21.4 Monitoring d’usage

Suivre :

- utilisateurs actifs ;
- fréquence de consultation ;
- pages les plus consultées ;
- pages ignorées ;
- temps moyen d’usage ;
- exports ;
- demandes support ;
- feedback qualitatif ;
- incidents d’interprétation ;
- adoption par persona.

### Actions possibles

| Signal | Action |
|---|---|
| Page peu consultée | Revoir utilité ou navigation |
| Export massif | Ajouter détail opérationnel ou clarifier besoin |
| Questions répétées | Améliorer microcopie ou glossaire |
| Faible adoption | Former, repositionner ou simplifier |
| Forte lenteur | Optimiser modèle ou visuels |
| Mauvaise interprétation | Corriger titre, légende, seuils ou design |

---

## 22. Gouvernance des rapports

## 22.1 Documentation obligatoire

Chaque rapport corporate doit documenter :

- objectif ;
- public cible ;
- owner métier ;
- owner technique ;
- semantic model utilisé ;
- sources principales ;
- KPI critiques ;
- règles de filtrage ;
- règles de sécurité ;
- fréquence de refresh ;
- limites connues ;
- version ;
- date de dernière validation ;
- statut ;
- contact support.

---

## 22.2 Changelog

Tout changement significatif doit être documenté.

```markdown
# Changelog

## 2026-06-12 — Version 2.0

### Added
- Nouvelle page Executive Summary.
- Ajout du drillthrough client.

### Changed
- Refonte des KPI cards.
- Simplification du panneau de filtres.

### Fixed
- Correction du titre dynamique sur la page région.
- Correction du tri des catégories produit.

### Removed
- Suppression du visuel custom non utilisé.

### Impact
- Aucun changement de définition KPI.
```

---

## 22.3 Gestion des demandes d’évolution

Les demandes doivent être qualifiées selon :

- valeur métier ;
- urgence ;
- population impactée ;
- effort estimé ;
- risque de dette UX ;
- impact performance ;
- impact sémantique ;
- dépendance modèle ;
- disponibilité des données.

### Critères de refus ou report

Une demande peut être refusée ou reportée si :

- elle duplique un rapport existant ;
- elle ajoute un visuel sans question métier ;
- elle complexifie fortement la page ;
- elle contredit une définition KPI ;
- elle dégrade la performance ;
- elle n’a pas de sponsor ;
- elle expose une donnée sensible ;
- elle relève d’une extraction ponctuelle et non d’un besoin récurrent.

---

## 22.4 Dépréciation et archivage

Un rapport doit être déprécié si :

- il n’est plus utilisé ;
- il duplique un rapport certifié ;
- ses données ne sont plus fiables ;
- son owner n’existe plus ;
- il n’est plus maintenable ;
- il expose un risque de sécurité ;
- il utilise un modèle obsolète.

Avant archivage :

- informer les utilisateurs ;
- proposer un rapport de remplacement ;
- vérifier les dépendances ;
- conserver la documentation si nécessaire ;
- mettre à jour les liens dans les apps.

---

## 23. Checklist qualité avant publication

## 23.1 Pertinence décisionnelle

- [ ] Le rapport répond à une décision ou un pilotage explicite.
- [ ] Chaque page a un persona principal.
- [ ] Chaque visuel répond à une question métier claire.
- [ ] Les KPI principaux sont interprétables en moins de 30 secondes.
- [ ] Les actions attendues sont identifiables.
- [ ] Les pages inutiles ou redondantes ont été supprimées.
- [ ] Les besoins d’export ont été challengés.

---

## 23.2 Cohérence visuelle

- [ ] Le thème corporate est appliqué.
- [ ] Les couleurs ont une signification constante.
- [ ] Les titres sont explicites.
- [ ] Les unités sont visibles.
- [ ] Les périodes sont visibles.
- [ ] Les légendes sont cohérentes.
- [ ] Les alignements sont propres.
- [ ] Les marges et espaces blancs sont maîtrisés.
- [ ] Les composants standards sont utilisés.
- [ ] La navigation est intuitive.

---

## 23.3 Exactitude analytique

- [ ] Les KPI concordent avec le semantic model validé.
- [ ] Les définitions KPI sont accessibles.
- [ ] Les filtres cachés sont documentés.
- [ ] Les contextes de calcul sont vérifiés.
- [ ] Les totaux sont cohérents.
- [ ] Les périodes incomplètes sont signalées.
- [ ] Les données absentes sont gérées explicitement.
- [ ] Les seuils sont validés par le métier.
- [ ] Les variations ont une base de comparaison claire.

---

## 23.4 Accessibilité

- [ ] Contraste validé.
- [ ] Textes lisibles sur écran portable.
- [ ] Textes lisibles sur projecteur.
- [ ] Navigation clavier testée.
- [ ] Ordre de tabulation logique.
- [ ] Alt text renseigné sur les visuels essentiels.
- [ ] Informations critiques non transmises uniquement par couleur.
- [ ] Acronymes expliqués.
- [ ] Pas de surcharge cognitive excessive.
- [ ] Page d’aide ou méthodologie disponible.

---

## 23.5 Performance

- [ ] Temps d’ouverture acceptable.
- [ ] Interactions principales fluides.
- [ ] Nombre de visuels maîtrisé.
- [ ] Visuels redondants supprimés.
- [ ] Custom visuals justifiés et testés.
- [ ] Tables détaillées filtrées par défaut.
- [ ] Slicers haute cardinalité évités.
- [ ] Performance Analyzer exécuté sur pages critiques.
- [ ] Mesures lentes identifiées.
- [ ] Optimisations documentées.

---

## 23.6 Gouvernance

- [ ] Owner métier identifié.
- [ ] Owner technique identifié.
- [ ] Support contact disponible.
- [ ] Semantic model identifié.
- [ ] Documentation disponible.
- [ ] Changelog mis à jour.
- [ ] Niveau de confidentialité défini.
- [ ] RLS/OLS testés si applicables.
- [ ] Statut lifecycle défini.
- [ ] Plan de monitoring disponible.

---

## 24. Lignes rouges

Les pratiques suivantes sont interdites pour les rapports corporate :

- surcharger une page avec des dizaines de visuels non priorisés ;
- utiliser des jauges décoratives sans valeur analytique ;
- mélanger plusieurs définitions d’un même KPI ;
- masquer les hypothèses ou limites des données ;
- publier un rapport sans tests de lisibilité multi-profils ;
- utiliser une carte géographique sans question spatiale ;
- utiliser uniquement la couleur pour transmettre une alerte critique ;
- publier un rapport sans owner ;
- publier un rapport sans définition accessible des KPI critiques ;
- créer des pages qui reproduisent Excel sans valeur analytique ;
- ajouter des slicers sans utilité métier ;
- utiliser des visuels custom non validés ;
- afficher des tables massives sans filtre par défaut ;
- laisser des filtres cachés non documentés ;
- utiliser des titres vagues pour des KPI critiques ;
- publier un rapport lent sans analyse de performance ;
- conserver un rapport obsolète dans une app corporate ;
- exposer des données sensibles sans justification et sécurité ;
- présenter une période incomplète comme comparable à une période complète ;
- supprimer le contexte nécessaire à l’interprétation.

---

## 25. Astuces expertes et gains rapides

## 25.1 Titres qui concluent

Remplacer les titres descriptifs par des titres orientés insight.

| Avant | Après |
|---|---|
| Évolution du CA | Le CA baisse de 8 % vs N-1 depuis mars |
| Analyse marge | La marge recule surtout sur les produits A et B |
| Performance région | La région Nord concentre 60 % de l’écart au budget |

---

## 25.2 Repère visuel constant

Conserver le même emplacement pour :

- KPI principal ;
- filtres ;
- navigation ;
- date de mise à jour ;
- statut données ;
- définitions ;
- alertes.

Cela réduit l’effort cognitif et accélère la lecture.

---

## 25.3 Bookmarks intelligents

Utiliser des bookmarks pour proposer :

- vue Executive ;
- vue Manager ;
- vue Opérationnelle ;
- vue “écarts critiques” ;
- vue “budget vs réel” ;
- panneau filtres ;
- mode présentation.

---

## 25.4 Tooltip narratif

Un bon tooltip doit expliquer :

- la valeur ;
- la variation ;
- la référence ;
- la cause probable ;
- le périmètre ;
- la limite éventuelle.

---

## 25.5 Progressive disclosure

Afficher :

1. le signal principal ;
2. l’explication synthétique ;
3. le détail au clic ;
4. la table opérationnelle en drillthrough.

---

## 25.6 Couleur avec modération

Recommandation :

- 1 couleur principale ;
- neutres pour contexte ;
- couleurs sémantiques uniquement pour statut ;
- pas plus de 5 à 7 couleurs catégorielles si possible ;
- privilégier le tri et le positionnement avant la couleur.

---

## 25.7 Microcopie orientée action

| Avant | Après |
|---|---|
| Filtre | Choisir la région à analyser |
| Détail | Voir les clients concernés |
| Drill | Comprendre la cause de l’écart |
| Reset | Réinitialiser les filtres |
| Info | Voir définition du KPI |

---

## 25.8 Page “Executive one-screen”

Pour les sponsors, créer une page qui répond à quatre questions :

```text
1. Sommes-nous dans la cible ?
2. Où sont les écarts ?
3. Pourquoi ?
4. Quelle décision est demandée ?
```

---

## 25.9 Tables d’exception

Plutôt qu’une table exhaustive, créer des tables filtrées :

- clients en risque ;
- produits sous seuil ;
- tickets hors SLA ;
- régions en alerte ;
- commandes bloquées ;
- anomalies qualité.

---

## 25.10 Notes de confiance

Ajouter un statut discret mais visible :

```text
Données mises à jour le 12/06/2026 à 07:00 CET.
Périmètre: ventes validées hors annulations.
Qualité: 98.7 % des lignes conformes.
```

---

## 26. Mode opératoire standard

## 26.1 SOP de création d’un rapport

1. Cadrer les décisions à supporter.
2. Identifier les personas.
3. Définir les questions métier.
4. Valider les KPI, seuils et références.
5. Identifier le semantic model.
6. Créer le decision brief.
7. Construire un wireframe.
8. Valider le wireframe avec le métier.
9. Créer le prototype Power BI.
10. Appliquer le thème corporate.
11. Construire les pages selon le parcours analytique.
12. Ajouter filtres, drillthrough et tooltips.
13. Ajouter définitions, glossaire et limites.
14. Tester les KPI.
15. Tester les filtres et interactions.
16. Tester l’accessibilité.
17. Tester la performance.
18. Réaliser UAT avec utilisateurs représentatifs.
19. Corriger les ambiguïtés.
20. Publier en workspace approprié.
21. Intégrer dans l’app si applicable.
22. Communiquer le lancement.
23. Former les utilisateurs.
24. Monitorer usage et feedback.
25. Planifier la revue périodique.

---

## 26.2 SOP de refonte d’un rapport existant

1. Analyser usage actuel.
2. Identifier pages peu utilisées.
3. Identifier KPI redondants ou ambigus.
4. Recueillir feedback utilisateurs.
5. Cartographier les décisions réellement supportées.
6. Supprimer ou fusionner les pages inutiles.
7. Recréer un parcours analytique.
8. Revoir les visuels selon les questions métier.
9. Optimiser performance.
10. Refaire UAT.
11. Documenter les changements.
12. Communiquer la nouvelle version.
13. Archiver l’ancienne version si nécessaire.

---

## 26.3 SOP de revue trimestrielle

1. Vérifier usage.
2. Vérifier performance.
3. Vérifier feedback.
4. Vérifier cohérence KPI.
5. Vérifier accessibilité.
6. Vérifier sécurité.
7. Vérifier liens et navigation.
8. Vérifier documentation.
9. Identifier dette UX.
10. Prioriser améliorations.
11. Mettre à jour changelog.
12. Confirmer maintien, refonte ou archivage.

---

## 27. Gouvernance documentaire

## 27.1 Révision du standard

Ce standard doit être revu :

- trimestriellement ;
- après incident majeur d’interprétation ;
- après évolution significative de Power BI ;
- après audit accessibilité ;
- après feedback utilisateur récurrent ;
- après introduction d’un nouveau design system ;
- après incident de performance important.

---

## 27.2 Registre des dérogations

```markdown
# Dérogation Design / UX

## Rapport concerné

Nom du rapport:

## Règle concernée

Règle du standard:

## Motif

Pourquoi la dérogation est demandée:

## Impact

- Impact compréhension:
- Impact accessibilité:
- Impact performance:
- Impact maintenance:

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

## 27.3 Incidents d’interprétation

Un incident d’interprétation doit être documenté si :

- un utilisateur prend une mauvaise décision à cause du rapport ;
- un KPI est compris différemment par plusieurs équipes ;
- une couleur ou un seuil induit en erreur ;
- une période incomplète est mal comparée ;
- une donnée absente est interprétée comme zéro ;
- un filtre caché modifie une décision ;
- un visuel donne une impression trompeuse.

### Template

```markdown
# Incident d’interprétation

## Rapport

Nom du rapport:

## Date

Date:

## Description

Que s’est-il passé ?

## Cause probable

- Titre ambigu
- KPI mal défini
- Filtre non visible
- Couleur trompeuse
- Période incomplète
- Autre:

## Impact métier

Décision ou analyse affectée:

## Correction

Action immédiate:

## Prévention

Modification du standard ou du rapport:
```

---

## 28. Annexes

## 28.1 Template de page analytique

```text
Bloc 1 — Haut:
KPI principal, variation, statut, période, filtre principal.

Bloc 2 — Milieu:
Tendance, comparaison, décomposition des causes.

Bloc 3 — Bas:
Détail opérationnel, liste d’exceptions, plan d’action.

Bloc 4 — Aide:
Définitions KPI, périmètre, date de mise à jour, limites.
```

---

## 28.2 Template Executive Summary

```text
Titre:
Message principal du rapport.

KPI:
- KPI 1: valeur, variation, statut.
- KPI 2: valeur, variation, statut.
- KPI 3: valeur, variation, statut.

Alertes:
- Alerte 1.
- Alerte 2.

Explication:
Principaux contributeurs ou causes.

Décision attendue:
Action ou arbitrage demandé.

Contexte:
Période, périmètre, dernière mise à jour.
```

---

## 28.3 Template page “Comment lire ce rapport”

```markdown
# Comment lire ce rapport

## Objectif

Ce rapport permet de...

## Public cible

- Persona 1
- Persona 2

## Structure

- Page 1: Synthèse
- Page 2: Analyse par région
- Page 3: Analyse par produit
- Page 4: Détail opérationnel
- Page 5: Définitions et méthode

## KPI principaux

| KPI | Définition courte | Référence |
|---|---|---|
| Sales Amount | Chiffre d’affaires validé | vs N-1 / budget |
| Gross Margin % | Marge brute rapportée au CA | vs objectif |

## Couleurs

- Vert: conforme
- Orange: à surveiller
- Rouge: action requise
- Gris: contexte ou non applicable

## Filtres

- Période
- Région
- Produit
- Segment client

## Limites

- Données du mois courant partielles.
- Les ventes annulées sont exclues.

## Support

Contact:
```

---

## 28.4 Template de définition KPI dans un rapport

```markdown
# Définition KPI

## Nom

Gross Margin %

## Définition métier

Part du chiffre d’affaires conservée après déduction des coûts directs.

## Formule logique

Gross Margin / Sales Amount

## Périmètre

- Inclut ventes validées
- Exclut commandes annulées
- Hors taxes si Sales Amount est hors taxes

## Base de comparaison

- N-1
- Budget
- Objectif mensuel

## Fréquence de mise à jour

Quotidienne

## Owner

Finance Controlling

## Limites

Les coûts indirects ne sont pas inclus.
```

---

## 28.5 Template checklist UAT

```markdown
# UAT Report Checklist

## Informations

Rapport:
Version:
Date:
Testeur:
Persona:

## Compréhension

- [ ] Je comprends l’objectif du rapport.
- [ ] Je comprends le KPI principal.
- [ ] Je comprends la période affichée.
- [ ] Je comprends les couleurs et statuts.
- [ ] Je trouve les définitions.

## Navigation

- [ ] Je sais revenir à l’accueil.
- [ ] Je trouve les pages principales.
- [ ] J’utilise les filtres sans confusion.
- [ ] Le drillthrough est clair.
- [ ] Les tooltips sont utiles.

## Exactitude

- [ ] Les KPI semblent corrects.
- [ ] Les totaux sont cohérents.
- [ ] Les filtres produisent le résultat attendu.
- [ ] Les variations sont compréhensibles.

## Performance

- [ ] Les pages s’ouvrent rapidement.
- [ ] Les filtres répondent correctement.
- [ ] Aucun visuel critique ne reste bloqué.

## Commentaires

Points positifs:
Points à corriger:
Décision UAT:
```

---

## 28.6 Template de rapport d’usage

```markdown
# Usage Review — Power BI Report

## Rapport

Nom:

## Période analysée

Du:
Au:

## Indicateurs d’usage

| Indicateur | Valeur | Commentaire |
|---|---:|---|
| Utilisateurs actifs |  |  |
| Vues totales |  |  |
| Pages les plus vues |  |  |
| Pages peu consultées |  |  |
| Exports |  |  |
| Tickets support |  |  |

## Analyse

Ce qui fonctionne:
Ce qui ne fonctionne pas:
Risques:
Actions recommandées:

## Décision

- Maintenir
- Optimiser
- Refonte partielle
- Déprécier
- Archiver
```

---

## 28.7 Confidentialité

Certaines données, exemples, noms de pages, captures, métriques ou structures peuvent être anonymisés, pseudonymisés ou légèrement modifiés afin de protéger les informations internes et sensibles.

Les exemples fournis dans ce standard sont illustratifs. Ils doivent être adaptés au contexte réel, aux règles de gouvernance, aux politiques de sécurité, au design system et aux besoins métier de l’organisation.

---

## 28.8 Références recommandées

- Microsoft Learn — Design Power BI reports for accessibility.
- Microsoft Learn — Power BI optimization guidance.
- Microsoft Learn — Tips for designing Power BI dashboards.
- Microsoft Learn — Keyboard shortcuts and accessibility tools in Power BI.
- WCAG — Web Content Accessibility Guidelines.
- IBCS — International Business Communication Standards, si adopté par l’organisation.
- Documentation interne — Design system corporate.
- Documentation interne — Glossaire KPI et Data Governance.

---

## 28.9 Résumé exécutif du standard

Un rapport Power BI corporate doit être :

- orienté décision ;
- structuré par persona ;
- clair en moins de 30 secondes ;
- visuellement cohérent ;
- sémantiquement fiable ;
- accessible ;
- performant ;
- actionnable ;
- documenté ;
- gouverné ;
- monitoré ;
- maintenable.

La qualité d’un rapport Power BI ne se mesure pas au nombre de visuels affichés, mais à sa capacité à guider un utilisateur vers une décision fiable, rapide et contextualisée.
