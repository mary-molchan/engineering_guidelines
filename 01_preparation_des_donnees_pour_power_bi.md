# Standard Corporate Power BI - Preparation des Donnees comme Source Optimale

## 1. En-tete du document

- **Titre**: Standard Corporate - Preparation des donnees pour une exploitation optimale dans Power BI
- **Version**: 1.0
- **Date**: 2026-06-10
- **Auteur / Role**: Equipe BI & Data Governance
- **Validation / Approbation**: Responsable BI, Data Architect, RSSI (si applicable)
- **Public cible**: Data Engineers, BI Developers, Data Analysts, Product Owners Data
- **Commanditaire**: Direction Data & Performance / Direction Metier
- **Statut**: Applicable a l'echelle de l'entreprise
- **Perimetre**: Tous les jeux de donnees utilises dans Power BI (Import, DirectQuery, Direct Lake, Dataflows)
- **Objectif du document**: Definir les regles, standards, controles qualite et meilleures pratiques pour preparer des donnees robustes, coherentes, maintenables et performantes pour Power BI.

## 1.1 Cadre editorial et niveau de norme

- **Niveau normatif**: Ce document est prescriptif. Les exigences sont applicables par defaut a tout projet BI.
- **Interpretation des termes**:
  - **MUST**: exigence obligatoire, non negociable hors derogation formelle.
  - **SHOULD**: exigence fortement recommandee; toute exception doit etre justifiee et documentee.
  - **MAY**: option autorisee selon le contexte, sans contradiction avec les exigences MUST/SHOULD.
- **Mode de redaction**: formulations courtes, orientees controle, audit et execution.
- **Gestion des exceptions**: toute derogation doit inclure un motif, une duree de validite, un plan de remediation et un approbateur nomme.
- **Precedence**: en cas de conflit, ce standard s'applique apres les obligations legales/reglementaires et avant les pratiques locales d'equipe.

## 2. Vision, principes directeurs et resultats attendus

### 2.1 Vision
Produire des donnees "BI-ready" des la source, avec une logique metier explicite, une qualite verifiable et une structure compatible avec les contraintes de modelisation et de performance Power BI.

### 2.2 Principes directeurs
1. **Data Quality by Design**: la qualite se construit en amont, pas uniquement dans les rapports.
2. **Single Source of Truth**: une definition metier unique pour chaque indicateur critique.
3. **Minimiser la transformation tardive**: privilegier l'ETL/ELT centralise a la logique locale dans les rapports.
4. **Traçabilite et auditabilite**: chaque champ critique doit etre documente et justifiable.
5. **Securite et confidentialite natives**: minimisation des donnees sensibles, masquage, gouvernance d'acces.
6. **Performance pragmatique**: optimiser la granularite, les cardinalites et la fraicheur selon l'usage.

### 2.3 Resultats attendus
- Reduction des anomalies de reporting.
- Diminution du temps de preparation des datasets.
- Acceleration des refresh et baisse des echecs de chargement.
- Homogeneite des definitions KPI inter-equipes.

## 3. Roles et responsabilites (RACI simplifie)

| Activite | Data Engineer | BI Developer | Data Steward | Metier (PO) | Architecte Data |
|---|---|---|---|---|---|
| Contrat de donnees (schema, SLA) | R | C | A | C | C |
| Regles de qualite (DQ) | R | C | A | C | C |
| Definitions metier | C | C | C | A/R | C |
| Publication des sources certifiees | R | C | A | I | C |
| Documentation / glossaire | C | C | A/R | C | I |
| Revue securite & conformite | C | I | C | I | A/R |

- **R**: Responsable execution
- **A**: Accountable (validation finale)
- **C**: Consulted
- **I**: Informed

## 4. Standard de preparation des donnees (regles obligatoires)

### 4.1 Contrat de donnees obligatoire
Chaque source doit disposer d'un "data contract" versionne:
- Nom fonctionnel et technique du dataset.
- Proprietaire metier et proprietaire technique.
- Schema attendu (colonnes, types, nullabilite, contraintes).
- Frequence de mise a jour et SLA de disponibilite.
- Politique de retention et historisation.
- Regles de securite et classification des donnees.

**Best practice**: stocker ce contrat dans un repository Git versionne et relie aux pipelines.

### 4.2 Nommage et conventions
- Noms explicites, stables, sans abreviations obscures.
- Eviter les espaces et caracteres speciaux au niveau technique.
- Uniformiser les suffixes: `_id`, `_code`, `_date`, `_amount`, `_flag`.
- Normaliser la casse (snake_case ou PascalCase selon standard entreprise).

**Anti-pattern**: renommer frequemment des colonnes critiques sans gestion d'impact.

### 4.3 Typage des donnees
- Interdire les types ambigus (texte pour dates/montants).
- Definir explicitement les types numeriques (entier, decimal, devise).
- Harmoniser fuseaux horaires et format des timestamps.
- Separer date et heure si les usages analytiques l'exigent.

**Power BI tip**: un type incorrect force des conversions couteuses en refresh et degrade la compression VertiPaq.

### 4.4 Gestion des valeurs manquantes et invalides
- Distinguer `NULL` technique, `inconnu`, `non applicable`, `non renseigne`.
- Creer des regles de remplacement traceables (jamais implicites).
- Isoler les rejets de qualite dans une table de quarantaine.
- Documenter le taux de nulles acceptable par colonne critique.

### 4.5 Dedoublonnage et unicite
- Definir les cles metier et cles techniques.
- Mesurer et monitorer le taux de duplication.
- Appliquer des regles de survivorship (source prioritaire, date la plus recente, etc.).
- Garantir l'unicite des dimensions de reference.

