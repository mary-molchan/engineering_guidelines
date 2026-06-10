# Standard Corporate Power BI - Analyse Visuelle et Data Storytelling

## 1. En-tete du document

- **Titre**: Standard Corporate - Analyse visuelle dans Power BI
- **Version**: 1.0
- **Date**: 2026-06-10
- **Auteur / Role**: Equipe BI & Data Governance
- **Validation / Approbation**: Responsable BI, UX Lead Analytics, Direction Metier
- **Public cible**: BI Developers, Data Analysts, Decision Makers, Product Owners
- **Commanditaire**: Direction Data & Performance / Direction Metier
- **Statut**: Applicable a l'echelle de l'entreprise
- **Perimetre**: Tous les rapports, dashboards et apps Power BI
- **Objectif du document**: Definir les regles de conception visuelle, d'analyse et de storytelling afin de produire des rapports clairs, actionnables, performants et accessibles.

## 1.1 Cadre editorial et niveau de norme

- **Niveau normatif**: Ce document est prescriptif. Les exigences sont applicables par defaut a tout rapport ou dashboard Power BI.
- **Interpretation des termes**:
	- **MUST**: exigence obligatoire, non negociable hors derogation formelle.
	- **SHOULD**: exigence fortement recommandee; toute exception doit etre justifiee et documentee.
	- **MAY**: option autorisee selon le contexte, sans contradiction avec les exigences MUST/SHOULD.
- **Mode de redaction**: formulations courtes, orientees decision, lisibilite et impact utilisateur.
- **Gestion des exceptions**: toute derogation doit inclure un motif, une duree de validite, un plan de remediation et un approbateur nomme.
- **Precedence**: en cas de conflit, ce standard s'applique apres les obligations legales/reglementaires et avant les pratiques locales d'equipe.

## 2. Vision, principes directeurs et resultats attendus

### 2.1 Vision
Transformer des donnees en decisions via des experiences de lecture visuelle coherentes, fiables et orientees action.

### 2.2 Principes directeurs
1. **Clarte avant densite**: chaque visuel doit repondre a une question metier explicite.
2. **Design system analytique**: cohérence de themes, couleurs, terminologie et interactions.
3. **Story first**: structurer les pages selon un fil narratif decisionnel.
4. **Actionnabilite**: chaque KPI doit mener a une interpretation ou decision possible.
5. **Accessibilite native**: lisibilite, contraste, navigation clavier, alternatives textuelles.
6. **Performance percue**: pages rapides, interactions fluides, temps de comprehension reduit.

### 2.3 Resultats attendus
- Adoption accrue des dashboards.
- Reduction des mauvaises interpretations des KPI.
- Diminution du temps de decision sur les revues de performance.
- Meilleure autonomie des utilisateurs metier.

## 3. Roles et responsabilites (RACI simplifie)

| Activite | BI Developer | UX/Data Viz Lead | Data Steward | Metier (PO) | Sponsor |
|---|---|---|---|---|---|
| Definition cas d'usage analytique | C | C | I | A/R | C |
| Design des pages et interactions | R | A/R | I | C | I |
| Validation semantique des KPI affiches | C | I | A | R | I |
| Tests utilisateurs / ergonomie | R | A | I | C | I |
| Validation executive finale | I | C | I | C | A/R |

## 4. Standard de conception visuelle (regles obligatoires)

### 4.1 Architecture d'un rapport
- Page d'accueil (contexte, definitions, navigation).
- Pages de pilotage (KPI executifs, tendances, alertes).
- Pages d'analyse detaillee (drill-down par axe).
- Page glossaire / methodologie / limites.

### 4.2 Hierarchie de l'information
- Zone haute: KPI prioritaires et signaux d'alerte.
- Zone centrale: explication causale (tendances, decomposition).
- Zone basse: detail operationnel et tableaux de verification.

### 4.3 Choix des visuels
- Courbes: evolution temporelle.
- Barres: comparaison de categories.
- Waterfall: contribution positive/negative.
- Scatter: correlation et segments.
- Cartes geographiques: uniquement si question spatiale legitime.
- Tableau detail: verification, export, audit.

**Interdit**: utiliser un visuel "par habitude" sans question analytique explicite.

### 4.4 Couleurs et semantique
- Palette corporate + palette analytique standardisee.
- Meme couleur = meme signification sur tout le rapport.
- Couleurs d'alerte reservees aux ecarts critiques.
- Verifier contraste et daltonisme (WCAG recommande).

### 4.5 Titres, labels, annotations
- Titres orientes insight (pas uniquement nom du visuel).
- Afficher unite, periode, filtre actif quand critique.
- Ajouter annotations pour ruptures majeures (promo, incident, changement de regle).

### 4.6 Interactions
- Slicers limites aux dimensions utiles.
- Drillthrough guide par questions metier.
- Tooltips enrichis mais concis.
- Eviter interactions croisees confuses sur toutes les visuals.

