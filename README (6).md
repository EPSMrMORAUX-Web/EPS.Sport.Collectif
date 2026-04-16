# 🥏 Sport Collectif — Équipes

> Application web autonome pour la gestion d'équipes, de tournois et de tableaux de score en EPS.  
> Fonctionne entièrement en local, sans serveur, sans connexion internet.

---

## 📋 Sommaire

- [Présentation](#présentation)
- [Démarrage rapide](#démarrage-rapide)
- [Fonctionnalités](#fonctionnalités)
- [Structure de l'application](#structure-de-lapplication)
- [Sécurité & données](#sécurité--données)
- [Import Excel Pronote](#import-excel-pronote)
- [Modes de génération d'équipes](#modes-de-génération-déquipes)
- [Gestion des statuts élèves](#gestion-des-statuts-élèves)
- [Compatibilité](#compatibilité)

---

## Présentation

**Sport Collectif — Équipes** est une application HTML monopage destinée aux enseignants d'EPS. Elle permet de :

- Gérer plusieurs classes indépendamment
- Importer les listes d'élèves depuis Pronote (fichier Excel)
- Générer des équipes selon différents critères pédagogiques
- Arbitrer des matchs avec tableau de score et chronomètre
- Organiser un tournoi round-robin automatique
- Consulter le classement et l'historique des résultats
- Exporter les données en Excel ou JSON

L'application tient dans **un seul fichier `.html`** — aucune installation, aucune dépendance externe, aucune connexion requise.

---

## Démarrage rapide

1. Ouvrir le fichier `creation-equipe-CA4.html` dans un navigateur moderne (Chrome, Firefox, Safari, Edge)
2. Saisir le mot de passe enseignant sur l'écran de connexion
3. Créer une classe via le bouton **+ Classe** dans l'onglet Élèves
4. Ajouter des élèves manuellement ou importer un fichier Excel Pronote
5. Générer les équipes dans l'onglet **Équipes**

> ⚠️ Le mot de passe est à demander à l'administrateur de l'application. Il n'est pas stocké en clair dans ce document.

---

## Fonctionnalités

### 👥 Onglet Élèves

| Fonctionnalité | Description |
|---|---|
| Ajout manuel | Nom, Prénom, Sexe, Classe, Niveau (1–5 ⭐) |
| Import Excel | Lecture automatique des fichiers Pronote (.xlsx / .xls / .csv) |
| Niveau inline | Clic direct sur les étoiles d'un élève pour modifier son niveau |
| Statut rapide | 1 clic avatar = Absent · 2 clics = Blessé · 3 clics = Actif |
| Rôle arbitre | Les élèves blessés peuvent être assignés comme arbitres (bouton 🟡) |
| Affinités | Enregistrer les élèves avec qui un élève préfère être |
| Contraintes | Enregistrer les élèves qui ne doivent jamais être ensemble |
| Filtres | Par statut (absent / blessé), par sexe, par recherche texte |
| Tri | Liste alphabétique automatique par nom de famille |
| Multi-classes | Sélecteur de classe en haut de page — les données ne se mélangent pas |

### 🎽 Onglet Équipes

| Fonctionnalité | Description |
|---|---|
| Nombre d'équipes | De 2 à 6 équipes |
| Format de jeu | 3v3 · 4v4 · 5v5 · 6v6 · 7v7 |
| Génération | 6 modes disponibles (voir ci-dessous) |
| Mixer | Remélange aléatoire en conservant le même mode |
| Modification manuelle | Édition des joueurs de chaque équipe |
| Arbitres | Affichage automatique des élèves blessés désignés arbitres |

### ⚽ Onglet Score

| Fonctionnalité | Description |
|---|---|
| Tableau de marque | Score en temps réel pour chaque équipe |
| Boutons score | +1 · +10 · +100 · +1000 · −1 · Reset individuel |
| Chronomètre | Démarrer / Pause / Reset · Durée configurable (popup modal) |
| Reset global | Remet tous les scores à 0 avec confirmation |

### 🏆 Onglet Tournoi

| Fonctionnalité | Description |
|---|---|
| Génération | Round-robin automatique (chaque équipe joue contre toutes les autres) |
| Multi-terrains | 1 à 4 terrains simultanés |
| Planning | Vue de tous les matchs par tour avec terrain assigné |
| Saisie des scores | Formulaire inline par match · modification possible |
| Classement | Points (V=3 · N=1 · D=0) · différence de buts · buts marqués |
| Export | Fichier Excel avec planning + classement |

### 📊 Onglet Résultats

| Fonctionnalité | Description |
|---|---|
| Scores live | Classement des équipes par score en direct |
| Classement tournoi | Tableau complet V/N/D/Pts/BP/BC/+- |
| Historique | Liste de tous les matchs joués avec résultat |
| Export Excel | Classement exportable en un clic |

---

## Structure de l'application

```
creation-equipe-CA4.html      ← fichier unique autonome
│
├── 🔐 Écran de connexion      Mot de passe enseignant (hash SHA-256)
│
├── 👥 Élèves                  Gestion des élèves par classe
├── 🎽 Équipes                 Génération et configuration des équipes
├── ⚽ Score                   Tableau de marque + chronomètre
├── 🏆 Tournoi                 Planning, scores et classement
└── 📊 Résultats               Vue synthétique résultats + historique
```

---

## Sécurité & données

### Accès

L'application est protégée par un mot de passe. Le mot de passe est **haché en SHA-256** via l'API `crypto.subtle` native du navigateur avant comparaison — il n'est jamais stocké en clair dans le code source ni dans les données.

### Stockage local

Toutes les données (élèves, classes, équipes, tournoi) sont stockées dans le `localStorage` du navigateur, **chiffrées** par un chiffrement XOR avec clé obfusquée et encodage Base64. Les données ne sont pas lisibles directement depuis les outils développeur.

**Isolation par appareil** : chaque navigateur sur chaque appareil possède son propre `localStorage` isolé. Les données d'un enseignant sur son téléphone ne sont pas visibles depuis un autre appareil ou un autre navigateur.

### Ce que l'application ne fait pas

- ❌ Pas de serveur, pas d'envoi de données sur internet
- ❌ Pas de cookies tiers
- ❌ Pas de télémétrie, pas de tracking
- ❌ Pas de compte en ligne requis

### Sauvegarde recommandée

Les données étant locales, il est conseillé d'**exporter régulièrement** via le bouton 💾 → *Tout → JSON* pour conserver une copie de sauvegarde.

---

## Import Excel Pronote

L'application lit automatiquement les fichiers exports de Pronote.

### Colonnes détectées automatiquement

| Colonne | Noms reconnus |
|---|---|
| Nom de famille | `Nom`, `NOM` |
| Prénom | `Prénom`, `PRENOM` |
| Sexe | `Sexe`, `Genre`, `Sex` (valeurs : F/M, Fille/Garçon) |

### Format attendu

- Fichier `.xlsx`, `.xls` ou `.csv`
- Première ligne = en-têtes de colonnes
- Une ligne par élève

### Comportement à l'import

- Si une classe est sélectionnée au moment de l'import, tous les élèves importés sont **automatiquement rattachés à cette classe**
- Le niveau est initialisé à ⭐⭐⭐ (3/5) par défaut pour tous les élèves importés
- Les doublons ne sont pas détectés automatiquement — vérifier avant import

---

## Modes de génération d'équipes

| Mode | Niveaux | Sexes | Usage recommandé |
|---|---|---|---|
| ⚖️ **Homogène mixte** | Équilibrés entre équipes | Mélangés | Tournoi équitable classique |
| 🔵 **Homogène démixé** | Équilibrés entre équipes | Séparés par équipe | Tournois non-mixtes |
| 🔀 **Hétérogène mixte** | Fort + faible dans chaque équipe | Mélangés | Tutorat, entraide |
| 🟠 **Hétérogène démixé** | Fort + faible dans chaque équipe | Séparés par équipe | Tutorat non-mixte |
| 💚 **Affinités** | Selon les niveaux | Selon les préférences | Cohésion de groupe |
| 🚫 **Contraintes** | Équilibré | Mélangé | Gestion de conflits |

> **Algorithme snake** : pour les modes homogènes, la distribution se fait en serpentin (1er → équipe A, 2e → équipe B, ..., retour en sens inverse) pour garantir l'équilibre des niveaux.

> **Bouton Mixer 🔄** : relance l'algorithme avec une variation aléatoire tout en conservant le même mode. Utile pour proposer une composition différente sans changer les critères.

---

## Gestion des statuts élèves

```
Clic 1 sur l'avatar  →  🚫 ABSENT     (exclu de la génération d'équipes)
Clic 2 sur l'avatar  →  🤕 BLESSÉ     (exclu + peut être assigné arbitre)
Clic 3 sur l'avatar  →  ✅ ACTIF      (réintégré)
Bouton 🟡            →  Arbitre       (visible dans l'onglet Équipes)
```

Les élèves absents ou blessés sont **automatiquement exclus** de la génération d'équipes. Seuls les élèves avec le statut **Actif** sont répartis dans les équipes.

---

## Compatibilité

| Navigateur | Support |
|---|---|
| Chrome / Chromium | ✅ Recommandé |
| Firefox | ✅ Complet |
| Safari (iOS / macOS) | ✅ Complet |
| Edge | ✅ Complet |
| Internet Explorer | ❌ Non supporté |

**Testé sur** : ordinateur, tablette, smartphone (mode plein écran recommandé sur mobile).

> L'application utilise les APIs natives `crypto.subtle`, `localStorage`, `TextEncoder/Decoder` et `FileReader` — toutes disponibles dans tout navigateur moderne depuis 2018.

---

## Export des données

| Format | Contenu | Accès |
|---|---|---|
| Excel équipes | Résumé + joueurs par équipe | Bouton 💾 → Équipes → Excel |
| Excel tournoi | Planning matchs + classement | Bouton 💾 → Tournoi → Excel |
| Excel élèves | Liste complète avec statuts | Bouton 💾 → Élèves → Excel |
| JSON complet | Toutes les données (sauvegarde) | Bouton 💾 → Tout → JSON |

---

## Développement

L'application est contenue dans **un seul fichier HTML** (~1 600 lignes) structuré en :

- **HTML** : structure des pages et modales
- **CSS** : design sombre, variables CSS, responsive
- **JavaScript vanilla** : logique complète sans framework ni dépendance
- **SheetJS** (CDN) : lecture et écriture de fichiers Excel

Aucun outil de build, aucun `npm install`, aucun serveur requis.  
Pour modifier l'application, ouvrir le fichier dans un éditeur de texte et éditer directement.

---

*Application développée pour un usage en EPS — Sport Collectif Équipes*