### 4.6 Harmonisation semantique
- Unifier les unites (EUR vs K EUR, kg vs g, etc.).
- Standardiser les referentiels (pays, devise, canaux, segments).
- Mapper les valeurs libres vers des listes de valeurs controlees.

### 4.7 Historisation et temporalite
- Preferer des modeles historises (SCD si pertinent) pour les dimensions evolutives.
- Inclure `valid_from`, `valid_to`, `is_current` si historique requis.
- Distinguer date de fait, date de saisie, date de traitement.

### 4.8 Granularite
- Definir la granularite cible selon les KPI et usages.
- Eviter de charger une granularite inutilement fine pour les usages executives.
- Pre-aggregations autorisees si la perte d'analyse est acceptable et documentee.

### 4.9 Securite, conformite, privacy
- Classifier chaque colonne (public, interne, sensible, critique).
- Limiter l'exposition des PII; appliquer pseudonymisation/masquage.
- Respecter les contraintes RGPD/locales (minimisation, finalite, retention).
- Journaliser les transformations affectant des donnees sensibles.

### 4.10 Refresh, incrementalite et fiabilite
- Definir des fenetres de refresh compatibles avec les SLA metier.
- Implementer l'incremental load (date/ID monotone).
- Prevoir idempotence et reprise sur incident.
- Alerter sur retard de flux, schema drift, volume anormal.

## 5. Checklist Qualite avant exposition a Power BI

### 5.1 Controle schema
- [ ] Colonnes conformes au contrat (nom, type, nullabilite).
- [ ] Aucune derive de schema non approuvee.

### 5.2 Controle contenu
- [ ] Taux de nulles sous seuil.
- [ ] Integrite referentielle validee.
- [ ] Duplicats critiques elimines ou traces.
- [ ] Bornes de valeurs metier respectees.

### 5.3 Controle temporalite
- [ ] Horodatage coherent (timezone, fraicheur).
- [ ] Historique correctement gere.

### 5.4 Controle performance
- [ ] Volumetrie sous controle.
- [ ] Partitionnement/filtrage prets pour incremental refresh.

### 5.5 Controle documentation
- [ ] Dictionnaire de donnees a jour.
- [ ] Definitions KPI associees et validees.
- [ ] Protocole de support et escalation defini.

## 6. Bonnes pratiques d'implementation (Power Query, Dataflows, Lakehouse, SQL)

### 6.1 Positionnement des transformations
- Prioriser transformations lourdes dans la couche data (SQL/ETL/Lakehouse) plutot que dans chaque rapport.
- Conserver Power Query pour transformations legeres, proches de la consommation.

### 6.2 Query Folding
- Favoriser les etapes pliables vers la source.
- Eviter operations qui cassent le folding trop tot.
- Verifier systematiquement le folding sur les flux critiques.

### 6.3 Reutilisabilite
- Centraliser les logiques communes dans Dataflows Gen2 / pipelines partages.
- Eviter la duplication de scripts M entre rapports.

### 6.4 Tests de non-regression
- Comparer volumes, distinct counts, totaux KPI avant/apres changement.
- Mettre en place un jeu de tests automatisables (schema + contenu + perf).

### 6.5 Observabilite
- Mesurer: duree refresh, lignes traitees, erreurs par etape, taux de rejet.
- Publier des tableaux de bord d'operabilite BI.

## 7. Lignes rouges (interdits)

- Construire des KPI critiques uniquement dans des visuels sans definition centralisee.
- Utiliser des colonnes texte comme dates ou montants.
- Exposer directement des champs PII non necessaires.
- Multiplier des copies locales de la meme logique de transformation.
- Ignorer les derives de schema en production.

## 8. Astuces expertes et gains rapides (lifehacks)

1. **Profilage automatique quotidien**: generer un rapport DQ (nulles, outliers, cardinalites) pour detecter les regressions avant les utilisateurs.
2. **Tables de reference certifiees**: conserver devises, calendriers, org units dans une couche "gold" versionnee.
3. **Surrogate keys numeriques**: ameliorent compression et joins dans Power BI.
4. **Flag de qualite par ligne**: simplifie exclusion/inclusion dynamique des enregistrements litigieux.
5. **Echantillons representatifs**: accelerent dev/test sans charger la totalite du volume.
6. **Contract tests en CI/CD**: bloquer deploiement si schema critique casse.
7. **Colonnes techniques cachees**: garder trace lineage sans polluer l'experience analyste.

## 9. Mode operatoire standard (SOP)

1. Cadrage metier et inventaire des sources.
2. Redaction/validation du data contract.
3. Profilage initial et definition des regles DQ.
4. Construction pipeline de preparation + controles.
5. Validation metier (jeu de recettes KPI).
6. Publication source certifiee pour Power BI.
7. Monitoring continue et revues periodiques.

## 10. Gouvernance documentaire

- Revision trimestrielle de ce standard.
- Toute derogation doit etre approuvee et tracee.
- Les incidents majeurs doivent nourrir une mise a jour du standard.

## 11. Annexes

### 11.1 Template minimal de data contract

```yaml
name: ventes_fact
owner_business: Direction Commerciale
owner_technical: Data Platform Team
refresh_sla: "J+0 07:00 CET"
granularity: "1 ligne = 1 transaction"
keys:
  primary: transaction_id
  foreign:
    - customer_id
    - product_id
schema:
  - name: transaction_id
    type: bigint
    nullable: false
  - name: transaction_date
    type: datetime
    nullable: false
  - name: amount_eur
    type: decimal(18,2)
    nullable: false
quality_rules:
  - "amount_eur >= 0"
  - "transaction_date <= now()"
security:
  classification:
    customer_email: sensitive
```

### 11.2 Confidentialite
Certaines donnees, exemples et captures peuvent etre anonymises ou legerement modifies afin de proteger les informations internes et sensibles.