### 4.7 Gestion des KPI
- Definir cible, seuils, statut (vert/orange/rouge).
- Afficher variation absolue et relative quand pertinent.
- Clarifier base de comparaison (N-1, budget, forecast).

### 4.8 Accessibilite
- Taille de police lisible sur ecrans standard.
- Navigation clavier exploitable.
- Alt text sur visuels essentiels.
- Eviter surcharge de texte en capitales et rotations.

### 4.9 Localisation et internationalisation
- Formats dates/nombres/devise alignes au public.
- Terminologie metier locale, stable, non ambigue.

### 4.10 Performance UX
- Limiter nombre de visuels par page (qualite > quantite).
- Eviter visuals personnalisees lourdes sans valeur ajoutee.
- Optimiser mesures et interactions pour garder fluidite.

## 5. Checklist Qualite avant publication d'un rapport

### 5.1 Pertinence decisionnelle
- [ ] Chaque visuel repond a une question metier claire.
- [ ] Les KPI principaux sont interpretable en moins de 30 secondes.

### 5.2 Coherence visuelle
- [ ] Theme corporate applique.
- [ ] Couleurs, legendes, unites coherentes sur toutes les pages.
- [ ] Navigation intuitive (menus, boutons, retour).

### 5.3 Exactitude analytique
- [ ] KPI concordants avec semantic model certifie.
- [ ] Filtres et contextes de calcul verifies.

### 5.4 Accessibilite
- [ ] Contraste valide.
- [ ] Textes lisibles sur ecran portable et projecteur.
- [ ] Elements critiques accessibles au clavier.

### 5.5 Performance
- [ ] Temps d'ouverture acceptable.
- [ ] Interactions principales fluides.
- [ ] Absence de visuels redondants couteux.

## 6. Bonnes pratiques d'implementation (UX analytics, storytelling, adoption)

### 6.1 Storytelling analytique
- Structurer chaque page en "Question -> Constat -> Explication -> Action".
- Commencer par le signal cle, finir par un levier actionnable.

### 6.2 Pages orientees persona
- Executif: peu de KPI, forte synthese, alertes.
- Manager: decomposition par segments, ecarts vs objectifs.
- Operationnel: detail transactionnel, causes racines.

### 6.3 Design system Power BI
- Maintenir un fichier theme JSON corporate (couleurs, typo, formats).
- Standardiser composants: cartes KPI, bandeaux filtres, blocs d'insight.

### 6.4 Validation utilisateur
- Tests rapides avec utilisateurs reels (5 a 8 personnes).
- Observer comprehension sans explication prealable.
- Prioriser corrections sur ambiguite et surcharge cognitive.

### 6.5 Adoption et conduite du changement
- Ajouter page "Comment lire ce dashboard".
- Former les sponsors metier sur interpretation des KPI.
- Suivre telemetry d'usage pour amelioration continue.

## 7. Lignes rouges (interdits)

- Surcharger une page avec des dizaines de visuels non priorises.
- Utiliser des jauges decoratives sans valeur analytique.
- Melanger plusieurs definitions d'un meme KPI.
- Masquer les hypotheses ou limites des donnees.
- Publier un rapport sans tests de lisibilite multi-profils.

## 8. Astuces expertes et gains rapides (lifehacks)

1. **Titres qui concluent**: ecrire "Le CA baisse de 8% vs N-1" plutot que "Evolution du CA".
2. **Repere visuel constant**: fixer meme emplacement pour KPI strategiques sur toutes les pages.
3. **Bookmarks intelligents**: proposer vues preconfigurees par role.
4. **Tooltip narratif**: afficher cause probable + contexte, pas seulement la valeur brute.
5. **Progressive disclosure**: montrer synthese d'abord, details au clic.
6. **Couleur avec moderation**: 1 couleur d'accent principale + neutres, pour eviter bruit visuel.
7. **Microcopie orientee action**: remplacer "Filtre" par "Choisir region a analyser".

## 9. Mode operatoire standard (SOP)

1. Cadrage des decisions a supporter.
2. Definition des KPI et scenarios d'usage par persona.
3. Wireframe puis prototype Power BI.
4. Validation metier rapide et iteration UX.
5. Tests de coherence KPI, accessibilite, performance.
6. Publication, communication, accompagnement utilisateurs.
7. Monitoring d'usage et optimisation continue.

## 10. Gouvernance documentaire

- Revision trimestrielle de ce standard.
- Toute derogation doit etre approuvee et tracee.
- Les retours utilisateurs et incidents d'interpretation alimentent la mise a jour du standard.

## 11. Annexes

### 11.1 Template de page analytique

```text
Bloc 1 (Haut): KPI principal, variation, statut
Bloc 2 (Milieu): tendance + decomposition des causes
Bloc 3 (Bas): detail operationnel et plan d'action
Bloc 4 (Aide): definitions KPI, perimetre, date de mise a jour
```

### 11.2 Confidentialite
Certaines donnees, exemples et captures peuvent etre anonymises ou legerement modifies afin de proteger les informations internes et sensibles.
