# Probabilités pour la gestion des risques

**Cours complet de probabilités appliquées à la banque, à l'assurance et à la finance, avec exercices corrigés, cas pratiques et notebooks Python.**

Auteur : **Emmanuel Evilafo**
Public : étudiants de Master 1 Gestion des Risques en Finance et en Assurance (GRAF), et toute personne qui veut comprendre la VaR et la TVaR à partir des bases.

[![Lire le cours (PDF)](https://img.shields.io/badge/Lire%20le%20cours-PDF-red)](cours/Cours_Probabilites_Gestion_Risques_M1GRAF.pdf)
[![Version Word](https://img.shields.io/badge/Version-Word-blue)](cours/Cours_Probabilites_Gestion_Risques_M1GRAF.docx)
[![Licence Apache 2.0](https://img.shields.io/badge/Licence-Apache%202.0-green)](LICENSE)

---

## Pourquoi ce cours

Beaucoup d'étudiants maîtrisent les premières notions de probabilités (espace probabilisé, conditionnement, lois usuelles), puis décrochent au moment des **mesures de risque** : à quoi sert la VaR, comment la calculer à partir d'une fonction de répartition, pourquoi la TVaR est préférée par les régulateurs.

Ce cours fait le pont. Il part des rappels de mathématiques et va jusqu'au calcul du capital d'une compagnie d'assurance ou d'une banque. Chaque formule est **expliquée**, **calculée à la main**, puis **vérifiée en Python**.

## Ce que contient le dépôt

| Contenu | Volume | Où le trouver |
| --- | --- | --- |
| Cours complet (PDF et Word) | 119 pages, 15 chapitres | [`cours/`](cours/) |
| Exemples résolus pas à pas | une vingtaine | dans chaque chapitre |
| Test de diagnostic, avec réponses | 14 questions | Partie 0 |
| Exercices de fin de chapitre, avec corrigés | 64 exercices | fin de chaque partie |
| Banque d'exercices en contexte, avec corrigés détaillés | 40 exercices | fin du cours |
| Cas pratiques complets, avec corrigés détaillés | 10 cas | fin du cours |
| Notebooks Jupyter exécutés | 11 notebooks | [`notebooks/`](notebooks/) |
| Source du cours (Markdown et LaTeX) | 1 fichier | [`cours/cours_source.md`](cours/cours_source.md) |

## Plan du cours

| Partie | Chapitres |
| --- | --- |
| **0. Bien démarrer** | Test de diagnostic ; rappels de maths (sommes, exp et ln, dérivées, intégrales) ; prise en main de Python pour les probabilités |
| **1. Les fondations** | 1. Espace probabilisé et dénombrement ; 2. Probabilités conditionnelles, indépendance, Bayes ; 3. Variables aléatoires : F, densité, survie, quantile ; 4. Espérance, variance, moments, inégalités |
| **2. Les outils** | 5. Lois usuelles discrètes et continues, queues de distribution ; 6. Couples de variables, covariance, corrélation ; 7. Loi des grands nombres, théorème central limite, mutualisation |
| **3. Le risque** | 8. Pourquoi mesurer le risque ; 9. VaR, TVaR et Expected Shortfall ; 10. Propriétés des mesures de risque, cohérence ; 11. Modèle collectif fréquence × sévérité |
| **4. Aller plus loin** | 12. Simulation Monte Carlo ; 13. Estimer une loi sur des données ; 14. Backtesting de la VaR ; étude de cas de synthèse |
| **Entraînement** | Banque de 40 exercices en contexte ; 10 cas pratiques complets |

### Les 10 cas pratiques

| Cas | Situation | Notions mobilisées |
| --- | --- | --- |
| 1 | Risque de crédit d'une institution de microfinance | Binomiale, variance totale, dépendance, tarification du risque |
| 2 | Portefeuille d'une caisse de retraite (actions BRVM, obligations, monétaire) | Matrice de covariance, VaR et ES, contributions, loi de Student, stress test |
| 3 | Assurance indicielle sécheresse pour une coopérative de cacao | Fonction d'une variable normale, risque systémique |
| 4 | Fraude interne dans une banque | Modèle collectif lognormal, VaR à 99,9 %, choix d'une mesure de maîtrise |
| 5 | Prévoyance décès d'une entreprise et scénario d'épidémie | Binomiale, mélange de lois, coût du capital |
| 6 | Bonus-malus d'un assureur automobile | Bayes, loi Gamma, binomiale négative, crédibilité |
| 7 | Risque de change d'un importateur | Lognormale, contrat à terme, option d'achat |
| 8 | Risque de taux d'un portefeuille obligataire | Duration, convexité, VaR, stress test |
| 9 | Concentration d'un portefeuille de crédits aux entreprises | Énumération de scénarios, VaR et TVaR discrètes |
| 10 | Inondation et réassurance d'un portefeuille habitation | Pareto, simulation, tranche de réassurance |

## Comment le cours est construit

Chaque notion se lit sur **trois niveaux** :

- **[N1] Intuition** : l'idée en une phrase, un exemple concret, à quoi ça sert ;
- **[N2] Calcul** : les définitions et les formules, la méthode pas à pas (niveau de l'examen) ;
- **[N3] Approfondissement** : démonstrations, cas particuliers, liens avec la suite.

Chaque chapitre suit le même plan : pourquoi c'est utile, le cours, des exemples résolus à la main puis en Python, une double vérification par simulation, les erreurs fréquentes, des exercices gradués (A : application, B : réflexion, C : type examen) et une fiche mémo.

## Utiliser les notebooks

Les notebooks contiennent tout le code du cours, déjà exécuté, avec les graphiques. Ils servent à **vérifier** un calcul fait à la main, à **aller vite** sur les calculs longs, et à **expérimenter** en changeant un paramètre.

| Notebook | Contenu | Ouvrir |
| --- | --- | --- |
| `00_Partie0_Rappels_et_Python` | SymPy, NumPy, SciPy, simulation | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/00_Partie0_Rappels_et_Python.ipynb) |
| `01_Chapitre1_Espace_probabilise_denombrement` | Dénombrement, paradoxe des anniversaires | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/01_Chapitre1_Espace_probabilise_denombrement.ipynb) |
| `02_Chapitre2_Conditionnement_Bayes` | Test médical, bonus-malus, indépendance | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/02_Chapitre2_Conditionnement_Bayes.ipynb) |
| `03_Chapitre3_Variables_aleatoires_F_S_quantile` | Lecture graphique du quantile, perte mixte | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/03_Chapitre3_Variables_aleatoires_F_S_quantile.ipynb) |
| `04_Chapitre4_Esperance_variance_moments` | Prime pure, franchise, espérance conditionnelle | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/04_Chapitre4_Esperance_variance_moments.ipynb) |
| `05_Partie2_Lois_couples_theoremes_limites` | Lois usuelles, queues, covariance, LGN, TCL | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/05_Partie2_Lois_couples_theoremes_limites.ipynb) |
| `06_Partie3_VaR_TVaR_modele_collectif` | VaR et TVaR, sous-additivité, Panjer | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/06_Partie3_VaR_TVaR_modele_collectif.ipynb) |
| `07_Partie4_Monte_Carlo_estimation_backtesting` | Bootstrap, ajustement de lois, Kupiec | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/07_Partie4_Monte_Carlo_estimation_backtesting.ipynb) |
| `08_Banque_exercices_verification` | Vérification des 40 exercices | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/08_Banque_exercices_verification.ipynb) |
| `09_Cas_pratiques` | Cas pratiques 1 à 5 | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/09_Cas_pratiques.ipynb) |
| `10_Cas_pratiques_6_a_10` | Cas pratiques 6 à 10 | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Evilafo/cours_probabilites/blob/main/notebooks/10_Cas_pratiques_6_a_10.ipynb) |

