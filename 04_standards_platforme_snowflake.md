# Standard Corporate - Plateforme Snowflake

## 1. En-tete du document

- **Titre**: Standard Corporate - Gouvernance et meilleures pratiques de la plateforme Snowflake
- **Version**: 1.0
- **Date**: 2026-06-10
- **Auteur / Role**: Equipe Data Platform & Data Governance
- **Validation / Approbation**: Head of Data Platform, Data Architect, RSSI
- **Public cible**: Data Engineers, Platform Engineers, Analytics Engineers, Admins Snowflake
- **Commanditaire**: Direction Data & Performance / DSI
- **Statut**: Applicable a l'echelle de l'entreprise
- **Perimetre**: Comptes Snowflake, environnements DEV/UAT/PROD, pipelines d'ingestion/transformations, partage de donnees
- **Objectif du document**: Definir les regles, standards et bonnes pratiques pour exploiter Snowflake de maniere securisee, performante, gouvernee et economiquement maitrisée.

## 1.1 Cadre editorial et niveau de norme

- **Niveau normatif**: Ce document est prescriptif. Les exigences sont applicables par defaut a tout usage de Snowflake.
- **Interpretation des termes**:
  - **MUST**: exigence obligatoire, non negociable hors derogation formelle.
  - **SHOULD**: exigence fortement recommandee; toute exception doit etre justifiee et documentee.
  - **MAY**: option autorisee selon le contexte, sans contradiction avec les exigences MUST/SHOULD.
- **Mode de redaction**: formulations courtes, orientees audit, securite, fiabilite et controle des couts.
- **Gestion des exceptions**: toute derogation doit inclure un motif, une duree de validite, un plan de remediation et un approbateur nomme.
- **Precedence**: en cas de conflit, ce standard s'applique apres les obligations legales/reglementaires et avant les pratiques locales d'equipe.

## 2. Vision, principes directeurs et resultats attendus

### 2.1 Vision
Operer Snowflake comme une plateforme data industrielle: fiable, securisee, scalable, observable et optimisee en cout total de possession.

### 2.2 Principes directeurs
1. **Security by Design**: securite des identites, des objets et des flux des la conception.
2. **Least Privilege**: droits minimaux, roles explicites, heredite controlable.
3. **Cost Awareness**: every credit counts; gouvernance proactive des couts compute et storage.
4. **Data Reliability**: pipelines idempotents, reprise sur incident, SLA mesurables.
5. **Environment Isolation**: separation stricte DEV/UAT/PROD et promotion controlee.
6. **Observability First**: metriques, logs, alertes et post-mortems standardises.

### 2.3 Resultats attendus
- Reduction des incidents d'acces et de conformite.
- Stabilite des chargements et transformations.
- Meilleure previsibilite des couts Snowflake.
- Acceleration des livraisons data avec moins de regressions.

## 3. Roles et responsabilites (RACI simplifie)

| Activite | Platform Engineer | Data Engineer | Data Steward | RSSI | Metier (PO) |
|---|---|---|---|---|---|
| Architecture comptes/roles | A/R | C | I | C | I |
| Provisionning entrepots et policies | A/R | C | I | C | I |
| Ingestion et orchestration pipelines | C | A/R | C | I | C |
| Gouvernance metadonnees & classification | C | C | A/R | C | C |
| Revue securite et conformite | C | I | C | A/R | I |
| Suivi couts et optimization credits | A/R | C | I | I | C |

## 4. Standard plateforme Snowflake (regles obligatoires)

### 4.1 Architecture des comptes et environnements
- MUST separer strictement DEV, UAT et PROD.
- MUST interdire toute ecriture cross-environment non approuvee.
- SHOULD aligner les conventions de nommage des objets entre environnements.

### 4.2 Gouvernance des identites et roles
- MUST appliquer RBAC de bout en bout; aucun acces direct ad hoc en PROD.
- MUST creer des roles metier, roles techniques et roles d'administration distincts.
- MUST utiliser des roles secondaires de maniere explicite et auditable.
- SHOULD documenter la matrice role -> privileges -> objets.

### 4.3 Securite des donnees
- MUST activer chiffrement au repos et en transit (standard plateforme).
- MUST classifier les donnees (public/interne/sensible/critique).
- MUST appliquer masking policies pour colonnes sensibles.
- MUST utiliser row access policies lorsque la segmentation d'acces est requise.
- SHOULD utiliser tag-based governance pour industrialiser les politiques.

### 4.4 Nommage et structuration des objets
- MUST definir conventions de nommage pour databases, schemas, tables, stages, pipes, tasks.
- MUST reserver un schema dedie pour objets temporaires/techniques.
- SHOULD separer schema brut (raw), intermediaire (curated), et metier (mart).

### 4.5 Entrepots (warehouses) et compute
- MUST dimensionner les warehouses selon profil de charge reel.
- MUST activer auto-suspend et auto-resume.
- MUST eviter warehouses partages pour charges incompatibles (ETL lourd vs BI interactive).
- SHOULD definir Resource Monitors avec seuils et alertes credits.
- MAY utiliser multi-cluster pour fortes concurrences, avec bornes explicites.

