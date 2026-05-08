# 🏃 EPS Cyclades — Calculateur de Notes Terminale

> Application web autonome destinée aux **enseignants d'EPS** pour calculer et exporter les notes de Terminale (Voie Générale et Voie Professionnelle) au format Cyclades.

---

## Aperçu

EPS Cyclades automatise entièrement le calcul des notes EPS pour la remontée Cyclades. L'interface de type tableau Excel permet la saisie directe des notes, le calcul instantané et l'export en Excel ou PDF, sans aucune installation requise.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Licence](https://img.shields.io/badge/licence-libre-green)
![Technologie](https://img.shields.io/badge/tech-HTML%20%2F%20CSS%20%2F%20JS-orange)

---

## Fonctionnalités

- Calcul automatique instantané (Voie Générale & Voie Professionnelle)
- Choix stratégique par élève (Options A, B, C)
- Import de listes de classe et de notes depuis Excel / CSV
- Export Excel et PDF prêt à l'impression
- Gestion multi-classes
- Sauvegarde locale automatique (localStorage)
- Interface responsive (PC / tablette)
- Aucune installation, aucun serveur requis

---

## Installation

Aucune installation nécessaire. L'application est un **fichier HTML autonome**.

1. Télécharger `eps_cyclades.html`
2. Ouvrir le fichier dans un navigateur moderne (Chrome, Firefox, Edge, Safari)
3. Commencer à saisir des notes

> L'application fonctionne entièrement hors ligne. Les bibliothèques externes (SheetJS, jsPDF) sont chargées depuis un CDN au premier lancement et mises en cache automatiquement.

---

## Formules de calcul

### Voie Générale

Les notes se composent de trois AFL :

| Composante | Barème brut |
|------------|-------------|
| AFL1       | /12         |
| AFL2       | /4          |
| AFL3       | /4          |

L'élève choisit une **répartition stratégique** qui détermine le poids final de AFL2 et AFL3 :

| Option | AFL2 converti | AFL3 converti | Total max |
|--------|--------------|--------------|-----------|
| A      | /2           | /6           | 20        |
| B      | /4           | /4           | 20        |
| C      | /6           | /2           | 20        |

**Formule de conversion :**

```
note_convertie = (note_brute / 4) × coefficient_choisi
```

**Calcul final :**

```
Note /20 = AFL1 + AFL2_converti + AFL3_converti
```

**Exemple (Choix B) :**

```
AFL1 = 9     → 9 pts
AFL2 = 3/4   → (3/4) × 4 = 3 pts
AFL3 = 3.5/4 → (3.5/4) × 4 = 3.5 pts
Note finale  = 9 + 3 + 3.5 = 15.5 / 20
```

---

### Voie Professionnelle

Les notes se composent de deux AFLP fixes et deux AFLP complémentaires :

| Composante              | Barème brut |
|------------------------|-------------|
| AFLP1                  | /7          |
| AFLP2                  | /5          |
| Complémentaire A       | /4          |
| Complémentaire B       | /4          |

L'élève choisit une **répartition stratégique** pour les deux complémentaires :

| Option | Comp. A converti | Comp. B converti | Total max |
|--------|-----------------|-----------------|-----------|
| A      | /2              | /6              | 20        |
| B      | /4              | /4              | 20        |
| C      | /6              | /2              | 20        |

**Calcul final :**

```
Note /20 = AFLP1 + AFLP2 + CompA_converti + CompB_converti
```

**Exemple (Choix A) :**

```
AFLP1  = 6     → 6 pts
AFLP2  = 4     → 4 pts
Comp.A = 3/4   → (3/4) × 2 = 1.5 pts
Comp.B = 3.5/4 → (3.5/4) × 6 = 5.25 pts
Note finale    = 6 + 4 + 1.5 + 5.25 = 16.75 / 20
```

---

## Guide d'utilisation

### 1. Créer une classe

Cliquez sur **+ Nouvelle classe** dans la barre latérale gauche et saisissez le nom du groupe (ex : `Terminale A`, `TLE PRO 2`).

### 2. Ajouter des élèves

**Manuellement :** cliquez sur **+ Ajouter élève**, renseignez le nom, prénom, classe et choix stratégique initial.

**Par import Excel :** cliquez sur **Import Classe** et chargez un fichier `.xlsx`, `.xls` ou `.csv`. Les colonnes suivantes sont détectées automatiquement :

| Colonne attendue | Variantes acceptées |
|-----------------|----------------------|
| `NOM`           | `nom`, `name`, `lastname` |
| `PRENOM`        | `prenom`, `prénom`, `firstname` |
| `CLASSE`        | `classe`, `class`, `groupe`, `section` |

### 3. Saisir les notes

Les notes se saisissent directement dans le tableau. Le calcul se met à jour **instantanément** à chaque frappe. Les cellules en rouge signalent une valeur hors barème.

### 4. Importer des notes depuis Excel

Cliquez sur **Import Notes** pour charger un fichier contenant les notes. Les colonnes détectées automatiquement sont :

**Voie Générale :** `AFL1`, `AFL2`, `AFL3`, `CHOIX`

**Voie Professionnelle :** `AFLP1`, `AFLP2`, `AFLA` (ou `COMPA`), `AFLB` (ou `COMPB`), `CHOIX`

> L'association avec les élèves existants se fait par correspondance du nom de famille.

### 5. Télécharger un modèle

Cliquez sur **Modèle** pour télécharger `EPS_Cyclades_Modele.xlsx`, un fichier Excel avec trois onglets préremplis (liste classe, notes Générale, notes Pro) que vous pouvez utiliser comme base.

### 6. Voir le détail d'un calcul

Cliquez sur l'icône **ℹ** à droite d'un élève pour afficher la décomposition complète du calcul : notes brutes, conversions appliquées, coefficient choisi et note finale.

### 7. Exporter

| Bouton | Format | Contenu |
|--------|--------|---------|
| **Export Excel** | `.xlsx` | Tableau complet avec notes brutes, converties et finales |
| **Export PDF** | `.pdf` | Tableau A4 paysage, notes colorées, statistiques de classe |

---

## Import Excel — Format attendu

### Fichier liste de classe

```
NOM         | PRENOM   | CLASSE
MARTIN      | Julie    | TLE A
DUPONT      | Thomas   | TLE A
BERNARD     | Emma     | TLE B
```

### Fichier notes Voie Générale

```
NOM      | PRENOM | AFL1  | AFL2 | AFL3 | CHOIX
MARTIN   | Julie  | 11    | 3    | 3.5  | A
DUPONT   | Thomas | 9     | 2    | 3    | B
```

### Fichier notes Voie Professionnelle

```
NOM      | PRENOM | AFLP1 | AFLP2 | AFLA | AFLB | CHOIX
MARTIN   | Julie  | 6     | 4     | 3.5  | 3    | A
DUPONT   | Thomas | 5     | 3.5   | 2    | 3    | B
```

---

## Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `Ctrl + G` | Basculer en Voie Générale |
| `Ctrl + P` | Basculer en Voie Professionnelle |
| `Entrée` | Valider un formulaire ouvert |
| `Échap` | Fermer la fenêtre modale |

---

## Sauvegarde des données

Les données sont sauvegardées **automatiquement** dans le `localStorage` du navigateur après chaque modification. L'indicateur **✓ Sauvegardé** s'affiche brièvement dans l'en-tête à chaque enregistrement.

> Les données persistent entre les sessions tant que le cache du navigateur n'est pas vidé. Pour sauvegarder de façon pérenne, utilisez la fonction **Export Excel**.

---

## Technologies utilisées

| Bibliothèque | Version | Rôle |
|---|---|---|
| [SheetJS (xlsx)](https://sheetjs.com) | 0.18.5 | Import et export Excel |
| [jsPDF](https://github.com/parallax/jsPDF) | 2.5.1 | Génération PDF |
| [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) | 3.5.31 | Tableaux dans le PDF |
| [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) | — | Police d'interface |
| [JetBrains Mono](https://www.jetbrains.com/legalnotices/jetbrains-mono/) | — | Police monospace pour les notes |

Aucun framework JavaScript (React, Vue, Angular) n'est utilisé. L'application est entièrement en **HTML5 / CSS3 / JavaScript vanilla**.

---

## Compatibilité navigateurs

| Navigateur | Support |
|-----------|---------|
| Chrome 90+ | ✅ Complet |
| Firefox 88+ | ✅ Complet |
| Edge 90+ | ✅ Complet |
| Safari 14+ | ✅ Complet |
| IE 11 | ❌ Non supporté |

---

## Structure du fichier

```
eps_cyclades.html
├── <head>
│   ├── Liens CDN (SheetJS, jsPDF, Google Fonts)
│   └── Styles CSS (variables, composants, responsive)
└── <body>
    ├── En-tête (logo, onglets Voie Générale / Pro, indicateur de sauvegarde)
    ├── Barre latérale (gestion des classes, statistiques, aide)
    ├── Zone principale
    │   ├── Barre d'outils (import, export, recherche)
    │   └── Tableau de notes (édition inline, calcul temps réel)
    ├── Modale (ajout élève, nouvelle classe, détail calcul)
    └── <script>
        ├── État de l'application (S)
        ├── Fonctions de calcul (calcGen, calcPro)
        ├── Rendu du tableau (renderTable, renderBody)
        ├── Import Excel (doImportClass, doImportNotes)
        ├── Export (doExportExcel, doExportPDF)
        └── Persistance (localStorage)
```

---

## Contribuer / Personnaliser

Le fichier étant entièrement autonome, vous pouvez le modifier directement :

- **Ajouter une APSA** : étendre l'objet étudiant avec un champ `apsa` et l'afficher dans le tableau
- **Modifier les barèmes** : ajuster les constantes `max` dans les appels `setGrade()`
- **Changer les couleurs** : modifier les variables CSS dans `:root` (accent, pro, success…)
- **Ajouter un onglet voie** : dupliquer la logique de `calcGen` / `calcPro` pour une troisième voie

---

## Licence

Libre d'utilisation pour tout usage éducatif et professionnel.

---

*Développé pour les enseignants d'EPS — Éducation Nationale française*
