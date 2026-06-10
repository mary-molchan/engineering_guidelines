# Standard Corporate Power BI - Modelisation des Donnees

## 1. En-tete du document

- **Titre**: Standard Corporate - Modelisation des donnees dans Power BI
- **Version**: 1.0
- **Date**: 2026-06-10
- **Auteur / Role**: Equipe BI & Data Governance
- **Validation / Approbation**: Responsable BI, Data Architect, Lead Analytics
- **Public cible**: BI Developers, Data Analysts, Analytics Engineers
- **Commanditaire**: Direction Data & Performance / Direction Metier
- **Statut**: Applicable a l'echelle de l'entreprise
- **Perimetre**: Tous les semantic models (datasets) Power BI
- **Objectif du document**: Definir les regles de modelisation pour garantir coherence metier, performance, maintenabilite et securite des modeles Power BI.

## 1.1 Cadre editorial et niveau de norme

- **Niveau normatif**: Ce document est prescriptif. Les exigences sont applicables par defaut a tout semantic model Power BI.
- **Interpretation des termes**:
	- **MUST**: exigence obligatoire, non negociable hors derogation formelle.
	- **SHOULD**: exigence fortement recommandee; toute exception doit etre justifiee et documentee.
	- **MAY**: option autorisee selon le contexte, sans contradiction avec les exigences MUST/SHOULD.
- **Mode de redaction**: formulations courtes, orientees gouvernance, performance et robustesse semantique.
- **Gestion des exceptions**: toute derogation doit inclure un motif, une duree de validite, un plan de remediation et un approbateur nomme.
- **Precedence**: en cas de conflit, ce standard s'applique apres les obligations legales/reglementaires et avant les pratiques locales d'equipe.

## 2. Vision, principes directeurs et resultats attendus

### 2.1 Vision
Construire des modeles analytiques standardises, performants et comprensibles, capables de servir des usages self-service et corporate sans divergence semantique.

### 2.2 Principes directeurs
1. **Star Schema First**: modelisation en schema en etoile comme standard par defaut.
2. **Mesures avant colonnes calculees**: privilegier DAX en mesures reutilisables.
3. **Semantique centralisee**: definitions KPI et logique de calcul unifiees.
4. **Simplicite visible, complexite maitrisable**: modele clair pour l'utilisateur final.
5. **Performance by Design**: cardinalite, relations, filtrage et agregations optimises.
6. **Securite appliquee au modele**: RLS/OLS selon les exigences de gouvernance.

### 2.3 Resultats attendus
- Reduire les ecarts de KPI entre rapports.
- Diminuer les temps de reponse et la charge capacitaire.
- Faciliter la maintenance et l'onboarding des equipes.
- Industrialiser la reutilisation des dimensions et mesures.

## 3. Roles et responsabilites (RACI simplifie)

| Activite | BI Developer | Data Architect | Data Steward | Metier (PO) | Admin Power BI |
|---|---|---|---|---|---|
| Design schema (facts/dimensions) | R | A | C | C | I |
| Definitions KPI DAX | R | C | C | A/R | I |
| Validation fonctionnelle | C | I | C | A/R | I |
| Securite modele (RLS/OLS) | C | C | C | I | A/R |
| Certification dataset | C | C | A | C | R |
| Documentation modele | R | C | A | C | I |

## 4. Standard de modelisation (regles obligatoires)

### 4.1 Architecture de reference
- Schema en etoile obligatoire sauf exception justifiee.
- Tables de faits separees des dimensions.
- Dimensions conformes partagees entre domaines quand possible.
- Eviter les flocons complexes non necessaires.

### 4.2 Relations
- Relations 1-* privilegiees de dimension vers faits.
- Sens de filtrage simple par defaut (single direction).
- Bidirectionnel uniquement sur cas justifies/documentes.
- Relations inactives autorisees avec activation explicite en DAX (`USERELATIONSHIP`).

### 4.3 Cles et cardinalite
- Utiliser des cles substituts numeriques pour dimensions.
- Eviter les cles texte longues dans les relations.
- Maitriser haute cardinalite (optimiser colonnes distinctes).

### 4.4 Gestion du temps
- Table calendrier enterprise obligatoire et marquee comme Date Table.
- Colonnes minimales: Date, Annee, Mois, Trimestre, Semaine, Fiscal.
- Interdire les time intelligence implicites non controles.

### 4.5 Mesures DAX
- Toute metrique metier dans des mesures, pas dans les visuels.
- Convention de nommage stable (prefixes metier/KPI).
- Commentaires DAX pour calculs complexes.
- Reutiliser des mesures atomiques pour composer des KPI.

### 4.6 Colonnes calculees
- Limiter les colonnes calculees aux besoins stricts.
- Preferer calcul en amont (ETL) si volumetrie importante.
- Evaluer impact memoire avant ajout massif.

### 4.7 Hierarchies et ergonomie semantic model
- Creer hierarchies naturelles (Geo, Produit, Temps).
- Cacher colonnes techniques non utiles aux analysts.
- Organiser les mesures par dossiers d'affichage.

