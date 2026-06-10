# Standard Corporate - Snowflake SQL

## 1. En-tete du document

- **Titre**: Standard Corporate - Regles et meilleures pratiques Snowflake SQL
- **Version**: 1.0
- **Date**: 2026-06-10
- **Auteur / Role**: Equipe Data Engineering & Analytics Engineering
- **Validation / Approbation**: Lead Data Engineer, Data Architect, Head of BI
- **Public cible**: Data Engineers, Analytics Engineers, BI Developers, Data Analysts avances
- **Commanditaire**: Direction Data & Performance / DSI
- **Statut**: Applicable a l'echelle de l'entreprise
- **Perimetre**: Scripts SQL Snowflake (ingestion, transformation, data marts, quality checks)
- **Objectif du document**: Definir les standards d'ecriture SQL Snowflake pour assurer lisibilite, performance, fiabilite, testabilite et maintenabilite.

## 1.1 Cadre editorial et niveau de norme

- **Niveau normatif**: Ce document est prescriptif. Les exigences sont applicables par defaut a tout code SQL Snowflake en entreprise.
- **Interpretation des termes**:
  - **MUST**: exigence obligatoire, non negociable hors derogation formelle.
  - **SHOULD**: exigence fortement recommandee; toute exception doit etre justifiee et documentee.
  - **MAY**: option autorisee selon le contexte, sans contradiction avec les exigences MUST/SHOULD.
- **Mode de redaction**: formulations courtes, orientees qualite de code, performance et auditabilite.
- **Gestion des exceptions**: toute derogation doit inclure un motif, une duree de validite, un plan de remediation et un approbateur nomme.
- **Precedence**: en cas de conflit, ce standard s'applique apres les obligations legales/reglementaires et avant les pratiques locales d'equipe.

## 2. Vision, principes directeurs et resultats attendus

### 2.1 Vision
Produire un SQL Snowflake robuste et standardise, facilement relisible par les equipes, performant a grande echelle et verifiable en CI/CD.

### 2.2 Principes directeurs
1. **Readable SQL is maintainable SQL**.
2. **Performance by intent**: optimiser en fonction des patterns d'acces reels.
3. **Determinisme**: resultat stable, logique explicite, pas d'ambiguite implicite.
4. **Test-first mindset**: controles de qualite couples au code.
5. **Small, composable steps**: transformations atomiques plutot que mega-queries opaques.
6. **Governed evolution**: changements versionnes, revus, et deployes via pipeline.

### 2.3 Resultats attendus
- Baisse des regressions en production.
- Amelioration des temps d'execution des requetes critiques.
- Relecture plus rapide en code review.
- Standard commun pour toutes les squads data.

## 3. Roles et responsabilites (RACI simplifie)

| Activite | Data Engineer | Analytics Engineer | Reviewer Tech Lead | Data Steward | Metier (PO) |
|---|---|---|---|---|---|
| Ecriture SQL transformation | A/R | R | C | C | I |
| Revue qualite/performance | C | C | A/R | I | I |
| Tests data quality | R | A/R | C | C | I |
| Validation semantique metier | C | C | I | C | A/R |
| Publication et monitoring | R | C | A | I | I |

## 4. Standard Snowflake SQL (regles obligatoires)

### 4.1 Style et formatage
- MUST utiliser un style SQL uniforme (mots-cles en majuscules, indentation coherente).
- MUST aliaser explicitement les tables.
- MUST commenter la logique non triviale (raison metier, hypothese, risque).
- SHOULD conserver une requete lisible en sections: source, transformation, output.

### 4.2 Determinisme et exactitude
- MUST enumerer explicitement les colonnes (interdit `SELECT *` en production).
- MUST preciser le fuseau/traitement temporel pour les calculs de dates.
- MUST eviter les cast implicites ambigus.
- SHOULD expliciter l'ordre logique dans les fenetres analytiques.

### 4.3 CTE, modularite et reutilisation
- MUST structurer les requetes complexes en CTE nommes semantiquement.
- SHOULD limiter la profondeur excessive des CTE imbriques.
- MAY materialiser des etapes intermediaires si gain de performance/verifiabilite.

### 4.4 Joins et cardinalite
- MUST definir la cle de jointure de facon explicite et documentee.
- MUST verifier la cardinalite attendue (1-1, 1-N, N-N) avant publication.
- MUST prevenir les duplications silencieuses via tests.
- SHOULD filtrer au plus tot le volume inutile.

### 4.5 Fenetres analytiques
- MUST preciser `PARTITION BY` et `ORDER BY` selon la logique metier.
- MUST traiter les ex-aequo de maniere explicite (`ROW_NUMBER` vs `RANK` vs `DENSE_RANK`).
- SHOULD eviter fenetres couteuses sur colonnes de tres haute cardinalite sans justification.