### 4.6 Ingestion et chargement
- MUST preferer pipelines idempotents (rejouables sans duplication).
- MUST valider schema drift et appliquer strategie de compatibilite.
- MUST historiser erreurs de chargement et lignes rejetees.
- SHOULD utiliser stages externes/internal avec gouvernance d'acces stricte.
- SHOULD fixer conventions de fichiers (partition, compression, encodage, checksum).

### 4.7 Time Travel, Fail-safe, retention
- MUST definir policies de retention par domaine de donnees.
- SHOULD adapter Time Travel selon criticite et cout.
- MUST documenter strategie de restauration et RTO/RPO.

### 4.8 Partage de donnees et consommation
- MUST valider juridiquement et securitairement tout data sharing externe.
- MUST isoler objets exposes pour consommation externe.
- SHOULD utiliser views contractuelles stables pour limiter rupture de schema.

### 4.9 Orchestration et planification
- MUST definir fenetres d'execution et dependances explicites.
- MUST gerer retries, alerting et dead-letter scenarios.
- SHOULD versionner la logique d'orchestration dans Git.

### 4.10 Observabilite, audit et conformite
- MUST activer audit logging et conservation des journaux.
- MUST mesurer durees jobs, volume traite, taux d'erreur, consommation credits.
- SHOULD publier un tableau de bord platform ops (SLA, incidents, couts).
- MUST produire post-mortem pour incident majeur.

## 5. Checklist Qualite avant mise en production

### 5.1 Securite
- [ ] Roles et privileges valides selon least privilege.
- [ ] Policies de masquage/acces appliquees et testees.

### 5.2 Fiabilite
- [ ] Pipelines rejouables et idempotents.
- [ ] Strategie de reprise documentee et testee.

### 5.3 Performance
- [ ] Warehouses calibres et auto-suspend actif.
- [ ] Requetes critiques benchmarkees.

### 5.4 Couts
- [ ] Resource Monitor configure.
- [ ] Estimation credits mensuelle validee.

### 5.5 Gouvernance
- [ ] Metadata, tags, owners et SLA documentes.
- [ ] Support runbook et escalation definis.

## 6. Bonnes pratiques d'implementation

### 6.1 Industrialisation
- Standardiser les templates de schemas, roles et policies.
- Automatiser le provisionning (IaC + CI/CD).

### 6.2 Performance plateforme
- Isoler workloads par warehouse et fenetre temporelle.
- Mettre en cache intelligemment via patterns de consommation repetitifs.

### 6.3 Maitrise des couts
- Surveiller top queries couteuses et optimizer prioritairement les 20% les plus impactantes.
- Deplacer traitements non critiques hors heures de pointe.

### 6.4 Gouvernance operationnelle
- Instaurer revues mensuelles securite/performance/couts.
- Exiger evidences d'audit lors des comites data.

### 6.5 Continuite de service
- Definir plan de contingence pour indisponibilite source/target.
- Documenter runbooks d'urgence par domaine.

## 7. Lignes rouges (interdits)

- Utiliser un role admin pour des usages quotidiens d'engineering.
- Executer des charges lourdes permanentes sans auto-suspend.
- Exposer des donnees sensibles sans masking/row policy.
- Deployer en PROD sans tests de charge ni validation securite.
- Ignorer des alertes de credits sur plusieurs cycles.

## 8. Astuces expertes et gains rapides (lifehacks)

1. **Conventions de tags des le jour 1**: facilite gouvernance et lineage.
2. **Warehouses specialises**: ETL, BI, Data Science separes pour stabilite et cout.
3. **Budget guardrails**: seuils credits + alertes Slack/Teams pour reaction rapide.
4. **Quarantine schema**: isoler donnees invalides sans bloquer toute la chaine.
5. **Playbooks incident**: reduit fortement le MTTR en production.
6. **Golden views contractuelles**: evitent cassures lors des evolutions backend.
7. **Revue trimestrielle des roles**: supprime privileges dormants et risques accumules.

## 9. Mode operatoire standard (SOP)

1. Cadrage besoins, SLA, classification des donnees.
2. Design roles, policies, schemas et warehouses.
3. Implementation pipelines + controles qualite + observabilite.
4. Tests securite, performance, couts et reprise.
5. Validation RACI et approbations.
6. Mise en production via pipeline CI/CD.
7. Suivi continu et optimisation trimestrielle.

## 10. Gouvernance documentaire

- Revision trimestrielle de ce standard.
- Toute derogation doit etre approuvee et tracee.
- Les incidents majeurs alimentent une mise a jour obligatoire.

## 11. Annexes

### 11.1 Template minimal de convention d'objet

```yaml
environment: PROD
database: SALES_PRD
schema_layers:
  - RAW
  - CURATED
  - MART
warehouse_classes:
  - ETL_WH
  - BI_WH
  - DS_WH
rbac_principles:
  - least_privilege
  - role_separation
monitoring:
  - credit_alert_50_80_95
  - job_failure_alert
```

### 11.2 Confidentialite
Certaines donnees, exemples et captures peuvent etre anonymises ou legerement modifies afin de proteger les informations internes et sensibles.