### 4.8 Aggregations et composite models
- Definir agregations pour accelerer requetes volumineuses.
- Choisir Import, DirectQuery, Composite selon SLA/perimetre.
- Documenter arbitrages latence vs flexibilite.

### 4.9 Incremental Refresh et partitionnement
- Configurer parametres RangeStart/RangeEnd sur tables de faits.
- Aligner politique de partitions avec frequence d'usage.
- Tester comportement de reprocessing sur donnees tardives.

### 4.10 Securite du modele
- RLS par role metier (territoire, business unit, entite).
- OLS pour masquer colonnes sensibles.
- Tester roles avec jeux de comptes representatifs.

## 5. Checklist Qualite avant publication du semantic model

### 5.1 Coherence semantique
- [ ] KPI critiques valides par le metier.
- [ ] Definitions alignees sur glossaire corporate.

### 5.2 Integrite du schema
- [ ] Relations sans ambiguite.
- [ ] Aucune relation inutile ou inactive non documentee.
- [ ] Dimensions conformes reutilisees quand disponibles.

### 5.3 Performance
- [ ] Taille modele maitrisee.
- [ ] Requetes frequentes sous seuil cible.
- [ ] Mesures critiques profilees (DAX performant).

### 5.4 Securite
- [ ] RLS/OLS testes et valides.
- [ ] Absence de fuite de donnees sensibles via dimensions.

### 5.5 Maintenabilite
- [ ] Naming conventions respectees.
- [ ] Documentation des mesures complexes fournie.
- [ ] Ownership du modele explicitement defini.

## 6. Bonnes pratiques d'implementation (DAX, Tabular, gouvernance)

### 6.1 Design DAX evolutif
- Construire une couche de mesures de base (`[Montant]`, `[Volume]`) puis derivees (`[Croissance %]`, `[YTD]`).
- Eviter duplication de logique entre mesures voisines.

### 6.2 Optimisation DAX
- Limiter iterateurs couteux sur larges tables (`SUMX`, `FILTER` non necessaires).
- Preferer filtres simples et colonnes de faible cardinalite.
- Utiliser variables `VAR` pour lisibilite et performance.

### 6.3 Reduction de la taille du modele
- Supprimer colonnes non exploitees.
- Baisser precision numerique lorsque compatible metier.
- Remplacer champs texte repetitifs par dimensions dediees.

### 6.4 Reutilisation inter-rapports
- Promouvoir les datasets certifies et partages.
- Limiter proliferation de modeles quasi-identiques.

### 6.5 CI/CD et gouvernance
- Versionner definitions Tabular (TMDL/metadata scripts).
- Mettre en place revues de modelisation obligatoires.
- Integrer tests DAX/KPI dans pipeline de deploiement.

## 7. Lignes rouges (interdits)

- Construire un modele en table unique pour des besoins analytiques multi-axes.
- Laisser des mesures implicites pour des KPI strategiques.
- Multiplier des relations bidirectionnelles sans justification.
- Publier un dataset sans table calendrier standard.
- Ignorer les impacts capacitaires des calculs DAX complexes.

## 8. Astuces expertes et gains rapides (lifehacks)

1. **Table de mesures dediee**: centralise la logique KPI et simplifie l'experience utilisateur.
2. **Branches de mesures**: creer des mesures basees sur d'autres mesures pour limiter la dette DAX.
3. **Dimensions degenerate controlees**: garder certaines cles transactionnelles seulement si besoin drillthrough.
4. **Format strings dynamiques**: adapter unites (K, M) sans multiplier les mesures.
5. **Calculation groups (si gouvernance mature)**: harmoniser time intelligence a grande echelle.
6. **Tests de regressions KPI**: comparer ancien/nouveau modele automatiquement avant promotion.
7. **Mapping de securite externalise**: table de droits maintainable plutot que logique hardcodee.

## 9. Mode operatoire standard (SOP)

1. Cadrage des besoins analytiques et KPI.
2. Design schema cible (facts, dimensions, calendrier).
3. Construction relations et conventions de nommage.
4. Implementation mesures DAX (base puis avancees).
5. Tests fonctionnels, performance et securite.
6. Documentation et certification du dataset.
7. Publication et monitoring d'usage.

## 10. Gouvernance documentaire

- Revision trimestrielle de ce standard.
- Toute derogation doit etre approuvee et tracee.
- Les incidents majeurs doivent alimenter une mise a jour des regles.

## 11. Annexes

### 11.1 Exemple de structure logique

```text
Dimensions:
- DimDate
- DimCustomer
- DimProduct
- DimGeography

Facts:
- FactSales
- FactInventory

Mesures:
- [Sales Amount]
- [Sales Amount YTD]
- [Gross Margin %]
```

### 11.2 Confidentialite
Certaines donnees, exemples et captures peuvent etre anonymises ou legerement modifies afin de proteger les informations internes et sensibles.