### 4.6 Performance Snowflake
- MUST optimiser predicate pushdown et pruning (filtres precis, colonnes utiles).
- MUST eviter fonctions sur colonnes dans predicates critiques lorsque possible.
- SHOULD favoriser clustering key seulement si benefice mesure et durable.
- SHOULD analyser plans d'execution des requetes les plus couteuses.

### 4.7 Incrementalite et idempotence
- MUST ecrire transformations rejouables sans effets de bord.
- MUST definir strategie merge/upsert deterministe.
- MUST tracer watermark/date de traitement et volume impacte.

### 4.8 Gestion des NULL et qualite
- MUST distinguer NULL, valeur inconnue et non applicable.
- MUST documenter regles de substitution (`COALESCE`) et leur impact metier.
- SHOULD exposer des indicateurs de qualite (taux nulles, rejets, anomalies).

### 4.9 DDL, DML et transactions
- MUST separer scripts de creation schema et scripts de chargement.
- MUST rendre les DDL evolutifs backward-compatible autant que possible.
- SHOULD encapsuler operations critiques avec controles avant/apres.

### 4.10 Securite et conformite dans le SQL
- MUST exclure les colonnes sensibles non necessaires des outputs.
- MUST appliquer les vues/policies de securite prevues.
- MUST eviter hardcode d'identifiants sensibles dans le code.

## 5. Checklist Qualite avant merge/deploiement

### 5.1 Qualite du code
- [ ] Style SQL respecte.
- [ ] Noms CTE et aliases explicites.
- [ ] Absence de `SELECT *` en production.

### 5.2 Qualite des donnees
- [ ] Tests duplicats, nulles et bornes executes.
- [ ] Reconciliation volumes source/target validee.

### 5.3 Performance
- [ ] Requetes critiques profilees.
- [ ] Predicates et scans optimises.

### 5.4 Fiabilite
- [ ] Transformation idempotente et rejouable.
- [ ] Strategie de reprise documentee.

### 5.5 Securite
- [ ] Donnees sensibles traitees selon policy.
- [ ] Objets cibles et privileges conformes.

## 6. Bonnes pratiques d'implementation

### 6.1 Patterns recommandes
- Construire un modele en couches: staging -> intermediate -> mart.
- Centraliser les macros/patterns repetitifs de qualite.

### 6.2 Revue de code SQL
- Exiger checklist standard en pull request.
- Refuser merge si non-determinisme, ambiguite metier ou risque perf critique.

### 6.3 Tests automatises
- Integrer tests de schema, tests de contenu et tests de non-regression.
- Comparer KPI cle avant/apres changement.

### 6.4 Documentation vivante
- Lier chaque requete strategique a une definition metier.
- Maintenir catalogue des tables et lineage simplifie.

### 6.5 Observabilite
- Suivre top requetes couteuses, temps d'execution et taux d'echec.
- Mettre en place alertes sur degradation de performance.

## 7. Lignes rouges (interdits)

- Deployer du SQL non relu en production.
- Conserver `SELECT *` dans scripts de transformation critiques.
- Ignorer duplications issues de joins N-N.
- Ajouter logique metier implicite non documentee.
- Modifier schema critique sans plan de compatibilite.

## 8. Astuces expertes et gains rapides (lifehacks)

1. **Nommer les CTE comme des etapes de pipeline**: facilite debug et revue.
2. **Mesurer avant optimiser**: prioriser requetes a fort cout reel.
3. **Limiter colonnes tres tot**: baisse scan et cout compute.
4. **Tests de cardinalite en CI**: detectent regressions silencieuses.
5. **MERGE avec cle technique stable**: evite doublons tardifs.
6. **Commentaires d'intention**: documentent "pourquoi", pas juste "quoi".
7. **Refactoring incrementaux**: petits changements testables plutot que rework massif.

## 9. Mode operatoire standard (SOP)

1. Cadrage besoin metier et contrat de donnees.
2. Ecriture SQL en couches avec conventions standard.
3. Ajout des tests qualite et performance.
4. Code review technique + validation semantique.
5. Deploiement CI/CD controle.
6. Monitoring post-deploiement et correction rapide.
7. Retrospective et ameliorations continues.

## 10. Gouvernance documentaire

- Revision trimestrielle de ce standard.
- Toute derogation doit etre approuvee et tracee.
- Les incidents majeurs et regressions SQL alimentent une mise a jour obligatoire.

## 11. Annexes

### 11.1 Template SQL de base (exemple structure)

```sql
WITH src AS (
    SELECT
        order_id,
        customer_id,
        order_ts,
        amount
    FROM RAW.ORDERS
    WHERE order_ts >= :watermark
),
clean AS (
    SELECT
        order_id,
        customer_id,
        CONVERT_TIMEZONE('UTC', order_ts) AS order_ts_utc,
        amount
    FROM src
    WHERE amount >= 0
)
SELECT
    order_id,
    customer_id,
    order_ts_utc,
    amount
FROM clean;
```

### 11.2 Confidentialite
Certaines donnees, exemples et captures peuvent etre anonymises ou legerement modifies afin de proteger les informations internes et sensibles.