### Sans rien installer

Cliquez sur un bouton **Open in Colab** ci-dessus : le notebook s'ouvre dans Google Colab, où toutes les bibliothèques nécessaires sont déjà disponibles.

### Sur votre ordinateur

```bash
git clone https://github.com/Evilafo/cours_probabilites.git
cd cours_probabilites
pip install -r requirements.txt
jupyter notebook notebooks/
```

Python 3.10 ou plus récent est recommandé. Les simulations utilisent des graines fixes : vous retrouvez exactement les chiffres du cours.

## Méthode de travail conseillée

1. Faites d'abord le **test de diagnostic** (Partie 0) : il vous indique par où commencer.
2. Lisez le niveau [N1] d'une notion, fermez le document et réexpliquez-la avec vos mots.
3. Lisez le niveau [N2] avec un papier et un crayon, et refaites chaque exemple sans regarder la solution.
4. Cherchez chaque exercice au moins 15 minutes avant de lire l'indication, puis le corrigé.
5. Vérifiez vos résultats avec le notebook correspondant.
6. Relisez la fiche mémo la veille de chaque séance ou examen.

## Conventions

- Dans tout le cours, **X désigne une perte** : une grande valeur de X est une mauvaise nouvelle.
- La VaR au niveau α est le quantile d'ordre α de la perte : VaR_α(X) = inf{x : F(x) ≥ α}.
- Les montants sont exprimés en francs CFA (FCFA) et les exemples sont tirés du contexte ouest-africain (microfinance, BRVM, assurance agricole, mobile money).
- Ce cours complète le cours officiel sans le remplacer : en cas de différence de notation, c'est celle de votre enseignant qui compte à l'examen.

## Structure du dépôt

```text
cours_probabilites/
├── cours/
│   ├── Cours_Probabilites_Gestion_Risques_M1GRAF.pdf    cours complet, version PDF
│   ├── Cours_Probabilites_Gestion_Risques_M1GRAF.docx   cours complet, version Word modifiable
│   └── cours_source.md                                  source Markdown (formules en LaTeX)
├── notebooks/                                           11 notebooks Jupyter exécutés
├── requirements.txt                                     bibliothèques Python nécessaires
├── LICENSE                                              licence Apache 2.0
├── NOTICE                                               mentions de droits d'auteur
└── README.md
```

### Régénérer le PDF à partir de la source

La source `cours/cours_source.md` est au format Markdown de [Pandoc](https://pandoc.org), avec les formules en LaTeX. Avec Pandoc et une distribution LaTeX installés :

```bash
pandoc cours/cours_source.md -o cours.pdf --pdf-engine=xelatex --toc \
  -V mainfont="DejaVu Serif" -V mathfont="Latin Modern Math" -V geometry:margin=2.2cm
pandoc cours/cours_source.md -o cours.docx --toc
```

## Contribuer

Les signalements d'erreurs et les propositions d'amélioration sont les bienvenus :

- ouvrez une **issue** en précisant le chapitre, la page ou l'exercice concerné ;
- ou proposez une **pull request** sur `cours/cours_source.md` ou sur un notebook.

Pour une erreur de calcul, indiquez si possible le résultat que vous trouvez et votre méthode.

## Licence

Ce cours et ses notebooks sont distribués sous **licence Apache 2.0** (voir [`LICENSE`](LICENSE) et [`NOTICE`](NOTICE)).

Vous pouvez les utiliser, les copier, les modifier et les redistribuer, y compris dans vos propres enseignements, à condition de conserver la mention de l'auteur et la licence, et d'indiquer les modifications apportées.

## Citer ce cours

> Evilafo, E. (2026). *Probabilités pour la gestion des risques : cours complet, Master 1 GRAF*. https://github.com/Evilafo/cours_probabilites

## Auteur

**Emmanuel Evilafo**, enseignant de Python en Master 1 Gestion des Risques en Finance et en Assurance.
GitHub : [@Evilafo](https://github.com/Evilafo)
