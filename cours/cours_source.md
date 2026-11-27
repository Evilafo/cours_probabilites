---
title: "Probabilités pour la gestion des risques"
subtitle: "Cours complet, Master 1 GRAF"
author: "Emmanuel Evilafo"
date: "Novembre 2026"
lang: fr-FR
---

## Mode d'emploi du cours

Ce cours vous mène de zéro jusqu'aux mesures de risque utilisées en banque et en assurance (VaR, TVaR). Chaque formule est expliquée, calculée à la main, puis vérifiée en Python.

Il s'adresse à tout étudiant de Master Gestion des Risques qui se sent perdu, quel que soit son niveau de départ. Il complète votre cours officiel sans le remplacer : en cas de différence de notation, c'est celle de votre enseignant qui compte à l'examen.

### Les trois niveaux de lecture

Chaque notion est découpée en trois niveaux. Vous pouvez vous arrêter au niveau 2 sans rien perdre d'essentiel pour l'examen.

| Repère | Niveau | Ce que vous y trouvez | Qui doit le lire |
| --- | --- | --- | --- |
| \[N1\] | Intuition | L'idée en une phrase, un exemple concret, à quoi ça sert | Tout le monde, en premier |
| \[N2\] | Calcul | La définition exacte, les formules, la méthode pas à pas | Tout le monde : c'est le niveau de l'examen |
| \[N3\] | Approfondissement | Démonstrations, cas particuliers, liens avec la suite | Ceux qui veulent aller plus loin |

### Comment est construit chaque chapitre

1. **Pourquoi c'est utile** : la situation réelle où la notion sert.
2. **Le cours** : les définitions et formules, expliquées terme par terme.
3. **Exemple résolu à la main**, puis le même exemple en Python.
4. **Double vérification** : on compare la formule exacte à une simulation. Si les deux concordent, le calcul est juste.
5. **Erreurs fréquentes** : les pièges classiques, pour ne pas y tomber.
6. **Exercices** en trois paliers : application directe (A), réflexion (B), type examen (C). Chaque exercice a une indication, et le corrigé complet est en fin de partie.
7. **Fiche mémo** : l'essentiel du chapitre sur une page, à relire avant un devoir.

### Conventions de notation

| Symbole | Se lit | Signification |
| --- | --- | --- |
| Ω | « oméga » | L'ensemble de tous les résultats possibles d'une expérience |
| P(A) | « probabilité de A » | Un nombre entre 0 et 1 qui mesure la chance que A se produise |
| X, Y, N | « variable aléatoire » | Une quantité dont la valeur dépend du hasard (une perte, un nombre de sinistres) |
| F~X~(x) | « fonction de répartition » | P(X ≤ x) |
| S~X~(x) | « fonction de survie » | P(X > x) = 1 − F~X~(x) |
| f~X~(x) | « densité » | Dérivée de F~X~, pour une variable continue |
| p~X~(k) | « loi de probabilité » | P(X = k), pour une variable discrète |
| E\[X\] | « espérance de X » | La valeur moyenne de X |
| Var(X) | « variance de X » | La dispersion de X autour de sa moyenne |
| α | « alpha » | Un niveau de confiance proche de 1 (0,95 ; 0,99 ; 0,995) |

Dans tout le cours, **X désigne une perte** : une grande valeur de X est une mauvaise nouvelle. C'est la convention de l'actuariat et de la réglementation prudentielle. Si votre cours travaille avec des gains, il suffit de remplacer X par −X.

### Le notebook Python

Chaque chapitre a son notebook Jupyter, qui contient tout le code du document, prêt à exécuter. Utilisez-le pour trois choses :

- **vérifier** un résultat trouvé à la main ;
- **aller vite** quand un calcul est long (sommes de nombreux termes, intégrales difficiles) ;
- **expérimenter** : changer un paramètre et voir ce qui bouge.

Python ne remplace pas le calcul à la main : à l'examen, c'est la méthode qui est notée. Faites toujours le calcul d'abord, puis vérifiez.

### Méthode de travail conseillée

1. Lisez le \[N1\] d'une notion, puis fermez le document et essayez de l'expliquer avec vos mots.
2. Lisez le \[N2\] avec un papier et un crayon, et refaites chaque exemple sans regarder la solution.
3. Cherchez chaque exercice au moins 15 minutes avant de lire l'indication, puis 15 minutes de plus avant le corrigé.
4. Vérifiez vos réponses avec le notebook.
5. Relisez la fiche mémo la veille de chaque séance.

## Partie 0. Test de diagnostic

Ce test de 14 questions vous dit par où commencer. Faites-le en 45 minutes maximum, sans calculatrice sauf pour A3, C3 et D2, puis comptez vos points avec les réponses plus bas.

### Bloc A : Outils mathématiques

- **A1.** Calculez la somme infinie 1 + 1/2 + 1/4 + 1/8 + …
- **A2.** Calculez l'intégrale de e^−2x^ entre 0 et +∞.
- **A3.** Résolvez l'équation 1 − e^−x/100^ = 0,95.
- **A4.** Dérivez la fonction g(x) = x · e^−x^.

### Bloc B : Probabilités de base

- **B1.** On lance deux dés équilibrés. Quelle est la probabilité que la somme fasse 7 ?
- **B2.** A et B sont indépendants, avec P(A) = 0,3 et P(B) = 0,5. Calculez P(A ∪ B).
- **B3.** Une urne contient 3 boules rouges et 2 bleues. On tire 2 boules sans remise. Quelle est la probabilité d'obtenir 2 rouges ?
- **B4.** Une maladie touche 1 % de la population. Un test détecte 95 % des malades, mais donne aussi un résultat positif à 5 % des personnes saines. Une personne est testée positive : quelle est la probabilité qu'elle soit malade ?

### Bloc C : Variables aléatoires

- **C1.** X est le résultat d'un dé équilibré. Calculez E\[X\] et Var(X).
- **C2.** X a pour densité f(x) = 2x sur \[0, 1\] (et 0 ailleurs). Donnez F~X~(x) pour x dans \[0, 1\], puis E\[X\].
- **C3.** X suit une loi exponentielle de paramètre λ = 0,5. Calculez P(X > 4).
- **C4.** Donnez la médiane d'une loi exponentielle de paramètre λ.

### Bloc D : Vers le risque

- **D1.** Expliquez en une phrase ce qu'est le quantile d'ordre 0,99 d'une perte X.
- **D2.** Une perte X suit une loi exponentielle de moyenne 1 000. Quel montant x n'est dépassé que dans 1 % des cas ?

### Réponses du test

| Question | Réponse | Chapitre à revoir si faux |
| --- | --- | --- |
| A1 | 2 (série géométrique de raison 1/2 : 1/(1 − 1/2)) | Rappels de maths |
| A2 | 1/2 | Rappels de maths |
| A3 | x = −100 · ln(0,05) ≈ 299,57 | Rappels de maths |
| A4 | g'(x) = (1 − x) · e^−x^ | Rappels de maths |
| B1 | 6/36 = 1/6 ≈ 0,167 | Chapitre 1 |
| B2 | 0,3 + 0,5 − 0,3 × 0,5 = 0,65 | Chapitres 1 et 2 |
| B3 | C(3,2)/C(5,2) = 3/10 = 0,3 | Chapitre 1 |
| B4 | 0,0095/0,059 ≈ 0,161, soit environ 16 % seulement | Chapitre 2 |
| C1 | E\[X\] = 3,5 ; Var(X) = 35/12 ≈ 2,917 | Chapitre 4 |
| C2 | F~X~(x) = x² ; E\[X\] = 2/3 | Chapitres 3 et 4 |
| C3 | e^−2^ ≈ 0,135 | Chapitre 3 |
| C4 | ln(2)/λ | Chapitre 3 |
| D1 | C'est le montant de perte qui n'est dépassé qu'avec une probabilité de 1 % | Partie 3 |
| D2 | x = 1 000 · ln(100) ≈ 4 605 | Chapitre 3, puis Partie 3 |

### Grille d'orientation

Comptez un point par bonne réponse, bloc par bloc, puis lisez la première ligne qui vous concerne.

| Votre résultat | Par où commencer |
| --- | --- |
| Bloc A : moins de 3/4 | Rappels de maths de la Partie 0, puis tout le cours dans l'ordre |
| Bloc B : moins de 3/4 | Chapitres 1 et 2 |
| Bloc C : moins de 3/4 | Chapitres 3 et 4 (la fonction de répartition et le quantile sont la base de la VaR) |
| Bloc D : moins de 2/2, le reste réussi | Relisez la section « Quantile » du chapitre 3, puis passez directement à la Partie 3 |
| Tout réussi | Partie 3, puis les niveaux \[N3\] |

La question B4 surprend presque tout le monde : on s'attend à 95 %, la bonne réponse est 16 %. Si vous vous êtes trompé, c'est normal, et le chapitre 2 explique pourquoi.

## Partie 0. Rappels de maths

Quatre outils reviennent dans tout le cours : les sommes, l'exponentielle et le logarithme, les dérivées et les intégrales. Les probabilités discrètes se calculent avec des sommes, les probabilités continues avec des intégrales, et la VaR d'une loi exponentielle avec un logarithme.

### 0.1 Les sommes

&#91;N1\] Le symbole Σ (« sigma ») est un raccourci pour écrire une addition de nombreux termes qui suivent la même règle.

&#91;N2\] On lit la formule ci-dessous « somme pour k allant de 1 à n de a~k~ ».

$$
\sum_{k=1}^{n} a_k = a_1 + a_2 + \dots + a_n
$$

**Règle de linéarité** : on peut sortir les constantes et séparer les sommes. Cette règle donnera plus tard la linéarité de l'espérance.

$$
\sum_{k=1}^{n} (c \, a_k + b_k) = c \sum_{k=1}^{n} a_k + \sum_{k=1}^{n} b_k
$$

**Les quatre sommes à connaître par cœur** :

$$
\sum_{k=1}^{n} k = \frac{n(n+1)}{2} \qquad \sum_{k=1}^{n} k^2 = \frac{n(n+1)(2n+1)}{6}
$$

$$
\sum_{k=0}^{n} q^k = \frac{1-q^{n+1}}{1-q} \;(q \neq 1) \qquad \sum_{k=0}^{+\infty} q^k = \frac{1}{1-q} \;(|q|<1)
$$

$$
\sum_{k=0}^{+\infty} \frac{\lambda^k}{k!} = e^{\lambda}
$$

La dernière, la série exponentielle, est celle qui permet de vérifier que la loi de Poisson est bien une loi de probabilité. Les séries géométriques servent pour la loi géométrique (chapitre 5).

**Exemple** : avec q = 0,9, la somme infinie vaut 1/(1 − 0,9) = 10.

### 0.2 Exponentielle et logarithme

&#91;N1\] L'exponentielle modélise ce qui décroît « de plus en plus lentement », comme la probabilité qu'un très gros sinistre se produise. Le logarithme népérien ln est sa fonction réciproque : il « défait » l'exponentielle.

&#91;N2\] Les règles de calcul, valables pour a, b > 0 :

| Règle | Exponentielle | Logarithme |
| --- | --- | --- |
| Produit | e^a+b^ = e^a^ · e^b^ | ln(ab) = ln(a) + ln(b) |
| Quotient | e^a−b^ = e^a^ / e^b^ | ln(a/b) = ln(a) − ln(b) |
| Puissance | (e^a^)^n^ = e^na^ | ln(a^n^) = n · ln(a) |
| Réciproque | ln(e^x^) = x | e^ln\ x^ = x |
| Valeurs clés | e^0^ = 1 | ln(1) = 0 ; ln(2) ≈ 0,693 ; ln(10) ≈ 2,303 |

**La méthode qui sert pour toutes les VaR « exponentielles »** : pour résoudre e^−λx^ = p, on prend le logarithme des deux côtés.

$$
e^{-\lambda x} = p \iff -\lambda x = \ln(p) \iff x = -\frac{\ln(p)}{\lambda}
$$

**Exemple** : e^−x/1000^ = 0,01 donne x = −1 000 · ln(0,01) = 1 000 · ln(100) ≈ 4 605. C'est exactement la question D2 du test de diagnostic.

### 0.3 Dérivées

&#91;N1\] La dérivée mesure la vitesse à laquelle une fonction varie. En probabilités, la densité f est la dérivée de la fonction de répartition F.

&#91;N2\] Les dérivées usuelles et les règles de calcul :

| Fonction | Dérivée |
| --- | --- |
| x^n^ | n · x^n−1^ |
| e^ax^ | a · e^ax^ |
| ln(x) | 1/x |
| u · v | u'v + uv' |
| u / v | (u'v − uv') / v² |
| u(v(x)) | v'(x) · u'(v(x)) |

**Exemple** : si F(x) = 1 − e^−2x^, alors f(x) = F'(x) = 2e^−2x^.

### 0.4 Intégrales

&#91;N1\] L'intégrale de f entre a et b est l'aire sous la courbe de f entre a et b. Pour une variable continue, une probabilité est une aire sous la densité.

&#91;N2\] Si F est une primitive de f (c'est-à-dire F' = f), alors :

$$
\int_a^b f(x)\,dx = F(b) - F(a)
$$

Les trois intégrales qui reviennent sans cesse, pour λ > 0 :

$$
\int_0^{+\infty} e^{-\lambda x}\,dx = \frac{1}{\lambda} \qquad \int_0^{+\infty} x\,e^{-\lambda x}\,dx = \frac{1}{\lambda^2} \qquad \int_0^{+\infty} x^2\,e^{-\lambda x}\,dx = \frac{2}{\lambda^3}
$$

**L'intégration par parties (IPP)** sert à intégrer un produit, par exemple x · e^−λx^ pour calculer une espérance :

$$
\int_a^b u(x)\,v'(x)\,dx = \big[u(x)\,v(x)\big]_a^b - \int_a^b u'(x)\,v(x)\,dx
$$

**Exemple résolu** : calculons l'intégrale de x · e^−λx^ entre 0 et +∞. On pose u = x (donc u' = 1) et v' = e^−λx^ (donc v = −e^−λx^/λ).

$$
\int_0^{+\infty} x\,e^{-\lambda x}\,dx = \Big[-\frac{x\,e^{-\lambda x}}{\lambda}\Big]_0^{+\infty} + \frac{1}{\lambda}\int_0^{+\infty} e^{-\lambda x}\,dx = 0 + \frac{1}{\lambda}\cdot\frac{1}{\lambda} = \frac{1}{\lambda^2}
$$

Le crochet vaut 0 car x · e^−λx^ tend vers 0 quand x tend vers +∞ : l'exponentielle l'emporte toujours sur les puissances.

&#91;N3\] **La fonction Gamma** généralise ces calculs. Pour un entier n, elle vaut (n − 1)!, d'où la formule générale ci-dessous. Elle servira pour la loi Gamma au chapitre 5.

$$
\Gamma(a) = \int_0^{+\infty} x^{a-1} e^{-x}\,dx \qquad \int_0^{+\infty} x^n e^{-\lambda x}\,dx = \frac{n!}{\lambda^{n+1}}
$$

### Vérifier avec Python

La bibliothèque SymPy fait du calcul symbolique : elle donne des résultats exacts, comme à la main.

```python
import sympy as sp

x, lam = sp.symbols('x lambda', positive=True)

# Sommes
k, n = sp.symbols('k n', integer=True, positive=True)
print(sp.simplify(sp.summation(k, (k, 1, n))))                      # n*(n + 1)/2
print(sp.summation(sp.Rational(9, 10)**k, (k, 0, sp.oo)))           # 10
print(sp.summation(lam**k / sp.factorial(k), (k, 0, sp.oo)))        # exp(lambda)

# Integrales
print(sp.integrate(sp.exp(-lam*x), (x, 0, sp.oo)))                  # 1/lambda
print(sp.integrate(x * sp.exp(-lam*x), (x, 0, sp.oo)))              # lambda**(-2)

# Equation e^(-x/1000) = 0.01
sol = sp.solve(sp.Eq(sp.exp(-x/1000), sp.Rational(1, 100)), x)
print(sp.N(sol[0]))                                                 # 4605.17... = 1000 ln(100)
```

### Erreurs fréquentes

- Écrire ln(a + b) = ln(a) + ln(b) : c'est faux. Le logarithme transforme les **produits** en sommes, pas les sommes.
- Oublier le signe moins : ln(0,01) est négatif (≈ −4,605). Une VaR négative pour une perte positive doit vous alerter.
- Appliquer la formule de la série géométrique infinie avec |q| ≥ 1 : la somme diverge.
- Dans une IPP, mal choisir u : on dérive le polynôme (il devient plus simple) et on intègre l'exponentielle.

### Exercices

- **0.1 (A)** Calculez la somme de 2 × 0,8^k^ pour k allant de 0 à +∞. *Indication : sortez la constante 2.*
- **0.2 (A)** Résolvez 1 − e^−0,02x^ = 0,995. *Indication : isolez d'abord l'exponentielle.*
- **0.3 (B)** Calculez l'intégrale de x² · e^−x^ entre 0 et +∞ par deux IPP successives. *Indication : après la première IPP, vous retrouvez l'exemple résolu avec λ = 1.*
- **0.4 (B)** Montrez que la somme de e^−λ^ · λ^k^ / k! pour k allant de 0 à +∞ vaut 1. *Indication : e^−λ^ ne dépend pas de k.*

Les corrigés sont à la fin de la Partie 1.

## Partie 0. Prise en main de Python pour les probas

Trois bibliothèques suffisent pour tout le cours : NumPy pour calculer et simuler, SciPy pour les lois de probabilité, Matplotlib pour tracer. SciPy fournit déjà F, S, la densité et le quantile de chaque loi usuelle.

### Installer et lancer

- **Sans rien installer** : ouvrez [Google Colab](https://colab.research.google.com), importez le notebook du chapitre et exécutez les cellules. Les trois bibliothèques y sont déjà.
- **Sur votre ordinateur** : installez Anaconda, qui contient Python, Jupyter et les bibliothèques. Sinon, avec Python déjà installé : `pip install numpy scipy matplotlib sympy jupyter`.

Chaque notebook commence par ces imports :

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

rng = np.random.default_rng(42)   # generateur aleatoire ; 42 = graine, pour retrouver les memes resultats
```

### NumPy : calculer sur des tableaux

&#91;N1\] Un tableau NumPy contient beaucoup de nombres d'un coup. Une opération s'applique à tous les éléments à la fois, sans boucle.

```python
x = np.array([100, 250, 400, 1200])   # 4 montants de sinistres
print(x.sum(), x.mean(), x.std())     # total, moyenne, ecart-type
print(x * 1.1)                         # tous les montants augmentes de 10 %
print(np.mean(x > 300))                # proportion de sinistres > 300 -> 0.5
```

La dernière ligne est l'astuce la plus utile du cours : `x > 300` donne un tableau de Vrai/Faux, et sa moyenne est la **proportion** de valeurs qui dépassent 300. C'est ainsi qu'on estime une probabilité à partir de données.

### SciPy : toutes les lois au même endroit

&#91;N2\] Chaque loi de `scipy.stats` s'utilise de la même façon, et chaque méthode correspond exactement à une notion du cours.

| Méthode SciPy | Notion du cours | Formule |
| --- | --- | --- |
| `.pmf(k)` | Loi d'une variable discrète | P(X = k) |
| `.pdf(x)` | Densité d'une variable continue | f~X~(x) |
| `.cdf(x)` | Fonction de répartition | F~X~(x) = P(X ≤ x) |
| `.sf(x)` | Fonction de survie | S~X~(x) = P(X > x) |
| `.ppf(a)` | Quantile d'ordre a, c'est-à-dire la VaR | F~X~⁻¹(a) |
| `.mean()`, `.var()`, `.std()` | Espérance, variance, écart-type | E\[X\], Var(X), σ |
| `.rvs(size=n, random_state=rng)` | Simulation de n valeurs | - |

```python
# Une loi exponentielle de moyenne 1000 (lambda = 1/1000)
X = stats.expon(scale=1000)

print(X.cdf(2000))    # P(X <= 2000) = 1 - e^(-2) = 0.8647
print(X.sf(2000))     # P(X > 2000)  = e^(-2)     = 0.1353
print(X.ppf(0.99))    # quantile a 99 % = 1000*ln(100) = 4605.17
print(X.mean())       # 1000
```

**Piège important** : SciPy paramètre beaucoup de lois avec une **échelle** (`scale`) et non avec un taux. Pour une loi exponentielle de paramètre λ, il faut écrire `scale = 1/λ`. Avec λ = 0,5, c'est `stats.expon(scale=2)`, pas `stats.expon(0.5)` (qui décalerait la loi de 0,5).

### Matplotlib : tracer une courbe

```python
x = np.linspace(0, 6000, 500)          # 500 points entre 0 et 6000
plt.plot(x, X.cdf(x), label='F(x)')
plt.plot(x, X.sf(x), label='S(x) = 1 - F(x)')
plt.axhline(0.99, ls='--', color='grey')   # ligne horizontale a 0.99
plt.xlabel('Perte x'); plt.ylabel('Probabilite'); plt.legend(); plt.show()
```

### Le principe de la simulation (Monte Carlo)

&#91;N1\] Pour vérifier une probabilité, on peut « rejouer » l'expérience un très grand nombre de fois et compter. Si l'événement se produit dans 13,5 % des simulations, sa probabilité est proche de 0,135.

&#91;N2\] C'est la loi des grands nombres (chapitre 7) : une proportion observée sur n répétitions se rapproche de la vraie probabilité quand n grandit. L'erreur est de l'ordre de 1/√n : avec 1 000 000 de simulations, on obtient environ 3 chiffres justes.

```python
simul = X.rvs(size=1_000_000, random_state=rng)   # 1 million de pertes simulees
print(np.mean(simul > 2000))        # proche de 0.1353 (valeur exacte : e^(-2))
print(np.quantile(simul, 0.99))     # proche de 4605 (valeur exacte : 1000 ln 100)
```

C'est la **double vérification** utilisée dans tout le cours : formule exacte d'un côté, simulation de l'autre. Si les deux résultats diffèrent nettement, l'un des deux calculs est faux.

### Erreurs fréquentes

- Confondre `scale` et taux dans SciPy (voir le piège ci-dessus).
- Utiliser `.pdf` pour une loi discrète (il faut `.pmf`) ou l'inverse.
- Oublier la graine `rng` : les résultats changent à chaque exécution. Ce n'est pas faux, mais c'est déroutant pour comparer.
- Simuler trop peu de valeurs : avec 100 simulations, un quantile à 99 % repose sur une ou deux valeurs seulement.

### Exercices

- **0.5 (A)** Avec SciPy, calculez P(X ≤ 3) et P(X > 3) pour X de loi exponentielle de paramètre λ = 0,5. *Indication : scale = 2.*
- **0.6 (A)** Simulez 100 000 lancers de dé avec `rng.integers(1, 7, size=100_000)` et estimez la probabilité d'obtenir 6.
- **0.7 (B)** Simulez 100 000 lancers de deux dés et vérifiez la réponse B1 du test de diagnostic.

Les corrigés sont à la fin de la Partie 1.

## Chapitre 1. Espace probabilisé et dénombrement

Une probabilité est une façon rigoureuse de mesurer l'incertitude : on liste tout ce qui peut arriver (Ω), puis on attribue à chaque événement un nombre entre 0 et 1. Quand tous les résultats sont également probables, calculer une probabilité revient à compter, d'où le dénombrement.

### Pourquoi c'est utile

Un assureur ne sait pas quels clients auront un accident cette année. Il sait en revanche décrire tous les scénarios possibles et leur attribuer une probabilité. Toute la tarification, toute la gestion des risques repose sur ce langage.

### 1.1 Expérience aléatoire, univers, événements

&#91;N1\] Une **expérience aléatoire** est une situation dont on ne connaît pas le résultat à l'avance : lancer un dé, observer si un assuré déclare un sinistre, regarder le cours d'une action demain.

&#91;N2\] Les définitions :

- L'**univers** Ω est l'ensemble de tous les résultats possibles. Pour un dé : Ω = {1, 2, 3, 4, 5, 6}.
- Un **événement** est une partie de Ω. « Obtenir un nombre pair » est l'événement A = {2, 4, 6}.
- Un **événement élémentaire** contient un seul résultat, par exemple {3}.

Le langage courant se traduit en opérations sur les ensembles :

| En français | Notation | Exemple avec le dé, A = {2, 4, 6}, B = {4, 5, 6} |
| --- | --- | --- |
| A ne se produit pas (contraire) | Ā | {1, 3, 5} |
| A **ou** B (au moins l'un des deux) | A ∪ B | {2, 4, 5, 6} |
| A **et** B (les deux à la fois) | A ∩ B | {4, 6} |
| A **sans** B | A ∩ B̄ | {2} |
| A et B ne peuvent pas arriver ensemble (incompatibles) | A ∩ B = $\varnothing$ | {1} et {2} |
| Événement impossible | $\varnothing$ | « obtenir 7 » |
| Événement certain | Ω | « obtenir entre 1 et 6 » |

**Lois de De Morgan** : le contraire de « A ou B » est « ni A ni B », et le contraire de « A et B » est « non A ou non B ».

$$
\overline{A \cup B} = \bar{A} \cap \bar{B} \qquad \overline{A \cap B} = \bar{A} \cup \bar{B}
$$

### 1.2 La probabilité et ses règles

&#91;N1\] Une probabilité P attribue à chaque événement un nombre entre 0 (impossible) et 1 (certain).

&#91;N2\] P doit respecter deux règles, appelées axiomes de Kolmogorov :

1. P(Ω) = 1 : il se passe forcément quelque chose.
2. Si A et B sont incompatibles, P(A ∪ B) = P(A) + P(B). Plus généralement, pour une suite d'événements deux à deux incompatibles, la probabilité de leur réunion est la somme de leurs probabilités.

Toutes les propriétés utiles en découlent :

| Propriété | Formule | Quand l'utiliser |
| --- | --- | --- |
| Contraire | P(Ā) = 1 − P(A) | Dès qu'on lit « au moins un » |
| Impossible | P($\varnothing$) = 0 | - |
| Inclusion | si A ⊂ B, alors P(A) ≤ P(B) | Pour vérifier un résultat |
| Réunion | P(A ∪ B) = P(A) + P(B) − P(A ∩ B) | « A ou B » quand A et B peuvent arriver ensemble |
| Différence | P(A ∩ B̄) = P(A) − P(A ∩ B) | « A seulement » |

**Pourquoi on retire P(A ∩ B) dans la réunion** : en additionnant P(A) et P(B), on compte deux fois la partie commune. On la retire une fois.

&#91;N3\] Pour trois événements, la formule de Poincaré (ou d'inclusion-exclusion) devient :

$$
P(A \cup B \cup C) = P(A)+P(B)+P(C) - P(A\cap B) - P(A\cap C) - P(B\cap C) + P(A\cap B\cap C)
$$

&#91;N3\] En toute rigueur, P n'est définie que sur une famille d'événements appelée **tribu** (stable par contraire et par réunion dénombrable). Le triplet (Ω, tribu, P) forme l'**espace probabilisé**. Pour un univers fini, on prend toutes les parties de Ω et la question ne se pose pas.

### 1.3 L'équiprobabilité : probabilité = comptage

&#91;N2\] Si Ω est fini et que tous les résultats ont la même chance (dé équilibré, tirage au hasard), alors :

$$
P(A) = \frac{\text{nombre de cas favorables}}{\text{nombre de cas possibles}} = \frac{|A|}{|\Omega|}
$$

**Exemple** : avec deux dés, Ω contient 6 × 6 = 36 couples. La somme vaut 7 pour (1,6), (2,5), (3,4), (4,3), (5,2), (6,1), soit 6 cas. Donc P(somme = 7) = 6/36 = 1/6.

La formule ne s'applique **que** si les résultats sont équiprobables. Les sommes de deux dés (2, 3, …, 12) ne le sont pas : 7 est bien plus fréquent que 2.

### 1.4 Le dénombrement

&#91;N1\] Pour compter vite, on se pose deux questions : **l'ordre compte-t-il ?** et **peut-on reprendre le même élément ?** On choisit k éléments parmi n.

&#91;N2\] Les quatre cas :

|  | Avec répétition (avec remise) | Sans répétition (sans remise) |
| --- | --- | --- |
| **L'ordre compte** | n^k^ (k-uplets) | A(n,k) = n!/(n − k)! (arrangements) |
| **L'ordre ne compte pas** | C(n + k − 1, k) \[N3\] | C(n,k) = n!/(k!(n − k)!) (combinaisons) |

Cas particulier : ranger n éléments dans un ordre donne n! **permutations** (n! = 1 × 2 × … × n, et 0! = 1).

| Situation | Ordre ? | Répétition ? | Nombre |
| --- | --- | --- | --- |
| Code PIN à 4 chiffres | Oui | Oui | 10⁴ = 10 000 |
| Podium de 3 parmi 10 coureurs | Oui | Non | 10 × 9 × 8 = 720 |
| Comité de 3 parmi 10 personnes | Non | Non | C(10,3) = 120 |
| Ordre de passage de 5 candidats | Oui | Non (tous) | 5! = 120 |

Les propriétés utiles des combinaisons :

$$
\binom{n}{k} = \binom{n}{n-k} \qquad \binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k} \qquad \sum_{k=0}^{n}\binom{n}{k} = 2^n
$$

Et la **formule du binôme de Newton**, qui servira pour la loi binomiale :

$$
(a+b)^n = \sum_{k=0}^{n} \binom{n}{k} a^k b^{n-k}
$$

### Exemple résolu : Audit de dossiers

**Énoncé** : un service de sinistres a 20 dossiers en cours, dont 4 sont frauduleux. Un auditeur en tire 3 au hasard. Quelle est la probabilité qu'il tombe sur au moins un dossier frauduleux ?

**Étape 1 : Modéliser.** On choisit 3 dossiers parmi 20, sans ordre et sans remise : |Ω| = C(20,3) = 1 140 tirages équiprobables.

**Étape 2 : Passer au contraire.** « Au moins un » a beaucoup de cas (1, 2 ou 3 fraudes). Son contraire, « aucun dossier frauduleux », n'en a qu'un : les 3 dossiers sont pris parmi les 16 honnêtes, soit C(16,3) = 560 tirages.

**Étape 3 : Calculer.**

$$
P(\text{au moins une fraude}) = 1 - \frac{\binom{16}{3}}{\binom{20}{3}} = 1 - \frac{560}{1140} \approx 1 - 0{,}4912 = 0{,}5088
$$

**Interprétation** : avec 3 dossiers sur 20, l'auditeur a environ une chance sur deux de détecter une fraude.

### En Python

```python
from math import comb, factorial, perm
import numpy as np
rng = np.random.default_rng(42)

# Calcul exact
print(comb(20, 3), comb(16, 3))          # 1140 560
print(1 - comb(16, 3) / comb(20, 3))     # 0.5088

# Double verification par simulation
dossiers = np.array([1]*4 + [0]*16)      # 1 = frauduleux
n_sim = 200_000
trouve = 0
for _ in range(n_sim):
    tirage = rng.choice(dossiers, size=3, replace=False)
    trouve += tirage.sum() >= 1
print(trouve / n_sim)                    # proche de 0.5088
```

### &#91;N3\] Pour aller plus loin : Le paradoxe des anniversaires

Dans un groupe de n personnes, quelle est la probabilité qu'au moins deux aient le même anniversaire (sur 365 jours équiprobables) ? Le contraire, « tous différents », compte A(365,n) cas sur 365^n^.

$$
P(\text{au moins deux identiques}) = 1 - \frac{365 \times 364 \times \dots \times (365-n+1)}{365^n}
$$

Cette probabilité dépasse 50 % dès n = 23. La leçon pour un gestionnaire de risques : notre intuition sous-estime fortement la probabilité des coïncidences quand le nombre de paires possibles est grand.

```python
n = 23
print(1 - perm(365, n) / 365**n)   # 0.5073
```

### Erreurs fréquentes

- Additionner P(A) + P(B) alors que A et B peuvent arriver ensemble : on compte deux fois A ∩ B.
- Calculer « au moins un » directement au lieu de passer par le contraire.
- Utiliser |A|/|Ω| alors que les résultats ne sont pas équiprobables.
- Confondre arrangement et combinaison : demandez-vous si échanger deux éléments donne un résultat différent.
- Confondre **incompatibles** (A ∩ B = $\varnothing$) et **indépendants** (chapitre 2) : ce sont deux notions différentes.

### Exercices

- **1.1 (A)** On sait que P(A) = 0,4, P(B) = 0,3 et P(A ∪ B) = 0,5. Calculez P(A ∩ B), puis P(A ∩ B̄). *Indication : utilisez la formule de la réunion à l'envers.*
- **1.2 (A)** Un code PIN a 4 chiffres choisis au hasard. Quelle est la probabilité que ses 4 chiffres soient tous différents ? *Indication : l'ordre compte ; comptez sans répétition au numérateur.*
- **1.3 (B)** Un portefeuille compte 50 contrats, dont 5 ont eu un sinistre. On en tire 4 au hasard. Quelle est la probabilité qu'exactement un des 4 soit sinistré ? *Indication : choisissez séparément le contrat sinistré et les 3 autres.*
- **1.4 (B)** Au Loto, on choisit 6 numéros parmi 49. Quelle est la probabilité d'avoir les 6 bons numéros ? Vérifiez avec `comb`.
- **1.5 (C, type examen)** Un assureur couvre un client en auto et en habitation. Soit A = « sinistre auto dans l'année » et H = « sinistre habitation dans l'année », avec P(A) = 0,10, P(H) = 0,05 et P(A ∩ H) = 0,02. Calculez la probabilité : (a) d'au moins un sinistre ; (b) d'aucun sinistre ; (c) d'exactement un sinistre ; (d) d'un sinistre auto seulement. *Indication : un diagramme de Venn avec les quatre zones aide beaucoup.*

### Fiche mémo : Chapitre 1

- **Ω** = tous les résultats possibles ; un événement est une partie de Ω.
- **ou** = ∪ ; **et** = ∩ ; **non** = barre.
- P(Ā) = 1 − P(A). « Au moins un » : passez par le contraire.
- P(A ∪ B) = P(A) + P(B) − P(A ∩ B).
- Équiprobabilité : P(A) = favorables / possibles.
- Ordre et répétition : n^k^ ; ordre sans répétition : n!/(n − k)! ; sans ordre ni répétition : C(n,k).
- Python : `math.comb`, `math.perm`, `math.factorial`.

## Chapitre 2. Probabilités conditionnelles, indépendance, Bayes

Conditionner, c'est mettre à jour une probabilité quand on reçoit une information. La formule de Bayes fait le chemin inverse : elle remonte d'un effet observé (un sinistre) vers sa cause probable (le profil du client).

### Pourquoi c'est utile

Un assureur ne connaît pas le « vrai » niveau de risque d'un nouveau client. Après un sinistre, il révise son jugement : ce client a plus de chances d'être un conducteur à risque. C'est exactement le principe du bonus-malus, et c'est un calcul de Bayes. En finance, on conditionne de la même façon : probabilité de défaut d'un emprunteur sachant sa note de crédit.

### 2.1 Probabilité conditionnelle

&#91;N1\] P(A | B) se lit « probabilité de A sachant B ». On sait que B s'est produit, et on se demande quelle chance il reste à A. Concrètement, l'univers se réduit à B.

&#91;N2\] Pour P(B) > 0 :

$$
P(A \mid B) = \frac{P(A \cap B)}{P(B)}
$$

On divise par P(B) pour que la nouvelle probabilité de l'univers réduit vaille 1.

**Exemple** : on lance un dé. Sachant que le résultat est pair (B = {2, 4, 6}), quelle est la probabilité d'avoir un 6 ? P(A ∩ B) = P({6}) = 1/6 et P(B) = 1/2, donc P(A | B) = (1/6)/(1/2) = 1/3. Intuitivement : il reste 3 résultats possibles, dont un seul est 6.

&#91;N3\] Pour B fixé, l'application A ↦ P(A | B) est elle-même une probabilité. Toutes les règles du chapitre 1 restent donc vraies « sachant B », par exemple P(Ā | B) = 1 − P(A | B).

### 2.2 Formule des probabilités composées

&#91;N2\] En multipliant la définition par P(B), on obtient la probabilité que deux événements arrivent ensemble, ou en chaîne :

$$
P(A \cap B) = P(B)\,P(A \mid B) \qquad P(A_1 \cap A_2 \cap A_3) = P(A_1)\,P(A_2 \mid A_1)\,P(A_3 \mid A_1 \cap A_2)
$$

**Exemple** : on tire 2 boules sans remise dans une urne de 3 rouges et 2 bleues. P(R1 ∩ R2) = P(R1) × P(R2 | R1) = 3/5 × 2/4 = 3/10. On retrouve la réponse B3 du test, sans dénombrement.

### 2.3 Formule des probabilités totales

&#91;N1\] Pour calculer la probabilité d'un événement, on découpe le monde en cas qui ne se chevauchent pas, on calcule dans chaque cas, puis on fait la moyenne pondérée.

&#91;N2\] Si B₁, …, B~n~ forment une **partition** de Ω (deux à deux incompatibles, et leur réunion fait Ω) :

$$
P(A) = \sum_{i=1}^{n} P(B_i)\,P(A \mid B_i)
$$

Le cas le plus courant utilise B et son contraire : P(A) = P(B) P(A | B) + P(B̄) P(A | B̄).

Sur un arbre de probabilités, chaque chemin donne un produit, et la formule additionne les chemins qui mènent à A :

```text
                   P(A|B) ---- A      chemin : P(B) x P(A|B)
       P(B) --- B
      /            P(Ā|B) --- Ā
Depart
      \            P(A|B̄) ---- A      chemin : P(B̄) x P(A|B̄)
       P(B̄) --- B̄
                   P(Ā|B̄) --- Ā

P(A) = somme des chemins qui menent a A
```

### 2.4 La formule de Bayes

&#91;N1\] Bayes inverse le conditionnement : on connaît P(effet | cause), on veut P(cause | effet).

&#91;N2\] Elle s'obtient en écrivant P(A ∩ B) de deux façons, puis en développant P(A) par les probabilités totales :

$$
P(B_j \mid A) = \frac{P(B_j)\,P(A \mid B_j)}{P(A)} = \frac{P(B_j)\,P(A \mid B_j)}{\sum_{i=1}^{n} P(B_i)\,P(A \mid B_i)}
$$

Le vocabulaire : P(B~j~) est la probabilité **a priori** (avant l'information), P(B~j~ | A) la probabilité **a posteriori** (après l'information), et P(A | B~j~) la **vraisemblance**.

**Retour sur la question B4 du test.** M = « malade », T = « test positif ». P(M) = 0,01, P(T | M) = 0,95, P(T | M̄) = 0,05.

$$
P(M \mid T) = \frac{0{,}01 \times 0{,}95}{0{,}01 \times 0{,}95 + 0{,}99 \times 0{,}05} = \frac{0{,}0095}{0{,}059} \approx 0{,}161
$$

Pourquoi si peu ? Sur 10 000 personnes, 100 sont malades et 95 sont détectées. Mais parmi les 9 900 saines, 495 sont aussi positives. Sur 590 positifs, seuls 95 sont malades. Quand un phénomène est rare, la plupart des alertes sont de fausses alertes. C'est aussi vrai pour la détection de fraude.

### 2.5 Indépendance

&#91;N1\] A et B sont indépendants si savoir que B s'est produit ne change rien à la probabilité de A.

&#91;N2\] La définition officielle, et sa traduction quand P(B) > 0 :

$$
A \text{ et } B \text{ indépendants} \iff P(A \cap B) = P(A)\,P(B) \iff P(A \mid B) = P(A)
$$

Si A et B sont indépendants, leurs contraires le sont aussi (A et B̄, Ā et B̄).

Pour n événements, l'**indépendance mutuelle** exige que la formule du produit soit vraie pour **toute** sous-famille. On en déduit la formule la plus utilisée en assurance :

$$
P(\text{au moins un sinistre parmi } n \text{ contrats indépendants}) = 1 - (1-p)^n
$$

**Exemple** : 10 contrats indépendants, chacun avec une probabilité de sinistre p = 0,05. P(au moins un sinistre) = 1 − 0,95^10^ ≈ 0,401.

**Indépendant n'est pas incompatible.** Deux événements incompatibles de probabilités non nulles sont toujours **dépendants** : si l'un arrive, l'autre devient impossible.

&#91;N3\] L'indépendance deux à deux ne suffit pas pour l'indépendance mutuelle. Avec deux pièces, prenez A = « pile à la 1re », B = « pile à la 2e », C = « les deux résultats sont identiques ». Chaque paire est indépendante, mais P(A ∩ B ∩ C) = 1/4 ≠ P(A)P(B)P(C) = 1/8.

### Exemple résolu : Le raisonnement du bonus-malus

**Énoncé** : un portefeuille auto compte 70 % de « bons » conducteurs, qui ont un sinistre dans l'année avec probabilité 0,05, et 30 % de conducteurs « à risque », avec probabilité 0,20. Un client déclare un sinistre. Quelle est la probabilité qu'il soit un conducteur à risque ?

**Étape 1 : Nommer.** R = « conducteur à risque », S = « sinistre dans l'année ». P(R) = 0,3, P(S | R) = 0,20, P(S | R̄) = 0,05.

**Étape 2 : Probabilités totales.**

$$
P(S) = 0{,}3 \times 0{,}20 + 0{,}7 \times 0{,}05 = 0{,}060 + 0{,}035 = 0{,}095
$$

**Étape 3 : Bayes.**

$$
P(R \mid S) = \frac{0{,}060}{0{,}095} \approx 0{,}632
$$

**Interprétation** : avant le sinistre, ce client avait 30 % de chances d'être à risque. Après, il en a 63 %. L'information « sinistre » a doublé cette probabilité, ce qui justifie une majoration de prime.

### En Python

```python
import numpy as np
rng = np.random.default_rng(42)

# Calcul exact
p_R, p_S_R, p_S_nonR = 0.3, 0.20, 0.05
p_S = p_R * p_S_R + (1 - p_R) * p_S_nonR
print(p_S, p_R * p_S_R / p_S)            # 0.095  0.6316

# Double verification : on simule 1 million de clients
n = 1_000_000
risque = rng.random(n) < p_R                         # True si conducteur a risque
proba_sinistre = np.where(risque, p_S_R, p_S_nonR)
sinistre = rng.random(n) < proba_sinistre
print(sinistre.mean())                    # proche de 0.095
print(risque[sinistre].mean())            # proche de 0.632 : parmi les sinistres, part des "a risque"
```

La ligne `risque[sinistre].mean()` est la traduction directe de P(R | S) : on ne garde que les clients sinistrés, et on regarde la proportion de conducteurs à risque parmi eux.

### Erreurs fréquentes

- **Confondre P(A | B) et P(B | A).** « 95 % des malades sont positifs » ne veut pas dire « 95 % des positifs sont malades ». On appelle cette erreur le sophisme du procureur.
- Oublier un cas dans la formule des probabilités totales : les B~i~ doivent couvrir tout Ω.
- Multiplier des probabilités sans vérifier l'indépendance.
- Croire que « incompatibles » signifie « indépendants ».
- Écrire P(A | B) = P(A ∩ B) / P(A) : on divise toujours par la probabilité de ce qu'on **sait** (ici B).

### Exercices

- **2.1 (A)** P(A) = 0,5 et P(B | A) = 0,4. Calculez P(A ∩ B).
- **2.2 (A)** A et B sont indépendants, P(A) = 0,2 et P(B) = 0,6. Calculez P(A ∩ B), P(A ∪ B) et P(A | B).
- **2.3 (B)** On lance deux dés. Sachant qu'au moins un dé montre 6, quelle est la probabilité d'un double 6 ? *Indication : comptez les couples qui contiennent au moins un 6 ; la réponse n'est pas 1/6.*
- **2.4 (B)** Un assureur a trois agences. A1 émet 40 % des contrats, A2 35 % et A3 25 %. Les taux de fraude sont 2 %, 3 % et 5 %. (a) Quelle est la probabilité qu'un contrat pris au hasard soit frauduleux ? (b) Un contrat frauduleux est découvert : quelle est la probabilité qu'il vienne de A3 ?
- **2.5 (C, type examen)** Un portefeuille compte 20 % de jeunes conducteurs (probabilité annuelle de sinistre 0,25) et 80 % de conducteurs expérimentés (probabilité 0,08). Pour un conducteur donné, les années sont indépendantes. (a) Calculez la probabilité qu'un assuré ait un sinistre en année 1. (b) Un assuré a eu un sinistre en année 1 : quelle est la probabilité qu'il soit jeune ? (c) Calculez la probabilité qu'il ait un sinistre en année 2 sachant qu'il en a eu un en année 1. (d) Comparez avec (a) : les sinistres des années 1 et 2 sont-ils indépendants pour un assuré pris au hasard ? Qu'en concluez-vous pour la tarification ? *Indication pour (c) : appliquez les probabilités totales à P(S1 ∩ S2) en conditionnant par la classe.*

### Fiche mémo : Chapitre 2

- P(A | B) = P(A ∩ B) / P(B) : on divise par ce qu'on sait.
- P(A ∩ B) = P(B) P(A | B).
- Probabilités totales : P(A) = Σ P(B~i~) P(A | B~i~), avec (B~i~) une partition.
- Bayes : P(B~j~ | A) = P(B~j~) P(A | B~j~) / P(A). A priori → a posteriori.
- Indépendance : P(A ∩ B) = P(A) P(B), ou P(A | B) = P(A).
- n contrats indépendants : P(au moins un sinistre) = 1 − (1 − p)^n^.
- Python : `masque.mean()` estime une probabilité ; `x[condition].mean()` estime une probabilité conditionnelle.

## Chapitre 3. Variables aléatoires : F, densité, survie, quantile

Ce chapitre est le pont vers les mesures de risque. La fonction de répartition F répond à « quelle est la probabilité de perdre au plus x ? », la fonction de survie S à « quelle est la probabilité de perdre plus que x ? », et le quantile fait le chemin inverse : « quel montant x correspond à une probabilité donnée ? ». Ce quantile, c'est la VaR.

### Pourquoi c'est utile

Un assureur ne s'intéresse pas aux résultats « pile ou face », mais à des **montants** : nombre de sinistres, coût d'un sinistre, perte d'un portefeuille. Une variable aléatoire transforme chaque scénario en un nombre. Sa loi dit quelles valeurs sont probables, et c'est elle qu'on utilise pour tarifer et pour fixer le capital.

### 3.1 Qu'est-ce qu'une variable aléatoire ?

&#91;N1\] Une variable aléatoire X est un nombre qui dépend du hasard : le nombre de sinistres d'un portefeuille cette année, le montant d'un sinistre, la perte d'un portefeuille d'actions en un jour.

&#91;N2\] Formellement, c'est une fonction X : Ω → ℝ qui associe un nombre réel à chaque résultat. On distingue :

- les variables **discrètes**, qui prennent des valeurs isolées que l'on peut énumérer (0, 1, 2, …) : un nombre de sinistres ;
- les variables **continues**, qui prennent toutes les valeurs d'un intervalle : un montant, une durée ;
- les variables **mixtes** \[N3\], qui mélangent les deux : la perte annuelle d'un assuré, qui vaut exactement 0 s'il n'a pas de sinistre, et un montant continu sinon.

### 3.2 Variable discrète : la loi de probabilité

&#91;N2\] La loi d'une variable discrète est la liste des valeurs possibles x~k~ avec leurs probabilités p(x~k~) = P(X = x~k~). Elle doit vérifier :

$$
p(x_k) \ge 0 \quad \text{pour tout } k \qquad \text{et} \qquad \sum_k p(x_k) = 1
$$

**Exemple fil rouge** : N est le nombre de sinistres d'un assuré dans l'année.

| k | 0 | 1 | 2 | 3 |
| --- | --- | --- | --- | --- |
| P(N = k) | 0,60 | 0,25 | 0,10 | 0,05 |
| F(k) = P(N ≤ k) | 0,60 | 0,85 | 0,95 | 1,00 |

La somme des probabilités fait bien 1. La troisième ligne s'obtient en additionnant au fur et à mesure : c'est la fonction de répartition.

### 3.3 La fonction de répartition F

&#91;N1\] F(x) est la probabilité que la perte ne dépasse pas x. Elle part de 0 (tout à gauche) et monte jusqu'à 1 (tout à droite).

&#91;N2\] La définition, valable pour toute variable :

$$
F_X(x) = P(X \le x) \qquad x \in \mathbb{R}
$$

Ses quatre propriétés caractéristiques :

1. F est **croissante** : perdre au plus 200 est plus probable que perdre au plus 100.
2. F tend vers 0 en −∞ et vers 1 en +∞.
3. F est **continue à droite**.
4. Pour a < b : P(a < X ≤ b) = F(b) − F(a).

Pour une variable discrète, F est **en escalier** : elle est constante entre deux valeurs possibles et fait un saut de hauteur P(X = x~k~) en chaque x~k~. Dans l'exemple fil rouge, F(1,7) = F(1) = 0,85, car N ne prend aucune valeur entre 1 et 1,7.

**Attention aux inégalités strictes en discret** : P(N < 2) = P(N ≤ 1) = 0,85, mais P(N ≤ 2) = 0,95. Pour une variable continue, cette distinction disparaît.

### 3.4 Variable continue : la densité f

&#91;N1\] Pour une variable continue, la probabilité d'une valeur exacte est nulle (la probabilité qu'un sinistre coûte exactement 1 234,567… FCFA est 0). On raisonne donc sur des intervalles, et la densité f indique où les valeurs se concentrent.

&#91;N2\] X a pour densité f si f ≥ 0, si l'intégrale de f sur ℝ vaut 1, et si :

$$
F_X(x) = \int_{-\infty}^{x} f_X(t)\,dt \qquad f_X(x) = F_X'(x) \qquad P(a < X \le b) = \int_a^b f_X(t)\,dt = F_X(b) - F_X(a)
$$

Une densité n'est **pas** une probabilité : elle peut dépasser 1. C'est l'aire sous la courbe qui est une probabilité.

**Exemple central : la loi exponentielle.** X suit une loi exponentielle de paramètre λ > 0 si :

$$
f_X(x) = \lambda e^{-\lambda x} \quad (x \ge 0) \qquad F_X(x) = 1 - e^{-\lambda x} \quad (x \ge 0)
$$

Sa moyenne vaut 1/λ (chapitre 4). On l'utilise pour des montants de sinistres « modérés » et pour des durées d'attente.

### 3.5 La fonction de survie S

&#91;N1\] S(x) est la probabilité que la perte **dépasse** x. C'est la question naturelle du gestionnaire de risques : « quelle est la probabilité que la perte dépasse mon capital ? », ou pour un réassureur : « quelle est la probabilité qu'un sinistre dépasse le seuil à partir duquel je paie ? ».

&#91;N2\] Sa définition et ses propriétés, qui sont celles de F « à l'envers » :

$$
S_X(x) = P(X > x) = 1 - F_X(x)
$$

S est décroissante, vaut 1 en −∞ et 0 en +∞. Pour la loi exponentielle, S(x) = e^−λx^ : la probabilité de dépasser x décroît exponentiellement.

La vitesse à laquelle S tend vers 0 s'appelle la **queue de distribution**. Elle est au cœur de la gestion des risques : une queue qui décroît lentement signifie que les très gros sinistres ne sont pas si rares. Le chapitre 5 comparera les queues exponentielles et les queues « lourdes » (loi de Pareto).

&#91;N3\] **Absence de mémoire de la loi exponentielle.** Pour s, t ≥ 0 :

$$
P(X > s + t \mid X > s) = \frac{S(s+t)}{S(s)} = \frac{e^{-\lambda(s+t)}}{e^{-\lambda s}} = e^{-\lambda t} = P(X > t)
$$

Savoir qu'un sinistre dépasse déjà s ne change pas la loi de son « excédent » au-delà de s. Cette propriété simplifiera beaucoup le calcul de la TVaR exponentielle dans la Partie 3.

### 3.6 Le quantile : la clé de la VaR

&#91;N1\] Le quantile fait le chemin inverse de F. F prend un montant et donne une probabilité ; le quantile prend une probabilité α et donne le montant q~α~ tel que « la perte ne dépasse q~α~ qu'avec une probabilité 1 − α ».

**Lecture graphique, à retenir** : on trace la courbe de F, on place α sur l'axe vertical, on trace une droite horizontale jusqu'à la courbe, puis on descend verticalement sur l'axe des x. L'abscisse lue est q~α~.

```text
 F(x)
 1 |                        _________________
   |                 ______/
 a |- - - - - - - - -/            <- 1) on part de alpha sur l'axe vertical
   |            ____/|
   |       ____/     |            <- 2) on rejoint la courbe de F
   |   ___/          |
   | _/              |            <- 3) on descend sur l'axe des x
 0 +-----------------+------------------> x
                    q_a = VaR
```

&#91;N2\] **Cas continu et strictement croissant** : F est une bijection, et le quantile est sa réciproque. On résout F(x) = α.

$$
q_\alpha = F_X^{-1}(\alpha) \iff F_X(q_\alpha) = \alpha \iff S_X(q_\alpha) = 1 - \alpha
$$

Pour la loi exponentielle, 1 − e^−λq^ = α donne :

$$
q_\alpha = -\frac{\ln(1-\alpha)}{\lambda}
$$

**Cas général, indispensable pour les lois discrètes** : F a des sauts et des paliers, donc l'équation F(x) = α peut n'avoir aucune solution (α tombe dans un saut) ou une infinité (α tombe sur un palier). On définit alors l'**inverse généralisé** :

$$
q_\alpha = F_X^{-1}(\alpha) = \inf\{\, x \in \mathbb{R} : F_X(x) \ge \alpha \,\}
$$

En mots : **q~α~ est la plus petite valeur x pour laquelle F(x) atteint ou dépasse α**. En pratique, en discret, on parcourt le tableau de F de gauche à droite et on s'arrête à la première valeur où F(x) ≥ α.

**Exemple fil rouge** (F(0) = 0,60 ; F(1) = 0,85 ; F(2) = 0,95 ; F(3) = 1) :

| α | Première valeur où F ≥ α | q~α~ |
| --- | --- | --- |
| 0,50 | F(0) = 0,60 ≥ 0,50 | 0 |
| 0,85 | F(1) = 0,85 ≥ 0,85 | 1 |
| 0,90 | F(1) = 0,85 < 0,90 ; F(2) = 0,95 ≥ 0,90 | 2 |
| 0,99 | F(2) = 0,95 < 0,99 ; F(3) = 1 ≥ 0,99 | 3 |

Quelques quantiles portent un nom : la **médiane** q~0,5~, les **quartiles** q~0,25~ et q~0,75~, les **centiles** q~0,01~, …, q~0,99~.

**Le lien avec la Partie 3** : la Value-at-Risk au niveau α d'une perte X est exactement son quantile, VaR~α~(X) = q~α~. Savoir calculer un quantile, c'est savoir calculer une VaR.

### 3.7 Transformer une variable aléatoire

&#91;N2\] Si Y = aX + b avec a > 0 (changement d'unité, ajout d'une franchise fixe, inflation), alors :

$$
F_Y(y) = F_X\Big(\frac{y-b}{a}\Big) \qquad q_\alpha(Y) = a\,q_\alpha(X) + b
$$

Plus généralement, **le quantile d'une fonction croissante de X est la fonction appliquée au quantile de X**. Si les sinistres augmentent de 10 % à cause de l'inflation, la VaR augmente aussi de 10 %.

&#91;N3\] **Méthode d'inversion.** Si U suit une loi uniforme sur \[0, 1\], alors F⁻¹(U) a pour fonction de répartition F. C'est ainsi que les ordinateurs simulent n'importe quelle loi à partir de nombres uniformes. Pour l'exponentielle : X = −ln(1 − U)/λ.

### Exemple résolu : Coût d'un sinistre

**Énoncé** : le coût X d'un sinistre auto, en milliers de FCFA, suit une loi exponentielle de moyenne 500 (donc λ = 1/500). Calculez : (a) P(X ≤ 1 000) ; (b) P(X > 1 000) ; (c) P(500 < X ≤ 1 500) ; (d) la médiane ; (e) le quantile à 99 %.

**(a)** F(1 000) = 1 − e^−1000/500^ = 1 − e^−2^ ≈ 1 − 0,1353 = 0,8647.

**(b)** S(1 000) = e^−2^ ≈ 0,1353. Environ 13,5 % des sinistres dépassent 1 million de FCFA.

**(c)** F(1 500) − F(500) = e^−1^ − e^−3^ ≈ 0,3679 − 0,0498 = 0,3181.

**(d)** On résout F(x) = 0,5 : x = −500 · ln(0,5) = 500 · ln(2) ≈ 346,6. La médiane est inférieure à la moyenne (500) : quelques gros sinistres tirent la moyenne vers le haut.

**(e)** q~0,99~ = −500 · ln(0,01) = 500 · ln(100) ≈ 2 302,6. Un sinistre sur 100 coûte plus de 2,3 millions de FCFA.

### En Python

```python
import numpy as np
from scipy import stats
rng = np.random.default_rng(42)

# Loi continue : exponentielle de moyenne 500
X = stats.expon(scale=500)
print(X.cdf(1000), X.sf(1000))              # 0.8647 0.1353
print(X.cdf(1500) - X.cdf(500))             # 0.3181
print(X.median(), X.ppf(0.99))              # 346.57 2302.59

# Double verification par simulation
sim = X.rvs(size=1_000_000, random_state=rng)
print(np.mean(sim > 1000), np.quantile(sim, 0.99))   # ~0.135 ~2300

# Loi discrete : l'exemple fil rouge
N = stats.rv_discrete(values=([0, 1, 2, 3], [0.60, 0.25, 0.10, 0.05]))
print(N.cdf([0, 1, 2, 3]))                  # [0.6  0.85 0.95 1.  ]
print(N.ppf([0.50, 0.85, 0.90, 0.99]))      # [0. 1. 2. 3.]  (inverse generalise)
```

SciPy applique bien l'inverse généralisé dans `ppf` pour les lois discrètes. Pour tracer F et lire un quantile :

```python
import matplotlib.pyplot as plt
x = np.linspace(0, 3000, 500)
plt.plot(x, X.cdf(x), label='F(x)')
plt.axhline(0.99, ls='--', color='grey')
plt.axvline(X.ppf(0.99), ls='--', color='red', label='q_0.99')
plt.xlabel('Cout du sinistre (milliers FCFA)'); plt.legend(); plt.show()
```

### &#91;N3\] Pour aller plus loin : Une perte mixte

Un assuré n'a aucun sinistre avec probabilité 0,9. S'il en a un, son coût suit une loi exponentielle de moyenne 1 000. Sa perte annuelle X vaut donc 0 avec probabilité 0,9, et sinon un montant continu. Pour x ≥ 0 :

$$
F_X(x) = 0{,}9 + 0{,}1\,\big(1 - e^{-x/1000}\big)
$$

F saute de 0 à 0,9 en x = 0, puis monte continument. Conséquences pour les quantiles :

- q~0,85~ = 0, car F(0) = 0,9 ≥ 0,85 : la VaR à 85 % est **nulle** ;
- q~0,95~ : on résout 0,9 + 0,1(1 − e^−x/1000^) = 0,95, d'où e^−x/1000^ = 0,5 et q~0,95~ = 1 000 · ln(2) ≈ 693,1.

Leçon : pour un risque rare, une VaR à un niveau trop bas ne voit rien du tout. C'est l'une des raisons pour lesquelles on utilisera aussi la TVaR.

### Erreurs fréquentes

- Croire qu'une densité est une probabilité (elle peut valoir 3, par exemple).
- Oublier que F est en escalier pour une variable discrète, et confondre P(X < x) et P(X ≤ x).
- En discret, chercher à résoudre F(x) = α exactement : il faut la **première valeur où F(x) ≥ α**.
- Confondre S(x) et F(x) : une VaR à 99 % résout F(x) = 0,99, c'est-à-dire S(x) = 0,01, et non S(x) = 0,99.
- Oublier que la médiane n'est pas la moyenne quand la loi est asymétrique.

### Exercices

- **3.1 (A)** Le nombre N de sinistres suit la loi : P(N = 0) = 0,50 ; P(N = 1) = 0,30 ; P(N = 2) = 0,15 ; P(N = 3) = 0,05. (a) Donnez F(k) pour k = 0, 1, 2, 3. (b) Calculez P(N ≥ 2). (c) Calculez q~0,5~, q~0,9~, q~0,95~ et q~0,99~. *Indication : pour (c), cherchez la première valeur où F(k) ≥ α, et attention au cas d'égalité.*
- **3.2 (A)** X a pour densité f(x) = c · x² sur \[0, 2\], et 0 ailleurs. (a) Trouvez c. (b) Calculez F(x) sur \[0, 2\]. (c) Calculez P(X > 1). (d) Calculez la médiane. *Indication : l'intégrale de f sur \[0, 2\] doit valoir 1.*
- **3.3 (B)** Le coût d'un sinistre suit une loi exponentielle de moyenne θ inconnue. On observe que 20 % des sinistres dépassent 1 000. (a) Trouvez θ. (b) Calculez le quantile à 99 %. *Indication : écrivez S(1 000) = 0,2.*
- **3.4 (B)** Démontrez l'absence de mémoire de la loi exponentielle, puis interprétez-la pour un réassureur qui paie la part de chaque sinistre au-delà de 2 000.
- **3.5 (C, type examen)** Un contrat n'a aucun sinistre avec probabilité 0,8. S'il y a un sinistre, son coût suit une loi uniforme sur \[0 ; 10 000\]. Soit X la perte annuelle. (a) Donnez F~X~(x) pour x < 0, pour 0 ≤ x ≤ 10 000 et pour x > 10 000. (b) Calculez P(X > 5 000). (c) Calculez q~0,5~, q~0,9~ et q~0,99~. (d) Commentez la valeur de q~0,5~ pour un gestionnaire de risques. *Indication : la loi uniforme sur \[0 ; 10 000\] a pour fonction de répartition x/10 000.*

### Fiche mémo : Chapitre 3

- **F(x) = P(X ≤ x)** : croissante, de 0 à 1, en escalier si X est discrète.
- **S(x) = P(X > x) = 1 − F(x)** : probabilité de dépasser x.
- **Densité** : f = F', P(a < X ≤ b) = F(b) − F(a) = aire sous f.
- **Quantile** : q~α~ = plus petite valeur x telle que F(x) ≥ α. En continu : résoudre F(x) = α.
- **Exponentielle (λ)** : F = 1 − e^−λx^, S = e^−λx^, q~α~ = −ln(1 − α)/λ.
- **VaR~α~(X) = q~α~(X)**.
- Quantile de aX + b (a > 0) = a · q~α~ + b.
- Python : `.cdf`, `.sf`, `.ppf`, `np.quantile`.

## Chapitre 4. Espérance, variance, moments, fonction génératrice, inégalités

L'espérance donne la perte moyenne, c'est-à-dire la prime pure. La variance mesure l'écart autour de cette moyenne. Deux outils de ce chapitre préparent directement la TVaR : l'espérance calculée avec la fonction de survie, et l'espérance conditionnelle E\[X | X > c\].

### Pourquoi c'est utile

Si un contrat coûte en moyenne 50 000 FCFA de sinistres par an, l'assureur doit encaisser au moins 50 000 FCFA de prime : c'est la **prime pure**. Mais deux portefeuilles de même moyenne peuvent être très différents : l'un très stable, l'autre avec de rares catastrophes. La variance et les moments mesurent cette différence, et les inégalités donnent une borne sur le risque quand on ne connaît pas la loi.

### 4.1 L'espérance

&#91;N1\] L'espérance E\[X\] est la moyenne des valeurs de X, chacune pondérée par sa probabilité. Si on répétait l'expérience un très grand nombre de fois, la moyenne observée se rapprocherait de E\[X\].

&#91;N2\] Les deux formules de définition :

$$
\text{Discret : } E[X] = \sum_k x_k \, P(X = x_k) \qquad \text{Continu : } E[X] = \int_{-\infty}^{+\infty} x \, f_X(x)\,dx
$$

**Exemple fil rouge** (P(N = 0) = 0,60 ; P(N = 1) = 0,25 ; P(N = 2) = 0,10 ; P(N = 3) = 0,05) :

$$
E[N] = 0 \times 0{,}60 + 1 \times 0{,}25 + 2 \times 0{,}10 + 3 \times 0{,}05 = 0{,}60
$$

En moyenne, un assuré déclare 0,6 sinistre par an.

**Loi exponentielle de paramètre λ** : par l'intégration par parties de la Partie 0, E\[X\] = λ × 1/λ² = 1/λ.

**Théorème de transfert** : pour calculer l'espérance de g(X), inutile de chercher la loi de g(X). On applique g dans la formule :

$$
E[g(X)] = \sum_k g(x_k)\,P(X = x_k) \qquad E[g(X)] = \int_{-\infty}^{+\infty} g(x)\,f_X(x)\,dx
$$

**Linéarité**, toujours vraie, même si X et Y sont dépendantes :

$$
E[aX + b] = a\,E[X] + b \qquad E[X + Y] = E[X] + E[Y]
$$

**L'espérance par la fonction de survie.** Pour une variable **positive**, il existe une formule très utile en actuariat, qui n'utilise que S :

$$
X \ge 0 : \; E[X] = \int_0^{+\infty} S_X(x)\,dx \qquad N \in \{0, 1, 2, \dots\} : \; E[N] = \sum_{k=0}^{+\infty} P(N > k)
$$

Vérification sur le fil rouge : P(N > 0) + P(N > 1) + P(N > 2) = 0,40 + 0,15 + 0,05 = 0,60. Et pour l'exponentielle, l'intégrale de e^−λx^ vaut 1/λ : on retrouve la moyenne sans IPP.

&#91;N3\] Démonstration en continu : on écrit x comme l'intégrale de 1 entre 0 et x, puis on échange les intégrales (théorème de Fubini).

$$
E[X] = \int_0^{\infty} \Big(\int_0^{x} dt\Big) f(x)\,dx = \int_0^{\infty} \Big(\int_t^{\infty} f(x)\,dx\Big) dt = \int_0^{\infty} S(t)\,dt
$$

### 4.2 La variance et l'écart-type

&#91;N1\] La variance mesure à quel point X s'écarte en moyenne de son espérance. Plus elle est grande, plus le risque est « imprévisible ». L'écart-type σ est sa racine carrée : il s'exprime dans la même unité que X (en FCFA si X est en FCFA).

&#91;N2\] La définition, et la formule de calcul (dite de König-Huygens) que l'on utilise en pratique :

$$
\mathrm{Var}(X) = E\big[(X - E[X])^2\big] = E[X^2] - \big(E[X]\big)^2 \qquad \sigma_X = \sqrt{\mathrm{Var}(X)}
$$

Propriétés : Var(X) ≥ 0 ; Var(b) = 0 pour une constante ; et :

$$
\mathrm{Var}(aX + b) = a^2\,\mathrm{Var}(X) \qquad \sigma_{aX+b} = |a|\,\sigma_X
$$

Ajouter une constante ne change pas la dispersion. En revanche, Var(X + Y) = Var(X) + Var(Y) **seulement** si X et Y sont indépendantes (ou non corrélées, chapitre 6).

**Fil rouge** : E\[N²\] = 0² × 0,60 + 1² × 0,25 + 2² × 0,10 + 3² × 0,05 = 0,25 + 0,40 + 0,45 = 1,10. Donc Var(N) = 1,10 − 0,60² = 0,74 et σ ≈ 0,86.

**Exponentielle** : E\[X²\] = 2/λ² (Partie 0), donc Var(X) = 2/λ² − 1/λ² = 1/λ², et σ = 1/λ = E\[X\].

Le **coefficient de variation** CV = σ/E\[X\] compare des risques de tailles différentes. Il vaut 1 pour toute loi exponentielle.

### 4.3 Moments, asymétrie et aplatissement

&#91;N2\] Le moment d'ordre n est E\[X^n^\], le moment centré d'ordre n est E\[(X − μ)^n^\] avec μ = E\[X\]. La variance est le moment centré d'ordre 2.

&#91;N3\] Deux indicateurs de forme, sans unité :

$$
\text{Asymétrie (skewness)} : \gamma_1 = \frac{E[(X-\mu)^3]}{\sigma^3} \qquad \text{Aplatissement (kurtosis)} : \gamma_2 = \frac{E[(X-\mu)^4]}{\sigma^4}
$$

γ₁ > 0 signale une queue étirée vers la droite (quelques très gros sinistres) : c'est le cas typique en assurance. L'exponentielle a γ₁ = 2. La loi normale a γ₁ = 0 et γ₂ = 3 ; un γ₂ supérieur à 3 indique des valeurs extrêmes plus fréquentes que sous la loi normale, ce qu'on observe sur les rendements financiers.

### 4.4 La fonction génératrice des moments

&#91;N1\] C'est une « carte d'identité » de la loi : elle contient tous les moments et caractérise la loi. Elle sert surtout à trouver la loi d'une somme de variables indépendantes (chapitre 6).

&#91;N2\] Quand elle existe autour de t = 0 :

$$
M_X(t) = E\big[e^{tX}\big] \qquad M_X^{(n)}(0) = E[X^n] \qquad X, Y \text{ indépendantes} \Rightarrow M_{X+Y}(t) = M_X(t)\,M_Y(t)
$$

**Exemple** : pour l'exponentielle, pour t < λ :

$$
M_X(t) = \int_0^{\infty} e^{tx}\,\lambda e^{-\lambda x}\,dx = \frac{\lambda}{\lambda - t} \qquad M_X'(t) = \frac{\lambda}{(\lambda-t)^2} \Rightarrow M_X'(0) = \frac{1}{\lambda} = E[X]
$$

&#91;N3\] Pour une variable à valeurs entières, on utilise plutôt la fonction génératrice des probabilités G~N~(s) = E\[s^N^\]. On a G~N~'(1) = E\[N\], et P(N = k) = G~N~^k^(0)/k!. Elle servira pour le modèle collectif (chapitre 11).

### 4.5 Les inégalités de Markov et de Tchebychev

&#91;N1\] Ces inégalités bornent la probabilité d'une grosse perte en ne connaissant que la moyenne (Markov) ou la moyenne et l'écart-type (Tchebychev). Les bornes sont grossières, mais valables pour **toutes** les lois.

&#91;N2\] Pour a > 0 et k > 0 :

$$
\text{Markov } (X \ge 0) : P(X \ge a) \le \frac{E[X]}{a} \qquad \text{Tchebychev} : P\big(|X - \mu| \ge k\sigma\big) \le \frac{1}{k^2}
$$

**Exemple** : un sinistre coûte en moyenne 500. Markov garantit que P(X ≥ 5 000) ≤ 500/5 000 = 0,1, quelle que soit la loi. Si la loi est exponentielle, la vraie valeur est e^−10^ ≈ 0,000045 : la borne est très prudente. Tchebychev dit qu'au moins 75 % des valeurs sont à moins de 2σ de la moyenne (k = 2).

&#91;N3\] Markov se démontre en une ligne : X ≥ a · 1(X ≥ a), puis on prend l'espérance. Tchebychev est Markov appliqué à (X − μ)² avec a = k²σ².

### 4.6 Deux espérances qui préparent la TVaR

**L'espérance conditionnelle à un événement.** \[N1\] « Sachant que la perte dépasse c, combien perd-on en moyenne ? »

&#91;N2\] Pour un événement A de probabilité non nulle, et en particulier A = {X > c} :

$$
E[X \mid A] = \frac{E[X\,\mathbf{1}_A]}{P(A)} \qquad E[X \mid X > c] = \frac{1}{S_X(c)} \int_c^{+\infty} x\,f_X(x)\,dx
$$

Pour l'exponentielle, l'absence de mémoire donne directement E\[X | X > c\] = c + 1/λ : on dépasse c, puis on ajoute un excédent moyen de 1/λ.

**La prime stop-loss (ou prime avec franchise).** \[N1\] Avec une franchise d, l'assureur ne paie que la partie du sinistre au-dessus de d : (X − d)₊ = max(X − d, 0).

&#91;N2\] On généralise la formule de l'espérance par la survie :

$$
E\big[(X - d)_+\big] = \int_d^{+\infty} S_X(x)\,dx \qquad E\big[\min(X, L)\big] = \int_0^{L} S_X(x)\,dx
$$

La première formule est la prime pure d'un contrat avec franchise d. La seconde est celle d'un contrat plafonné à L. Ces deux formules interviendront dans la formule de la TVaR.

### Exemple résolu : Tarifer un contrat avec franchise

**Énoncé** : le coût X d'un sinistre (milliers de FCFA) suit une loi exponentielle de moyenne 500. (a) Calculez E\[X\], Var(X), σ et le CV. (b) Le contrat prévoit une franchise de 200 : calculez la prime pure. (c) Calculez le coût moyen des sinistres qui dépassent 1 000.

**(a)** λ = 1/500, donc E\[X\] = 500, Var(X) = 500² = 250 000, σ = 500 et CV = 1.

**(b)** La prime pure est E\[(X − 200)₊\] :

$$
E[(X-200)_+] = \int_{200}^{\infty} e^{-x/500}\,dx = 500\,e^{-200/500} = 500\,e^{-0{,}4} \approx 335{,}2
$$

La franchise fait baisser la prime pure de 500 à environ 335, soit −33 %.

**(c)** Par absence de mémoire, E\[X | X > 1 000\] = 1 000 + 500 = 1 500. Les sinistres supérieurs à 1 million coûtent en moyenne 1,5 million.

### En Python

```python
import numpy as np
from scipy import stats, integrate
rng = np.random.default_rng(42)

# Fil rouge discret
k = np.array([0, 1, 2, 3]); p = np.array([0.60, 0.25, 0.10, 0.05])
E = np.sum(k * p); E2 = np.sum(k**2 * p)
print(E, E2 - E**2)                          # 0.6 0.74

# Exponentielle de moyenne 500
X = stats.expon(scale=500)
print(X.mean(), X.var(), X.std())            # 500 250000 500
print(X.stats(moments='s'))                  # asymetrie = 2

# Prime stop-loss par integration numerique de S
stop_loss, _ = integrate.quad(X.sf, 200, np.inf)
print(stop_loss)                             # 335.16

# E[X | X > 1000] : integrale de x f(x) au-dela de 1000, divisee par S(1000)
num, _ = integrate.quad(lambda x: x * X.pdf(x), 1000, np.inf)
print(num / X.sf(1000))                      # 1500

# Double verification par simulation
sim = X.rvs(size=1_000_000, random_state=rng)
print(np.maximum(sim - 200, 0).mean())       # ~335
print(sim[sim > 1000].mean())                # ~1500
```

### Erreurs fréquentes

- Écrire E\[X²\] = (E\[X\])² : c'est faux dès que X n'est pas constante (la différence est justement la variance).
- Écrire Var(aX) = a Var(X) : le facteur sort **au carré**.
- Additionner les variances de variables dépendantes.
- Utiliser E\[X\] = ∫ S(x) dx pour une variable qui peut être négative : la formule exige X ≥ 0.
- Calculer E\[X | X > c\] sans diviser par P(X > c).

### Exercices

- **4.1 (A)** Avec la loi de l'exercice 3.1 (P(N = 0) = 0,50 ; 0,30 ; 0,15 ; 0,05), calculez E\[N\] de deux façons (définition et somme des P(N > k)), puis Var(N).
- **4.2 (A)** X a pour densité f(x) = 2x sur \[0, 1\]. Calculez E\[X\], E\[X²\] et Var(X).
- **4.3 (B)** Montrez que la loi uniforme sur \[a, b\] a pour espérance (a + b)/2 et pour variance (b − a)²/12.
- **4.4 (B)** Une perte X a une moyenne de 1 000 et un écart-type de 300. Donnez une borne supérieure de P(X ≥ 2 000) avec Markov, puis avec Tchebychev. Laquelle est la meilleure, et pourquoi ? *Indication : X ≥ 2 000 implique |X − 1 000| ≥ 1 000.*
- **4.5 (B)** Calculez la fonction génératrice des moments d'une variable de Bernoulli de paramètre p (X = 1 avec probabilité p, 0 sinon), et déduisez-en E\[X\] et Var(X).
- **4.6 (C, type examen)** Le coût X d'un sinistre suit une loi exponentielle de moyenne 2 000. (a) Donnez E\[X\] et Var(X). (b) Calculez la prime pure d'un contrat avec franchise de 500. (c) Calculez la prime pure d'un contrat plafonné à 5 000 (l'assureur paie min(X ; 5 000)). (d) Calculez E\[X | X > 4 000\]. (e) Vérifiez (b), (c) et (d) en Python par simulation.

### Fiche mémo : Chapitre 4

- E\[X\] = Σ x p(x) ou ∫ x f(x) dx. Linéaire : E\[aX + b\] = aE\[X\] + b, E\[X + Y\] = E\[X\] + E\[Y\].
- X ≥ 0 : E\[X\] = ∫₀^∞^ S(x) dx ; N entier : E\[N\] = Σ P(N > k).
- Var(X) = E\[X²\] − E\[X\]² ; Var(aX + b) = a² Var(X).
- Exponentielle (λ) : E = 1/λ, Var = 1/λ², CV = 1, M(t) = λ/(λ − t).
- Markov : P(X ≥ a) ≤ E\[X\]/a. Tchebychev : P(|X − μ| ≥ kσ) ≤ 1/k².
- E\[X | X > c\] = E\[X · 1(X > c)\] / S(c) ; exponentielle : c + 1/λ.
- Stop-loss : E\[(X − d)₊\] = ∫~d~^∞^ S(x) dx.
- Python : `.mean()`, `.var()`, `integrate.quad`, `sim[sim > c].mean()`.

## Corrigés des exercices : Parties 0 et 1

Chaque corrigé donne la méthode, puis le résultat. Tous les résultats numériques ont été vérifiés en Python, et le notebook contient le code de vérification de chaque exercice.

### Partie 0

**0.1** On sort la constante : 2 × Σ 0,8^k^ = 2 × 1/(1 − 0,8) = 2 × 5 = **10**.

**0.2** On isole l'exponentielle : e^−0,02x^ = 1 − 0,995 = 0,005. On prend le logarithme : −0,02x = ln(0,005), d'où x = −ln(0,005)/0,02 = ln(200)/0,02 ≈ 5,298/0,02 ≈ **264,9**.

**0.3** Première IPP avec u = x² et v' = e^−x^ : le crochet \[−x²e^−x^\] vaut 0 entre 0 et +∞, et il reste 2 fois l'intégrale de x · e^−x^. Celle-ci vaut 1 (exemple résolu avec λ = 1). Résultat : **2**, ce qui correspond à 2!/1³ dans la formule Gamma.

**0.4** e^−λ^ ne dépend pas de k, on le sort de la somme :

$$
\sum_{k=0}^{\infty} e^{-\lambda}\frac{\lambda^k}{k!} = e^{-\lambda}\sum_{k=0}^{\infty}\frac{\lambda^k}{k!} = e^{-\lambda}\,e^{\lambda} = 1
$$

C'est la preuve que la loi de Poisson est bien une loi de probabilité (chapitre 5).

**0.5** `stats.expon(scale=2)` donne P(X ≤ 3) = 1 − e^−1,5^ ≈ **0,777** et P(X > 3) = e^−1,5^ ≈ **0,223**.

**0.6** `np.mean(rng.integers(1, 7, size=100_000) == 6)` donne une valeur proche de 1/6 ≈ **0,167** (par exemple 0,164 ou 0,168 selon la graine).

**0.7** On simule deux tableaux de dés `d1` et `d2`, puis `np.mean(d1 + d2 == 7)` donne une valeur proche de **1/6**, comme en B1.

### Chapitre 1

**1.1** P(A ∩ B) = P(A) + P(B) − P(A ∪ B) = 0,4 + 0,3 − 0,5 = **0,2**. Puis P(A ∩ B̄) = P(A) − P(A ∩ B) = 0,4 − 0,2 = **0,2**.

**1.2** Nombre total de codes : 10⁴ = 10 000 (ordre et répétition). Codes à chiffres tous différents : 10 × 9 × 8 × 7 = 5 040. Probabilité : 5 040/10 000 = **0,504**.

**1.3** On choisit 4 contrats parmi 50 sans ordre : C(50,4) = 230 300 cas. Cas favorables : 1 sinistré parmi 5, et 3 non sinistrés parmi 45.

$$
P = \frac{\binom{5}{1}\binom{45}{3}}{\binom{50}{4}} = \frac{5 \times 14\,190}{230\,300} = \frac{70\,950}{230\,300} \approx 0{,}308
$$

Cette loi s'appelle la loi hypergéométrique (chapitre 5).

**1.4** Il y a C(49,6) = 13 983 816 grilles possibles, et une seule gagnante : P = 1/13 983 816 ≈ **7,15 × 10⁻⁸**.

**1.5** Le diagramme de Venn a quatre zones : auto seulement = 0,10 − 0,02 = 0,08 ; habitation seulement = 0,05 − 0,02 = 0,03 ; les deux = 0,02 ; aucun = le reste.

| Question | Calcul | Résultat |
| --- | --- | --- |
| (a) Au moins un sinistre | 0,10 + 0,05 − 0,02 | **0,13** |
| (b) Aucun sinistre | 1 − 0,13 | **0,87** |
| (c) Exactement un | 0,08 + 0,03 (ou 0,13 − 0,02) | **0,11** |
| (d) Auto seulement | 0,10 − 0,02 | **0,08** |

### Chapitre 2

**2.1** P(A ∩ B) = P(A) × P(B | A) = 0,5 × 0,4 = **0,2**.

**2.2** Indépendance : P(A ∩ B) = 0,2 × 0,6 = **0,12**. Réunion : 0,2 + 0,6 − 0,12 = **0,68**. Et P(A | B) = P(A) = **0,2**, car savoir B ne change rien.

**2.3** Soit D = « double 6 » et B = « au moins un 6 ». B contient 11 couples sur 36 : (6,1) à (6,6) et (1,6) à (5,6). D ∩ B = D = {(6,6)}.

$$
P(D \mid B) = \frac{1/36}{11/36} = \frac{1}{11} \approx 0{,}091
$$

Ce n'est pas 1/6 : l'information « au moins un 6 » ne dit pas lequel des deux dés le montre.

**2.4** (a) Probabilités totales : 0,40 × 0,02 + 0,35 × 0,03 + 0,25 × 0,05 = 0,0080 + 0,0105 + 0,0125 = **0,031**. (b) Bayes : P(A3 | fraude) = 0,0125/0,031 ≈ **0,403**. A3 émet 25 % des contrats mais concentre 40 % des fraudes : c'est là qu'il faut renforcer les contrôles.

**2.5** Notons J = « jeune », S1 et S2 les sinistres des années 1 et 2.

- (a) P(S1) = 0,2 × 0,25 + 0,8 × 0,08 = 0,050 + 0,064 = **0,114**.
- (b) P(J | S1) = 0,050/0,114 ≈ **0,439**. Avant le sinistre, 20 % de chances d'être jeune ; après, 44 %.
- (c) Pour une classe donnée, les années sont indépendantes, donc P(S1 ∩ S2 | J) = 0,25². Par les probabilités totales :

$$
P(S_1 \cap S_2) = 0{,}2 \times 0{,}25^2 + 0{,}8 \times 0{,}08^2 = 0{,}0125 + 0{,}00512 = 0{,}01762
$$

$$
P(S_2 \mid S_1) = \frac{P(S_1 \cap S_2)}{P(S_1)} = \frac{0{,}01762}{0{,}114} \approx 0{,}155
$$

- (d) P(S2 | S1) ≈ 0,155 > P(S2) = 0,114 : pour un assuré pris au hasard, les sinistres des deux années **ne sont pas indépendants**, alors qu'ils le sont à classe connue. C'est l'ignorance de la classe qui crée la dépendance : un sinistre passé révèle de l'information sur le profil. Conclusion pour la tarification : il est justifié d'augmenter la prime après un sinistre. C'est le fondement mathématique du bonus-malus.

### Chapitre 3

**3.1** (a) On cumule : F(0) = 0,50 ; F(1) = 0,80 ; F(2) = 0,95 ; F(3) = 1. (b) P(N ≥ 2) = 1 − F(1) = **0,20**. (c) On cherche la première valeur où F(k) ≥ α :

| α | Raisonnement | q~α~ |
| --- | --- | --- |
| 0,50 | F(0) = 0,50 ≥ 0,50 (égalité : on s'arrête) | **0** |
| 0,90 | F(1) = 0,80 < 0,90 ; F(2) = 0,95 ≥ 0,90 | **2** |
| 0,95 | F(2) = 0,95 ≥ 0,95 (égalité) | **2** |
| 0,99 | F(2) = 0,95 < 0,99 ; F(3) = 1 ≥ 0,99 | **3** |

**3.2** (a) L'intégrale de c · x² entre 0 et 2 vaut c × 8/3. Elle doit valoir 1, donc **c = 3/8**. (b) F(x) = intégrale de 3t²/8 entre 0 et x = **x³/8**. (c) P(X > 1) = 1 − F(1) = 1 − 1/8 = **7/8**. (d) On résout x³/8 = 0,5, donc x³ = 4 et la médiane vaut 4^1/3^ ≈ **1,587**.

**3.3** (a) S(1 000) = e^−1000/θ^ = 0,2, donc 1 000/θ = ln(5) et θ = 1 000/ln(5) ≈ **621,3**. (b) q~0,99~ = θ · ln(100) ≈ 621,3 × 4,605 ≈ **2 861**.

**3.4** La démonstration est celle du paragraphe 3.5 : le rapport S(s + t)/S(s) vaut e^−λt^. Interprétation : pour un sinistre qui dépasse 2 000, la part au-delà de 2 000, X − 2 000, suit la même loi exponentielle que X. Le réassureur est touché avec probabilité S(2 000), et quand il l'est, il paie en moyenne 1/λ, comme si le seuil n'existait pas. Pour des sinistres à queue lourde (Pareto), ce ne sera plus vrai : l'excédent moyen augmente avec le seuil.

**3.5** (a) La fonction de répartition par morceaux :

$$
F_X(x) = \begin{cases} 0 & x < 0 \\ 0{,}8 + 0{,}2\,\dfrac{x}{10\,000} & 0 \le x \le 10\,000 \\ 1 & x > 10\,000 \end{cases}
$$

(b) P(X > 5 000) = 1 − F(5 000) = 1 − (0,8 + 0,1) = **0,1**. (c) q~0,5~ = **0**, car F(0) = 0,8 ≥ 0,5. Pour q~0,9~ : 0,8 + 0,2x/10 000 = 0,9 donne x = **5 000**. Pour q~0,99~ : 0,8 + 0,2x/10 000 = 0,99 donne x = **9 500**. (d) Une VaR à 50 % nulle ne dit rien du risque : elle signifie seulement qu'une année sur deux au moins, il n'y a pas de perte. Il faut des niveaux α élevés, et même alors, la VaR ne dit pas combien on perd au-delà du quantile. C'est le rôle de la TVaR (Partie 3).

### Chapitre 4

**4.1** Définition : E\[N\] = 0 × 0,50 + 1 × 0,30 + 2 × 0,15 + 3 × 0,05 = **0,75**. Par la survie : P(N > 0) + P(N > 1) + P(N > 2) = 0,50 + 0,20 + 0,05 = **0,75**. Variance : E\[N²\] = 0,30 + 4 × 0,15 + 9 × 0,05 = 1,35, donc Var(N) = 1,35 − 0,75² = **0,7875**.

**4.2** E\[X\] = intégrale de 2x² entre 0 et 1 = **2/3**. E\[X²\] = intégrale de 2x³ = **1/2**. Var(X) = 1/2 − 4/9 = **1/18** ≈ 0,056.

**4.3** La densité vaut 1/(b − a) sur \[a, b\].

$$
E[X] = \frac{1}{b-a}\int_a^b x\,dx = \frac{b^2-a^2}{2(b-a)} = \frac{a+b}{2} \qquad E[X^2] = \frac{b^3-a^3}{3(b-a)} = \frac{a^2+ab+b^2}{3}
$$

$$
\mathrm{Var}(X) = \frac{a^2+ab+b^2}{3} - \frac{(a+b)^2}{4} = \frac{4a^2+4ab+4b^2-3a^2-6ab-3b^2}{12} = \frac{(b-a)^2}{12}
$$

**4.4** Markov : P(X ≥ 2 000) ≤ 1 000/2 000 = **0,5**. Tchebychev : X ≥ 2 000 implique |X − 1 000| ≥ 1 000, et 1 000 = (10/3)σ, donc P(X ≥ 2 000) ≤ (3/10)² = **0,09**. Tchebychev est bien meilleur, car il utilise une information en plus : l'écart-type.

**4.5** M(t) = E\[e^tX^\] = (1 − p) × e⁰ + p × e^t^ = 1 − p + p e^t^. Alors M'(t) = M''(t) = p e^t^, donc E\[X\] = M'(0) = **p**, E\[X²\] = M''(0) = p, et Var(X) = p − p² = **p(1 − p)**.

**4.6** λ = 1/2 000.

| Question | Formule | Résultat |
| --- | --- | --- |
| (a) Moyenne, variance | 1/λ, 1/λ² | **2 000** ; **4 000 000** (σ = 2 000) |
| (b) Franchise 500 | ∫₅₀₀^∞^ e^−x/2000^ dx = 2 000 e^−0,25^ | **≈ 1 557,6** |
| (c) Plafond 5 000 | ∫₀^5000^ e^−x/2000^ dx = 2 000 (1 − e^−2,5^) | **≈ 1 835,8** |
| (d) E\[X ∣ X > 4 000\] | 4 000 + 1/λ | **6 000** |

(e) Code de vérification :

```python
Y = stats.expon(scale=2000)
s = Y.rvs(size=1_000_000, random_state=rng)
print(np.maximum(s - 500, 0).mean())    # ~1558
print(np.minimum(s, 5000).mean())       # ~1836
print(s[s > 4000].mean())               # ~6000
```

Remarque utile : franchise et plafond au même seuil se complètent, car X = min(X, d) + (X − d)₊. Donc E\[min(X, 500)\] = 2 000 − 1 557,6 = 442,4.

## Partie 2. Les outils

## Chapitre 5. Les lois usuelles (1) : lois discrètes

Six lois discrètes suffisent pour modéliser presque tous les **nombres** rencontrés en gestion des risques : nombre de sinistres, de défauts, d'années avant un événement. Pour chacune, il faut savoir reconnaître la situation, écrire P(X = k), et connaître E\[X\] et Var(X).

### Pourquoi c'est utile

Choisir une loi, c'est choisir un modèle du monde. Si un actuaire modélise le nombre de sinistres par une loi de Poisson alors que les données sont plus dispersées, il sous-estime le risque de « mauvaise année », donc la VaR. Reconnaître la bonne loi est la première étape de tout calcul de risque.

### 5.1 Bernoulli : oui ou non

&#91;N1\] Une seule expérience à deux issues : le contrat a un sinistre ou non, l'emprunteur fait défaut ou non.

&#91;N2\] X vaut 1 avec probabilité p et 0 avec probabilité 1 − p. On a E\[X\] = p et Var(X) = p(1 − p) (exercice 4.5).

### 5.2 Binomiale : nombre de succès sur n essais indépendants

&#91;N1\] On répète n fois une Bernoulli de même paramètre p, de façon indépendante, et on compte les succès. Exemple : nombre de sinistres parmi n contrats indépendants ayant chacun une probabilité p de sinistre.

&#91;N2\] On choisit les k contrats sinistrés parmi n (C(n,k) façons), chacun avec probabilité p, et les n − k autres sans sinistre :

$$
P(X = k) = \binom{n}{k}\,p^k\,(1-p)^{n-k} \quad (k = 0, \dots, n) \qquad E[X] = np \qquad \mathrm{Var}(X) = np(1-p)
$$

L'espérance et la variance viennent du fait que X est une somme de n Bernoulli indépendantes.

### 5.3 Poisson : nombre d'événements rares

&#91;N1\] La loi de Poisson compte des événements qui arrivent « au hasard » dans le temps, de façon indépendante, avec une fréquence moyenne λ : nombre de sinistres d'un portefeuille dans l'année, nombre d'appels à un centre de gestion par heure.

&#91;N2\] Pour λ > 0 :

$$
P(X = k) = e^{-\lambda}\,\frac{\lambda^k}{k!} \quad (k = 0, 1, 2, \dots) \qquad E[X] = \mathrm{Var}(X) = \lambda
$$

Trois propriétés à retenir :

- **Moyenne = variance.** Si les données ont une variance nettement plus grande que leur moyenne (on parle de surdispersion), la Poisson ne convient pas : on passe à la binomiale négative.
- **Stabilité par somme** : si X et Y sont indépendantes de lois de Poisson de paramètres λ et μ, alors X + Y suit une loi de Poisson de paramètre λ + μ.
- **Approximation de la binomiale** : si n est grand et p petit, la binomiale (n, p) est proche de la Poisson de paramètre λ = np. On l'appelle la loi des événements rares.

&#91;N3\] Pourquoi l'approximation marche : avec p = λ/n, C(n,k) p^k^ (1 − p)^n−k^ tend vers e^−λ^ λ^k^/k! quand n tend vers l'infini, car (1 − λ/n)^n^ tend vers e^−λ^.

### 5.4 Géométrique : attendre le premier succès

&#91;N1\] On répète des essais indépendants de probabilité p jusqu'au premier succès, et X est le rang de ce succès. Exemple : nombre d'années jusqu'au premier sinistre d'un contrat.

&#91;N2\] Pour k = 1, 2, … : il faut k − 1 échecs, puis un succès.

$$
P(X = k) = (1-p)^{k-1}\,p \qquad P(X > k) = (1-p)^k \qquad E[X] = \frac{1}{p} \qquad \mathrm{Var}(X) = \frac{1-p}{p^2}
$$

Avec p = 0,1, on attend en moyenne 10 ans avant le premier sinistre. Comme l'exponentielle, la géométrique est sans mémoire : P(X > s + t | X > s) = P(X > t).

### 5.5 Binomiale négative : des sinistres plus dispersés

&#91;N1\] En pratique, les assurés n'ont pas tous le même risque. Mélanger des profils différents augmente la dispersion du nombre de sinistres (chapitre 2, exercice 2.5). La binomiale négative capte cette hétérogénéité : sa variance dépasse sa moyenne.

&#91;N2\] Dans la paramétrisation de SciPy, X compte les échecs avant le r-ième succès (r > 0, p dans \]0, 1\[) :

$$
P(X = k) = \binom{k + r - 1}{k}\,p^r (1-p)^k \qquad E[X] = \frac{r(1-p)}{p} \qquad \mathrm{Var}(X) = \frac{r(1-p)}{p^2} > E[X]
$$

&#91;N3\] Si, pour chaque assuré, le nombre de sinistres suit une Poisson de paramètre Λ, et que Λ varie d'un assuré à l'autre selon une loi Gamma, alors le nombre de sinistres d'un assuré pris au hasard suit une binomiale négative. C'est le modèle de base de la tarification « bonus-malus ».

### 5.6 Hypergéométrique : tirages sans remise

&#91;N2\] On tire n éléments sans remise dans une population de N éléments, dont K « marqués » (fraudes, contrats sinistrés), et X compte les éléments marqués tirés. C'est l'exercice 1.3 :

$$
P(X = k) = \frac{\binom{K}{k}\binom{N-K}{n-k}}{\binom{N}{n}} \qquad E[X] = n\,\frac{K}{N}
$$

Quand N est très grand devant n, tirer avec ou sans remise revient presque au même, et la loi est proche de la binomiale (n, K/N).

### Récapitulatif des lois discrètes

| Loi | Situation type | P(X = k) | E\[X\] | Var(X) | SciPy |
| --- | --- | --- | --- | --- | --- |
| Bernoulli (p) | Un sinistre ou non | p si k = 1, 1 − p si k = 0 | p | p(1 − p) | `stats.bernoulli(p)` |
| Binomiale (n, p) | Sinistres sur n contrats indépendants | C(n,k) p^k^ (1 − p)^n−k^ | np | np(1 − p) | `stats.binom(n, p)` |
| Poisson (λ) | Nombre d'événements rares | e^−λ^ λ^k^/k! | λ | λ | `stats.poisson(lam)` |
| Géométrique (p) | Rang du premier succès | (1 − p)^k−1^ p | 1/p | (1 − p)/p² | `stats.geom(p)` |
| Binomiale négative (r, p) | Sinistres surdispersés | C(k+r−1, k) p^r^ (1 − p)^k^ | r(1 − p)/p | r(1 − p)/p² | `stats.nbinom(r, p)` |
| Hypergéométrique (N, K, n) | Tirage sans remise | voir 5.6 | nK/N | voir SciPy | `stats.hypergeom(N, K, n)` |

### Exemple résolu : Binomiale ou Poisson ?

**Énoncé** : un assureur couvre 200 contrats indépendants, chacun avec une probabilité annuelle de sinistre de 3 %. Soit N le nombre de sinistres. Calculez E\[N\], Var(N), P(N = 0), P(N ≤ 3) et P(N ≥ 10), exactement puis avec l'approximation de Poisson.

**Modèle exact** : N suit une binomiale (200 ; 0,03), donc E\[N\] = 6 et Var(N) = 200 × 0,03 × 0,97 = 5,82. **Approximation** : Poisson de paramètre λ = 200 × 0,03 = 6.

| Probabilité | Binomiale (exacte) | Poisson (λ = 6) |
| --- | --- | --- |
| P(N = 0) = 0,97^200^ ou e^−6^ | 0,00226 | 0,00248 |
| P(N ≤ 3) | 0,1472 | 0,1512 |
| P(N ≥ 10) | 0,0808 | 0,0839 |

L'approximation est bonne à quelques millièmes près. Elle est légèrement prudente : la Poisson a une variance un peu plus grande (6 contre 5,82).

### En Python

```python
from scipy import stats

B = stats.binom(200, 0.03)
P = stats.poisson(6)
print(B.pmf(0), P.pmf(0))          # P(N = 0)
print(B.cdf(3), P.cdf(3))          # P(N <= 3)
print(B.sf(9), P.sf(9))            # P(N >= 10) = P(N > 9)
print(B.mean(), B.var())           # 6.0 5.82
```

**Piège des lois discrètes dans SciPy** : `.sf(k)` donne P(X > k), strictement. Pour P(X ≥ 10), il faut donc `.sf(9)`.

### Erreurs fréquentes

- Utiliser la binomiale alors que les contrats ne sont pas indépendants (un cyclone touche tous les contrats d'une même zone).
- Confondre les deux conventions de la géométrique : rang du premier succès (k ≥ 1, celle de SciPy) ou nombre d'échecs avant le succès (k ≥ 0).
- Garder une Poisson alors que la variance des données est bien plus grande que leur moyenne.
- Écrire P(X ≥ k) = `sf(k)` pour une loi discrète : c'est `sf(k − 1)`.

## Chapitre 5. Les lois usuelles (2) : lois continues et queues de distribution

Pour les **montants** (coût d'un sinistre, perte d'un portefeuille, rendement), on utilise des lois continues. Ce qui les distingue pour un gestionnaire de risques, c'est surtout leur **queue** : la vitesse à laquelle la probabilité de très grandes pertes diminue. Une queue lourde (lognormale, Pareto) produit des sinistres extrêmes bien plus souvent qu'une queue fine (normale, exponentielle).

### 5.7 Uniforme et exponentielle (rappels)

- **Uniforme sur \[a, b\]** : densité 1/(b − a), E\[X\] = (a + b)/2, Var(X) = (b − a)²/12. Elle sert surtout en simulation (méthode d'inversion, chapitre 3).
- **Exponentielle (λ)** : S(x) = e^−λx^, E\[X\] = 1/λ, Var(X) = 1/λ², sans mémoire. Elle modélise des durées d'attente et des sinistres « modérés ».

### 5.8 La loi Gamma

&#91;N1\] La somme de plusieurs exponentielles indépendantes de même paramètre suit une loi Gamma. Elle est plus souple que l'exponentielle : sa densité peut avoir une bosse, ce qui colle mieux à beaucoup de coûts de sinistres.

&#91;N2\] Pour une forme α > 0 et une échelle θ > 0, avec la fonction Γ de la Partie 0 :

$$
f(x) = \frac{x^{\alpha-1}\,e^{-x/\theta}}{\Gamma(\alpha)\,\theta^\alpha} \quad (x > 0) \qquad E[X] = \alpha\theta \qquad \mathrm{Var}(X) = \alpha\theta^2
$$

Avec α = 1, on retrouve l'exponentielle de moyenne θ. La somme de n exponentielles indépendantes de moyenne θ suit une Gamma (n, θ). Exemple : la somme de 2 sinistres exponentiels de moyenne 500 suit une Gamma (2, 500), de moyenne 1 000 et de quantile à 99 % ≈ 3 319.

### 5.9 La loi normale

&#91;N1\] La célèbre courbe en cloche. Elle apparaît dès qu'on additionne beaucoup de petits effets indépendants (théorème central limite, chapitre 7). On l'utilise pour des rendements de portefeuilles diversifiés et pour approcher des sommes de sinistres.

&#91;N2\] X suit une loi normale N(μ, σ²) si :

$$
f(x) = \frac{1}{\sigma\sqrt{2\pi}}\,\exp\Big(-\frac{(x-\mu)^2}{2\sigma^2}\Big) \qquad E[X] = \mu \qquad \mathrm{Var}(X) = \sigma^2
$$

**Standardisation** : Z = (X − μ)/σ suit la loi normale standard N(0, 1), de fonction de répartition Φ. Tout calcul se ramène donc à Φ :

$$
P(X \le x) = \Phi\Big(\frac{x - \mu}{\sigma}\Big) \qquad \Phi(-z) = 1 - \Phi(z)
$$

**Repères à connaître** : environ 68 % des valeurs sont à moins de 1σ de la moyenne, 95 % à moins de 2σ (plus précisément 1,96σ) et 99,7 % à moins de 3σ.

**Exemple** : le rendement mensuel R d'un fonds suit N(1 %, 5 %²). P(R < −5 %) = Φ((−5 − 1)/5) = Φ(−1,2) = 1 − Φ(1,2) ≈ 1 − 0,8849 = 0,1151.

**Limite pour le risque** : la queue normale décroît très vite. Un écart de 5σ a une probabilité d'environ 3 sur 10 millions, alors que les marchés financiers en connaissent plusieurs par décennie. La normale sous-estime donc souvent les pertes extrêmes.

### 5.10 La loi lognormale

&#91;N1\] X est lognormale si son logarithme est normal : X = e^Y^ avec Y de loi N(μ, σ²). X est toujours positive et asymétrique à droite. On l'utilise pour les coûts de sinistres corporels et pour les prix d'actions (modèle de Black-Scholes).

&#91;N2\] Les formules :

$$
F_X(x) = \Phi\Big(\frac{\ln x - \mu}{\sigma}\Big) \qquad \text{médiane} = e^{\mu} \qquad E[X] = e^{\mu + \sigma^2/2} \qquad \mathrm{Var}(X) = \big(e^{\sigma^2} - 1\big)\,e^{2\mu + \sigma^2}
$$

**Exemple** : avec μ = 6 et σ = 1 (milliers de FCFA), la médiane vaut e⁶ ≈ 403 et la moyenne e^6^,5 ≈ 665. La moyenne dépasse nettement la médiane : quelques gros sinistres tirent la moyenne vers le haut.

**Attention à SciPy** : `stats.lognorm(s=sigma, scale=np.exp(mu))`. Le paramètre `s` est σ, et l'échelle est e^μ^, pas μ.

### 5.11 La loi de Pareto

&#91;N1\] C'est la loi des gros sinistres : incendies industriels, catastrophes, grandes fortunes. Sa queue décroît comme une puissance de x, beaucoup plus lentement qu'une exponentielle. Les réassureurs l'utilisent pour tarifer les sinistres au-delà d'un seuil.

&#91;N2\] Pour un seuil x~m~ > 0 et un indice de queue a > 0, sur x ≥ x~m~ :

$$
S(x) = \Big(\frac{x_m}{x}\Big)^a \qquad E[X] = \frac{a\,x_m}{a-1} \;(a > 1) \qquad \mathrm{Var}(X) = \frac{a\,x_m^2}{(a-1)^2(a-2)} \;(a > 2)
$$

Plus a est petit, plus la queue est lourde. Si a ≤ 2, la variance est **infinie** ; si a ≤ 1, même la moyenne est infinie. Ces cas existent en pratique pour certains risques catastrophiques.

SciPy : `stats.pareto(b=a, scale=x_m)`.

### 5.12 La loi de Weibull

&#91;N2\] S(x) = e^−(x/θ)k^ pour x ≥ 0, avec une forme k > 0 et une échelle θ > 0. Avec k = 1, on retrouve l'exponentielle. Avec k < 1, la queue est plus lourde que l'exponentielle ; avec k > 1, elle est plus fine. On l'utilise en fiabilité (durée de vie de matériel) et pour certains sinistres. SciPy : `stats.weibull_min(c=k, scale=theta)`.

&#91;N3\] Pour les rendements financiers, on utilise aussi la loi de **Student** à ν degrés de liberté : symétrique comme la normale, mais avec des queues lourdes, d'autant plus que ν est petit.

### 5.13 Comparer les queues

Voici la probabilité de dépasser x pour quatre lois de **même moyenne 1** : exponentielle, lognormale (σ = 1), Pareto (a = 2,5 ; x~m~ = 0,6) et Weibull (k = 0,5 ; θ = 0,5).

| x (en multiples de la moyenne) | Exponentielle | Lognormale | Pareto | Weibull (k = 0,5) |
| --- | --- | --- | --- | --- |
| 5 | 6,7 × 10⁻³ | 1,7 × 10⁻² | 5,0 × 10⁻³ | 4,2 × 10⁻² |
| 10 | 4,5 × 10⁻⁵ | 2,5 × 10⁻³ | 8,8 × 10⁻⁴ | 1,1 × 10⁻² |
| 20 | 2,1 × 10⁻⁹ | 2,4 × 10⁻⁴ | 1,6 × 10⁻⁴ | 1,8 × 10⁻³ |
| 50 | 1,9 × 10⁻²² | 5,1 × 10⁻⁶ | 1,6 × 10⁻⁵ | 4,5 × 10⁻⁵ |

Lecture : un sinistre de 50 fois la moyenne est pratiquement impossible sous l'exponentielle (10⁻²²), mais arrive environ une fois sur 60 000 sous la Pareto. Plus x augmente, plus la Pareto prend l'avantage, car sa queue décroît comme une puissance. **Le choix de la loi change la VaR et la TVaR élevées d'un facteur qui peut dépasser 2** (chapitre 9, section 9.11).

&#91;N3\] **La fonction d'excès moyen** e(u) = E\[X − u | X > u\] permet de reconnaître une queue sur des données. Elle est constante pour l'exponentielle (e(u) = 1/λ), croissante et linéaire pour la Pareto (e(u) = u/(a − 1)), et décroissante pour la normale. On trace e(u) estimée sur les données et on regarde sa pente. On note aussi que TVaR~α~ = VaR~α~ + e(VaR~α~) pour une loi continue.

### Récapitulatif des lois continues

| Loi | Usage typique | E\[X\] | Var(X) | Queue | SciPy |
| --- | --- | --- | --- | --- | --- |
| Uniforme \[a, b\] | Simulation | (a + b)/2 | (b − a)²/12 | Bornée | `stats.uniform(loc=a, scale=b-a)` |
| Exponentielle (λ) | Durées, sinistres modérés | 1/λ | 1/λ² | Fine | `stats.expon(scale=1/lam)` |
| Gamma (α, θ) | Coûts de sinistres | αθ | αθ² | Fine | `stats.gamma(a=alpha, scale=theta)` |
| Normale (μ, σ) | Rendements, grandes sommes | μ | σ² | Très fine | `stats.norm(mu, sigma)` |
| Lognormale (μ, σ) | Sinistres corporels, prix | e^μ\ +\ σ²/2^ | (e^σ²^ − 1) e^2μ\ +\ σ²^ | Modérément lourde | `stats.lognorm(s=sigma, scale=np.exp(mu))` |
| Pareto (x~m~, a) | Gros sinistres, réassurance | a x~m~/(a − 1) | voir 5.11 | Lourde | `stats.pareto(b=a, scale=x_m)` |
| Weibull (k, θ) | Fiabilité, sinistres | θ Γ(1 + 1/k) | voir SciPy | Lourde si k < 1 | `stats.weibull_min(c=k, scale=theta)` |

### Comment choisir une loi

1. **Nombre ou montant ?** Un nombre se modélise par une loi discrète (Poisson, binomiale négative), un montant par une loi continue positive (gamma, lognormale, Pareto).
2. **Comparer moyenne et variance** des données de fréquence : égales → Poisson ; variance plus grande → binomiale négative.
3. **Regarder la queue** : histogramme, plus grosses valeurs observées, fonction d'excès moyen.
4. **Estimer et tester** les paramètres sur les données (cours de statistique), puis comparer les VaR et TVaR obtenues avec plusieurs lois. En cas de doute, retenir la loi la plus prudente.

### En Python

```python
import numpy as np
from scipy import stats

# Normale : rendement mensuel N(1 %, 5 %)
R = stats.norm(0.01, 0.05)
print(R.cdf(-0.05))                              # 0.1151

# Lognormale mu = 6, sigma = 1
L = stats.lognorm(s=1, scale=np.exp(6))
print(L.median(), L.mean())                      # 403.4 665.1

# Gamma(2, 500) = somme de 2 exponentielles de moyenne 500
G = stats.gamma(a=2, scale=500)
print(G.mean(), G.var(), G.ppf(0.99))            # 1000 500000 3319

# Comparer les queues de 4 lois de moyenne 1
lois = {'expo': stats.expon(), 'lognormale': stats.lognorm(s=1, scale=np.exp(-0.5)),
        'Pareto': stats.pareto(b=2.5, scale=0.6), 'Weibull': stats.weibull_min(c=0.5, scale=0.5)}
for nom, loi in lois.items():
    print(f'{nom:10s} moyenne={loi.mean():.2f}', ['%.1e' % loi.sf(x) for x in [5, 10, 20, 50]])
```

### Erreurs fréquentes

- Paramétrer la lognormale dans SciPy avec `scale=mu` au lieu de `scale=np.exp(mu)`.
- Utiliser la normale pour des montants de sinistres : elle autorise des valeurs négatives et sa queue est trop fine.
- Oublier qu'une Pareto avec a ≤ 2 a une variance infinie : la moyenne empirique est alors très instable.
- Confondre σ de la lognormale (écart-type de ln X) avec l'écart-type de X.

### Exercices du chapitre 5

- **5.1 (A)** Un portefeuille compte 50 contrats indépendants, chacun avec probabilité de sinistre 4 %. Calculez P(N = 0), P(N ≤ 2), E\[N\] et Var(N).
- **5.2 (A)** Le nombre de sinistres suit une loi de Poisson de paramètre 4. Calculez P(N = 0), P(N = 4) et P(N ≥ 6).
- **5.3 (B)** N suit une binomiale (1 000 ; 0,002). Calculez P(N ≤ 1) exactement, puis avec l'approximation de Poisson.
- **5.4 (B)** Le rendement mensuel R d'un fonds suit N(1 % ; 5 %²). (a) Calculez P(R < −5 %). (b) La perte est X = −R : calculez sa VaR à 99 %.
- **5.5 (B)** Le coût X d'un sinistre corporel suit une lognormale de paramètres μ = 6 et σ = 1 (milliers de FCFA). Calculez la médiane, la moyenne et P(X > 2 000). *Indication : ln(2 000) ≈ 7,601.*
- **5.6 (C, type examen)** Les gros sinistres d'un assureur (au-delà de 10 millions de FCFA) suivent une Pareto de paramètres x~m~ = 10 et a = 2,5 (en millions). (a) Calculez E\[X\] et Var(X). (b) Calculez P(X > 50). (c) Calculez l'excès moyen e(50) = E\[X − 50 | X > 50\]. (d) Un réassureur prend en charge la part de chaque gros sinistre au-delà de 50 millions. Calculez sa charge moyenne par gros sinistre, E\[(X − 50)₊\]. (e) Vérifiez (d) en Python. *Indication pour (d) : E\[(X − u)₊\] = S(u) × e(u).*

## Chapitre 6. Couples de variables, covariance, corrélation, sommes

Un portefeuille contient plusieurs risques, et son risque total dépend de la façon dont ils **bougent ensemble**. La covariance et la corrélation mesurent ce lien, et la formule Var(X + Y) = Var(X) + Var(Y) + 2 Cov(X, Y) montre pourquoi diversifier réduit le risque.

### Pourquoi c'est utile

Un assureur qui couvre l'auto et l'habitation d'un même client, une banque qui détient des actions de plusieurs secteurs, un fonds qui mélange actions et obligations : tous doivent savoir si leurs risques ont tendance à se cumuler (corrélation positive) ou à se compenser (corrélation négative). C'est la base de la diversification (chapitre 10) et de la gestion de portefeuille.

### 6.1 Loi jointe, lois marginales

&#91;N1\] La loi jointe d'un couple (X, Y) donne la probabilité de chaque combinaison de valeurs. En additionnant une ligne ou une colonne, on retrouve la loi de X seule ou de Y seule : ce sont les lois **marginales**.

&#91;N2\] Dans le cas discret, p(x, y) = P(X = x, Y = y), et :

$$
P(X = x) = \sum_y p(x, y) \qquad P(Y = y) = \sum_x p(x, y) \qquad \sum_x \sum_y p(x, y) = 1
$$

Dans le cas continu, une densité jointe f(x, y) remplace p(x, y), et les sommes deviennent des intégrales : f~X~(x) = ∫ f(x, y) dy.

**Indépendance** : X et Y sont indépendantes si et seulement si la loi jointe est le produit des marginales, pour **toutes** les valeurs :

$$
p(x, y) = P(X = x)\,P(Y = y) \quad \text{pour tous } x, y \qquad \big(\text{continu : } f(x, y) = f_X(x)\,f_Y(y)\big)
$$

Il suffit d'une seule case où l'égalité est fausse pour conclure que X et Y sont dépendantes.

**Loi conditionnelle** : P(Y = y | X = x) = p(x, y)/P(X = x). On divise la ligne x par son total.

### 6.2 Covariance et corrélation

&#91;N1\] La covariance est positive quand X et Y ont tendance à être grandes en même temps, négative quand l'une est grande pendant que l'autre est petite. La corrélation est la même idée, ramenée entre −1 et 1 pour être comparable.

&#91;N2\] Les définitions, et la formule de calcul :

$$
\mathrm{Cov}(X, Y) = E\big[(X - E[X])(Y - E[Y])\big] = E[XY] - E[X]\,E[Y] \qquad \rho(X, Y) = \frac{\mathrm{Cov}(X, Y)}{\sigma_X\,\sigma_Y}
$$

Propriétés utiles :

| Propriété | Formule |
| --- | --- |
| Symétrie | Cov(X, Y) = Cov(Y, X) |
| Lien avec la variance | Cov(X, X) = Var(X) |
| Bilinéarité | Cov(aX + b, cY + d) = ac Cov(X, Y) |
| Variance d'une somme | Var(X + Y) = Var(X) + Var(Y) + 2 Cov(X, Y) |
| Variance d'une combinaison | Var(aX + bY) = a² Var(X) + b² Var(Y) + 2ab Cov(X, Y) |
| Bornes de la corrélation | −1 ≤ ρ ≤ 1 ; ρ = ±1 si et seulement si Y = aX + b |

**Indépendantes implique non corrélées** : si X et Y sont indépendantes, E\[XY\] = E\[X\] E\[Y\], donc Cov(X, Y) = 0. **La réciproque est fausse** : une corrélation nulle ne prouve pas l'indépendance. La corrélation ne mesure que le lien **linéaire** (exercice 6.3).

### 6.3 La variance d'un portefeuille

&#91;N2\] Un portefeuille investit une part w dans un actif 1 (rendement R₁, écart-type σ₁) et 1 − w dans un actif 2 (σ₂), avec une corrélation ρ :

$$
\sigma_p^2 = w^2\sigma_1^2 + (1-w)^2\sigma_2^2 + 2w(1-w)\,\rho\,\sigma_1\sigma_2
$$

**Exemple** : w = 60 %, σ₁ = 20 %, σ₂ = 10 %, ρ = 0,2. Alors σ~p~² = 0,0144 + 0,0016 + 0,00192 = 0,01792, et σ~p~ ≈ 13,4 %. C'est moins que la moyenne pondérée des écarts-types (0,6 × 20 % + 0,4 × 10 % = 16 %) : la différence est le gain de diversification.

&#91;N3\] Avec n actifs, de vecteur de poids w et de matrice de covariance Σ (Σ\_ij = Cov(R~i~, R~j~)), la variance du portefeuille s'écrit σ~p~² = wᵀ Σ w. C'est la base de la théorie de Markowitz et des VaR paramétriques des banques.

### 6.4 Sommes de variables indépendantes

&#91;N2\] La loi d'une somme de variables indépendantes s'obtient par **convolution**. En discret :

$$
P(X + Y = s) = \sum_{k} P(X = k)\,P(Y = s - k)
$$

En pratique, on retient les familles **stables par somme** (pour des variables indépendantes) :

| X | Y | X + Y |
| --- | --- | --- |
| Binomiale (n, p) | Binomiale (m, p) | Binomiale (n + m, p) |
| Poisson (λ) | Poisson (μ) | Poisson (λ + μ) |
| Normale (μ₁, σ₁²) | Normale (μ₂, σ₂²) | Normale (μ₁ + μ₂, σ₁² + σ₂²) |
| Gamma (α, θ) | Gamma (β, θ) | Gamma (α + β, θ) |
| Exponentielle (moyenne θ) | Exponentielle (moyenne θ) | Gamma (2, θ) |

La fonction génératrice du chapitre 4 démontre ces résultats en une ligne : M~(X+Y)~ = M~X~ × M~Y~, et on reconnaît la fonction génératrice de la loi annoncée.

### 6.5 Espérance conditionnelle

&#91;N1\] E\[Y | X\] est la meilleure prévision de Y quand on connaît X. C'est une variable aléatoire, car elle dépend de la valeur prise par X.

&#91;N2\] Deux formules très utilisées, notamment pour le modèle collectif (chapitre 11) :

$$
E[Y] = E\big[E[Y \mid X]\big] \qquad \mathrm{Var}(Y) = E\big[\mathrm{Var}(Y \mid X)\big] + \mathrm{Var}\big(E[Y \mid X]\big)
$$

La seconde dit que la variance totale est la somme de la variance moyenne à l'intérieur des groupes et de la variance entre les groupes. Elle explique pourquoi un portefeuille hétérogène est plus dispersé qu'un portefeuille homogène de même moyenne.

&#91;N3\] **Au-delà de la corrélation.** Deux risques peuvent être faiblement corrélés en temps normal mais très liés dans les crises (les marchés chutent ensemble). Pour modéliser cette dépendance dans les queues, on utilise des **copules**, au programme des cours avancés de gestion des risques.

### Exemple résolu : Sinistres auto et habitation d'un même client

**Énoncé** : pour un client, A est le nombre de sinistres auto et H le nombre de sinistres habitation dans l'année. Leur loi jointe est :

|  | H = 0 | H = 1 | Loi de A |
| --- | --- | --- | --- |
| A = 0 | 0,60 | 0,10 | 0,70 |
| A = 1 | 0,15 | 0,08 | 0,23 |
| A = 2 | 0,03 | 0,04 | 0,07 |
| Loi de H | 0,78 | 0,22 | 1 |

**(a) Indépendance ?** P(A = 0, H = 0) = 0,60, alors que P(A = 0) P(H = 0) = 0,70 × 0,78 = 0,546. Les deux diffèrent : A et H sont **dépendants**.

**(b) Moments.** E\[A\] = 0,23 + 2 × 0,07 = 0,37 ; E\[H\] = 0,22 ; E\[A²\] = 0,23 + 4 × 0,07 = 0,51, donc Var(A) = 0,51 − 0,37² = 0,3731 ; Var(H) = 0,22 × 0,78 = 0,1716.

**(c) Covariance et corrélation.** Seules les cases où A et H sont non nuls comptent dans E\[AH\] :

$$
E[AH] = 1 \times 1 \times 0{,}08 + 2 \times 1 \times 0{,}04 = 0{,}16 \qquad \mathrm{Cov}(A, H) = 0{,}16 - 0{,}37 \times 0{,}22 = 0{,}0786
$$

$$
\rho = \frac{0{,}0786}{\sqrt{0{,}3731 \times 0{,}1716}} \approx 0{,}31
$$

**(d) Le total T = A + H.** E\[T\] = 0,59 et Var(T) = 0,3731 + 0,1716 + 2 × 0,0786 = 0,7019. Si on avait supposé A et H indépendants, on aurait trouvé 0,5447 : on aurait sous-estimé la variance de 22 %.

**(e) Loi conditionnelle.** P(H = 1 | A = 2) = 0,04/0,07 ≈ 0,57, contre P(H = 1 | A = 0) = 0,10/0,70 ≈ 0,14. Un client qui a beaucoup de sinistres auto en a aussi plus en habitation : c'est un indice de « profil » commun.

### En Python

```python
import numpy as np

J = np.array([[0.60, 0.10],
              [0.15, 0.08],
              [0.03, 0.04]])         # lignes : A = 0, 1, 2 ; colonnes : H = 0, 1
a, h = np.array([0, 1, 2]), np.array([0, 1])
pA, pH = J.sum(axis=1), J.sum(axis=0)          # marginales
print(pA, pH)                                   # [0.7 0.23 0.07] [0.78 0.22]
print(np.allclose(J, np.outer(pA, pH)))         # False -> dependants

EA, EH = a @ pA, h @ pH
EAH = a @ J @ h                                 # somme des a*h*p(a,h)
cov = EAH - EA * EH
rho = cov / np.sqrt((a**2 @ pA - EA**2) * (h**2 @ pH - EH**2))
print(cov, rho)                                 # 0.0786 0.31

# Portefeuille de deux actifs
w, s1, s2, r = 0.6, 0.20, 0.10, 0.2
print(np.sqrt(w**2*s1**2 + (1-w)**2*s2**2 + 2*w*(1-w)*r*s1*s2))   # 0.134

# Forme matricielle w' Sigma w
Sigma = np.array([[s1**2, r*s1*s2], [r*s1*s2, s2**2]])
wv = np.array([w, 1 - w])
print(np.sqrt(wv @ Sigma @ wv))                 # 0.134
```

### Erreurs fréquentes

- Additionner les variances de variables corrélées sans le terme 2 Cov(X, Y).
- Conclure à l'indépendance parce que la corrélation est nulle.
- Vérifier l'indépendance sur une seule case : il faut que l'égalité tienne pour **toutes** les cases (une seule case fausse suffit en revanche à prouver la dépendance).
- Oublier que Var(X − Y) = Var(X) + Var(Y) − 2 Cov(X, Y) : les variances s'additionnent même pour une différence.

### Exercices du chapitre 6

- **6.1 (A)** Un couple (X, Y) prend les valeurs (0, 0), (0, 1), (1, 0), (1, 1) avec les probabilités 0,5 ; 0,2 ; 0,2 ; 0,1. Donnez les lois marginales, dites si X et Y sont indépendantes, puis calculez Cov(X, Y) et ρ.
- **6.2 (A)** Var(X) = 4, Var(Y) = 9 et Cov(X, Y) = 1. Calculez Var(2X − 3Y).
- **6.3 (B)** X vaut −1, 0 ou 1 avec probabilité 1/3 chacun, et Y = X². Montrez que Cov(X, Y) = 0 mais que X et Y ne sont pas indépendantes.
- **6.4 (B)** Avec σ₁ = 20 %, σ₂ = 10 % et ρ = 0,2, trouvez la part w qui minimise la variance du portefeuille, puis l'écart-type minimal. *Indication : dérivez σ~p~² par rapport à w.*
- **6.5 (C, type examen)** Les sinistres auto N₁ et habitation N₂ d'un portefeuille sont indépendants, de lois de Poisson de paramètres 3 et 2. (a) Quelle est la loi de N = N₁ + N₂ ? Calculez P(N = 0) et P(N ≥ 8). (b) Sachant que N = 5, montrez que N₁ suit une binomiale (5 ; 3/5) et calculez P(N₁ = 3 | N = 5). (c) Vérifiez (b) par simulation en Python. *Indication pour (b) : écrivez P(N₁ = k, N₂ = 5 − k)/P(N = 5).*

## Chapitre 7. Loi des grands nombres et théorème central limite

Ces deux théorèmes expliquent pourquoi l'assurance fonctionne. La loi des grands nombres dit que la perte moyenne par contrat devient prévisible quand le nombre de contrats grandit. Le théorème central limite dit que la perte totale suit approximativement une loi normale, ce qui permet de calculer le chargement de sécurité à ajouter à la prime pure.

### Pourquoi c'est utile

Pour un seul assuré, le coût de l'année est très imprévisible : souvent 0, parfois très élevé. Pour 10 000 assurés indépendants, le coût **moyen** par assuré est presque certain. C'est la **mutualisation** : l'assureur transforme beaucoup de risques individuels imprévisibles en une charge globale prévisible. Ces deux théorèmes en donnent la justification mathématique et ses limites.

### 7.1 La moyenne empirique

&#91;N2\] Soient X₁, …, X~n~ indépendantes, de même loi, de moyenne μ et d'écart-type σ. Leur moyenne empirique et leur somme vérifient :

$$
\bar{X}_n = \frac{1}{n}\sum_{i=1}^n X_i \qquad E[\bar{X}_n] = \mu \qquad \mathrm{Var}(\bar{X}_n) = \frac{\sigma^2}{n} \qquad S_n = \sum_{i=1}^n X_i : \; E[S_n] = n\mu, \; \sigma_{S_n} = \sigma\sqrt{n}
$$

La variance de la moyenne est divisée par n, donc son écart-type par √n. C'est le cœur de la mutualisation.

### 7.2 La loi des grands nombres (LGN)

&#91;N1\] Plus on a d'observations indépendantes, plus leur moyenne se rapproche de l'espérance. C'est aussi ce qui justifie la simulation Monte Carlo (Partie 0).

&#91;N2\] **Loi faible des grands nombres** : pour tout ε > 0,

$$
P\big(|\bar{X}_n - \mu| \ge \varepsilon\big) \le \frac{\sigma^2}{n\,\varepsilon^2} \xrightarrow[n \to \infty]{} 0
$$

La démonstration est l'inégalité de Tchebychev (chapitre 4) appliquée à la moyenne empirique, dont la variance est σ²/n.

&#91;N3\] La **loi forte** affirme davantage : la moyenne empirique converge vers μ avec probabilité 1. Il suffit pour cela que E\[|X|\] soit fini, même si la variance est infinie.

**Application : la mutualisation.** Le risque relatif de la charge totale, mesuré par son coefficient de variation, diminue comme 1/√n :

$$
\frac{\sigma_{S_n}}{E[S_n]} = \frac{\sigma\sqrt{n}}{n\mu} = \frac{1}{\sqrt{n}} \cdot \frac{\sigma}{\mu}
$$

Avec un contrat de moyenne 100 et d'écart-type 300 (CV = 3), le CV de la charge totale vaut 0,3 pour 100 contrats, 0,095 pour 1 000 contrats et 0,03 pour 10 000 contrats.

### 7.3 Le théorème central limite (TCL)

&#91;N1\] Une somme d'un grand nombre de variables indépendantes de même loi suit approximativement une loi normale, **quelle que soit** la loi de départ (pourvu que sa variance soit finie). C'est pour cela que la loi normale est partout.

&#91;N2\] Quand n tend vers l'infini, pour tout z :

$$
P\Big(\frac{S_n - n\mu}{\sigma\sqrt{n}} \le z\Big) \longrightarrow \Phi(z) \qquad \text{donc, pour n grand : } S_n \approx \mathcal{N}(n\mu,\; n\sigma^2)
$$

**Approximations classiques** :

- Binomiale (n, p) ≈ N(np, np(1 − p)) quand np(1 − p) est assez grand (règle pratique : au moins 10).
- Poisson (λ) ≈ N(λ, λ) quand λ est grand.
- &#91;N3\] **Correction de continuité** : pour approcher une loi entière par la normale, on écrit P(N ≥ k) ≈ P(normale ≥ k − 0,5). Cela améliore nettement la précision.

### Exemple résolu : Le chargement de sécurité

**Énoncé** : un assureur couvre n contrats indépendants. Chaque contrat a une perte annuelle de moyenne 100 et d'écart-type 300. (a) Avec n = 1 000, si l'assureur ne fait payer que la prime pure majorée de 5 %, quelle est la probabilité que la charge dépasse les primes ? (b) Quel chargement par contrat faut-il pour que cette probabilité tombe à 1 % ? (c) Même question avec 10 000 contrats.

**(a)** E\[S\] = 100 000 et σ~S~ = 300 √1 000 ≈ 9 487. Les primes valent 105 000. Par le TCL :

$$
P(S > 105\,000) \approx 1 - \Phi\Big(\frac{5000}{9487}\Big) = 1 - \Phi(0{,}527) \approx 0{,}30
$$

Un chargement de 5 % laisse 30 % de risque de perte : c'est beaucoup trop.

**(b)** Il faut une prime totale égale à E\[S\] + z~0,99~ σ~S~ = 100 000 + 2,3263 × 9 487 ≈ 122 070, soit un chargement de **22,07 par contrat** (22 % de la prime pure).

**(c)** Avec n = 10 000 : σ~S~ = 300 × 100 = 30 000, chargement total 2,3263 × 30 000 ≈ 69 790, soit **6,98 par contrat** (7 %).

| Nombre de contrats | CV de la charge | Chargement par contrat (risque de perte 1 %) |
| --- | --- | --- |
| 100 | 0,300 | 69,79 |
| 1 000 | 0,095 | 22,07 |
| 10 000 | 0,030 | 6,98 |
| 100 000 | 0,009 | 2,21 |

**Interprétation** : le chargement nécessaire par contrat est divisé par √10 ≈ 3,16 chaque fois que le portefeuille est multiplié par 10. Un grand assureur peut donc proposer des primes plus basses à risque de ruine égal : c'est l'avantage de la taille.

### 7.4 Les limites de la mutualisation

- **La dépendance.** Si un cyclone ou une inondation touche en même temps beaucoup de contrats, les pertes ne sont plus indépendantes. La variance de la somme contient alors les covariances (chapitre 6), et le risque relatif ne tend plus vers 0. On ne peut pas mutualiser un risque systémique : on le réassure.
- **Les queues très lourdes.** Pour une Pareto avec a ≤ 2, la variance est infinie et le TCL classique ne s'applique pas. Pour a ≤ 1, même la loi des grands nombres échoue : la moyenne empirique ne se stabilise jamais (exercice 7.4).
- **L'asymétrie.** Pour un n modéré et des sinistres asymétriques, l'approximation normale sous-estime la queue droite, comme on l'a vu avec le modèle collectif (chapitre 11). Pour des VaR élevées, on préfère la simulation ou l'algorithme de Panjer.

### En Python

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats
rng = np.random.default_rng(42)

# LGN : la moyenne de pertes exponentielles (moyenne 100) se stabilise
x = rng.exponential(100, size=100_000)
moyennes = np.cumsum(x) / np.arange(1, len(x) + 1)
plt.plot(moyennes); plt.axhline(100, color='red'); plt.xscale('log'); plt.show()

# TCL : la somme de 50 exponentielles ressemble deja a une normale
S = rng.exponential(100, size=(100_000, 50)).sum(axis=1)
plt.hist(S, bins=100, density=True)
z = np.linspace(S.min(), S.max(), 300)
plt.plot(z, stats.norm(50*100, 100*np.sqrt(50)).pdf(z), 'r'); plt.show()

# Chargement de securite selon la taille du portefeuille
for n in [100, 1_000, 10_000, 100_000]:
    print(n, round(stats.norm.ppf(0.99) * 300 * np.sqrt(n) / n, 2))
```

### Erreurs fréquentes

- Croire que la LGN dit que la **somme** se stabilise : c'est la **moyenne** qui se stabilise. L'écart-type de la somme, lui, grandit comme √n.
- Appliquer le TCL à des risques dépendants ou à variance infinie.
- Oublier la correction de continuité pour une petite binomiale.
- Utiliser l'approximation normale pour une VaR à 99,5 % d'une charge très asymétrique.

### Exercices du chapitre 7

- **7.1 (A)** Un portefeuille compte 400 contrats indépendants, de perte moyenne 50 et d'écart-type 100. (a) Donnez l'espérance et l'écart-type de la perte moyenne par contrat. (b) Majorez P(|perte moyenne − 50| ≥ 10) avec Tchebychev. (c) Approchez cette probabilité avec le TCL et comparez.
- **7.2 (B)** N suit une binomiale (1 000 ; 0,05). Approchez P(N ≥ 60) par la loi normale, avec puis sans correction de continuité, et comparez à la valeur exacte calculée en Python.
- **7.3 (C, type examen)** Un assureur couvre 2 500 contrats indépendants de perte moyenne 200 et d'écart-type 600. (a) Calculez E\[S\] et σ~S~. (b) Quelle prime par contrat faut-il demander pour que la probabilité que la charge dépasse les primes soit au plus 0,5 % ? (c) Même question pour 10 000 contrats. (d) Citez deux raisons pour lesquelles ce calcul peut être trop optimiste.
- **7.4 (B, \[N3\])** Simulez en Python les moyennes cumulées de 100 000 tirages d'une Pareto avec x~m~ = 1 et a = 1,5 (moyenne 3, variance infinie), puis avec a = 0,8 (moyenne infinie). Tracez-les et décrivez ce que vous observez.

## Fiche mémo : Partie 2

- **Poisson (λ)** : E = Var = λ ; somme de Poisson indépendantes = Poisson ; approche la binomiale (n, p) avec λ = np.
- **Binomiale (n, p)** : E = np, Var = np(1 − p). **Binomiale négative** : Var > E (surdispersion).
- **Normale** : standardiser, Z = (X − μ)/σ ; Φ(−z) = 1 − Φ(z) ; 68 / 95 / 99,7 %.
- **Lognormale** : médiane e^μ^, moyenne e^μ\ +\ σ²/2^ ; SciPy `scale=np.exp(mu)`.
- **Pareto** : S(x) = (x~m~/x)^a^ ; moyenne finie si a > 1, variance finie si a > 2 ; excès moyen e(u) = u/(a − 1).
- **Queues** : normale < exponentielle < lognormale, Pareto (plus lourdes).
- **Covariance** : Cov = E\[XY\] − E\[X\]E\[Y\] ; Var(X + Y) = Var X + Var Y + 2 Cov ; indépendance ⇒ Cov = 0, pas l'inverse.
- **Portefeuille** : σ~p~² = w²σ₁² + (1 − w)²σ₂² + 2w(1 − w)ρσ₁σ₂.
- **LGN** : Var(moyenne) = σ²/n. **TCL** : S~n~ ≈ N(nμ, nσ²). Chargement par contrat = z~α~ σ/√n.

## Corrigés des exercices : Partie 2

### Chapitre 5

**5.1** N suit une binomiale (50 ; 0,04). P(N = 0) = 0,96^50^ ≈ **0,130** ; P(N ≤ 2) ≈ **0,677** ; E\[N\] = **2** ; Var(N) = 50 × 0,04 × 0,96 = **1,92**.

**5.2** P(N = 0) = e^−4^ ≈ **0,0183** ; P(N = 4) = e^−4^ 4⁴/4! ≈ **0,195** ; P(N ≥ 6) = 1 − P(N ≤ 5) ≈ **0,215**.

**5.3** Exact : P(N ≤ 1) = 0,998^1000^ + 1 000 × 0,002 × 0,998^999^ ≈ **0,4057**. Poisson (λ = 2) : e^−2^(1 + 2) = 3e^−2^ ≈ **0,4060**. L'approximation est excellente car n est grand et p petit.

**5.4** (a) P(R < −5 %) = Φ((−5 − 1)/5) = Φ(−1,2) ≈ **0,115**. (b) X = −R suit N(−1 % ; 5 %²), donc VaR~0,99~ = −1 % + 5 % × 2,3263 ≈ **10,63 %** de la valeur du fonds.

**5.5** Médiane = e⁶ ≈ **403** ; moyenne = e^6^,5 ≈ **665** ; P(X > 2 000) = 1 − Φ((7,601 − 6)/1) = 1 − Φ(1,601) ≈ **0,055**.

**5.6** (a) E\[X\] = 2,5 × 10/1,5 ≈ **16,67** millions ; Var(X) = 2,5 × 100/(1,5² × 0,5) ≈ **222,2** (σ ≈ 14,9). (b) P(X > 50) = (10/50)^2^,5 = 0,2^2^,5 ≈ **0,0179**. (c) e(50) = 50/(2,5 − 1) ≈ **33,3** millions : un sinistre qui dépasse 50 millions le dépasse en moyenne de 33 millions. (d) E\[(X − 50)₊\] = S(50) × e(50) ≈ 0,0179 × 33,33 ≈ **0,596** million par gros sinistre. (e) `stats.pareto(b=2.5, scale=10).expect(lambda x: max(x - 50, 0))` donne 0,5963.

### Chapitre 6

**6.1** Lois marginales : P(X = 1) = 0,2 + 0,1 = 0,3 et P(Y = 1) = 0,3. P(X = 1, Y = 1) = 0,1 ≠ 0,3 × 0,3 = 0,09 : X et Y sont **dépendantes**. Cov = E\[XY\] − E\[X\]E\[Y\] = 0,1 − 0,09 = **0,01** ; Var(X) = Var(Y) = 0,3 × 0,7 = 0,21 ; ρ = 0,01/0,21 ≈ **0,048**.

**6.2** Var(2X − 3Y) = 4 Var(X) + 9 Var(Y) − 2 × 2 × 3 × Cov(X, Y) = 16 + 81 − 12 = **85**.

**6.3** E\[X\] = 0 et E\[XY\] = E\[X³\] = (−1 + 0 + 1)/3 = 0, donc Cov(X, Y) = 0 − 0 × E\[Y\] = **0**. Pourtant P(X = 0, Y = 0) = P(X = 0) = 1/3, alors que P(X = 0) P(Y = 0) = 1/3 × 1/3 = 1/9 : X et Y sont **dépendantes** (Y est même entièrement déterminée par X). La corrélation ne voit pas ce lien car il n'est pas linéaire.

**6.4** On dérive σ~p~² par rapport à w et on annule :

$$
w^* = \frac{\sigma_2^2 - \rho\sigma_1\sigma_2}{\sigma_1^2 + \sigma_2^2 - 2\rho\sigma_1\sigma_2} = \frac{0{,}01 - 0{,}004}{0{,}04 + 0{,}01 - 0{,}008} = \frac{0{,}006}{0{,}042} = \frac{1}{7} \approx 14{,}3\,\%
$$

L'écart-type minimal vaut alors ≈ **9,56 %**, moins que l'actif le moins risqué seul (10 %) : c'est l'effet de la diversification.

**6.5** (a) N suit une Poisson de paramètre 3 + 2 = **5**. P(N = 0) = e^−5^ ≈ **0,0067** ; P(N ≥ 8) ≈ **0,133**. (b) Par indépendance :

$$
P(N_1 = k \mid N = 5) = \frac{e^{-3}\frac{3^k}{k!}\; e^{-2}\frac{2^{5-k}}{(5-k)!}}{e^{-5}\frac{5^5}{5!}} = \binom{5}{k}\Big(\frac{3}{5}\Big)^k\Big(\frac{2}{5}\Big)^{5-k}
$$

C'est la binomiale (5 ; 3/5). P(N₁ = 3 | N = 5) = C(5,3) × 0,6³ × 0,4² = **0,3456**. (c) On simule N₁ et N₂, on ne garde que les cas où N₁ + N₂ = 5, et on calcule la proportion de N₁ = 3 : le notebook trouve environ 0,345.

### Chapitre 7

**7.1** (a) E = **50** ; écart-type = 100/√400 = **5**. (b) Tchebychev : P ≤ 5²/10² = **0,25**. (c) TCL : P(|Z| ≥ 10/5 = 2) = 2(1 − Φ(2)) ≈ **0,046**. La borne de Tchebychev est valable pour toutes les lois, mais très prudente.

**7.2** E\[N\] = 50 ; σ = √(1 000 × 0,05 × 0,95) ≈ 6,892. Sans correction : 1 − Φ((60 − 50)/6,892) = 1 − Φ(1,451) ≈ **0,073**. Avec correction : 1 − Φ((59,5 − 50)/6,892) = 1 − Φ(1,378) ≈ **0,084**. Valeur exacte (`stats.binom(1000, 0.05).sf(59)`) : **0,087**. La correction de continuité réduit nettement l'erreur.

**7.3** (a) E\[S\] = 2 500 × 200 = **500 000** ; σ~S~ = 600 × √2 500 = **30 000**. (b) z~0,995~ = 2,5758, chargement total 2,5758 × 30 000 ≈ 77 275, soit 30,91 par contrat : prime ≈ **230,91**. (c) Avec 10 000 contrats : σ~S~ = 60 000, chargement par contrat 2,5758 × 60 000/10 000 ≈ 15,45 : prime ≈ **215,45**. (d) Deux raisons parmi : les contrats peuvent être dépendants (événement climatique) ; la charge est asymétrique et l'approximation normale sous-estime la queue à 99,5 % ; les paramètres 200 et 600 sont eux-mêmes estimés avec erreur ; les frais de gestion ne sont pas inclus.

**7.4** Avec a = 1,5, la moyenne cumulée finit par s'approcher de 3, mais très lentement et avec de grands sauts à chaque sinistre géant : la LGN s'applique, pas le TCL. Avec a = 0,8, la moyenne cumulée ne se stabilise jamais et continue de monter par sauts : la moyenne théorique est infinie et aucune prime pure finie ne couvre ce risque. Le code est dans le notebook de la Partie 2.

## Partie 3. Le risque

## Chapitre 8. Pourquoi mesurer le risque

Une mesure de risque résume toute la loi d'une perte en **un seul montant**, en général le capital qu'il faut mettre de côté. La VaR répond à « combien de capital pour ne faire faillite que dans 0,5 % des cas ? », et la TVaR à « si on tombe dans ces 0,5 %, combien perd-on en moyenne ? ».

### 8.1 Le problème de l'assureur

&#91;N1\] Un assureur encaisse des primes au début de l'année et paie les sinistres pendant l'année. Il ne connaît pas à l'avance le total des sinistres : c'est une variable aléatoire X. Si X dépasse ce dont il dispose, il est en ruine et ne peut plus payer ses clients. Pour l'éviter, il détient du **capital** en plus des primes.

La question devient : **combien de capital c faut-il pour que la ruine soit très improbable ?** Avoir c = le pire scénario possible coûterait beaucoup trop cher, voire serait infini. On accepte donc un petit risque de ruine, par exemple 0,5 % par an.

&#91;N2\] On cherche le plus petit montant c tel que la probabilité de dépasser c soit au plus 1 − α :

$$
P(X > c) \le 1 - \alpha \iff S_X(c) \le 1-\alpha \iff F_X(c) \ge \alpha
$$

Le plus petit c qui vérifie F(c) ≥ α, c'est exactement le quantile d'ordre α du chapitre 3. On vient de découvrir la Value-at-Risk :

$$
\mathrm{VaR}_\alpha(X) = \inf\{\, c : F_X(c) \ge \alpha \,\} = q_\alpha(X)
$$

Voilà à quoi sert la VaR : c'est le **capital minimal** qui garantit que la perte ne le dépasse qu'avec une probabilité 1 − α.

### 8.2 Pourquoi la moyenne ne suffit pas

&#91;N1\] La moyenne dit combien on perd « en temps normal ». Elle ne dit rien des mauvaises années, qui sont justement celles qui mènent à la faillite.

**Exemple** : deux portefeuilles ont la même perte moyenne de 100.

| Portefeuille | Loi de la perte | E\[X\] | VaR à 99,5 % |
| --- | --- | --- | --- |
| A | 100 avec certitude | 100 | 100 |
| B | 0 avec probabilité 0,99 ; 10 000 avec probabilité 0,01 | 100 | 10 000 |

Un assureur qui fixerait son capital sur la moyenne serait ruiné une année sur cent avec B. La VaR voit la différence : B demande 100 fois plus de capital.

**Et la variance ?** Elle mesure la dispersion, mais elle a trois défauts pour fixer un capital : elle pénalise autant les bonnes surprises que les mauvaises, elle ne s'exprime pas en FCFA mais en FCFA², et elle décrit mal les queues de distribution. On l'utilise plutôt pour tarifer (chapitre 4) et dans le cas gaussien.

### 8.3 Ce qu'est une mesure de risque

&#91;N2\] Une mesure de risque ρ associe à chaque perte X un nombre ρ(X), exprimé dans la même unité que X, et interprété comme un capital. Trois choix la définissent :

| Élément | Question | Exemples |
| --- | --- | --- |
| La variable X | Qu'est-ce qu'on mesure ? | Total des sinistres, perte d'un portefeuille d'actions, perte sur un prêt |
| L'horizon | Sur quelle durée ? | 1 jour, 10 jours (marchés) ; 1 an (assurance, crédit) |
| Le niveau α | Quelle prudence ? | 95 %, 97,5 %, 99 %, 99,5 % |

**Gains et pertes.** En finance de marché, on observe plutôt la variation de valeur V₁ − V₀ d'un portefeuille. La perte est son opposé : X = V₀ − V₁. Une perte négative est un gain. Toutes les formules de ce cours s'appliquent à X ainsi définie.

### 8.4 Où les mesures de risque sont utilisées

| Domaine | Usage | Mesure |
| --- | --- | --- |
| Assurance (Solvabilité II, Union européenne) | Capital de solvabilité requis (SCR) | VaR à 99,5 % sur un an ([directive 2009/138/CE, art. 101](https://www.eiopa.europa.eu/rulebook/solvency-ii/article-2188_en)) |
| Banque, risque de marché (Bâle, FRTB) | Capital pour le portefeuille de négociation (modèles internes) | Expected Shortfall à 97,5 % ([Bâle, MAR33](https://www.bis.org/committees/bcbs/basel-framework/81327/chapter.pdf)), qui a remplacé la VaR à 99 % |
| Gestion interne | Limites de risque par desk ou par branche, allocation du capital | VaR, TVaR |
| Tarification | Chargement de sécurité au-dessus de la prime pure | Variance, écart-type, TVaR |
| Réassurance | Choix du seuil et de la portée d'un traité | Quantiles, primes stop-loss |

Les régulateurs africains, dont ceux de la zone CIMA pour l'assurance et de l'UMOA pour la banque, ont leurs propres textes, qui s'inspirent de ces référentiels à des degrés divers. Pour les règles applicables, c'est votre cours de réglementation qui fait foi. Les méthodes de calcul de ce cours, elles, sont les mêmes partout.

### Erreurs fréquentes

- Confondre perte et gain : une VaR se calcule sur la **perte**. Pour un rendement R, la perte est −R, et la VaR à 99 % se lit dans la queue **gauche** de R.
- Croire que la VaR est la perte maximale : elle est dépassée avec probabilité 1 − α, et on ne sait pas de combien.
- Oublier l'horizon : une VaR à 99 % sur 1 jour et une VaR à 99 % sur 1 an sont deux choses très différentes.

### Exercices

- **8.1 (A)** Pour les portefeuilles A et B de l'exemple 8.2, calculez la VaR à 98 %, puis à 99,5 %. Quel portefeuille paraît le moins risqué avec la VaR à 98 % ? Commentez. *Indication : pour B, F(0) = 0,99.*
- **8.2 (B)** Un portefeuille vaut 100 aujourd'hui. Sa valeur dans 10 jours suit une loi normale de moyenne 101 et d'écart-type 4. (a) Exprimez la perte X et donnez sa loi. (b) Calculez la VaR à 99 % sur 10 jours, sachant que le quantile à 99 % de la loi normale standard vaut 2,3263. *Indication : si Z est normale standard, μ + σZ a pour quantile μ + σ q~α~(Z) (chapitre 3, 3.7).*

## Chapitre 9. La Value-at-Risk (VaR)

La VaR au niveau α est le quantile d'ordre α de la perte : on la calcule à partir de F en résolvant F(x) = α dans le cas continu, ou en cherchant la première valeur où F(x) ≥ α dans le cas discret. Ce chapitre donne une recette en quatre étapes, puis l'applique aux trois situations possibles : une loi continue, une loi discrète, des données.

### 9.1 Définition et interprétation

&#91;N1\] « Avec une probabilité α, la perte ne dépassera pas la VaR. » Avec α = 99 % sur un an : une année sur cent en moyenne, la perte sera plus grande que la VaR.

&#91;N2\] Pour une perte X de fonction de répartition F et un niveau α dans \]0, 1\[ :

$$
\mathrm{VaR}_\alpha(X) = F_X^{-1}(\alpha) = \inf\{\, x \in \mathbb{R} : F_X(x) \ge \alpha \,\}
$$

La VaR vérifie toujours deux inégalités, qui servent à contrôler un résultat :

$$
P\big(X \le \mathrm{VaR}_\alpha\big) \ge \alpha \qquad P\big(X > \mathrm{VaR}_\alpha\big) \le 1 - \alpha
$$

### 9.2 La recette en quatre étapes

1. **Écrire F** (ou S = 1 − F) de la perte, avec son domaine.
2. **Identifier le cas** : F continue et strictement croissante, F en escalier (discret), ou F avec un saut puis une partie continue (mixte).
3. **Calculer** :
   - cas continu : résoudre F(x) = α, ou de façon équivalente S(x) = 1 − α ;
   - cas discret : parcourir le tableau de F de gauche à droite et prendre la **première** valeur où F(x) ≥ α ;
   - cas mixte : regarder d'abord si α tombe dans le saut (alors la VaR est la valeur du saut), sinon résoudre dans la partie continue.
4. **Contrôler** : S(VaR) ≤ 1 − α ; la VaR augmente quand α augmente ; elle est dans le domaine de X ; une perte positive donne une VaR positive ou nulle.

### 9.3 Cas continu : les formules à connaître

&#91;N2\] Chaque formule s'obtient en résolvant F(x) = α.

| Loi de X | F(x) ou S(x) | VaR~α~(X) |
| --- | --- | --- |
| Uniforme sur \[a, b\] | F(x) = (x − a)/(b − a) | a + α(b − a) |
| Exponentielle (λ), moyenne θ = 1/λ | S(x) = e^−λx^ | −ln(1 − α)/λ = θ ln(1/(1 − α)) |
| Normale N(μ, σ²) | F(x) = Φ((x − μ)/σ) | μ + σ z~α~ |
| Lognormale (μ, σ) : X = e^Y^ avec Y normale | F(x) = Φ((ln x − μ)/σ) | e^μ\ +\ σ\ z~α~^ |
| Pareto (x~m~, a), x ≥ x~m~ | S(x) = (x~m~/x)^a^ | x~m~ (1 − α)^−1/a^ |

Ici Φ est la fonction de répartition de la loi normale standard et z~α~ = Φ⁻¹(α) son quantile. Les valeurs à connaître :

| α | 0,90 | 0,95 | 0,975 | 0,99 | 0,995 |
| --- | --- | --- | --- | --- | --- |
| z~α~ | 1,2816 | 1,6449 | 1,9600 | 2,3263 | 2,5758 |

**Détail pour la normale** : X = μ + σZ avec Z normale standard. La fonction x ↦ μ + σx est croissante, donc par la propriété 3.7, VaR~α~(X) = μ + σ VaR~α~(Z) = μ + σ z~α~. Le même argument avec x ↦ e^x^ donne la lognormale.

**Détail pour la Pareto** : on résout (x~m~/x)^a^ = 1 − α, donc x~m~/x = (1 − α)^1/a^ et x = x~m~ (1 − α)^−1/a^.

### Exemple résolu 1 : VaR d'un sinistre exponentiel

Le coût d'un sinistre suit une loi exponentielle de moyenne 1 000. On veut la VaR à 99 %.

- **Étape 1** : F(x) = 1 − e^−x/1000^ pour x ≥ 0.
- **Étape 2** : F est continue et strictement croissante sur \[0, +∞\[.
- **Étape 3** : 1 − e^−x/1000^ = 0,99 donne e^−x/1000^ = 0,01, donc x = 1 000 · ln(100) ≈ **4 605**.
- **Étape 4** : S(4 605) = e^−4,605^ ≈ 0,01 = 1 − α. C'est cohérent.

### Exemple résolu 2 : VaR d'un portefeuille gaussien

La perte à 10 jours d'un portefeuille d'actions, en millions de FCFA, suit une loi normale de moyenne −2 (on gagne 2 millions en moyenne) et d'écart-type 10.

$$
\mathrm{VaR}_{0{,}99} = -2 + 10 \times 2{,}3263 \approx 21{,}26 \text{ millions de FCFA}
$$

Une période de 10 jours sur 100, en moyenne, la perte dépassera 21,26 millions.

### 9.4 Cas discret

&#91;N2\] F est en escalier. On applique l'inverse généralisé : la VaR est la plus petite valeur possible x~k~ telle que F(x~k~) ≥ α. Quand α tombe exactement sur une valeur de F, on s'arrête à cette valeur.

### Exemple résolu 3 : Une perte discrète

La perte annuelle L d'un petit contrat (en milliers de FCFA) suit la loi :

| x | 0 | 100 | 500 | 2 000 |
| --- | --- | --- | --- | --- |
| P(L = x) | 0,80 | 0,15 | 0,04 | 0,01 |
| F(x) | 0,80 | 0,95 | 0,99 | 1,00 |

| α | Raisonnement | VaR~α~(L) |
| --- | --- | --- |
| 0,90 | F(0) = 0,80 < 0,90 ; F(100) = 0,95 ≥ 0,90 | **100** |
| 0,95 | F(100) = 0,95 ≥ 0,95 (égalité) | **100** |
| 0,99 | F(500) = 0,99 ≥ 0,99 (égalité) | **500** |
| 0,995 | F(500) = 0,99 < 0,995 ; F(2 000) = 1 | **2 000** |

Remarquez que la VaR à 90 % et la VaR à 95 % sont égales : en discret, la VaR reste constante tant que α ne dépasse pas la marche suivante.

### 9.5 Sur des données : la VaR empirique (simulation historique)

&#91;N1\] Quand on n'a pas de loi mais un historique de pertes, on remplace F par la **fonction de répartition empirique** F~n~ : F~n~(x) est la proportion d'observations inférieures ou égales à x. Les banques appellent cette méthode la simulation historique.

&#91;N2\] On trie les n observations : x~(1)~ ≤ x~(2)~ ≤ … ≤ x~(n)~. Comme F~n~(x~(k)~) = k/n, la première valeur où F~n~ ≥ α est :

$$
\widehat{\mathrm{VaR}}_\alpha = x_{(\lceil n\alpha \rceil)} \qquad \lceil \cdot \rceil = \text{arrondi à l'entier supérieur}
$$

### Exemple résolu 4 : VaR sur 20 sinistres observés

Voici 20 coûts de sinistres (milliers de FCFA), déjà triés : 3, 4, 9, 12, 15, 18, 22, 28, 37, 45, 55, 61, 70, 78, 95, 140, 150, 310, 520, 980.

Pour α = 0,9 : nα = 18, donc la VaR empirique est la 18e valeur, **310**. Contrôle : 2 observations sur 20, soit 10 %, dépassent 310.

**Piège Python** : par défaut, `np.quantile` interpole entre deux observations et donne ici 331. Pour la définition du cours (inverse généralisé), il faut `method='inverted_cdf'`. Les deux conventions existent dans la pratique ; l'essentiel est d'annoncer celle qu'on utilise.

&#91;N3\] Avec 20 observations, une VaR à 90 % repose sur les 2 plus grosses valeurs : l'estimation est très instable. Une VaR à 99,5 % demanderait au moins quelques centaines d'observations, d'où le recours aux lois paramétriques et à la simulation (Partie 4).

### En Python

```python
import numpy as np
from scipy import stats

# Continu : la methode .ppf donne directement la VaR
print(stats.expon(scale=1000).ppf(0.99))           # 4605.17
print(stats.norm(loc=-2, scale=10).ppf(0.99))      # 21.26
print(stats.norm.ppf([0.90, 0.95, 0.975, 0.99, 0.995]))   # table des z_alpha
print(stats.pareto(b=2.5, scale=600).ppf(0.99))    # Pareto x_m = 600, a = 2.5 -> 3785.7

# Discret : premiere valeur ou F >= alpha
def var_discrete(valeurs, probas, alpha):
    F = np.cumsum(probas)
    return valeurs[np.argmax(F >= alpha - 1e-12)]   # 1e-12 : tolerance d'arrondi

v, p = np.array([0, 100, 500, 2000]), np.array([0.80, 0.15, 0.04, 0.01])
for a in [0.90, 0.95, 0.99, 0.995]:
    print(a, var_discrete(v, p, a))                # 100 100 500 2000

# Donnees : VaR empirique
x = np.array([3, 4, 9, 12, 15, 18, 22, 28, 37, 45, 55, 61, 70, 78, 95, 140, 150, 310, 520, 980])
print(np.quantile(x, 0.9, method='inverted_cdf'))  # 310 (definition du cours)
print(np.quantile(x, 0.9))                         # 331 (interpolation par defaut)
```

La tolérance `1e-12` évite un piège informatique : 0,8 + 0,15 peut valoir 0,9499999… en machine, et la comparaison F ≥ 0,95 échouerait à tort.

### Ce que la VaR ne dit pas

La VaR dit où commence la queue de distribution, pas ce qu'il y a dedans. Deux pertes peuvent avoir la même VaR à 99 % alors que l'une perd en moyenne 10 % de plus au-delà et l'autre 10 fois plus. Dans l'exemple 3, la VaR à 95 % vaut 100, alors que la perte peut atteindre 2 000. Pour mesurer ce qui se passe au-delà de la VaR, on utilise la TVaR.

## Chapitre 9 (suite). La TVaR, ou Expected Shortfall

La TVaR au niveau α est la perte moyenne dans les (1 − α) pires cas. Elle se calcule à partir de la fonction de survie : TVaR = VaR + (1/(1 − α)) × l'intégrale de S au-delà de la VaR. Elle est toujours supérieure ou égale à la VaR, et elle voit ce que la VaR ignore : la taille des pertes extrêmes.

### 9.6 Idée et définition

&#91;N1\] On classe tous les scénarios possibles du moins grave au plus grave, et on garde les (1 − α) plus graves. Avec α = 99 %, ce sont les 1 % pires années. La TVaR est la perte **moyenne** sur ces pires années. La VaR est le seuil d'entrée dans cette zone ; la TVaR dit ce qu'il y a dedans.

&#91;N2\] La définition officielle fait la moyenne de toutes les VaR au-delà du niveau α :

$$
\mathrm{TVaR}_\alpha(X) = \frac{1}{1-\alpha}\int_\alpha^1 \mathrm{VaR}_u(X)\,du
$$

On divise par 1 − α, la longueur de l'intervalle \[α, 1\], pour obtenir une moyenne.

**Vocabulaire.** Les noms suivants désignent la même mesure : TVaR (Tail Value-at-Risk), Expected Shortfall (ES), CVaR (Conditional VaR). Une mesure voisine, la CTE (Conditional Tail Expectation), vaut E\[X | X > VaR~α~\]. TVaR et CTE sont égales pour une loi continue, mais peuvent différer pour une loi discrète (exemple résolu 7).

### 9.7 Les trois formules de calcul

&#91;N2\] Selon la situation, on utilise l'une des trois formules équivalentes ci-dessous. Notons v = VaR~α~(X).

**Formule 1, par la fonction de survie (la plus utile).** Elle vaut pour toutes les lois :

$$
\mathrm{TVaR}_\alpha(X) = v + \frac{1}{1-\alpha}\,E\big[(X - v)_+\big] = v + \frac{1}{1-\alpha}\int_v^{+\infty} S_X(x)\,dx
$$

Lecture : la TVaR est la VaR plus l'excédent moyen au-delà de la VaR. On retrouve la prime stop-loss du chapitre 4, avec une franchise égale à la VaR.

**Formule 2, espérance conditionnelle (loi continue).** Si F est continue :

$$
\mathrm{TVaR}_\alpha(X) = E[X \mid X > v] = \frac{1}{1-\alpha}\int_v^{+\infty} x\,f_X(x)\,dx
$$

**Formule 3, cas général (indispensable en discret).** Elle vaut pour toutes les lois :

$$
\mathrm{TVaR}_\alpha(X) = \frac{1}{1-\alpha}\Big( E\big[X\,\mathbf{1}_{\{X > v\}}\big] + v\,\big(F_X(v) - \alpha\big) \Big)
$$

Le terme v × (F(v) − α) corrige un effet propre au discret : quand la loi a une masse au point v, une partie de cette masse appartient aux (1 − α) pires cas et doit compter dans la moyenne. En continu, F(v) = α et ce terme disparaît.

&#91;N3\] **Pourquoi les formules 1 et 3 sont équivalentes.** On écrit X · 1(X > v) = v · 1(X > v) + (X − v)₊, donc E\[X · 1(X > v)\] = v S(v) + E\[(X − v)₊\]. En remplaçant dans la formule 3, et comme S(v) + F(v) = 1 :

$$
\frac{1}{1-\alpha}\Big( v\,S(v) + E[(X-v)_+] + v\,F(v) - v\,\alpha \Big) = \frac{1}{1-\alpha}\Big( v\,(1-\alpha) + E[(X-v)_+] \Big) = v + \frac{E[(X-v)_+]}{1-\alpha}
$$

&#91;N3\] **Pourquoi la formule 2 découle de la définition en continu.** On fait le changement de variable u = F(x) dans l'intégrale des VaR : VaR~u~ = x, du = f(x) dx, et u = α correspond à x = v.

### 9.8 La recette de la TVaR en quatre étapes

1. **Calculer la VaR** v avec la recette 9.2.
2. **Choisir la formule** : loi continue avec S simple → formule 1 ; loi continue avec densité simple → formule 2 ; loi discrète ou mixte → formule 3 ; données → moyenne des plus grandes valeurs (9.10).
3. **Calculer**, en divisant par **1 − α** (et non par α).
4. **Contrôler** : TVaR ≥ VaR, et TVaR ≥ E\[X\].

### 9.9 Les formules fermées à connaître

| Loi de X | VaR~α~ | TVaR~α~ |
| --- | --- | --- |
| Uniforme sur \[a, b\] | a + α(b − a) | (VaR~α~ + b)/2 |
| Exponentielle (λ) | −ln(1 − α)/λ | VaR~α~ + 1/λ |
| Normale N(μ, σ²) | μ + σ z~α~ | μ + σ φ(z~α~)/(1 − α) |
| Lognormale (μ, σ) | e^μ\ +\ σ\ z~α~^ | e^μ\ +\ σ²/2^ Φ(σ − z~α~)/(1 − α) |
| Pareto (x~m~, a), avec a > 1 | x~m~ (1 − α)^−1/a^ | a/(a − 1) × VaR~α~ |

Ici φ est la densité de la loi normale standard, φ(z) = e^−z²/2^/√(2π). Pour la normale, le coefficient φ(z~α~)/(1 − α) se lit dans ce tableau :

| α | 0,90 | 0,95 | 0,975 | 0,99 | 0,995 |
| --- | --- | --- | --- | --- | --- |
| z~α~ (pour la VaR) | 1,2816 | 1,6449 | 1,9600 | 2,3263 | 2,5758 |
| φ(z~α~)/(1 − α) (pour la TVaR) | 1,7550 | 2,0627 | 2,3378 | 2,6652 | 2,8919 |

**Démonstration pour l'exponentielle** (formule 1) : S(x) = e^−λx^, donc l'intégrale de S au-delà de v vaut e^−λv^/λ = S(v)/λ = (1 − α)/λ. En divisant par 1 − α, il reste 1/λ, d'où TVaR = VaR + 1/λ. C'est l'absence de mémoire : au-delà de n'importe quel seuil, l'excédent moyen vaut toujours 1/λ.

**Démonstration pour la Pareto** (formule 1) : S(x) = (x~m~/x)^a^, donc l'intégrale de S au-delà de v vaut v · S(v)/(a − 1) = v(1 − α)/(a − 1). D'où TVaR = v + v/(a − 1) = a v/(a − 1). Ici l'excédent moyen **grandit avec le seuil** : c'est la signature d'une queue lourde.

### Exemple résolu 5 : TVaR d'un sinistre exponentiel

Même sinistre qu'à l'exemple 1 : exponentielle de moyenne 1 000, α = 99 %, VaR = 4 605.

$$
\int_{4605}^{\infty} e^{-x/1000}\,dx = 1000\,e^{-4{,}605} = 1000 \times 0{,}01 = 10 \qquad \mathrm{TVaR}_{0{,}99} = 4605 + \frac{10}{0{,}01} = 5605
$$

Interprétation : dans le 1 % des cas où le sinistre dépasse 4 605, il coûte en moyenne 5 605.

### Exemple résolu 6 : TVaR du portefeuille gaussien

Perte normale de moyenne −2 et d'écart-type 10, α = 99 % : TVaR = −2 + 10 × 2,6652 ≈ **24,65** millions, contre une VaR de 21,26 millions. Pour une loi normale, l'écart entre VaR et TVaR reste modéré, car sa queue est très fine.

### Exemple résolu 7 : TVaR d'une perte discrète, et le piège de la CTE

Reprenons la perte L de l'exemple 3 : 0 (0,80), 100 (0,15), 500 (0,04), 2 000 (0,01). On veut la TVaR à α = 90 %.

**Étape 1** : VaR~0,9~ = 100, car F(0) = 0,80 < 0,9 ≤ F(100) = 0,95.

**Étape 2-3, avec la formule 3** :

$$
E[L\,\mathbf{1}_{\{L>100\}}] = 500 \times 0{,}04 + 2000 \times 0{,}01 = 40 \qquad \mathrm{TVaR}_{0{,}9} = \frac{40 + 100\,(0{,}95 - 0{,}90)}{0{,}10} = \frac{45}{0{,}10} = 450
$$

**Vérification avec l'idée de départ** : les 10 % pires cas se composent de 5 % de cas à 100 (la partie de la marche 100 située au-dessus de 0,90), 4 % à 500 et 1 % à 2 000. Leur moyenne vaut (0,05 × 100 + 0,04 × 500 + 0,01 × 2 000)/0,10 = 45/0,10 = **450**.

**Le piège** : la CTE vaut E\[L | L > 100\] = 40/0,05 = 800. Elle ne garde que les 5 % de cas strictement au-dessus de 100, et non les 10 % pires cas. En discret, utilisez toujours la formule 3 pour la TVaR.

**Étape 4** : TVaR = 450 ≥ VaR = 100, et 450 ≥ E\[L\] = 15 + 20 + 20 = 55. C'est cohérent.

### 9.10 Sur des données : la TVaR empirique

&#91;N2\] Avec n observations triées, si k = n(1 − α) est entier, la TVaR empirique est la moyenne des k plus grandes valeurs. Sinon, on applique la formule 3 à la loi empirique, où chaque observation a le poids 1/n.

**Exemple résolu 8** : avec les 20 sinistres de l'exemple 4 et α = 0,9, on a k = 20 × 0,1 = 2. La TVaR est la moyenne des 2 plus grandes valeurs : (520 + 980)/2 = **750**, contre une VaR de 310 et une moyenne de 132,6.

### 9.11 Ce que la TVaR révèle : queue fine contre queue lourde

Comparons deux lois de **même moyenne 1 000** pour le coût d'un sinistre : une exponentielle, et une Pareto de paramètres x~m~ = 600 et a = 2,5 (de moyenne a x~m~/(a − 1) = 1 000).

| α | VaR exponentielle | VaR Pareto | TVaR exponentielle | TVaR Pareto |
| --- | --- | --- | --- | --- |
| 95 % | 2 996 | 1 989 | 3 996 | 3 315 |
| 99 % | 4 605 | 3 786 | 5 605 | 6 310 |
| 99,5 % | 5 298 | 4 995 | 6 298 | 8 325 |
| 99,9 % | 6 908 | 9 509 | 7 908 | 15 849 |

À 99 %, la VaR jugerait la Pareto **moins** risquée que l'exponentielle (3 786 contre 4 605), alors que sa TVaR est **plus** élevée (6 310 contre 5 605). La VaR ne voit pas que la Pareto produit, rarement, des sinistres énormes. La TVaR le voit. C'est la raison principale pour laquelle le Comité de Bâle a remplacé la VaR par l'Expected Shortfall pour le risque de marché.

### En Python

```python
import numpy as np
from scipy import stats, integrate
rng = np.random.default_rng(42)

def tvar_continue(loi, alpha):
    # Formule 1 : VaR + integrale de S au-dela de la VaR, divisee par 1 - alpha
    v = loi.ppf(alpha)
    queue, _ = integrate.quad(loi.sf, v, np.inf)
    return v + queue / (1 - alpha)

def tvar_discrete(valeurs, probas, alpha):
    # Formule 3 : cas general, valable pour les lois discretes
    valeurs, probas = np.asarray(valeurs, float), np.asarray(probas, float)
    F = np.cumsum(probas)
    i = np.argmax(F >= alpha - 1e-12)
    v = valeurs[i]
    au_dela = valeurs > v
    return (np.sum(valeurs[au_dela] * probas[au_dela]) + v * (F[i] - alpha)) / (1 - alpha)

def tvar_empirique(x, alpha):
    # Formule 3 appliquee a la loi empirique (poids 1/n)
    x = np.sort(np.asarray(x, float)); n = len(x)
    return tvar_discrete(x, np.full(n, 1 / n), alpha)

print(tvar_continue(stats.expon(scale=1000), 0.99))                 # 5605.17
print(tvar_continue(stats.norm(loc=-2, scale=10), 0.99))            # 24.65
print(tvar_continue(stats.pareto(b=2.5, scale=600), 0.99))          # 6309.6
print(tvar_discrete([0, 100, 500, 2000], [0.80, 0.15, 0.04, 0.01], 0.90))   # 450

x = [3, 4, 9, 12, 15, 18, 22, 28, 37, 45, 55, 61, 70, 78, 95, 140, 150, 310, 520, 980]
print(tvar_empirique(x, 0.90))                                      # 750

# Double verification par simulation : moyenne des 1 % pires cas
sim = stats.expon(scale=1000).rvs(size=1_000_000, random_state=rng)
v = np.quantile(sim, 0.99)
print(v, sim[sim > v].mean())                                       # ~4605 ~5605
```

### Erreurs fréquentes

- Diviser par α au lieu de 1 − α.
- Oublier d'ajouter la VaR dans la formule 1 (on obtient alors l'excédent moyen, pas la TVaR).
- Utiliser E\[X | X > VaR\] pour une loi discrète : c'est la CTE, qui peut différer de la TVaR.
- Trouver une TVaR inférieure à la VaR : c'est toujours une erreur de calcul.
- Pour la Pareto, appliquer la formule a/(a − 1) × VaR avec a ≤ 1 : la moyenne est alors infinie, et la TVaR aussi.

### Exercices des chapitres 8 et 9 (suite)

- **9.1 (A)** Le coût d'un sinistre suit une loi exponentielle de moyenne 2 000. Calculez la VaR et la TVaR à 95 % et à 99 %.
- **9.2 (A)** Une perte X vaut 0, 1 000, 5 000 ou 20 000 avec les probabilités 0,70 ; 0,20 ; 0,08 ; 0,02. (a) Calculez la VaR à 90 %, 95 %, 98 % et 99 %. (b) Calculez la TVaR à 95 % avec la formule 3, puis vérifiez-la en décomposant les 5 % pires cas. (c) Calculez la CTE à 95 % et comparez. *Indication : F(5 000) = 0,98.*
- **9.3 (B)** X suit une loi uniforme sur \[0, b\]. Montrez que VaR~α~ = αb et TVaR~α~ = (1 + α)b/2 avec la formule 1. Interprétez : où se situe la TVaR par rapport à la VaR et à b ?
- **9.4 (B)** La perte d'un portefeuille suit une loi normale de moyenne 5 et d'écart-type 20 (millions de FCFA). Calculez la VaR et l'Expected Shortfall à 97,5 %, le niveau réglementaire de Bâle.
- **9.5 (B)** On observe 10 pertes : 5, 12, 7, 30, 18, 2, 45, 9, 15, 60. (a) Calculez la VaR et la TVaR empiriques à 80 %. (b) \[N3\] Calculez la TVaR empirique à 75 % avec la formule 3. *Indication pour (b) : n(1 − α) = 2,5 n'est pas entier ; VaR~0,75~ = x~(8)~.*
- **9.6 (C, type examen)** Deux modèles sont proposés pour le coût X d'un grand sinistre : (M1) une Pareto de paramètres x~m~ = 1 000 et a = 3 ; (M2) une exponentielle de même moyenne. (a) Calculez la moyenne de M1 et donnez le paramètre de M2. (b) Calculez la VaR et la TVaR à 99 % sous chaque modèle. (c) Même question à 99,9 %. (d) Quel modèle est le plus prudent ? Votre réponse dépend-elle de la mesure et du niveau choisis ? Que conseillez-vous à un assureur qui hésite ?

## Chapitre 10. Propriétés des mesures de risque

Une « bonne » mesure de risque doit récompenser la diversification : le risque d'un portefeuille ne devrait jamais dépasser la somme des risques de ses parties. La TVaR respecte toujours cette règle ; la VaR peut la violer, et ce chapitre montre un exemple simple où elle le fait.

### Pourquoi c'est utile

Une banque calcule un capital par desk de trading, puis un capital global. Un groupe d'assurance fait de même par branche (auto, santé, habitation). Si la mesure de risque ne récompense pas la diversification, découper une activité en morceaux peut faire baisser artificiellement le capital affiché. Les propriétés de ce chapitre disent quand une mesure est fiable pour ces usages.

### 10.1 Les quatre axiomes de cohérence

&#91;N1\] En 1999, Artzner, Delbaen, Eber et Heath ont listé quatre propriétés qu'une mesure de risque « raisonnable » devrait avoir. Une mesure qui les vérifie toutes est dite **cohérente**.

&#91;N2\] Pour des pertes X et Y, une constante c et un réel λ > 0 :

| Axiome | Formule | Signification |
| --- | --- | --- |
| Monotonie | si X ≤ Y, alors ρ(X) ≤ ρ(Y) | Une perte toujours plus petite demande moins de capital |
| Invariance par translation | ρ(X + c) = ρ(X) + c | Ajouter une perte certaine c ajoute exactement c au capital |
| Homogénéité positive | ρ(λX) = λ ρ(X) | Doubler la position double le capital ; changer d'unité ne change rien |
| Sous-additivité | ρ(X + Y) ≤ ρ(X) + ρ(Y) | Regrouper deux risques ne crée pas de risque en plus : la diversification ne coûte rien |

La VaR vérifie les trois premiers axiomes (c'est la propriété 3.7 des quantiles). La TVaR vérifie les quatre : elle est cohérente. La VaR n'est pas toujours sous-additive, comme le montre l'exemple suivant.

### Exemple résolu 9 : Quand la VaR pénalise la diversification

**Énoncé** : une banque détient deux obligations indépendantes. Chacune fait défaut avec probabilité 4 %, et la perte est alors de 100 (sinon 0). On compare VaR et TVaR à 95 % pour chaque obligation, puis pour le portefeuille S = X + Y.

**Une obligation seule** : P(X = 0) = 0,96 et P(X = 100) = 0,04. Comme F(0) = 0,96 ≥ 0,95, VaR~0,95~(X) = **0**. Pour la TVaR (formule 3) :

$$
\mathrm{TVaR}_{0{,}95}(X) = \frac{100 \times 0{,}04 + 0 \times (0{,}96 - 0{,}95)}{0{,}05} = 80
$$

**Le portefeuille** : par indépendance, S suit la loi ci-dessous.

| s | 0 | 100 | 200 |
| --- | --- | --- | --- |
| P(S = s) | 0,96² = 0,9216 | 2 × 0,96 × 0,04 = 0,0768 | 0,04² = 0,0016 |
| F(s) | 0,9216 | 0,9984 | 1 |

Comme F(0) = 0,9216 < 0,95 ≤ F(100), VaR~0,95~(S) = **100**. Et :

$$
\mathrm{TVaR}_{0{,}95}(S) = \frac{200 \times 0{,}0016 + 100 \times (0{,}9984 - 0{,}95)}{0{,}05} = \frac{0{,}32 + 4{,}84}{0{,}05} = 103{,}2
$$

**Bilan** :

| Mesure | Obligation X | Obligation Y | Somme X + Y séparées | Portefeuille S | Sous-additive ? |
| --- | --- | --- | --- | --- | --- |
| VaR à 95 % | 0 | 0 | 0 | 100 | Non : 100 > 0 |
| TVaR à 95 % | 80 | 80 | 160 | 103,2 | Oui : 103,2 ≤ 160 |

Avec la VaR, détenir les deux obligations dans deux entités séparées demanderait 0 de capital, mais les regrouper en demanderait 100. Un trader pourrait donc réduire son capital affiché en découpant son portefeuille. La TVaR évite ce paradoxe.

### 10.2 Le cas gaussien : la VaR redevient sous-additive

&#91;N2\] Si X et Y sont normales (et le couple (X, Y) gaussien), de moyennes nulles, d'écarts-types σ~X~ et σ~Y~, et de corrélation ρ (chapitre 6), alors X + Y est normale et :

$$
\sigma_{X+Y} = \sqrt{\sigma_X^2 + \sigma_Y^2 + 2\rho\,\sigma_X\sigma_Y} \le \sigma_X + \sigma_Y \qquad \mathrm{VaR}_\alpha(X+Y) = z_\alpha\,\sigma_{X+Y}
$$

Comme ρ ≤ 1, on a toujours VaR(X + Y) ≤ VaR(X) + VaR(Y), avec égalité seulement si ρ = 1. La différence s'appelle le **gain de diversification**.

**Exemple** : deux desks de pertes N(0, 10²), corrélés à ρ = 0,3, α = 99 %. Chaque VaR vaut 10 × 2,3263 = 23,26, soit 46,53 au total. Pour le portefeuille, σ = √(100 + 100 + 60) ≈ 16,12 et la VaR vaut 37,51. Le gain de diversification est de 9,02, soit 19 %.

Leçon : la VaR pose problème surtout pour les risques **très asymétriques** (défauts de crédit, catastrophes), précisément ceux qu'on rencontre en assurance et en risque de crédit.

### 10.3 Autres propriétés utiles

- **Ordre** : E\[X\] ≤ TVaR~α~(X) et VaR~α~(X) ≤ TVaR~α~(X).
- **Croissance en α** : VaR~α~ et TVaR~α~ augmentent quand α augmente.
- **Cas limite** : quand α tend vers 0, la TVaR tend vers E\[X\] (on fait la moyenne sur tous les cas).
- &#91;N3\] **Additivité comonotone** : si X et Y sont des fonctions croissantes d'un même facteur (deux risques qui « bougent toujours ensemble »), alors VaR et TVaR sont additives : ρ(X + Y) = ρ(X) + ρ(Y). C'est le pire cas de dépendance pour la TVaR.
- &#91;N3\] **Élicitabilité et backtesting** : la VaR est « élicitable », ce qui rend son backtesting simple : on compte les jours où la perte a dépassé la VaR, qui devraient être environ 1 % des jours pour une VaR à 99 %. L'Expected Shortfall seule ne l'est pas, ce qui rend son backtesting plus délicat (Partie 4).

### VaR ou TVaR : le bilan

| Critère | VaR | TVaR |
| --- | --- | --- |
| Facile à expliquer | Oui : « perte dépassée 1 fois sur 100 » | Assez : « perte moyenne dans le pire 1 % » |
| Voit la taille des pertes extrêmes | Non | Oui |
| Cohérente (sous-additive) | Non en général | Oui |
| Facile à estimer avec peu de données | Plutôt oui | Moins : dépend des quelques plus grosses pertes |
| Backtesting | Simple | Plus délicat |
| Usage réglementaire | Solvabilité II (99,5 %) | Bâle FRTB, risque de marché (97,5 %) |

### En Python

```python
import numpy as np
from scipy import stats
rng = np.random.default_rng(42)

# Exemple 9 : deux obligations independantes (tvar_discrete et var_discrete du chapitre 9)
v1, p1 = np.array([0, 100]), np.array([0.96, 0.04])
vS, pS = np.array([0, 100, 200]), np.array([0.96**2, 2*0.96*0.04, 0.04**2])
print(var_discrete(v1, p1, 0.95), var_discrete(vS, pS, 0.95))      # 0 100
print(tvar_discrete(v1, p1, 0.95), tvar_discrete(vS, pS, 0.95))    # 80 103.2

# Verification par simulation
n = 2_000_000
X = 100 * (rng.random(n) < 0.04); Y = 100 * (rng.random(n) < 0.04)
print(np.quantile(X + Y, 0.95, method='inverted_cdf'))            # 100

# Cas gaussien : gain de diversification
z = stats.norm.ppf(0.99)
sx, sy, rho = 10, 10, 0.3
s_port = np.sqrt(sx**2 + sy**2 + 2*rho*sx*sy)
print(z*sx + z*sy, z*s_port, z*(sx + sy - s_port))                # 46.53 37.51 9.02
```

### Erreurs fréquentes

- Croire que la VaR est toujours sous-additive : elle l'est pour des lois normales, pas en général.
- Additionner les VaR de plusieurs desks pour obtenir la VaR globale : c'est exact seulement pour des risques comonotones.
- Confondre « cohérente » et « précise » : la TVaR est cohérente, mais son estimation sur peu de données reste fragile.

### Exercices

- **10.1 (A)** X suit une loi exponentielle de moyenne 1 000, et Y = 2X + 500 (un sinistre dont le coût est doublé et augmenté de frais fixes de 500). Sans recalculer de loi, donnez VaR~0,99~(Y) et TVaR~0,99~(Y). *Indication : utilisez les axiomes d'invariance par translation et d'homogénéité.*
- **10.2 (B)** Deux desks ont des pertes normales de moyennes nulles et d'écarts-types 10 et 20. Calculez la VaR à 99 % de chaque desk, puis du total pour ρ = 0,5 et pour ρ = −0,5. Calculez le gain de diversification dans chaque cas et commentez.
- **10.3 (C, type examen)** Deux prêts indépendants font chacun défaut avec probabilité 3 %, avec une perte de 1 000 en cas de défaut. (a) Calculez la VaR et la TVaR à 95 % de chaque prêt. (b) Donnez la loi de la perte totale, puis sa VaR et sa TVaR à 95 %. (c) La VaR est-elle sous-additive ici ? Et la TVaR ? (d) Expliquez en quelques lignes à un directeur des risques pourquoi ce résultat plaide pour la TVaR.

## Chapitre 11. Le modèle collectif : fréquence × sévérité

La charge annuelle d'un portefeuille d'assurance est une somme d'un nombre aléatoire de sinistres de montants aléatoires. Sa moyenne et sa variance se calculent avec deux formules simples, mais sa VaR et sa TVaR demandent en général un calcul numérique (algorithme de Panjer ou simulation). L'approximation normale sous-estime le plus souvent la VaR, parce que la charge totale est asymétrique.

### Pourquoi c'est utile

Un assureur auto ne sait ni combien de sinistres il aura dans l'année (la **fréquence**), ni combien chacun coûtera (la **sévérité**). Le modèle collectif combine les deux pour obtenir la loi de la charge totale, puis la prime pure, la VaR et la TVaR du portefeuille. C'est le modèle de base de l'actuariat non-vie.

### 11.1 Le modèle

&#91;N2\] La charge totale S de l'année s'écrit :

$$
S = \sum_{i=1}^{N} X_i \qquad (S = 0 \text{ si } N = 0)
$$

Sous trois hypothèses : N est le nombre de sinistres ; les montants X₁, X₂, … sont indépendants et de même loi que X ; et ils sont indépendants de N.

La loi la plus utilisée pour N est la **loi de Poisson** de paramètre λ > 0 (étudiée au chapitre 5). Avec la série exponentielle de la Partie 0, on vérifie que les probabilités somment à 1 :

$$
P(N = k) = e^{-\lambda}\,\frac{\lambda^k}{k!} \quad (k = 0, 1, 2, \dots) \qquad E[N] = \mathrm{Var}(N) = \lambda
$$

On dit alors que S suit une loi de **Poisson composée**.

### 11.2 Moyenne et variance de la charge totale

&#91;N1\] La charge moyenne est le nombre moyen de sinistres multiplié par le coût moyen. La variance a deux sources : l'incertitude sur le coût de chaque sinistre et l'incertitude sur leur nombre.

&#91;N2\] Les formules de Wald :

$$
E[S] = E[N]\,E[X] \qquad \mathrm{Var}(S) = E[N]\,\mathrm{Var}(X) + \mathrm{Var}(N)\,\big(E[X]\big)^2
$$

Pour N de loi de Poisson, E\[N\] = Var(N) = λ, et la formule se simplifie :

$$
E[S] = \lambda\,E[X] \qquad \mathrm{Var}(S) = \lambda\,E[X^2]
$$

&#91;N3\] **Démonstration** par conditionnement sur N. Sachant N = n, S est une somme de n variables indépendantes, donc E\[S | N\] = N E\[X\] et Var(S | N) = N Var(X). On conclut avec la formule de l'espérance totale, E\[S\] = E\[E\[S | N\]\], et celle de la variance totale :

$$
\mathrm{Var}(S) = E\big[\mathrm{Var}(S \mid N)\big] + \mathrm{Var}\big(E[S \mid N]\big) = E[N]\,\mathrm{Var}(X) + \mathrm{Var}(N)\,E[X]^2
$$

### 11.3 La loi de S : trois méthodes

&#91;N2\] La fonction de répartition de S s'écrit comme un mélange, où F~X~^\*n^ désigne la fonction de répartition d'une somme de n montants :

$$
F_S(x) = \sum_{n=0}^{\infty} P(N = n)\,F_X^{*n}(x) \qquad P(S = 0) = P(N = 0) = e^{-\lambda} \text{ (si } X > 0\text{)}
$$

S a donc une **masse en 0** (les années sans sinistre), puis une partie continue ou discrète : c'est une loi mixte, comme au chapitre 3. Cette formule n'a presque jamais de forme fermée. On utilise l'une des trois méthodes suivantes.

| Méthode | Principe | Avantage | Limite |
| --- | --- | --- | --- |
| Approximation normale | S ≈ N(E\[S\], Var(S)), donc VaR ≈ E\[S\] + z~α~ σ~S~ | Calcul immédiat | Ignore l'asymétrie : sous-estime en général la VaR élevée |
| Algorithme de Panjer \[N3\] | Récurrence exacte quand X prend des valeurs entières (ou discrétisées) | Exact et rapide | Demande une sévérité discrète |
| Simulation Monte Carlo | On simule N, puis N montants, des centaines de milliers de fois | Marche pour n'importe quel modèle | Erreur d'échantillonnage, calcul plus long |

### Exemple résolu 10 : Capital d'un portefeuille auto

**Énoncé** : un portefeuille a en moyenne 50 sinistres par an (N de loi de Poisson, λ = 50). Le coût d'un sinistre suit une loi exponentielle de moyenne 500 (milliers de FCFA). Calculez la prime pure du portefeuille, l'écart-type de la charge, puis la VaR à 99,5 % par approximation normale et par simulation.

**Moments** : E\[X\] = 500 et E\[X²\] = 2 × 500² = 500 000.

$$
E[S] = 50 \times 500 = 25\,000 \qquad \mathrm{Var}(S) = 50 \times 500\,000 = 25\,000\,000 \qquad \sigma_S = 5\,000
$$

**Approximation normale** : VaR~0,995~ ≈ 25 000 + 2,5758 × 5 000 ≈ **37 879**.

**Simulation** (400 000 années simulées) : VaR~0,995~ ≈ **39 200** et TVaR~0,995~ ≈ **41 200**.

**Interprétation** : l'approximation normale sous-estime la VaR d'environ 1 300, car la loi de S est asymétrique à droite. Le capital à détenir au-delà de la prime pure est d'environ 39 200 − 25 000 = 14 200 : c'est la logique d'un capital de solvabilité, VaR moins perte moyenne attendue.

### 11.4 \[N3\] L'algorithme de Panjer (Poisson composée)

On exprime les montants dans une unité (par exemple 1 000 FCFA) pour que X prenne des valeurs entières 0, 1, 2, … avec probabilités f~X~(j). Alors, pour N de loi de Poisson (λ) :

$$
f_S(0) = e^{-\lambda\,(1 - f_X(0))} \qquad f_S(s) = \frac{\lambda}{s}\sum_{j=1}^{s} j\,f_X(j)\,f_S(s-j) \quad (s \ge 1)
$$

On calcule f~S~(1), puis f~S~(2), et ainsi de suite. On obtient la loi **exacte** de S, sur laquelle on applique la VaR et la TVaR discrètes du chapitre 9.

### En Python

```python
import numpy as np
from scipy import stats
rng = np.random.default_rng(42)

# Exemple 10 : Poisson(50) x Exponentielle(moyenne 500)
lam, theta = 50, 500
ES, sdS = lam * theta, np.sqrt(lam * 2 * theta**2)
print(ES, sdS, ES + stats.norm.ppf(0.995) * sdS)      # 25000 5000 37879

n_sim = 400_000
N = rng.poisson(lam, size=n_sim)
montants = rng.exponential(theta, size=N.sum())        # tous les sinistres d'un coup
annee = np.repeat(np.arange(n_sim), N)                 # annee de chaque sinistre
S = np.bincount(annee, weights=montants, minlength=n_sim)
v = np.quantile(S, 0.995, method='inverted_cdf')
print(S.mean(), S.std(), v, S[S > v].mean())           # ~25000 ~5000 ~39200 ~41200

# Panjer pour une severite discrete (unite = 1000 FCFA)
def panjer_poisson(lam, fx, smax):
    fx = np.asarray(fx, float)
    fs = np.zeros(smax + 1)
    fs[0] = np.exp(-lam * (1 - fx[0]))
    for s in range(1, smax + 1):
        j = np.arange(1, min(s, len(fx) - 1) + 1)
        fs[s] = lam / s * np.sum(j * fx[j] * fs[s - j])
    return fs
```

Le calcul `np.bincount(annee, weights=montants)` additionne en une seule opération les sinistres de chaque année simulée. C'est beaucoup plus rapide qu'une boucle sur 400 000 années.

### Erreurs fréquentes

- Oublier le second terme de la variance, Var(N) E\[X\]² : l'incertitude sur le nombre de sinistres compte aussi.
- Écrire Var(S) = λ Var(X) pour une Poisson composée : c'est λ E\[X²\], pas λ Var(X).
- Oublier la masse en 0 de S quand on calcule ses quantiles.
- Se contenter de l'approximation normale pour une VaR à 99,5 % : elle est souvent trop optimiste.

### Exercices

- **11.1 (A)** N suit une loi de Poisson de paramètre 20, et le coût d'un sinistre suit une loi exponentielle de moyenne 1 000. Calculez E\[S\], Var(S) et σ~S~.
- **11.2 (B)** Le nombre de sinistres a une moyenne de 10 et une variance de 15 (loi binomiale négative). Le coût d'un sinistre a une moyenne de 2 000 et un écart-type de 1 000. Calculez E\[S\] et σ~S~. Quelle part de la variance vient de l'incertitude sur le nombre de sinistres ?
- **11.3 (C, type examen)** N suit une loi de Poisson de paramètre 10. Un sinistre coûte 1 000 avec probabilité 0,7 et 5 000 avec probabilité 0,3. (a) Calculez E\[X\] et E\[X²\]. (b) Calculez E\[S\], Var(S) et σ~S~. (c) Donnez la VaR à 99 % par approximation normale. (d) Avec l'algorithme de Panjer en Python (unité = 1 000), calculez la VaR exacte à 95 %, 99 % et 99,5 %, et la TVaR exacte à 99 %. (e) Comparez (c) et (d) et commentez. Quel capital faut-il détenir au-delà de la prime pure pour un niveau de 99,5 % ?

## Fiche mémo : VaR et TVaR (Partie 3)

Cette page résume toute la Partie 3. Relisez-la avant un devoir.

**Les définitions**

- VaR~α~(X) = plus petite valeur x telle que F(x) ≥ α. C'est le quantile d'ordre α de la perte.
- TVaR~α~(X) = moyenne des VaR~u~ pour u entre α et 1 = perte moyenne dans les (1 − α) pires cas.
- On a toujours E\[X\] ≤ TVaR~α~ et VaR~α~ ≤ TVaR~α~.

**Les trois formules de la TVaR** (v = VaR~α~)

1. Toutes lois : TVaR = v + (1/(1 − α)) × ∫~v~^∞^ S(x) dx.
2. Loi continue : TVaR = E\[X | X > v\] = (1/(1 − α)) × ∫~v~^∞^ x f(x) dx.
3. Toutes lois, indispensable en discret : TVaR = \[E\[X · 1(X > v)\] + v (F(v) − α)\] / (1 − α).

**Les recettes**

| Situation | VaR | TVaR |
| --- | --- | --- |
| Loi continue | Résoudre F(x) = α, ou S(x) = 1 − α | Formule 1 ou 2 |
| Loi discrète | Première valeur où F(x) ≥ α | Formule 3, ou décomposer les (1 − α) pires cas |
| Loi mixte | α dans le saut : valeur du saut ; sinon partie continue | Formule 1 ou 3 |
| Données (n valeurs triées) | x~(⌈nα⌉)~ | Moyenne des n(1 − α) plus grandes valeurs |
| Modèle collectif | Panjer ou simulation | Panjer ou simulation |

**Les formules fermées**

| Loi | VaR~α~ | TVaR~α~ |
| --- | --- | --- |
| Uniforme \[a, b\] | a + α(b − a) | (VaR + b)/2 |
| Exponentielle (λ) | −ln(1 − α)/λ | VaR + 1/λ |
| Normale (μ, σ) | μ + σ z~α~ | μ + σ φ(z~α~)/(1 − α) |
| Pareto (x~m~, a > 1) | x~m~ (1 − α)^−1/a^ | a VaR/(a − 1) |

**Les valeurs normales** : z~0,95~ = 1,6449 ; z~0,975~ = 1,9600 ; z~0,99~ = 2,3263 ; z~0,995~ = 2,5758. Coefficients de TVaR : 2,0627 ; 2,3378 ; 2,6652 ; 2,8919.

**Les propriétés** : VaR et TVaR sont monotones, invariantes par translation et positivement homogènes. Seule la TVaR est toujours sous-additive (cohérente).

**Le modèle collectif** : E\[S\] = E\[N\] E\[X\] ; Var(S) = E\[N\] Var(X) + Var(N) E\[X\]² ; Poisson : Var(S) = λ E\[X²\].

**Python** : `loi.ppf(alpha)` donne la VaR ; `np.quantile(x, alpha, method='inverted_cdf')` donne la VaR empirique ; `x[x > v].mean()` donne la CTE simulée ; les fonctions `var_discrete`, `tvar_discrete`, `tvar_continue` et `panjer_poisson` sont dans le notebook de la Partie 3.

## Corrigés des exercices : Partie 3

### Chapitre 8

**8.1** Portefeuille A : la perte vaut toujours 100, donc VaR~0,98~ = VaR~0,995~ = **100**. Portefeuille B : F(0) = 0,99. Pour α = 0,98, F(0) ≥ 0,98, donc VaR~0,98~ = **0** ; pour α = 0,995, F(0) < 0,995, donc VaR~0,995~ = **10 000**. À 98 %, la VaR juge B **moins** risqué que A, alors que B peut perdre 10 000 : le sinistre de B a une probabilité (1 %) inférieure à 1 − α (2 %), et la VaR ne le voit pas. La TVaR, elle, le voit : TVaR~0,98~(B) = (10 000 × 0,01)/0,02 = 5 000.

**8.2** (a) X = 100 − V₁. Comme V₁ est normale N(101, 4²), X est normale de moyenne 100 − 101 = −1 et d'écart-type 4. (b) VaR~0,99~ = −1 + 4 × 2,3263 ≈ **8,31**.

### Chapitre 9

**9.1** Avec θ = 2 000 : VaR = θ ln(1/(1 − α)) et TVaR = VaR + θ.

| α | VaR | TVaR |
| --- | --- | --- |
| 95 % | 2 000 × ln(20) ≈ **5 991** | **7 991** |
| 99 % | 2 000 × ln(100) ≈ **9 210** | **11 210** |

**9.2** (a) F(0) = 0,70 ; F(1 000) = 0,90 ; F(5 000) = 0,98 ; F(20 000) = 1. Donc VaR~0,90~ = **1 000** (égalité), VaR~0,95~ = **5 000**, VaR~0,98~ = **5 000** (égalité), VaR~0,99~ = **20 000**.

(b) Formule 3 avec v = 5 000 :

$$
\mathrm{TVaR}_{0{,}95} = \frac{20\,000 \times 0{,}02 + 5000 \times (0{,}98 - 0{,}95)}{0{,}05} = \frac{400 + 150}{0{,}05} = 11\,000
$$

Décomposition : les 5 % pires cas sont 3 % à 5 000 et 2 % à 20 000, de moyenne (150 + 400)/0,05 = **11 000**.

(c) CTE~0,95~ = E\[X | X > 5 000\] = **20 000**. Elle ne retient que les 2 % de cas à 20 000 et oublie les 3 % à 5 000 qui font partie des 5 % pires cas.

**9.3** F(x) = x/b sur \[0, b\], donc VaR~α~ = αb. Avec la formule 1 :

$$
\int_{\alpha b}^{b} \Big(1 - \frac{x}{b}\Big)dx = \frac{b\,(1-\alpha)^2}{2} \qquad \mathrm{TVaR}_\alpha = \alpha b + \frac{b\,(1-\alpha)}{2} = \frac{(1+\alpha)\,b}{2}
$$

La TVaR est exactement au milieu entre la VaR et la perte maximale b : au-delà de la VaR, la loi reste uniforme.

**9.4** VaR~0,975~ = 5 + 20 × 1,96 ≈ **44,20** ; ES~0,975~ = 5 + 20 × 2,3378 ≈ **51,76** millions de FCFA.

**9.5** Valeurs triées : 2, 5, 7, 9, 12, 15, 18, 30, 45, 60. (a) nα = 8, donc VaR~0,8~ = x~(8)~ = **30** ; n(1 − α) = 2, donc TVaR~0,8~ = (45 + 60)/2 = **52,5**. (b) VaR~0,75~ = x~(⌈7,5⌉)~ = x~(8)~ = 30, avec F~n~(30) = 0,8. Formule 3 :

$$
\mathrm{TVaR}_{0{,}75} = \frac{(45 + 60)/10 + 30 \times (0{,}80 - 0{,}75)}{0{,}25} = \frac{10{,}5 + 1{,}5}{0{,}25} = 48
$$

**9.6** (a) Moyenne de M1 : a x~m~/(a − 1) = 3 × 1 000/2 = **1 500** ; M2 est donc une exponentielle de moyenne 1 500. (b) et (c) :

| Niveau | VaR M1 (Pareto) | TVaR M1 | VaR M2 (exponentielle) | TVaR M2 |
| --- | --- | --- | --- | --- |
| 99 % | 1 000 × 100^1/3^ ≈ 4 642 | 1,5 × 4 642 ≈ 6 962 | 1 500 ln(100) ≈ 6 908 | 8 408 |
| 99,9 % | 1 000 × 1 000^1/3^ = 10 000 | 15 000 | 1 500 ln(1 000) ≈ 10 362 | 11 862 |

(d) À 99 %, l'exponentielle paraît plus prudente avec les deux mesures. À 99,9 %, les VaR sont proches (10 000 contre 10 362), mais la TVaR de la Pareto est nettement plus élevée (15 000 contre 11 862) : loin dans la queue, la Pareto produit des sinistres bien plus gros. Le classement dépend donc de la mesure et du niveau. Conseil : vérifier sur les données lequel des deux modèles décrit le mieux les gros sinistres, raisonner en TVaR à un niveau élevé, et, en cas de doute, retenir le modèle à queue lourde par prudence.

### Chapitre 10

**10.1** Par homogénéité et invariance par translation : VaR~0,99~(Y) = 2 × 4 605,2 + 500 ≈ **9 710,3** et TVaR~0,99~(Y) = 2 × 5 605,2 + 500 ≈ **11 710,3**.

**10.2** VaR~0,99~ du desk 1 : 10 × 2,3263 = 23,26 ; du desk 2 : 46,53 ; somme : 69,79.

| ρ | σ du total | VaR du total | Gain de diversification |
| --- | --- | --- | --- |
| 0,5 | √(100 + 400 + 200) ≈ 26,46 | ≈ 61,55 | ≈ 8,24 |
| −0,5 | √(100 + 400 − 200) ≈ 17,32 | ≈ 40,29 | ≈ 29,50 |

Plus la corrélation est faible, plus la diversification réduit le risque. Une corrélation négative est une couverture : les pertes d'un desk sont en partie compensées par les gains de l'autre.

**10.3** (a) Pour chaque prêt, F(0) = 0,97 ≥ 0,95, donc VaR = **0**, et TVaR = (1 000 × 0,03)/0,05 = **600**.

(b) Loi du total : 0 avec probabilité 0,97² = 0,9409 ; 1 000 avec probabilité 2 × 0,97 × 0,03 = 0,0582 ; 2 000 avec probabilité 0,03² = 0,0009. F(0) = 0,9409 < 0,95 ≤ F(1 000) = 0,9991, donc VaR = **1 000**.

$$
\mathrm{TVaR}_{0{,}95} = \frac{2000 \times 0{,}0009 + 1000 \times (0{,}9991 - 0{,}95)}{0{,}05} = \frac{1{,}8 + 49{,}1}{0{,}05} = 1018
$$

(c) VaR : 1 000 > 0 + 0, elle n'est **pas** sous-additive. TVaR : 1 018 ≤ 600 + 600 = 1 200, elle **est** sous-additive.

(d) Avec la VaR, chaque prêt isolé semble sans risque, alors que le portefeuille demande du capital. Un métier pourrait réduire son capital affiché en découpant ses positions. La TVaR mesure la gravité des défauts et récompense toujours la diversification : c'est une base plus sûre pour fixer et répartir le capital.

### Chapitre 11

**11.1** E\[S\] = 20 × 1 000 = **20 000** ; Var(S) = λ E\[X²\] = 20 × 2 × 1 000² = **40 000 000** ; σ~S~ ≈ **6 325**.

**11.2** E\[S\] = 10 × 2 000 = **20 000**. Var(S) = 10 × 1 000² + 15 × 2 000² = 10 000 000 + 60 000 000 = 70 000 000, donc σ~S~ ≈ **8 367**. La part due à l'incertitude sur le nombre est 60/70 ≈ **86 %**.

**11.3** (a) E\[X\] = 0,7 × 1 000 + 0,3 × 5 000 = **2 200** ; E\[X²\] = 0,7 × 1 000² + 0,3 × 5 000² = **8 200 000**.

(b) E\[S\] = 10 × 2 200 = **22 000** ; Var(S) = 10 × 8 200 000 = **82 000 000** ; σ~S~ ≈ **9 055**.

(c) VaR~0,99~ ≈ 22 000 + 2,3263 × 9 055 ≈ **43 066**.

(d) Avec Panjer (unité = 1 000 : f~X~(1) = 0,7 et f~X~(5) = 0,3) :

```python
fx = np.zeros(6); fx[1], fx[5] = 0.7, 0.3
fs = panjer_poisson(10, fx, 200)
valeurs = np.arange(201) * 1000
for a in [0.95, 0.99, 0.995]:
    print(a, var_discrete(valeurs, fs, a), tvar_discrete(valeurs, fs, a))
```

| Niveau | VaR exacte | TVaR exacte |
| --- | --- | --- |
| 95 % | **38 000** | ≈ 43 109 |
| 99 % | **46 000** | ≈ **50 546** |
| 99,5 % | **49 000** | ≈ 53 476 |

(e) L'approximation normale donne 43 066 au lieu de 46 000 à 99 % : elle sous-estime la VaR d'environ 3 000, soit 6 %, car la charge totale est asymétrique à droite (les sinistres de 5 000 tirent la queue). Capital au-delà de la prime pure à 99,5 % : 49 000 − 22 000 = **27 000**.

## Partie 4. Aller plus loin

## Chapitre 12. La simulation Monte Carlo

La simulation remplace un calcul impossible à la main par une expérience répétée des milliers de fois. Son erreur diminue comme 1/√n : pour diviser l'erreur par 10, il faut 100 fois plus de simulations. Une VaR simulée doit donc toujours être accompagnée de sa précision.

### Pourquoi c'est utile

Dès qu'un modèle combine plusieurs risques (sinistres, marchés, crédit), des franchises, des plafonds ou de la dépendance, la loi de la perte totale n'a plus de formule. Les modèles internes des assureurs et des banques reposent alors sur la simulation : on simule des centaines de milliers de scénarios, puis on lit la VaR et la TVaR sur les pertes simulées.

### 12.1 Le principe

&#91;N1\] Pour estimer une espérance, on simule beaucoup de scénarios indépendants et on fait la moyenne. Pour estimer une probabilité, on compte la proportion de scénarios où l'événement se produit.

&#91;N2\] Avec X₁, …, X~n~ simulées indépendamment selon la loi de X :

$$
\widehat{E[g(X)]} = \frac{1}{n}\sum_{i=1}^n g(X_i) \qquad \widehat{P(X \in A)} = \frac{1}{n}\sum_{i=1}^n \mathbf{1}_{\{X_i \in A\}}
$$

La loi des grands nombres (chapitre 7) garantit que ces estimations convergent vers les vraies valeurs quand n grandit.

### 12.2 La précision : erreur standard et intervalle de confiance

&#91;N2\] Par le théorème central limite, l'estimateur est approximativement normal, d'écart-type appelé **erreur standard** (se). On en déduit un intervalle de confiance à 95 % :

$$
\mathrm{se} = \frac{s}{\sqrt{n}} \qquad \text{IC}_{95\%} = \hat{\theta} \pm 1{,}96\,\mathrm{se} \qquad \text{pour une probabilité : } \mathrm{se} = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
$$

Ici s est l'écart-type des valeurs simulées g(X~i~).

**Combien de simulations ?** Pour estimer p ≈ 0,135 à ± 0,001 près (avec 95 % de confiance), il faut 1,96² × p(1 − p)/0,001² ≈ **450 000** simulations. La règle générale : la précision progresse comme √n.

### 12.3 Simuler les lois dont on a besoin

- **Lois usuelles** : `rng.exponential`, `rng.normal`, `rng.poisson`, ou `loi.rvs(size=n, random_state=rng)` pour toute loi de SciPy.
- **Toute loi dont on connaît F⁻¹** : méthode d'inversion, X = F⁻¹(U) avec U uniforme (chapitre 3).
- **Une loi discrète quelconque** : `rng.choice(valeurs, size=n, p=probas)`.
- &#91;N3\] **Deux normales corrélées** : avec Z₁ et Z₂ normales standard indépendantes, les variables Z₁ et ρZ₁ + √(1 − ρ²) Z₂ sont normales standard de corrélation ρ. En dimension supérieure, on utilise la décomposition de Cholesky de la matrice de corrélation.

### 12.4 VaR et TVaR par simulation, et leur précision

&#91;N2\] On simule n pertes, on lit la VaR empirique (chapitre 9, section 9.5) et la TVaR empirique (moyenne des pertes au-delà). Mais un quantile extrême repose sur peu de scénarios : seulement n(1 − α) d'entre eux dépassent la VaR. Pour une loi continue de densité f, l'écart-type de la VaR simulée vaut environ :

$$
\mathrm{se}\big(\widehat{\mathrm{VaR}}_\alpha\big) \approx \frac{1}{f(\mathrm{VaR}_\alpha)}\sqrt{\frac{\alpha(1-\alpha)}{n}}
$$

Pour une exponentielle de moyenne 1 000 et α = 99 % (f(VaR) = 0,01/1 000) :

| Nombre de simulations n | Scénarios au-delà de la VaR | Écart-type de la VaR simulée |
| --- | --- | --- |
| 1 000 | 10 | ≈ 315 |
| 10 000 | 100 | ≈ 99 |
| 1 000 000 | 10 000 | ≈ 10 |

En pratique, on ne connaît pas f : on estime la précision par **bootstrap**. On ré-échantillonne avec remise les pertes simulées, on recalcule la VaR des milliers de fois, et on regarde la dispersion obtenue.

### Exemple résolu : Ce que vaut une simulation de 10 000 scénarios

**Énoncé** : on simule 10 000 sinistres exponentiels de moyenne 1 000. On estime P(X > 2 000), la VaR et la TVaR à 99 %, avec leur précision, et on compare aux valeurs exactes.

| Quantité | Valeur exacte | Estimation (n = 10 000) | Intervalle à 95 % |
| --- | --- | --- | --- |
| P(X > 2 000) | e^−2^ ≈ 0,1353 | 0,1332 | \[0,1265 ; 0,1399\] (formule) |
| VaR à 99 % | 4 605 | 4 575 | \[4 392 ; 4 732\] (bootstrap) |
| TVaR à 99 % | 5 605 | 5 508 | \[5 247 ; 5 790\] (bootstrap) |

**Lecture** : les trois valeurs exactes sont bien dans leurs intervalles. Mais l'incertitude sur la VaR (environ ± 170) et surtout sur la TVaR (environ ± 270) est importante. Pour un capital réglementaire, on simule plutôt des centaines de milliers ou des millions de scénarios.

### 12.5 \[N3\] Réduire la variance : les variables antithétiques

Si U est uniforme, 1 − U l'est aussi, et les deux sont négativement corrélées. En simulant les paires F⁻¹(U) et F⁻¹(1 − U) et en faisant la moyenne de chaque paire, les erreurs se compensent en partie. Pour estimer la moyenne d'une exponentielle avec 10 000 évaluations, l'erreur standard passe d'environ 10 (simulation simple) à environ 6 (5 000 paires antithétiques) : c'est comme si on avait simulé 2,7 fois plus de scénarios.

### En Python

```python
import numpy as np
rng = np.random.default_rng(42)

n = 10_000
x = rng.exponential(1000, size=n)

# Probabilite et intervalle de confiance
p = np.mean(x > 2000); se = np.sqrt(p * (1 - p) / n)
print(p, (p - 1.96*se, p + 1.96*se))

# VaR et TVaR simulees
v = np.quantile(x, 0.99, method='inverted_cdf'); t = x[x > v].mean()

# Precision par bootstrap
vs, ts = [], []
for _ in range(2000):
    y = rng.choice(x, size=n, replace=True)
    q = np.quantile(y, 0.99, method='inverted_cdf')
    vs.append(q); ts.append(y[y > q].mean())
print(v, np.percentile(vs, [2.5, 97.5]))
print(t, np.percentile(ts, [2.5, 97.5]))

# Deux normales correlees (rho = 0.5)
z1, z2 = rng.standard_normal(n), rng.standard_normal(n)
w = 0.5 * z1 + np.sqrt(1 - 0.5**2) * z2
print(np.corrcoef(z1, w)[0, 1])                 # ~0.5
```

### Erreurs fréquentes

- Donner une VaR simulée sans sa précision.
- Simuler trop peu de scénarios pour un niveau élevé : à 99,5 % avec 1 000 scénarios, la VaR repose sur 5 valeurs.
- Changer de graine entre deux calculs que l'on veut comparer : les écarts observés mélangent alors l'effet étudié et le bruit de simulation.
- Simuler des risques indépendants alors qu'ils sont liés : la VaR du total est alors sous-estimée.

## Chapitre 13. Estimer une loi sur des données

Pour calculer une VaR sur des données réelles, on a deux voies : lire directement les quantiles des données (méthode historique), ou ajuster une loi puis appliquer ses formules (méthode paramétrique). Deux lois qui décrivent aussi bien le cœur des données peuvent donner des VaR élevées très différentes : le choix se joue dans la queue.

### Pourquoi c'est utile

Dans la réalité, personne ne vous donne la loi des sinistres : vous avez un fichier d'historique. Avec peu de données, la méthode historique ne peut pas dépasser la plus grosse perte observée. La méthode paramétrique peut extrapoler au-delà, à condition que la loi choisie soit la bonne.

### 13.1 Historique ou paramétrique ?

| Critère | Méthode historique | Méthode paramétrique |
| --- | --- | --- |
| Principe | Quantiles et moyennes de queue lus sur les données | On ajuste une loi, puis on applique ses formules |
| Hypothèse | Le futur ressemble au passé observé | La loi choisie est la bonne |
| Avantage | Aucune hypothèse de forme | Extrapole au-delà des données, formules explicites |
| Limite | Ne voit rien au-delà de la plus grosse perte observée | Un mauvais choix de loi fausse la VaR élevée |
| Données nécessaires | Beaucoup, surtout pour α élevé | Moins, mais elles doivent bien représenter la queue |

### 13.2 La méthode des moments

&#91;N1\] On choisit les paramètres pour que la moyenne (et si besoin la variance) de la loi soient égales à celles des données.

&#91;N2\] Avec la moyenne empirique x̄ et la variance empirique s² :

| Loi | Paramètres par les moments |
| --- | --- |
| Exponentielle (moyenne θ) | θ̂ = x̄ |
| Poisson (λ) | λ̂ = x̄ |
| Gamma (α, θ) | α̂ = x̄²/s² ; θ̂ = s²/x̄ |
| Normale (μ, σ) | μ̂ = x̄ ; σ̂ = s |

### 13.3 Le maximum de vraisemblance

&#91;N1\] On choisit les paramètres qui rendent les données observées les plus probables. C'est la méthode de référence en statistique et en actuariat.

&#91;N2\] Pour des observations indépendantes x₁, …, x~n~ et une densité f(x ; θ), on maximise la log-vraisemblance :

$$
\ell(\theta) = \sum_{i=1}^n \ln f(x_i ; \theta) \qquad \hat{\theta} = \arg\max_\theta \ell(\theta)
$$

**Exemple : l'exponentielle de moyenne θ.** Avec f(x ; θ) = e^−x/θ^/θ :

$$
\ell(\theta) = -n\ln\theta - \frac{1}{\theta}\sum_{i=1}^n x_i \qquad \ell'(\theta) = -\frac{n}{\theta} + \frac{1}{\theta^2}\sum_{i=1}^n x_i = 0 \iff \hat{\theta} = \bar{x}
$$

Les estimateurs à connaître :

| Loi | Estimateur du maximum de vraisemblance | SciPy |
| --- | --- | --- |
| Exponentielle | θ̂ = x̄ | `stats.expon.fit(x, floc=0)` |
| Poisson | λ̂ = x̄ | moyenne des données |
| Normale | μ̂ = x̄ ; σ̂² = (1/n) Σ (x~i~ − x̄)² | `stats.norm.fit(x)` |
| Lognormale | μ̂ et σ̂ = moyenne et écart-type des ln(x~i~) | `stats.lognorm.fit(x, floc=0)` |
| Pareto (x~m~ connu) | â = n / Σ ln(x~i~/x~m~) | formule directe |

Dans SciPy, `floc=0` fixe le décalage à 0. Sans cette option, SciPy ajuste aussi un paramètre de position, ce qui donne souvent des résultats déroutants.

### Exemple résolu : Quelle loi pour nos 20 sinistres ?

**Énoncé** : reprenons les 20 sinistres du chapitre 9 (moyenne 132,6). Ajustez une exponentielle et une lognormale, puis comparez les VaR et TVaR obtenues avec les valeurs historiques.

**Ajustements** : exponentielle, θ̂ = x̄ = 132,6. Lognormale : moyenne des logarithmes μ̂ ≈ 3,836 et écart-type σ̂ ≈ 1,474.

| Mesure | Historique | Exponentielle | Lognormale |
| --- | --- | --- | --- |
| VaR à 90 % | 310 | 305 | 306 |
| TVaR à 90 % | 750 | 438 | 792 |
| VaR à 99 % | non estimable (20 données) | 611 | 1 430 |
| Test de Kolmogorov-Smirnov (p-valeur) | - | 0,12 | 0,99 |

**Lecture** : les trois méthodes donnent presque la même VaR à 90 %. Pourtant, l'exponentielle sous-estime fortement la TVaR (438 contre 750 observés), alors que la lognormale la reproduit (792). Elle décrit mieux la queue, et son test d'ajustement est bien meilleur. À 99 %, les deux modèles diffèrent d'un facteur 2,3. On retient la lognormale.

**Mais attention à l'incertitude** : avec seulement 20 données, un bootstrap sur les paramètres de la lognormale donne pour la VaR à 99 % un intervalle à 95 % d'environ \[350 ; 4 000\]. Le chiffre de 1 430 est donc très fragile : il faudrait beaucoup plus de données avant de fixer un capital avec.

### 13.4 Valider un ajustement

1. **Comparer visuellement** l'histogramme des données et la densité ajustée.
2. **Tracer un QQ-plot** : quantiles empiriques contre quantiles théoriques. Si la loi est bonne, les points sont alignés sur la diagonale. Des points qui s'en écartent vers le haut à droite signalent une queue plus lourde que le modèle.
3. **Faire un test d'ajustement** (Kolmogorov-Smirnov, `stats.kstest`) : une p-valeur faible (moins de 0,05) conduit à rejeter la loi.
4. **Comparer les mesures de queue** : TVaR modèle contre TVaR historique, et fonction d'excès moyen (chapitre 5).

&#91;N3\] Pour les très gros sinistres, la **théorie des valeurs extrêmes** ne modélise que les dépassements d'un seuil élevé, avec une loi de Pareto généralisée (méthode POT, « Peaks Over Threshold »). L'estimateur de Hill estime l'indice de queue a d'une Pareto à partir des k plus grandes observations.

### En Python

```python
import numpy as np
from scipy import stats, integrate
import matplotlib.pyplot as plt

x = np.array([3, 4, 9, 12, 15, 18, 22, 28, 37, 45, 55, 61, 70, 78, 95, 140, 150, 310, 520, 980.])

E = stats.expon(scale=x.mean())                         # exponentielle (max. de vraisemblance)
mu, sigma = np.log(x).mean(), np.log(x).std()           # lognormale (max. de vraisemblance)
L = stats.lognorm(s=sigma, scale=np.exp(mu))

for nom, loi in [('expo', E), ('lognormale', L)]:
    v = loi.ppf(0.9)
    tv = v + integrate.quad(loi.sf, v, np.inf)[0] / 0.1
    print(nom, round(v), round(tv), round(loi.ppf(0.99)), stats.kstest(x, loi.cdf).pvalue)

# QQ-plot de la lognormale
n = len(x); probs = (np.arange(1, n + 1) - 0.5) / n
plt.scatter(L.ppf(probs), np.sort(x)); plt.plot([0, 1000], [0, 1000], 'r')
plt.xlabel('quantiles theoriques'); plt.ylabel('quantiles observes'); plt.show()
```

### Erreurs fréquentes

- Choisir la loi sur le cœur des données seulement : c'est la queue qui détermine la VaR et la TVaR élevées.
- Oublier `floc=0` dans les ajustements SciPy de lois positives.
- Annoncer une VaR à 99,5 % estimée sur 50 données sans dire qu'elle est très incertaine.
- Accepter une loi parce que le test ne la rejette pas : avec peu de données, un test rejette rarement quoi que ce soit.

## Chapitre 14. Le backtesting de la VaR

Backtester une VaR, c'est vérifier après coup qu'elle a été dépassée aussi souvent que prévu : environ 1 jour sur 100 pour une VaR à 99 %. Si le modèle est juste, le nombre de dépassements suit une loi binomiale, ce qui permet de construire un test. Mais sur 250 jours, ce test détecte mal un modèle faux.

### Pourquoi c'est utile

Une VaR est une prévision. Comme toute prévision, elle doit être confrontée à la réalité. Les régulateurs bancaires imposent ce contrôle : une banque dont la VaR est trop souvent dépassée voit son exigence de capital augmentée, voire perd le droit d'utiliser son modèle interne.

### 14.1 Les exceptions

&#91;N2\] Chaque jour t, la banque calcule sa VaR à un jour, VaR~t~, puis observe sa perte réelle L~t~. Il y a **exception** si L~t~ > VaR~t~. On note I~t~ = 1 en cas d'exception, 0 sinon, et N le nombre d'exceptions sur n jours.

Si le modèle est correct, chaque jour est une exception avec probabilité 1 − α, indépendamment des autres jours. Donc :

$$
N = \sum_{t=1}^{n} I_t \sim \text{Binomiale}(n,\; 1-\alpha) \qquad E[N] = n(1-\alpha)
$$

Avec n = 250 jours (une année boursière) et α = 99 %, on attend 2,5 exceptions. Voici la loi de N pour un modèle correct :

| Nombre d'exceptions k | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P(N = k) | 0,081 | 0,205 | 0,257 | 0,215 | 0,134 | 0,067 | 0,028 | 0,010 | 0,003 | 0,001 | 0,000 |
| P(N ≤ k) | 0,081 | 0,286 | 0,543 | 0,758 | 0,892 | 0,959 | 0,986 | 0,996 | 0,999 | 1,000 | 1,000 |

Même avec un modèle parfait, on observe 5 exceptions ou plus une année sur dix environ (1 − 0,892 ≈ 0,108).

### 14.2 Le test de Kupiec

&#91;N1\] On compare la proportion d'exceptions observée, x/n, à la proportion attendue, p = 1 − α. Si l'écart est trop grand dans un sens ou dans l'autre, on rejette le modèle.

&#91;N2\] La statistique du test (rapport de vraisemblance, « proportion of failures ») compare la vraisemblance sous p et sous la proportion observée p̂ = x/n :

$$
\mathrm{LR} = -2\ln\Big[(1-p)^{n-x}\,p^{x}\Big] + 2\ln\Big[(1-\hat{p})^{n-x}\,\hat{p}^{x}\Big]
$$

Si le modèle est correct, LR suit approximativement une loi du χ² à 1 degré de liberté. Au seuil de 5 %, on **rejette le modèle si LR > 3,841**.

| Exceptions sur 250 jours (VaR à 99 %) | LR | p-valeur | Décision au seuil de 5 % |
| --- | --- | --- | --- |
| 0 | 5,03 | 0,025 | Rejet : modèle trop prudent |
| 2 | 0,11 | 0,74 | Accepté |
| 5 | 1,96 | 0,16 | Accepté |
| 7 | 5,50 | 0,019 | Rejet : VaR sous-estimée |
| 10 | 12,96 | 0,0003 | Rejet net |

Remarquez la première ligne : zéro exception n'est pas une bonne nouvelle. Une VaR jamais dépassée est trop élevée, donc la banque immobilise trop de capital.

### 14.3 Les feux tricolores de Bâle

&#91;N2\] Le Comité de Bâle classe les modèles selon le nombre d'exceptions de la VaR à 99 % à un jour, sur 250 jours ([Bâle, MAR32](https://www.bis.org/committees/bcbs/basel-framework/81326/chapter.pdf)) :

| Zone | Exceptions sur 250 jours | Probabilité si le modèle est correct | Conséquence |
| --- | --- | --- | --- |
| Verte | 0 à 4 | 89,2 % | Pas de pénalité liée au backtesting |
| Jaune | 5 à 9 | 10,8 % | Multiplicateur de capital augmenté, explications demandées |
| Rouge | 10 ou plus | 0,03 % | Problème presque certain du modèle, pénalité maximale |

Les probabilités de la troisième colonne viennent directement de la loi binomiale (250 ; 0,01) du tableau 14.1.

### Exemple résolu : Un modèle faux peut passer le test

**Énoncé** : les vraies pertes quotidiennes d'un desk suivent une loi de Student à 4 degrés de liberté (queues lourdes), ramenée à un écart-type d'un million de FCFA. La banque utilise une VaR normale de même écart-type : VaR~0,99~ = 2,3263 millions. Que donne le backtesting ?

**Vraie probabilité d'exception** : sous la loi de Student, P(L > 2,3263) ≈ 1,51 %, au lieu du 1 % annoncé. Le modèle sous-estime le risque : on attend 3,8 exceptions par an au lieu de 2,5.

**Probabilité de chaque zone avec ce modèle faux** (binomiale (250 ; 0,0151)) :

| Zone | Modèle correct | Modèle normal faux |
| --- | --- | --- |
| Verte | 89,2 % | 67,3 % |
| Jaune | 10,8 % | 32,2 % |
| Rouge | 0,03 % | 0,5 % |

**Leçon** : deux fois sur trois, ce modèle faux passe en zone verte. Sur une simulation d'une année, on a même observé une seule exception. Un an de données ne suffit pas à démasquer un modèle qui sous-estime modérément les queues : le backtesting a peu de **puissance**. Il faut le compléter par l'analyse de la taille des dépassements, des stress tests et des historiques plus longs.

### 14.4 \[N3\] Aller plus loin

- **L'indépendance des exceptions** : si les exceptions arrivent en grappes (plusieurs jours de suite pendant une crise), le modèle réagit trop lentement. Le test de Christoffersen vérifie en plus du nombre que les exceptions sont indépendantes dans le temps.
- **Backtester l'Expected Shortfall** : l'ES n'est pas élicitable à elle seule (chapitre 10), ce qui complique son backtesting direct. Dans le cadre de Bâle, les banques continuent de backtester des VaR, aux niveaux 97,5 % et 99 %, en plus de calculer l'ES.

### En Python

```python
import numpy as np
from scipy import stats
rng = np.random.default_rng(7)

def kupiec(x, n, p):
    p_hat = x / n
    l0 = (n - x) * np.log(1 - p) + x * np.log(p)
    l1 = (n - x) * np.log(1 - p_hat) + (x * np.log(p_hat) if x > 0 else 0)
    LR = -2 * (l0 - l1)
    return LR, stats.chi2.sf(LR, df=1)

def zone_bale(x):
    return 'verte' if x <= 4 else ('jaune' if x <= 9 else 'rouge')

# Pertes reelles Student(4) ramenees a un ecart-type de 1 ; VaR normale
pertes = stats.t(4).rvs(250, random_state=rng) / np.sqrt(2)
var_normale = stats.norm.ppf(0.99)
x = int(np.sum(pertes > var_normale))
print(x, kupiec(x, 250, 0.01), zone_bale(x))

# Probabilite de chaque zone, modele correct ou faux
for p in [0.01, stats.t(4).sf(var_normale * np.sqrt(2))]:
    B = stats.binom(250, p)
    print(round(p, 4), B.cdf(4), B.cdf(9) - B.cdf(4), B.sf(9))
```

### Erreurs fréquentes

- Croire que zéro exception prouve que le modèle est bon : il peut être trop prudent.
- Conclure qu'un modèle est juste parce qu'il est en zone verte : le test a peu de puissance sur 250 jours.
- Compter les exceptions sur des pertes et une VaR qui ne portent pas sur le même périmètre (positions modifiées en cours de journée, commissions incluses).
- Oublier que le test suppose des exceptions indépendantes.

## Étude de cas de synthèse : Le capital d'une petite compagnie d'assurance

Cette étude de cas mobilise tout le cours : estimation, modèle collectif, simulation, VaR, TVaR et réassurance. Elle peut servir de sujet de devoir maison ou de projet en groupe.

**Contexte** : une compagnie d'assurance d'Abidjan lance un produit. Sur les cinq dernières années, un produit comparable a enregistré 42, 51, 47, 55 et 45 sinistres. Une étude des montants (en milliers de FCFA) conclut à une loi lognormale de paramètres μ = 6 et σ = 1.

**Questions** :

1. Estimez la fréquence annuelle λ par maximum de vraisemblance (loi de Poisson). Comparez la moyenne et la variance empirique des nombres de sinistres : la Poisson est-elle raisonnable ?
2. Calculez E\[X\] et E\[X²\] pour un sinistre, puis E\[S\] et σ~S~ pour la charge annuelle.
3. Donnez la VaR à 99,5 % par l'approximation normale.
4. Par simulation (500 000 années), estimez la VaR et la TVaR à 99,5 %. Comparez avec la question 3.
5. Le capital de solvabilité est défini comme VaR~99,5~ % − E\[S\]. Calculez-le.
6. La compagnie souscrit une réassurance qui paie, pour chaque sinistre, la part au-delà de 3 000 (traité en excédent de sinistre). Calculez la prime pure annuelle de ce traité, puis le nouveau capital de solvabilité. La réassurance est-elle intéressante ?
7. Rédigez en cinq lignes une note pour la direction générale : capital nécessaire, intérêt de la réassurance, et principales incertitudes du calcul.

**Corrigé** :

1. λ̂ = (42 + 51 + 47 + 55 + 45)/5 = **48**. La variance empirique vaut 26, inférieure à la moyenne : pas de surdispersion, la Poisson est raisonnable. Avec cinq années seulement, cette vérification reste fragile.
2. E\[X\] = e^6\ +\ 0,5^ ≈ **665,1** ; E\[X²\] = e^2μ\ +\ 2σ²^ = e^14^ ≈ 1 202 604. Donc E\[S\] = 48 × 665,1 ≈ **31 927** et σ~S~ = √(48 × 1 202 604) ≈ **7 598**.
3. VaR ≈ 31 927 + 2,5758 × 7 598 ≈ **51 497**.
4. La simulation donne VaR~99,5~ % ≈ **56 400** et TVaR~99,5~ % ≈ **61 500**. L'approximation normale sous-estime la VaR d'environ 4 900, soit 9 % : la charge est asymétrique à cause de la queue lognormale.
5. Capital ≈ 56 400 − 31 900 ≈ **24 500**.
6. Prime pure du traité : λ × E\[(X − 3 000)₊\] = 48 × ∫₃₀₀₀^∞^ S(x) dx ≈ 48 × 37,3 ≈ **1 790** par an. Après réassurance, la charge conservée a une moyenne d'environ 30 130 et une VaR~99,5~ % d'environ 47 960 : le capital tombe à environ **17 800**. Pour environ 1 800 de prime pure par an (plus le chargement du réassureur), la compagnie économise environ 6 600 de capital. C'est en général intéressant, à comparer au coût du capital de la compagnie.
7. Exemple de note : « Le produit demande un capital d'environ 24,5 millions de FCFA au niveau de 99,5 %, au-delà d'une charge moyenne de 31,9 millions. Un traité en excédent de sinistre au-delà de 3 millions par sinistre réduit ce capital à environ 17,8 millions, pour une prime pure d'environ 1,8 million par an. Ces chiffres reposent sur cinq ans d'historique et sur une loi lognormale des montants ; une queue plus lourde augmenterait sensiblement le capital. Nous recommandons d'affiner l'estimation de la queue des sinistres avant de fixer le capital. »

Le code complet est dans le notebook de la Partie 4.

## Exercices de la Partie 4

- **12.1 (A)** Sur 40 000 scénarios simulés, 1 080 dépassent un seuil. Estimez la probabilité de dépassement et donnez son intervalle de confiance à 95 %.
- **12.2 (B)** Combien de simulations faut-il pour que l'erreur standard de la VaR à 99,5 % d'une exponentielle de moyenne 1 000 soit d'environ 20 ? *Indication : f(VaR~α~) = (1 − α)/θ pour une exponentielle.*
- **13.1 (A)** Les nombres annuels de sinistres sont 42, 51, 47, 55 et 45. Estimez λ d'une loi de Poisson par maximum de vraisemblance, puis calculez la variance empirique (diviseur n − 1). Commentez.
- **13.2 (B)** Cinq gros sinistres (en millions) dépassent le seuil x~m~ = 10 : 12, 15, 25, 11, 40. Estimez l'indice a d'une Pareto par maximum de vraisemblance, puis la moyenne et la VaR à 99 % correspondantes. Que pensez-vous de la fiabilité de ces chiffres ?
- **13.3 (B)** Montrez que l'estimateur du maximum de vraisemblance du paramètre λ d'une loi de Poisson est la moyenne empirique.
- **14.1 (A)** Sur 250 jours, une VaR à 99 % a été dépassée 6 fois. Dans quelle zone de Bâle se trouve le modèle ? Faites le test de Kupiec au seuil de 5 %.
- **14.2 (C, type examen)** Une banque backteste sa VaR à 97,5 % sur 250 jours et observe 12 exceptions. (a) Combien d'exceptions attendait-on ? (b) Calculez P(N ≥ 12) sous l'hypothèse d'un modèle correct. (c) Faites le test de Kupiec au seuil de 5 %. (d) Quelles causes possibles proposez-vous, et que recommandez-vous ?

## Corrigés des exercices : Partie 4

**12.1** p̂ = 1 080/40 000 = **0,027** ; se = √(0,027 × 0,973/40 000) ≈ 0,00081 ; IC à 95 % ≈ **\[0,0254 ; 0,0286\]**.

**12.2** f(VaR) = 0,005/1 000 = 5 × 10⁻⁶. On veut (1/f) √(0,995 × 0,005/n) = 20, donc √(0,004975/n) = 10⁻⁴ et n = 0,004975/10⁻⁸ ≈ **500 000** simulations.

**13.1** λ̂ = **48** ; variance empirique = (36 + 9 + 1 + 49 + 9)/4 = **26**. La variance n'excède pas la moyenne : aucun signe de surdispersion, la Poisson convient. Avec 5 observations, la conclusion reste fragile.

**13.2** Σ ln(x~i~/10) = ln 1,2 + ln 1,5 + ln 2,5 + ln 1,1 + ln 4 ≈ 2,986, donc â = 5/2,986 ≈ **1,67**. Moyenne : â × 10/(â − 1) ≈ **24,8** millions ; VaR~0,99~ = 10 × 0,01^−1/â^ ≈ **156** millions. Avec 5 données et un indice inférieur à 2 (variance infinie), ces chiffres sont extrêmement incertains : un seul sinistre de plus peut les changer fortement.

**13.3** Log-vraisemblance : $\ell$(λ) = Σ (−λ + x~i~ ln λ − ln x~i~!) = −nλ + (Σ x~i~) ln λ + constante. $\ell$'(λ) = −n + (Σ x~i~)/λ = 0 donne **λ̂ = x̄**. La dérivée seconde −(Σ x~i~)/λ² est négative : c'est bien un maximum.

**14.1** 6 exceptions : **zone jaune** (5 à 9). Kupiec : LR ≈ 3,56 < 3,841 (p-valeur ≈ 0,059), on ne rejette **pas** le modèle au seuil de 5 %, de justesse. Il faut surveiller le modèle et expliquer les exceptions.

**14.2** (a) 250 × 0,025 = **6,25**. (b) P(N ≥ 12) ≈ **0,025** sous la binomiale (250 ; 0,025). (c) LR ≈ 4,29 > 3,841 (p-valeur ≈ 0,038) : on **rejette** le modèle au seuil de 5 %. (d) Causes possibles : queues plus lourdes que supposé (hypothèse normale), volatilité sous-estimée lors d'une période agitée, historique trop ancien ou trop court pour calibrer, corrélations qui augmentent en période de crise. Recommandations : regarder si les exceptions sont regroupées dans le temps, tester une loi à queue plus lourde (Student) ou une simulation historique récente, et compléter par des stress tests.

## Banque d'exercices

Cette banque rassemble 40 exercices supplémentaires, tous tirés de situations réelles de banque, d'assurance et de microfinance. Chaque corrigé suit le même plan : ce que l'on cherche, la méthode, le calcul détaillé, l'interprétation pour un gestionnaire de risques, et le piège à éviter.

**Comment l'utiliser** : choisissez un thème, cherchez l'exercice au moins 20 minutes, puis lisez seulement la partie « Méthode » du corrigé et reprenez votre calcul. Ne lisez le calcul complet qu'en dernier. Les exercices marqués (C) sont du niveau d'un sujet d'examen. Tous les résultats sont vérifiés dans le notebook 08.

| Thème | Chapitres | Exercices |
| --- | --- | --- |
| 1. Probabilités, conditionnement, Bayes | 1 et 2 | 1 à 5 |
| 2. Variables aléatoires, F, S, quantiles | 3 | 6 à 10 |
| 3. Espérance, variance, franchises | 4 | 11 à 15 |
| 4. Lois usuelles | 5 | 16 à 21 |
| 5. Couples, corrélation, portefeuilles | 6 | 22 à 25 |
| 6. Théorèmes limites et mutualisation | 7 | 26 à 28 |
| 7. VaR, TVaR, modèle collectif | 8 à 11 | 29 à 35 |
| 8. Simulation, estimation, backtesting | 12 à 14 | 36 à 40 |

### Thème 1 : Probabilités, conditionnement, Bayes

**Exercice 1 (B) : Détecter la fraude au mobile money.** Un opérateur de mobile money estime que 0,2 % des transactions sont frauduleuses. Son système d'alerte signale 98 % des fraudes, mais aussi 1 % des transactions légitimes. (a) Quelle proportion des transactions déclenche une alerte ? (b) Une transaction déclenche une alerte : quelle est la probabilité qu'elle soit réellement frauduleuse ? (c) Sur un million de transactions par jour, combien d'alertes l'équipe de contrôle doit-elle traiter, et combien sont de vraies fraudes ? (d) Jusqu'où faut-il réduire le taux de fausses alertes pour qu'au moins une alerte sur deux soit une vraie fraude ? *Indication : probabilités totales, puis Bayes.*

**Exercice 2 (A) : Le contrôle d'un comité de crédit.** Une institution de microfinance a 12 dossiers de crédit en attente, dont 3 présentent un risque élevé. Un auditeur en examine 4, tirés au hasard. Calculez la probabilité qu'il ne trouve aucun dossier à risque, exactement un, puis au moins deux. *Indication : tirage sans remise, donc combinaisons.*

**Exercice 3 (B) : L'assurance récolte d'un planteur de cacao.** Chaque année, une plantation subit une sécheresse avec probabilité 0,15 et une maladie du cacaoyer avec probabilité 0,10, de façon indépendante. La récolte est perdue dès qu'au moins un des deux événements se produit. (a) Calculez la probabilité de perte de récolte une année donnée. (b) La récolte a été perdue : quelle est la probabilité qu'il y ait eu une sécheresse ? (c) Les années étant indépendantes, quelle est la probabilité d'au moins deux années de perte sur cinq ans ? *Indication pour (a) : passez par le contraire.*

**Exercice 4 (A) : Le serveur de secours d'une banque.** Le serveur principal d'une banque tombe en panne avec probabilité 2 % par jour, le serveur de secours avec probabilité 5 %. Le service s'arrête si les deux sont en panne le même jour. (a) Calculez la probabilité d'arrêt en supposant les pannes indépendantes. (b) En réalité, les deux serveurs partagent la même alimentation électrique, et P(secours en panne | principal en panne) = 0,30. Recalculez. (c) Commentez pour le responsable des risques opérationnels.

**Exercice 5 (C) : La politique d'octroi d'une banque.** Une banque classe ses emprunteurs en trois catégories : A (50 % des clients, taux de défaut 1 %), B (35 %, 4 %) et C (15 %, 12 %). (a) Calculez le taux de défaut global du portefeuille. (b) Un client fait défaut : quelle est la probabilité qu'il soit de catégorie C ? (c) La banque décide de refuser désormais la catégorie C. Quel est le nouveau taux de défaut parmi les clients acceptés ? (d) Quels arguments pour et contre cette décision pouvez-vous avancer ?

### Thème 2 : Variables aléatoires, F, S, quantiles

**Exercice 6 (A) : Les retards de paiement.** Le retard X (en jours) d'une facture impayée suit une loi exponentielle de moyenne 20 jours. (a) Calculez P(X > 30). (b) Calculez la médiane du retard. (c) Calculez le quantile à 95 %. (d) Une facture a déjà 10 jours de retard : quelle est la probabilité que le retard dépasse 40 jours ? Interprétez.

**Exercice 7 (A) : Les pannes de distributeurs automatiques.** Le nombre hebdomadaire N de pannes des distributeurs d'une agence suit la loi : P(N = 0) = 0,40 ; P(N = 1) = 0,30 ; P(N = 2) = 0,15 ; P(N = 3) = 0,10 ; P(N = 4) = 0,05. (a) Donnez la fonction de répartition. (b) Calculez E\[N\] et Var(N). (c) Calculez les quantiles d'ordre 0,5 ; 0,8 ; 0,9 et 0,95. (d) Combien de techniciens d'astreinte faut-il prévoir pour couvrir toutes les pannes de la semaine dans au moins 90 % des cas, si un technicien traite une panne par semaine ?

**Exercice 8 (B) : Une densité triangulaire.** La perte X (en millions de FCFA) d'une opération a pour densité f(x) = c(10 − x) sur \[0, 10\], et 0 ailleurs. (a) Trouvez c. (b) Calculez F(x) sur \[0, 10\]. (c) Calculez P(X > 5) et E\[X\]. (d) Calculez la VaR à 90 %. *Indication pour (d) : on obtient une équation du second degré ; gardez la racine située dans \[0, 10\].*

**Exercice 9 (B) : Une mutuelle de santé.** Dans une mutuelle, un adhérent n'a aucune dépense remboursable dans l'année avec probabilité 0,7. Sinon, le remboursement suit une loi uniforme sur \[0 ; 200 000\] FCFA. (a) Écrivez la fonction de répartition du remboursement annuel X. (b) Calculez P(X > 100 000) et E\[X\]. (c) Calculez les quantiles à 50 %, 80 % et 95 %. (d) Expliquez pourquoi la médiane ne sert à rien pour piloter ce risque.

**Exercice 10 (A) : L'effet de l'inflation sur la VaR.** Le coût X d'un sinistre suit une loi exponentielle de moyenne 500 000 FCFA. L'an prochain, chaque sinistre coûtera Y = 1,08X + 20 000 (8 % d'inflation et des frais fixes de gestion). Calculez VaR~0,99~(X), puis VaR~0,99~(Y) sans recalculer la loi de Y.

### Thème 3 : Espérance, variance, franchises

**Exercice 11 (A) : Un billet de loterie.** Un billet coûte 500 FCFA. Il rapporte 10 000 FCFA avec probabilité 1/100 et 1 000 000 FCFA avec probabilité 1/10 000. Soit G le gain net du joueur. (a) Calculez E\[G\]. (b) Calculez l'écart-type de G. (c) Pourquoi les gens jouent-ils malgré tout ? Quel est le lien avec l'assurance ?

**Exercice 12 (B) : La prime pure des motos-taxis.** Pour un contrat de moto-taxi, le nombre annuel de sinistres N vaut 0, 1 ou 2 avec les probabilités 0,80 ; 0,15 ; 0,05. Le coût de chaque sinistre suit une loi exponentielle de moyenne 300 000 FCFA, indépendamment de N. (a) Calculez la prime pure E\[S\]. (b) Calculez l'écart-type de la charge annuelle S. *Indication : formules de Wald (chapitre 11).*

**Exercice 13 (B) : Franchise et plafond.** Un contrat habitation prévoit une franchise de 50 000 FCFA et un plafond de remboursement de 1 000 000 FCFA par sinistre : l'assureur paie min((X − 50 000)₊ ; 1 000 000). Le coût X suit une loi exponentielle de moyenne 400 000. (a) Calculez la prime pure par sinistre. (b) Comparez avec un contrat sans plafond (franchise seule). *Indication : le paiement moyen est l'intégrale de S entre d et d + L.*

**Exercice 14 (A) : Borner une perte sans connaître sa loi.** La perte mensuelle d'une institution de microfinance a une moyenne de 2 millions et un écart-type de 0,5 million. Majorez la probabilité que la perte atteigne 4 millions avec Markov, puis avec Tchebychev.

**Exercice 15 (B) : Les gros sinistres incendie.** Les sinistres incendie d'une zone industrielle, au-delà de 1 million, suivent une Pareto de paramètres x~m~ = 1 et a = 3 (en millions). (a) Calculez E\[X\]. (b) Calculez P(X > 5). (c) Calculez E\[X | X > 5\] et l'excès moyen au-delà de 5 millions. Comparez avec ce que donnerait une exponentielle.

### Thème 4 : Lois usuelles

**Exercice 16 (A) : Une flotte de véhicules.** Une entreprise de transport possède 20 véhicules. Chacun a, indépendamment, une probabilité annuelle d'accident de 8 %. Calculez la probabilité d'aucun accident, d'au moins trois accidents, puis l'espérance et la variance du nombre d'accidents.

**Exercice 17 (B) : Les réclamations critiques d'un service client.** Un opérateur reçoit en moyenne 3 réclamations critiques par heure, selon une loi de Poisson. (a) Calculez la probabilité de n'en recevoir aucune en une heure. (b) Calculez la probabilité d'en recevoir au moins 6 en une heure. (c) Sur une journée de 8 heures, calculez la probabilité d'en recevoir au moins 30, exactement puis par approximation normale avec correction de continuité.

**Exercice 18 (A) : La durée de vie d'un crédit.** Chaque mois, indépendamment, un emprunteur fait défaut avec probabilité 2 %. (a) Combien de mois s'écoulent en moyenne avant le défaut ? (b) Quelle est la probabilité qu'un crédit sur 24 mois soit remboursé sans défaut ? Et un crédit sur 12 mois ?

**Exercice 19 (B) : Le rendement d'un fonds actions.** Le rendement annuel R d'un fonds investi en actions régionales est supposé normal, de moyenne 8 % et d'écart-type 15 %. (a) Calculez P(R < 0) et P(R < −20 %). (b) Calculez le quantile à 5 % de R. (c) Pour un portefeuille de 100 millions de FCFA, donnez la VaR à 95 % à un an, en FCFA.

**Exercice 20 (B) : Les frais d'hospitalisation.** Le coût X d'une hospitalisation (FCFA) suit une lognormale de paramètres μ = 13 et σ = 0,8. (a) Calculez la médiane et la moyenne. (b) Calculez P(X > 1 000 000). (c) Calculez le quantile à 95 %. *Indication : ln(1 000 000) ≈ 13,816.*

**Exercice 21 (C) : Les grands incendies industriels.** Les sinistres incendie de plus de 5 millions suivent une Pareto de paramètres x~m~ = 5 (millions) et a = 1,8. (a) Calculez la moyenne. La variance est-elle finie ? (b) Calculez P(X > 50). (c) Calculez la VaR et la TVaR à 99 %. (d) Expliquez pourquoi un assureur de taille moyenne a intérêt à se réassurer pour ce risque.

### Thème 5 : Couples, corrélation, portefeuilles

**Exercice 22 (B) : Deux emprunteuses du même village.** Une institution de microfinance prête à deux commerçantes du même marché. Chacune fait défaut avec probabilité 0,10, et elles font défaut toutes les deux avec probabilité 0,03. Soient D₁ et D₂ les indicatrices de défaut. (a) Les défauts sont-ils indépendants ? (b) Calculez Cov(D₁, D₂) et ρ. (c) Calculez la probabilité d'au moins un défaut et la variance du nombre de défauts, puis comparez avec le cas indépendant. (d) Donnez une raison économique à cette dépendance.

**Exercice 23 (B) : Un portefeuille de deux actions.** Un investisseur place 100 millions de FCFA, moitié dans l'action 1 (rendement moyen 10 %, écart-type 25 %) et moitié dans l'action 2 (rendement moyen 8 %, écart-type 18 %), avec une corrélation de 0,4. (a) Calculez le rendement moyen et l'écart-type du portefeuille. (b) En supposant les rendements normaux, calculez la VaR à 99 % à un an en FCFA. (c) Que vaudrait la VaR si l'on additionnait simplement les risques des deux lignes (corrélation 1) ?

**Exercice 24 (B) : Deux agences.** Les sinistres déclarés chaque mois à l'agence d'Abidjan suivent une Poisson de paramètre 12, ceux de l'agence de Bouaké une Poisson de paramètre 5, indépendamment. (a) Quelle est la loi du total ? Calculez la probabilité qu'il atteigne au moins 20. (b) Un mois, on a reçu 17 sinistres au total. Quelle est la probabilité que 12 viennent d'Abidjan ?

**Exercice 25 (C) : Pourquoi les sinistres sont surdispersés.** Un portefeuille compte 60 % de « bons » conducteurs (nombre annuel de sinistres de Poisson de paramètre 0,05) et 40 % de conducteurs « à risque » (Poisson de paramètre 0,20). Soit N le nombre de sinistres d'un assuré pris au hasard. (a) Calculez E\[N\]. (b) Calculez Var(N) avec la formule de la variance totale. (c) Comparez E\[N\] et Var(N) : une Poisson simple convient-elle ? Quelle loi proposer ?

### Thème 6 : Théorèmes limites et mutualisation

**Exercice 26 (C) : La prime d'une mutuelle de santé.** Une mutuelle compte 5 000 adhérents indépendants. Le coût annuel par adhérent a une moyenne de 60 000 FCFA et un écart-type de 150 000 FCFA. (a) Calculez l'espérance et l'écart-type de la charge totale. (b) Si la cotisation est la prime pure majorée de 5 %, quelle est la probabilité que la charge dépasse les cotisations ? (c) Quelle cotisation par adhérent faut-il pour ramener cette probabilité à 1 % ? (d) Citez deux raisons pour lesquelles ce calcul peut être trop optimiste en santé.

**Exercice 27 (B) : Les défauts d'un portefeuille de microcrédits.** Une institution accorde 2 000 microcrédits indépendants, chacun avec une probabilité de défaut de 3 %. (a) Donnez l'espérance et l'écart-type du nombre de défauts. (b) Approchez la probabilité d'avoir plus de 75 défauts par la loi normale, avec correction de continuité. (c) Comparez à la valeur exacte calculée en Python.

**Exercice 28 (B) : Combien d'assurés observer ?** Un assureur veut estimer la fréquence annuelle de sinistres d'un nouveau produit, qu'il pense proche de 5 %. Combien d'assurés doit-il observer pendant un an pour connaître cette fréquence à 10 % près en valeur relative (soit ± 0,5 point), avec 95 % de confiance ? *Indication : la proportion observée a pour écart-type √(p(1 − p)/n).*

### Thème 7 : VaR, TVaR, modèle collectif

**Exercice 29 (A) : VaR et TVaR d'un sinistre exponentiel.** Le coût d'un sinistre suit une loi exponentielle de moyenne 2 millions. Calculez la VaR et la TVaR à 95 %, 99 % et 99,5 %.

**Exercice 30 (B) : Le cyber-risque d'une PME.** La perte annuelle due aux cyberattaques d'une PME (millions de FCFA) vaut 0, 5, 20 ou 100 avec les probabilités 0,85 ; 0,10 ; 0,04 ; 0,01. (a) Calculez la VaR à 90 %, 95 % et 99 %. (b) Calculez la TVaR et la CTE à 90 %, puis à 95 %. (c) Expliquez pourquoi TVaR et CTE diffèrent à 90 % mais pas à 95 %.

**Exercice 31 (B) : La VaR d'un desk de trading.** La perte quotidienne d'un desk est normale, de moyenne nulle et d'écart-type 50 millions de FCFA. (a) Calculez la VaR à 99 % à un jour. (b) Calculez la VaR à 99 % à 10 jours en supposant les jours indépendants et de même loi. (c) Calculez l'Expected Shortfall à 97,5 % à un jour et comparez-la à la VaR à 99 %.

**Exercice 32 (C) : Trois prêts et la diversification.** Une banque détient trois prêts indépendants de 10 millions, chacun avec une probabilité de défaut de 2 % (perte totale en cas de défaut). (a) Calculez la VaR et la TVaR à 95 % d'un prêt. (b) Donnez la loi de la perte totale, puis sa VaR et sa TVaR à 95 %. (c) Concluez sur la sous-additivité des deux mesures.

**Exercice 33 (C) : La flotte de taxis.** Un assureur couvre une flotte de taxis. Le nombre annuel de sinistres suit une Poisson de paramètre 30, et le coût d'un sinistre une loi Gamma de forme 2 et d'échelle 250 000 FCFA. (a) Calculez E\[X\], E\[X²\], E\[S\] et σ~S~. (b) Donnez la VaR à 99,5 % par approximation normale. (c) Estimez la VaR et la TVaR à 99,5 % par simulation. (d) Quel capital faut-il au-delà de la prime pure ?

**Exercice 34 (B) : Un traité de réassurance en excédent de sinistre.** Les sinistres d'un portefeuille industriel suivent une Pareto de paramètres x~m~ = 2 et a = 2,5 (millions). Un réassureur prend en charge, pour chaque sinistre, la part au-delà de 10 millions (la priorité). (a) Calculez la probabilité qu'un sinistre touche le réassureur. (b) Calculez la charge moyenne du réassureur par sinistre. (c) Avec 40 sinistres par an en moyenne, quelle est la prime pure annuelle du traité ?

**Exercice 35 (B) : Tarifer avec le coût du capital.** La charge annuelle d'un portefeuille suit une loi exponentielle de moyenne 10 millions. L'assureur doit détenir un capital égal à TVaR~0,99~ − E\[S\], et ses actionnaires exigent une rémunération de 10 % par an sur ce capital. (a) Calculez le capital. (b) Calculez la prime commerciale hors frais : prime pure plus coût du capital.

### Thème 8 : Simulation, estimation, backtesting

**Exercice 36 (A) : La précision d'une simulation.** Sur 50 000 scénarios simulés, 600 conduisent à la ruine. (a) Estimez la probabilité de ruine et donnez un intervalle de confiance à 95 %. (b) Combien de scénarios faudrait-il pour que la demi-largeur de l'intervalle soit de 0,0005 ?

**Exercice 37 (B) : Ajuster une exponentielle.** Voici 8 sinistres observés (milliers de FCFA) : 120, 340, 85, 610, 230, 150, 95, 470. (a) Estimez la moyenne θ d'une exponentielle par maximum de vraisemblance. (b) Calculez la VaR et la TVaR à 95 % du modèle. (c) Pourquoi ne peut-on pas estimer sérieusement la VaR à 95 % directement sur ces 8 données ?

**Exercice 38 (A) : Une lognormale ajustée.** Sur un historique de frais médicaux, les logarithmes des coûts ont une moyenne de 12,5 et un écart-type de 0,9. Donnez les paramètres de la lognormale ajustée, la médiane, la moyenne et la VaR à 99 % d'un dossier.

**Exercice 39 (B) : Deux ans de backtesting.** Une VaR à 99 % a été dépassée 9 fois sur 500 jours. (a) Combien d'exceptions attendait-on ? (b) Calculez P(N ≥ 9) si le modèle est correct. (c) Faites le test de Kupiec au seuil de 5 %. Concluez.

**Exercice 40 (C) : Un modèle interne à revoir ?** Une banque backteste sa VaR à 95 % sur 250 jours et observe 20 exceptions. (a) Combien en attendait-on ? (b) Calculez P(N ≥ 20) sous un modèle correct. (c) Faites le test de Kupiec. (d) Rédigez trois recommandations pour l'équipe de modélisation.

## Corrigés détaillés de la banque d'exercices

### Exercice 1 : Détecter la fraude au mobile money

**Ce qu'on cherche.** On connaît la qualité du système dans le sens « fraude → alerte ». On veut l'information utile au contrôleur, dans le sens inverse : « alerte → fraude ». C'est une situation de Bayes.

**Méthode.** On nomme F = « fraude » et A = « alerte ». Données : P(F) = 0,002 ; P(A | F) = 0,98 ; P(A | F̄) = 0,01. On calcule P(A) par les probabilités totales, puis P(F | A) par Bayes.

**Calcul.**

$$
P(A) = 0{,}002 \times 0{,}98 + 0{,}998 \times 0{,}01 = 0{,}00196 + 0{,}00998 = 0{,}01194
$$

$$
P(F \mid A) = \frac{0{,}00196}{0{,}01194} \approx 0{,}164
$$

(c) Sur 1 000 000 de transactions : environ 11 940 alertes, dont 1 960 vraies fraudes, soit environ 10 000 fausses alertes par jour. (d) On veut 0,00196/(0,00196 + 0,998 f) ≥ 0,5, soit 0,998 f ≤ 0,00196, donc f ≤ 0,00196 ≈ **0,2 %** : il faut diviser le taux de fausses alertes par 5.

**Interprétation.** Même avec un système qui détecte 98 % des fraudes, seulement 16 % des alertes sont de vraies fraudes. Le problème vient de la rareté de la fraude : les 1 % d'erreurs sur une très grande masse de transactions légitimes noient les vraies fraudes. L'équipe de contrôle doit dimensionner ses moyens pour 12 000 alertes par jour, pas pour 2 000.

**Piège.** Confondre P(A | F) = 98 % avec P(F | A). C'est exactement l'erreur du test médical (question B4 du diagnostic).

### Exercice 2 : Le contrôle d'un comité de crédit

**Ce qu'on cherche.** La loi du nombre de dossiers à risque tirés, dans un tirage **sans remise** : c'est une loi hypergéométrique.

**Méthode.** Nombre de tirages possibles : C(12,4) = 495. On choisit k dossiers parmi les 3 à risque et 4 − k parmi les 9 sains.

**Calcul.**

| Résultat | Calcul | Probabilité |
| --- | --- | --- |
| Aucun dossier à risque | C(9,4)/495 = 126/495 | ≈ 0,255 |
| Exactement un | C(3,1) C(9,3)/495 = 3 × 84/495 = 252/495 | ≈ 0,509 |
| Au moins deux | 1 − 0,255 − 0,509 | ≈ 0,236 |

**Interprétation.** Dans un cas sur quatre, l'auditeur ne voit aucun dossier à risque : un contrôle sur 4 dossiers seulement peut donner une fausse impression de sécurité. Pour être plus sûr, il faut augmenter l'échantillon ou cibler les dossiers selon des critères de risque.

**Piège.** Utiliser une binomiale (tirage avec remise) : avec seulement 12 dossiers, retirer un dossier change nettement la proportion restante.

### Exercice 3 : L'assurance récolte d'un planteur de cacao

**Ce qu'on cherche.** (a) la probabilité d'une réunion d'événements indépendants ; (b) une probabilité conditionnelle ; (c) une probabilité binomiale.

**Méthode.** Pour « au moins un », on passe par le contraire : pas de sécheresse **et** pas de maladie.

**Calcul.** (a) P(perte) = 1 − 0,85 × 0,90 = 1 − 0,765 = **0,235**. (b) Sécheresse implique perte, donc P(S ∩ perte) = P(S) = 0,15 et P(S | perte) = 0,15/0,235 ≈ **0,638**. (c) Le nombre K d'années de perte sur 5 ans suit une binomiale (5 ; 0,235) :

$$
P(K \ge 2) = 1 - 0{,}765^5 - 5 \times 0{,}235 \times 0{,}765^4 \approx 1 - 0{,}262 - 0{,}402 \approx 0{,}336
$$

**Interprétation.** Un planteur a une chance sur trois de perdre au moins deux récoltes en cinq ans. C'est l'argument commercial de l'assurance récolte. Pour l'assureur, si les sécheresses touchent toute une région en même temps, les sinistres des planteurs ne sont plus indépendants entre eux : c'est un risque systémique à réassurer.

**Piège.** Additionner 0,15 + 0,10 = 0,25 : on compterait deux fois les années où les deux événements surviennent (0,015).

### Exercice 4 : Le serveur de secours d'une banque

**Ce qu'on cherche.** La probabilité que les deux serveurs tombent en panne le même jour, avec puis sans indépendance.

**Calcul.** (a) Indépendance : 0,02 × 0,05 = **0,001**, soit un jour d'arrêt tous les 1 000 jours. (b) Probabilités composées : P(P ∩ S) = P(P) × P(S | P) = 0,02 × 0,30 = **0,006**, soit six fois plus.

**Interprétation.** La dépendance, ici une cause commune (l'alimentation électrique), multiplie le risque d'arrêt par six. Le secours ne protège vraiment que s'il est indépendant du système principal : alimentation séparée, site distant. C'est un principe central du risque opérationnel et des plans de continuité.

**Piège.** Multiplier des probabilités sans vérifier l'indépendance.

### Exercice 5 : La politique d'octroi d'une banque

**Méthode.** Probabilités totales pour (a), Bayes pour (b), probabilité conditionnelle à « accepté » pour (c).

**Calcul.** (a) 0,50 × 0,01 + 0,35 × 0,04 + 0,15 × 0,12 = 0,005 + 0,014 + 0,018 = **0,037**. (b) P(C | défaut) = 0,018/0,037 ≈ **0,486**. (c) Parmi les acceptés (A et B, soit 85 % des clients) :

$$
P(\text{défaut} \mid A \cup B) = \frac{0{,}005 + 0{,}014}{0{,}85} \approx 0{,}0224
$$

**Interprétation.** La catégorie C représente 15 % des clients mais presque la moitié des défauts. La refuser fait baisser le taux de défaut de 3,7 % à 2,2 %. (d) Pour : moins de pertes et moins de capital à détenir. Contre : perte de 15 % du chiffre d'affaires, alors que ces clients paient des taux plus élevés ; et, en microfinance, exclusion de clients qui ont peut-être le plus besoin de crédit. Une alternative est de garder la catégorie C avec un taux d'intérêt qui couvre son risque, ou des garanties.

**Piège.** En (c), diviser par 1 au lieu de 0,85 : on raisonne désormais sur la population des clients acceptés.

### Exercice 6 : Les retards de paiement

**Méthode.** Exponentielle de moyenne 20 : S(x) = e^−x/20^ et q~α~ = −20 ln(1 − α).

**Calcul.** (a) P(X > 30) = e^−1,5^ ≈ **0,223**. (b) Médiane = 20 ln 2 ≈ **13,9 jours**. (c) q~0,95~ = 20 ln 20 ≈ **59,9 jours**. (d) Par absence de mémoire, P(X > 40 | X > 10) = P(X > 30) ≈ **0,223**.

**Interprétation.** La moitié des retards durent moins de 14 jours, mais 5 % dépassent deux mois. Pour (d) : avec ce modèle, une facture déjà en retard n'est ni plus ni moins susceptible d'être réglée bientôt. Dans la réalité, un long retard signale souvent un client en difficulté : si les données montrent que la probabilité de paiement baisse avec le retard, l'exponentielle n'est pas le bon modèle.

**Piège.** Prendre la moyenne (20 jours) pour la médiane.

### Exercice 7 : Les pannes de distributeurs automatiques

**Calcul.** (a) F(0) = 0,40 ; F(1) = 0,70 ; F(2) = 0,85 ; F(3) = 0,95 ; F(4) = 1. (b) E\[N\] = 0,30 + 0,30 + 0,30 + 0,20 = **1,1** ; E\[N²\] = 0,30 + 0,60 + 0,90 + 0,80 = 2,6 ; Var(N) = 2,6 − 1,21 = **1,39**. (c) Première valeur où F ≥ α :

| α | Raisonnement | Quantile |
| --- | --- | --- |
| 0,5 | F(0) = 0,40 < 0,5 ; F(1) = 0,70 | **1** |
| 0,8 | F(1) = 0,70 < 0,8 ; F(2) = 0,85 | **2** |
| 0,9 | F(2) = 0,85 < 0,9 ; F(3) = 0,95 | **3** |
| 0,95 | F(3) = 0,95 ≥ 0,95 (égalité) | **3** |

(d) Il faut k techniciens tels que P(N ≤ k) ≥ 0,9 : c'est le quantile à 90 %, soit **3 techniciens**.

**Interprétation.** La question (d) montre qu'un quantile est un outil de dimensionnement : on choisit une capacité suffisante dans 90 % des cas. C'est exactement le raisonnement de la VaR, appliqué à des moyens humains au lieu d'un capital.

**Piège.** Répondre 1,1 technicien en utilisant la moyenne : une semaine sur deux, il y aurait plus de pannes que de techniciens.

### Exercice 8 : Une densité triangulaire

**Calcul.** (a) L'intégrale de c(10 − x) sur \[0, 10\] vaut c × 50 = 1, donc **c = 1/50**. (b) Pour x dans \[0, 10\] :

$$
F(x) = \frac{1}{50}\int_0^x (10 - t)\,dt = \frac{20x - x^2}{100}
$$

(c) P(X > 5) = 1 − (100 − 25)/100 = **0,25** ; E\[X\] = (1/50) × intégrale de x(10 − x) = (1/50)(500 − 1000/3) = **10/3 ≈ 3,33** millions. (d) On résout (20x − x²)/100 = 0,9, soit x² − 20x + 90 = 0. Les racines sont 10 ± √10 ; seule 10 − √10 est dans \[0, 10\] :

$$
\mathrm{VaR}_{0{,}9} = 10 - \sqrt{10} \approx 6{,}84 \text{ millions}
$$

**Piège.** Garder la racine 10 + √10 ≈ 13,2, qui sort du domaine de X : une VaR doit toujours être une valeur possible de la perte.

### Exercice 9 : Une mutuelle de santé

**Méthode.** C'est une loi mixte : une masse de 0,7 en 0, puis une partie uniforme qui porte la probabilité restante 0,3.

**Calcul.** (a) F(x) = 0 si x < 0 ; F(x) = 0,7 + 0,3x/200 000 si 0 ≤ x ≤ 200 000 ; F(x) = 1 au-delà. (b) P(X > 100 000) = 0,3 × 0,5 = **0,15** ; E\[X\] = 0,3 × 100 000 = **30 000 FCFA**. (c) q~0,5~ = **0** car F(0) = 0,7 ≥ 0,5 ; q~0,8~ : 0,7 + 0,3x/200 000 = 0,8 donne x ≈ **66 667** ; q~0,95~ donne x ≈ **166 667**.

**Interprétation.** (d) La médiane vaut 0 : elle dit seulement que la majorité des adhérents ne coûtent rien. La mutuelle doit piloter la moyenne (pour fixer la cotisation, 30 000 FCFA au minimum) et les quantiles élevés (pour ses réserves).

**Piège.** Oublier la masse en 0 et résoudre directement x/200 000 = α.

### Exercice 10 : L'effet de l'inflation sur la VaR

**Méthode.** La fonction x ↦ 1,08x + 20 000 est croissante, donc le quantile de Y est cette fonction appliquée au quantile de X (chapitre 3, section 3.7).

**Calcul.** VaR~0,99~(X) = 500 000 × ln 100 ≈ **2 302 585 FCFA**. VaR~0,99~(Y) = 1,08 × 2 302 585 + 20 000 ≈ **2 506 792 FCFA**.

**Interprétation.** Le capital nécessaire augmente de 8,9 % : l'inflation des coûts se répercute directement sur la VaR. Un assureur qui oublie l'inflation dans ses projections sous-estime son capital.

**Piège.** Ajouter seulement 8 % de la moyenne au lieu de transformer le quantile lui-même.

### Exercice 11 : Un billet de loterie

**Méthode.** On liste les valeurs du gain net G (gain moins prix du billet) avec leurs probabilités, puis on applique les définitions.

| G (FCFA) | −500 | 9 500 | 999 500 |
| --- | --- | --- | --- |
| Probabilité | 0,9899 | 0,01 | 0,0001 |

**Calcul.** (a) Plus simple : E\[gain brut\] = 10 000 × 0,01 + 1 000 000 × 0,0001 = 100 + 100 = 200, donc E\[G\] = 200 − 500 = **−300 FCFA**. (b) E\[G²\] ≈ 0,9899 × 500² + 0,01 × 9 500² + 0,0001 × 999 500², et l'écart-type vaut environ **10 048 FCFA**.

**Interprétation.** (c) En moyenne, le joueur perd 300 FCFA par billet, soit 60 % de sa mise. Il accepte cette perte moyenne pour une faible chance de gros gain : il **achète du risque**. L'assuré fait exactement l'inverse : il paie une prime supérieure à sa perte moyenne pour **se débarrasser** d'un risque. Dans les deux cas, l'organisateur (loterie ou assureur) gagne en moyenne grâce à la loi des grands nombres.

**Piège.** Oublier de retirer le prix du billet dans chaque issue.

### Exercice 12 : La prime pure des motos-taxis

**Méthode.** Formules de Wald : E\[S\] = E\[N\] E\[X\] et Var(S) = E\[N\] Var(X) + Var(N) E\[X\]².

**Calcul.** E\[N\] = 0,15 + 2 × 0,05 = 0,25 ; E\[N²\] = 0,15 + 4 × 0,05 = 0,35 ; Var(N) = 0,35 − 0,0625 = 0,2875. Pour X : E\[X\] = 300 000 et Var(X) = 300 000² = 9 × 10¹⁰.

$$
E[S] = 0{,}25 \times 300\,000 = 75\,000 \qquad \mathrm{Var}(S) = (0{,}25 + 0{,}2875) \times 9 \times 10^{10} = 4{,}84 \times 10^{10}
$$

Donc σ~S~ ≈ **219 943 FCFA**.

**Interprétation.** La prime pure est de 75 000 FCFA, mais l'écart-type de la charge d'un contrat est presque trois fois plus grand : un contrat isolé est très risqué. C'est seulement en regroupant des milliers de motos-taxis que l'assureur rend sa charge prévisible (chapitre 7).

**Piège.** Oublier le terme Var(N) E\[X\]², qui représente ici plus de la moitié de la variance.

### Exercice 13 : Franchise et plafond

**Méthode.** Le paiement vaut 0 si X ≤ d, X − d si d < X ≤ d + L, et L au-delà. Son espérance est l'intégrale de S entre d et d + L (même raisonnement qu'au chapitre 4).

**Calcul.** Avec θ = 400 000, d = 50 000 et L = 1 000 000 :

$$
E[\text{paiement}] = \int_{d}^{d+L} e^{-x/\theta}\,dx = \theta\big(e^{-d/\theta} - e^{-(d+L)/\theta}\big) = 400\,000\,\big(e^{-0{,}125} - e^{-2{,}625}\big) \approx 324\,023
$$

Sans plafond : 400 000 × e^−0,125^ ≈ **352 999**. Le plafond réduit la prime pure d'environ 29 000 FCFA (8 %).

**Interprétation.** La franchise supprime les petits sinistres (coûteux à gérer) et le plafond supprime les très gros, ceux qui pèsent sur la VaR. Ce sont les deux leviers classiques pour adapter un contrat au budget du client et à l'appétit au risque de l'assureur.

**Piège.** Calculer (E\[X\] − d) : la franchise ne réduit pas chaque sinistre de d, car les sinistres inférieurs à d ne coûtent pas « moins que zéro ».

### Exercice 14 : Borner une perte sans connaître sa loi

**Calcul.** Markov : P(X ≥ 4) ≤ 2/4 = **0,5**. Tchebychev : X ≥ 4 implique |X − 2| ≥ 2 = 4σ, donc P ≤ 1/4² = **0,0625**.

**Interprétation.** Sans aucune hypothèse de loi, on peut garantir que doubler la perte moyenne arrive au plus 6,25 % des mois. C'est utile pour un premier ordre de grandeur, ou quand les données sont trop rares pour choisir une loi.

**Piège.** Appliquer Markov à une variable qui peut être négative.

### Exercice 15 : Les gros sinistres incendie

**Calcul.** (a) E\[X\] = 3 × 1/2 = **1,5 million**. (b) P(X > 5) = (1/5)³ = **0,008**. (c) Pour une Pareto, E\[X | X > u\] = a u/(a − 1) = 3 × 5/2 = **7,5 millions**, et l'excès moyen vaut e(5) = 7,5 − 5 = **2,5 millions**.

**Interprétation.** Au-delà de 5 millions, un sinistre dépasse le seuil de 2,5 millions en moyenne, et cet excès grandit avec le seuil (e(u) = u/2). Pour une exponentielle de même moyenne, l'excès moyen resterait constant, égal à 1,5 million, quel que soit le seuil. C'est la signature d'une queue lourde : plus un sinistre est déjà gros, plus il risque d'être très gros.

**Piège.** Utiliser l'absence de mémoire, qui n'est vraie que pour l'exponentielle.

### Exercice 16 : Une flotte de véhicules

**Méthode.** 20 véhicules indépendants, même probabilité : N suit une binomiale (20 ; 0,08).

**Calcul.** P(N = 0) = 0,92²⁰ ≈ **0,189** ; P(N ≥ 3) = 1 − P(N ≤ 2) ≈ **0,212** ; E\[N\] = **1,6** ; Var(N) = 20 × 0,08 × 0,92 = **1,472**.

**Interprétation.** Une année sur cinq environ, la flotte n'a aucun accident, et une année sur cinq elle en a au moins trois. Une petite flotte a donc des résultats très variables d'une année à l'autre : il ne faut pas juger la qualité des chauffeurs sur une seule année.

**Piège.** Calculer P(N ≥ 3) comme 1 − P(N ≤ 3).

### Exercice 17 : Les réclamations critiques d'un service client

**Calcul.** (a) P(N = 0) = e^−3^ ≈ **0,050**. (b) P(N ≥ 6) = 1 − P(N ≤ 5) ≈ **0,084**. (c) Sur 8 heures, le total suit une Poisson de paramètre 24 (somme de Poisson indépendantes). Exact : P(N ≥ 30) ≈ **0,132**. Approximation normale avec correction : 1 − Φ((29,5 − 24)/√24) = 1 − Φ(1,123) ≈ **0,131**.

**Interprétation.** Une heure sur douze environ, le service reçoit au moins 6 réclamations critiques, soit le double de la moyenne. Le dimensionnement des équipes doit se faire sur un quantile, pas sur la moyenne de 3 par heure.

**Piège.** Oublier de multiplier λ par 8 pour passer à la journée.

### Exercice 18 : La durée de vie d'un crédit

**Méthode.** Le rang du mois de défaut suit une loi géométrique de paramètre 0,02.

**Calcul.** (a) E = 1/0,02 = **50 mois**. (b) Survivre 24 mois, c'est 24 mois sans défaut : 0,98²⁴ ≈ **0,616**. Sur 12 mois : 0,98¹² ≈ **0,785**.

**Interprétation.** Un taux de défaut de 2 % par mois paraît faible, mais il conduit à 38 % de défauts sur un crédit de deux ans. Il faut toujours ramener un taux mensuel à la durée du crédit avant de juger un risque.

**Piège.** Calculer 24 × 2 % = 48 % : on additionnerait des probabilités d'événements qui ne sont pas incompatibles.

### Exercice 19 : Le rendement d'un fonds actions

**Calcul.** (a) P(R < 0) = Φ((0 − 8)/15) = Φ(−0,533) ≈ **0,297** ; P(R < −20 %) = Φ(−1,867) ≈ **0,031**. (b) q~0,05~(R) = 8 % − 1,6449 × 15 % ≈ **−16,7 %**. (c) La perte est X = −R × 100 millions ; sa VaR à 95 % vaut 16,7 % × 100 millions ≈ **16,7 millions de FCFA**.

**Interprétation.** Même avec un rendement moyen de 8 %, le fonds perd de l'argent près d'une année sur trois, et perd plus de 16,7 % une année sur vingt. Un épargnant doit connaître ces chiffres avant d'investir.

**Piège.** Lire la VaR dans la queue droite du rendement : pour une perte, c'est la queue **gauche** de R qui compte.

### Exercice 20 : Les frais d'hospitalisation

**Calcul.** (a) Médiane = e¹³ ≈ **442 413 FCFA** ; moyenne = e^13\ +\ 0,32^ ≈ **609 260 FCFA**. (b) P(X > 1 000 000) = 1 − Φ((13,816 − 13)/0,8) = 1 − Φ(1,020) ≈ **0,154**. (c) q~0,95~ = e^13\ +\ 0,8\ ×\ 1,6449^ ≈ **1 649 335 FCFA**.

**Interprétation.** La moyenne dépasse la médiane de 38 % : quelques hospitalisations très coûteuses tirent la moyenne. Une mutuelle qui budgéterait sur la médiane serait déficitaire.

**Piège.** Prendre e^μ^ pour la moyenne.

### Exercice 21 : Les grands incendies industriels

**Calcul.** (a) E\[X\] = 1,8 × 5/0,8 = **11,25 millions**. La variance est **infinie**, car a = 1,8 ≤ 2. (b) P(X > 50) = (5/50)^1^,8 = 0,1^1^,8 ≈ **0,0158**. (c) VaR~0,99~ = 5 × 0,01^−1/1,8^ ≈ **64,6 millions** ; TVaR~0,99~ = (1,8/0,8) × 64,6 ≈ **145,3 millions**.

**Interprétation.** (d) Un sinistre sur 63 dépasse 50 millions, et dans le pire 1 % des cas le coût moyen dépasse 145 millions, soit 13 fois la moyenne. Avec une variance infinie, la mutualisation fonctionne très mal (chapitre 7) : quelques sinistres peuvent déséquilibrer les comptes d'un assureur moyen. La réassurance en excédent de sinistre transfère ces sinistres extrêmes à un réassureur, qui peut les mutualiser à l'échelle mondiale.

**Piège.** Calculer un écart-type avec la formule de la variance de Pareto alors que a ≤ 2 : on obtiendrait une valeur négative ou absurde.

### Exercice 22 : Deux emprunteuses du même village

**Méthode.** Pour des indicatrices, E\[D\] = P(D = 1), Var(D) = p(1 − p) et E\[D₁D₂\] = P(D₁ = 1, D₂ = 1).

**Calcul.** (a) Si les défauts étaient indépendants, on aurait P(deux défauts) = 0,1 × 0,1 = 0,01. On observe 0,03 : ils sont **dépendants**. (b) Cov = 0,03 − 0,01 = **0,02** ; Var(Dᵢ) = 0,1 × 0,9 = 0,09 ; ρ = 0,02/0,09 ≈ **0,22**. (c) P(au moins un) = 0,1 + 0,1 − 0,03 = **0,17** (contre 0,19 si indépendants) ; Var(D₁ + D₂) = 0,09 + 0,09 + 2 × 0,02 = **0,22** (contre 0,18).

**Interprétation.** La dépendance a deux effets opposés : la probabilité d'au moins un défaut baisse un peu, mais la probabilité du scénario grave (les deux défauts) triple, et la variance augmente de 22 %. Pour un gestionnaire de risques, c'est le scénario grave qui compte. (d) Les deux commerçantes subissent les mêmes chocs : baisse de l'activité du marché, inondation, mauvaise récolte dans la région. En microfinance, la caution solidaire peut aussi lier leurs remboursements.

**Piège.** Conclure qu'une corrélation de 0,22 est « faible » donc négligeable : elle triple ici la probabilité du pire cas.

### Exercice 23 : Un portefeuille de deux actions

**Calcul.** (a) Rendement moyen : 0,5 × 10 % + 0,5 × 8 % = **9 %**. Variance :

$$
\sigma_p^2 = 0{,}25 \times 0{,}25^2 + 0{,}25 \times 0{,}18^2 + 2 \times 0{,}25 \times 0{,}4 \times 0{,}25 \times 0{,}18 = 0{,}032725 \qquad \sigma_p \approx 18{,}09\,\%
$$

(b) La perte en pourcentage est −R, de moyenne −9 %. VaR~0,99~ = −9 % + 2,3263 × 18,09 % ≈ 33,08 %, soit **33,1 millions de FCFA**. (c) Avec ρ = 1, σ~p~ = 0,5 × 25 % + 0,5 × 18 % = 21,5 % et VaR ≈ −9 % + 2,3263 × 21,5 % ≈ **41,0 millions**.

**Interprétation.** La diversification économise environ 7,9 millions de capital. Mais attention : en période de crise, les corrélations entre actions augmentent souvent et ce gain fond précisément au moment où on en a besoin.

**Piège.** Oublier le rendement moyen dans la VaR. Sur un an, 9 % de rendement moyen réduit sensiblement la perte potentielle.

### Exercice 24 : Deux agences

**Calcul.** (a) La somme de Poisson indépendantes est une Poisson de paramètre 12 + 5 = 17. P(N ≥ 20) = 1 − P(N ≤ 19) ≈ **0,264**. (b) Sachant N = 17, le nombre venant d'Abidjan suit une binomiale (17 ; 12/17) (exercice 6.5). P = C(17,12) (12/17)¹² (5/17)⁵ ≈ **0,208**.

**Interprétation.** Le résultat (b) est très utile en gestion : connaissant le total, on répartit les sinistres entre agences proportionnellement à leurs fréquences. Si une agence dépasse nettement sa part attendue plusieurs mois de suite, il faut chercher une cause locale (fraude, mauvaise sélection des risques).

**Piège.** Croire que le total est « 17 en moyenne, donc 12 Abidjan à coup sûr » : la répartition reste aléatoire.

### Exercice 25 : Pourquoi les sinistres sont surdispersés

**Méthode.** Variance totale : Var(N) = E\[Var(N | classe)\] + Var(E\[N | classe\]). Pour une Poisson, Var(N | classe) = E\[N | classe\] = λ.

**Calcul.** (a) E\[N\] = 0,6 × 0,05 + 0,4 × 0,20 = **0,11**. (b) E\[Var(N | classe)\] = 0,11 ; Var(E\[N | classe\]) = 0,6 × 0,05² + 0,4 × 0,20² − 0,11² = 0,0175 − 0,0121 = 0,0054. Donc Var(N) = **0,1154**.

**Interprétation.** (c) Var(N) > E\[N\] : le portefeuille est surdispersé, alors que chaque classe est une Poisson parfaite. C'est l'hétérogénéité des assurés qui crée la surdispersion. On modélise alors avec une binomiale négative, ou mieux, on segmente le tarif par classe de risque.

**Piège.** Calculer la variance comme 0,6 × 0,05 + 0,4 × 0,20 en oubliant la variance entre les classes.

### Exercice 26 : La prime d'une mutuelle de santé

**Calcul.** (a) E\[S\] = 5 000 × 60 000 = **300 millions** ; σ~S~ = 150 000 × √5 000 ≈ **10,6 millions**. (b) Cotisations = 315 millions. Par le TCL :

$$
P(S > 315 \text{ M}) \approx 1 - \Phi\Big(\frac{15}{10{,}61}\Big) = 1 - \Phi(1{,}414) \approx 0{,}079
$$

(c) Chargement total : 2,3263 × 10,61 millions ≈ 24,7 millions, soit environ **4 935 FCFA par adhérent** : la cotisation doit être d'environ **64 935 FCFA** (+8,2 %).

**Interprétation.** Avec 5 % de marge, la mutuelle serait déficitaire environ une année sur douze. (d) En santé, le calcul peut être trop optimiste : une épidémie touche beaucoup d'adhérents en même temps (dépendance) ; quelques hospitalisations très coûteuses rendent la charge asymétrique ; les personnes malades ont tendance à adhérer davantage (anti-sélection) ; et les coûts médicaux augmentent d'une année à l'autre.

**Piège.** Multiplier l'écart-type individuel par 5 000 au lieu de √5 000.

### Exercice 27 : Les défauts d'un portefeuille de microcrédits

**Calcul.** (a) E\[N\] = 2 000 × 0,03 = **60** ; σ = √(2 000 × 0,03 × 0,97) ≈ **7,63**. (b) « Plus de 75 » signifie N ≥ 76. Avec correction de continuité : 1 − Φ((75,5 − 60)/7,63) = 1 − Φ(2,03) ≈ **0,021**. (c) Valeur exacte (`stats.binom(2000, 0.03).sf(75)`) : **0,024**.

**Interprétation.** L'approximation sous-estime un peu la probabilité, car la binomiale avec p petit est légèrement asymétrique à droite. Pour une décision de capital dans la queue, on préfère le calcul exact, immédiat en Python.

**Piège.** Traduire « plus de 75 » par N ≥ 75.

### Exercice 28 : Combien d'assurés observer ?

**Méthode.** On veut 1,96 √(p(1 − p)/n) ≤ 0,005, donc n ≥ 1,96² p(1 − p)/0,005².

**Calcul.** n ≥ 3,8416 × 0,05 × 0,95/0,000025 ≈ **7 300 assurés**.

**Interprétation.** Pour un événement rare, même une précision modeste demande beaucoup d'observations. Un petit assureur qui lance un produit avec 1 000 contrats connaîtra sa fréquence de sinistres à ± 1,35 point seulement (entre 3,65 % et 6,35 %). Il doit compléter par des données de marché ou une marge de prudence.

**Piège.** Viser une précision absolue de 10 points (0,1) au lieu de 10 % de la fréquence.

### Exercice 29 : VaR et TVaR d'un sinistre exponentiel

**Méthode.** VaR~α~ = θ ln(1/(1 − α)) et TVaR~α~ = VaR~α~ + θ, avec θ = 2 millions.

| α | VaR (millions) | TVaR (millions) |
| --- | --- | --- |
| 95 % | 2 ln 20 ≈ 5,99 | 7,99 |
| 99 % | 2 ln 100 ≈ 9,21 | 11,21 |
| 99,5 % | 2 ln 200 ≈ 10,60 | 12,60 |

**Interprétation.** Pour l'exponentielle, l'écart TVaR − VaR est toujours égal à la moyenne (2 millions), quel que soit le niveau : c'est l'absence de mémoire. Passer de 99 % à 99,5 % ajoute seulement 1,39 million : la queue est fine.

**Piège.** Utiliser le paramètre λ = 0,5 à la place de la moyenne θ = 2 dans la formule.

### Exercice 30 : Le cyber-risque d'une PME

**Calcul.** F(0) = 0,85 ; F(5) = 0,95 ; F(20) = 0,99 ; F(100) = 1. (a) VaR~0,90~ = **5** ; VaR~0,95~ = **5** (égalité) ; VaR~0,99~ = **20** millions. (b) Avec la formule 3 :

| Niveau | TVaR | CTE = E\[X ∣ X > VaR\] |
| --- | --- | --- |
| 90 % | (20 × 0,04 + 100 × 0,01 + 5 × (0,95 − 0,90))/0,10 = **20,5** | (0,8 + 1)/0,05 = **36** |
| 95 % | (0,8 + 1 + 5 × 0)/0,05 = **36** | **36** |

**Interprétation.** (c) À 90 %, les 10 % pires cas comprennent 5 % de cas à 5 millions (une partie de la marche en 5) en plus des cas à 20 et 100 millions. La TVaR les compte, la CTE les oublie et surestime. À 95 %, F(5) = 0,95 exactement : la marche en 5 est entièrement sous le niveau, et les deux mesures coïncident. Pour la PME, le message est clair : une année sur cent, une cyberattaque peut coûter 100 millions, ce qui justifie une assurance cyber ou un plan de prévention.

**Piège.** Utiliser la CTE à la place de la TVaR pour une loi discrète.

### Exercice 31 : La VaR d'un desk de trading

**Calcul.** (a) VaR~0,99~ = 2,3263 × 50 ≈ **116,3 millions**. (b) Sur 10 jours indépendants, l'écart-type est multiplié par √10 : VaR ≈ 116,3 × 3,162 ≈ **367,8 millions**. (c) ES~0,975~ = 50 × 2,3378 ≈ **116,9 millions**.

**Interprétation.** Pour une loi normale, l'ES à 97,5 % est presque égale à la VaR à 99 %. C'est pour cela que le Comité de Bâle a choisi ce niveau : le passage à l'ES ne change pas grand-chose si les pertes sont normales, mais il augmente le capital si les queues sont plus lourdes, ce qui est le cas sur les marchés réels.

**Piège.** Multiplier la VaR à un jour par 10 au lieu de √10. La règle de la racine carrée du temps suppose en outre des jours indépendants, ce qui est discutable en période de crise.

### Exercice 32 : Trois prêts et la diversification

**Calcul.** (a) Un prêt : P(0) = 0,98 ≥ 0,95, donc VaR = **0** ; TVaR = (10 × 0,02 + 0 × (0,98 − 0,95))/0,05 = **4 millions**. (b) Le nombre de défauts suit une binomiale (3 ; 0,02) :

| Perte totale (millions) | 0 | 10 | 20 | 30 |
| --- | --- | --- | --- | --- |
| Probabilité | 0,941192 | 0,057624 | 0,001176 | 0,000008 |
| F | 0,941192 | 0,998816 | 0,999992 | 1 |

F(0) < 0,95 ≤ F(10), donc VaR = **10 millions**. Formule 3 :

$$
\mathrm{TVaR}_{0{,}95} = \frac{20 \times 0{,}001176 + 30 \times 0{,}000008 + 10 \times (0{,}998816 - 0{,}95)}{0{,}05} \approx 10{,}24 \text{ millions}
$$

(c) VaR : 10 > 0 + 0 + 0, elle n'est **pas sous-additive**. TVaR : 10,24 ≤ 3 × 4 = 12, elle **l'est**.

**Interprétation.** C'est le même phénomène qu'au chapitre 10, avec trois prêts. La VaR à 95 % ne voit pas un événement de probabilité 2 % pris isolément, mais le voit dès que l'on regroupe les prêts. La TVaR mesure correctement la gravité et récompense la diversification.

**Piège.** Croire que la VaR d'un prêt est nulle parce que le prêt est sans risque.

### Exercice 33 : La flotte de taxis

**Calcul.** (a) Gamma (2 ; 250 000) : E\[X\] = 2 × 250 000 = **500 000** ; Var(X) = 2 × 250 000² = 1,25 × 10¹¹ ; E\[X²\] = Var(X) + E\[X\]² = **3,75 × 10¹¹**. Pour la Poisson composée : E\[S\] = 30 × 500 000 = **15 millions** ; Var(S) = 30 × 3,75 × 10¹¹, d'où σ~S~ ≈ **3,35 millions**. (b) VaR normale ≈ 15 + 2,5758 × 3,354 ≈ **23,6 millions**. (c) Simulation (400 000 années) : VaR ≈ **24,5 millions** et TVaR ≈ **25,9 millions**. (d) Capital ≈ 24,5 − 15 = **9,5 millions**.

**Interprétation.** L'approximation normale sous-estime la VaR d'environ 0,9 million (4 %). L'écart est plus faible qu'à l'exemple du chapitre 11, car la Gamma de forme 2 est moins asymétrique qu'une lognormale.

**Piège.** Écrire Var(S) = λ Var(X) au lieu de λ E\[X²\].

### Exercice 34 : Un traité de réassurance en excédent de sinistre

**Calcul.** (a) P(X > 10) = (2/10)^2^,5 ≈ **0,0179**. (b) L'excès moyen d'une Pareto au-delà de u vaut u/(a − 1) = 10/1,5 ≈ 6,67 millions, donc E\[(X − 10)₊\] = 0,0179 × 6,67 ≈ **0,119 million** par sinistre. (c) Prime pure annuelle : 40 × 0,119 ≈ **4,77 millions**.

**Interprétation.** En moyenne, moins d'un sinistre par an (40 × 0,018 ≈ 0,7) touche le réassureur, mais quand c'est le cas, il paie en moyenne 6,7 millions. Le réassureur ajoutera un chargement important à cette prime pure, car ce risque est très volatil.

**Piège.** Calculer E\[X − 10\] sans la partie positive : les sinistres inférieurs à 10 millions ne réduisent pas la charge du réassureur.

### Exercice 35 : Tarifer avec le coût du capital

**Calcul.** (a) Pour une exponentielle de moyenne θ = 10 millions, TVaR~0,99~ = VaR~0,99~ + θ, donc le capital vaut TVaR − E\[S\] = VaR~0,99~ = 10 × ln 100 ≈ **46,05 millions**. (b) Coût du capital : 10 % × 46,05 ≈ 4,61 millions. Prime commerciale hors frais ≈ 10 + 4,61 = **14,61 millions**.

**Interprétation.** Le capital immobilisé a un coût : les actionnaires qui l'apportent veulent être rémunérés. Ce coût s'ajoute à la prime pure et explique en grande partie la différence entre prime pure et prime commerciale pour les risques très volatils. Plus la queue de la charge est lourde, plus la TVaR est élevée et plus le produit coûte cher.

**Piège.** Prendre la TVaR entière comme capital, alors que la perte moyenne est déjà couverte par la prime pure.

### Exercice 36 : La précision d'une simulation

**Calcul.** (a) p̂ = 600/50 000 = **0,012** ; se = √(0,012 × 0,988/50 000) ≈ 0,000487 ; IC à 95 % ≈ **\[0,0110 ; 0,0130\]**. (b) n = 1,96² × 0,012 × 0,988/0,0005² ≈ **182 000** scénarios.

**Interprétation.** Avec 50 000 scénarios, la probabilité de ruine est connue à ± 0,1 point environ. Pour diviser cette incertitude par deux, il faut presque quatre fois plus de scénarios.

**Piège.** Oublier que l'erreur diminue comme 1/√n, et non comme 1/n.

### Exercice 37 : Ajuster une exponentielle

**Calcul.** (a) θ̂ = moyenne = (120 + 340 + 85 + 610 + 230 + 150 + 95 + 470)/8 = **262,5**. (b) VaR~0,95~ = 262,5 × ln 20 ≈ **786,4** ; TVaR~0,95~ ≈ 786,4 + 262,5 = **1 048,9** (milliers de FCFA).

**Interprétation.** (c) Avec 8 données, la VaR empirique à 95 % est la 8e valeur, c'est-à-dire le maximum observé (610) : on ne peut rien dire au-delà. Le modèle paramétrique extrapole jusqu'à 786, mais cette valeur dépend entièrement de l'hypothèse exponentielle, qu'on ne peut pas vérifier sérieusement avec si peu de données.

**Piège.** Présenter 786,4 comme un chiffre précis sans mentionner l'incertitude sur la loi et sur θ.

### Exercice 38 : Une lognormale ajustée

**Calcul.** Par maximum de vraisemblance, μ̂ = 12,5 et σ̂ = 0,9. Médiane = e^12^,5 ≈ **268 337 FCFA** ; moyenne = e^12,5\ +\ 0,405^ ≈ **402 319 FCFA** ; VaR~0,99~ = e^12,5\ +\ 0,9\ ×\ 2,3263^ ≈ **2 177 555 FCFA**.

**Interprétation.** Un dossier sur cent coûte plus de 2,2 millions, soit plus de 5 fois la moyenne. La mutuelle doit prévoir ces dossiers lourds dans ses réserves, ou les réassurer.

**Piège.** Confondre les paramètres de la lognormale (calculés sur les logarithmes) avec la moyenne et l'écart-type des coûts eux-mêmes.

### Exercice 39 : Deux ans de backtesting

**Calcul.** (a) 500 × 0,01 = **5** exceptions attendues. (b) P(N ≥ 9) ≈ **0,067** sous la binomiale (500 ; 0,01). (c) LR ≈ 2,61 < 3,841 (p-valeur ≈ 0,106) : on **ne rejette pas** le modèle au seuil de 5 %.

**Interprétation.** Neuf exceptions au lieu de cinq, c'est beaucoup, mais un modèle correct produit ce résultat environ une fois sur quinze. Le test ne permet pas de conclure. Il faut regarder si les exceptions sont regroupées dans le temps et quelle est leur taille, et continuer la surveillance.

**Piège.** Conclure « le modèle est bon » : ne pas rejeter n'est pas prouver.

### Exercice 40 : Un modèle interne à revoir ?

**Calcul.** (a) 250 × 0,05 = **12,5** exceptions attendues. (b) P(N ≥ 20) ≈ **0,027**. (c) LR ≈ 4,04 > 3,841 (p-valeur ≈ 0,044) : on **rejette** le modèle au seuil de 5 %.

**Interprétation.** (d) Trois recommandations possibles : 1) analyser les dates et la taille des exceptions, pour savoir si elles sont concentrées dans une période de forte volatilité ; 2) tester une loi à queues plus lourdes (Student) ou une simulation historique sur une fenêtre plus récente ; 3) compléter la VaR par l'Expected Shortfall et des stress tests, en attendant la correction du modèle.

**Piège.** Appliquer les zones de Bâle, définies pour une VaR à 99 %, à une VaR à 95 %.

## Cas pratiques complets

Ces dix cas pratiques reproduisent des missions réelles d'un gestionnaire de risques, dans des métiers variés : microfinance, gestion d'actifs, assurance agricole, automobile et habitation, banque, trésorerie et prévoyance. Chaque cas mélange plusieurs chapitres du cours et se termine par une recommandation, comme dans un vrai rapport. Comptez deux à trois heures par cas. Tous les résultats sont vérifiés dans les notebooks 09 (cas 1 à 5) et 10 (cas 6 à 10).

| Cas | Métier | Notions mobilisées |
| --- | --- | --- |
| 1 | Microfinance | Binomiale, variance totale, VaR et TVaR discrètes, dépendance, tarification |
| 2 | Gestion d'actifs | Matrice de covariance, VaR et ES normales, contributions, loi de Student, stress test |
| 3 | Assurance agricole | Fonction d'une variable normale, espérance par intégration, risque systémique |
| 4 | Banque, risque opérationnel | Modèle collectif, lognormale, simulation, VaR à 99,9 %, choix d'une mesure de maîtrise |
| 5 | Prévoyance | Binomiale, approximations, mélange de lois, scénario extrême, coût du capital |
| 6 | Assurance automobile | Bayes, loi Gamma, binomiale négative, crédibilité, bonus-malus |
| 7 | Entreprise importatrice | Lognormale, quantiles, TVaR, couverture à terme, option d'achat |
| 8 | Trésorerie bancaire | Transformation linéaire, règle de la racine du temps, VaR et ES, stress test de taux |
| 9 | Banque, crédit aux entreprises | Loi discrète par énumération des scénarios, VaR et TVaR, concentration |
| 10 | Assurance habitation | Modèle collectif, Pareto, simulation, tranche de réassurance |

### Cas pratique 1 : Le risque de crédit d'une institution de microfinance

**Contexte.** Une institution de microfinance du nord du pays accorde 500 crédits d'un an de 300 000 FCFA chacun à des commerçantes, soit un encours de 150 millions de FCFA. L'historique indique un taux de défaut annuel moyen de 4 %. En cas de défaut, l'institution récupère en moyenne 40 % du montant grâce à l'épargne obligatoire et aux garanties. Le conseil d'administration demande au responsable des risques de chiffrer les provisions, le capital à détenir et la marge de risque à intégrer dans le taux d'intérêt.

**Questions.**

1. Calculez la perte en cas de défaut d'un crédit, puis la perte attendue (EL) du portefeuille.
2. On suppose d'abord les défauts indépendants. Donnez la loi du nombre N de défauts, son espérance et son écart-type. Calculez la VaR et la TVaR à 99 % de la perte, puis le capital économique, défini comme VaR − EL.
3. En réalité, l'activité des commerçantes dépend de la conjoncture régionale. Une année sur cinq est « mauvaise » (sécheresse, chute des prix agricoles) avec un taux de défaut de 8 % ; les autres années, il est de 3 %. Sachant la conjoncture, les défauts sont indépendants. Vérifiez que le taux moyen reste 4 %, puis calculez Var(N), la VaR et la TVaR à 99 % et le capital.
4. Calculez P(N ≥ 30) dans les deux modèles.
5. La marge de risque à ajouter au taux d'intérêt doit couvrir la perte attendue et rémunérer le capital à 15 % par an. Calculez-la, en pourcentage de l'encours, dans les deux modèles.
6. Rédigez trois recommandations pour le conseil d'administration.

#### Corrigé du cas 1

**1. Perte attendue.** Le taux de perte en cas de défaut (LGD) vaut 1 − 40 % = 60 %. Un défaut coûte donc 0,6 × 300 000 = **180 000 FCFA**. La perte attendue est le nombre moyen de défauts multiplié par la perte unitaire :

$$
EL = n \times PD \times EAD \times LGD = 500 \times 0{,}04 \times 300\,000 \times 0{,}6 = 3\,600\,000 \text{ FCFA}
$$

C'est 2,4 % de l'encours. **Interprétation** : ce montant doit être provisionné et couvert par les intérêts, car il est « normal » : il se produira en moyenne chaque année.

**2. Modèle indépendant.** N suit une binomiale (500 ; 0,04), avec E\[N\] = 20 et σ = √(500 × 0,04 × 0,96) ≈ 4,38. Le quantile à 99 % de N est la première valeur où F(k) ≥ 0,99 : on trouve **31 défauts** (l'approximation normale donne 20 + 2,3263 × 4,38 ≈ 30,2, qu'on arrondit à l'entier supérieur). Donc :

| Mesure | Calcul | Résultat |
| --- | --- | --- |
| VaR à 99 % | 31 × 180 000 | 5 580 000 FCFA |
| TVaR à 99 % | formule 3 sur la loi de N, × 180 000 | ≈ 5 860 000 FCFA |
| Capital = VaR − EL | 5 580 000 − 3 600 000 | 1 980 000 FCFA |

**3. Modèle avec conjoncture.** Le taux moyen est 0,8 × 3 % + 0,2 × 8 % = **4 %** : la moyenne n'a pas changé, et l'EL non plus. Mais la variance augmente. Avec la formule de la variance totale (chapitre 6) :

$$
E[\mathrm{Var}(N \mid C)] = 0{,}8 \times 500 \times 0{,}03 \times 0{,}97 + 0{,}2 \times 500 \times 0{,}08 \times 0{,}92 = 11{,}64 + 7{,}36 = 19
$$

$$
\mathrm{Var}(E[N \mid C]) = 0{,}8 \times 15^2 + 0{,}2 \times 40^2 - 20^2 = 180 + 320 - 400 = 100 \qquad \mathrm{Var}(N) = 119, \; \sigma \approx 10{,}9
$$

La loi de N est un mélange : 0,8 × binomiale (500 ; 0,03) + 0,2 × binomiale (500 ; 0,08). Son quantile à 99 %, calculé en Python, vaut **50 défauts**.

| Mesure | Indépendance | Avec conjoncture |
| --- | --- | --- |
| Écart-type de N | 4,38 | 10,9 |
| VaR à 99 % | 5,58 millions | 9,00 millions |
| TVaR à 99 % | 5,86 millions | 9,53 millions |
| Capital (VaR − EL) | 1,98 million | 5,40 millions |

**Interprétation** : le capital est multiplié par 2,7 alors que la perte moyenne est identique. La dépendance entre emprunteuses, créée par un choc commun, est le premier facteur de risque d'une institution de microfinance régionale.

**4. Probabilité d'au moins 30 défauts.** Indépendance : **2,0 %**. Avec conjoncture : **19,3 %**, soit presque une année sur cinq, ce qui correspond en fait aux mauvaises années.

**5. Marge de risque.** Marge = (EL + 15 % × capital)/encours.

| Modèle | Calcul | Marge de risque |
| --- | --- | --- |
| Indépendance | (3,6 + 0,15 × 1,98)/150 | ≈ 2,60 % |
| Avec conjoncture | (3,6 + 0,15 × 5,40)/150 | ≈ 2,94 % |

Cette marge s'ajoute au coût de refinancement et aux frais de fonctionnement, qui sont élevés en microfinance.

**6. Recommandations.** (a) Ne pas calibrer le capital sous l'hypothèse d'indépendance : elle le sous-estime d'un facteur 2,7. (b) Diversifier le portefeuille entre régions et secteurs d'activité pour réduire l'effet du choc commun. (c) Constituer en bonne année une réserve pour les mauvaises années, et étudier une couverture de type assurance indicielle sécheresse (cas pratique 3).

**Pièges à éviter.** Calculer la variance du mélange comme la moyenne des variances (on oublie 100 sur 119). Oublier la LGD et compter la totalité du crédit comme perdue. Utiliser l'approximation normale pour le modèle avec conjoncture : la loi de mélange a deux « bosses » et n'est pas normale.

### Cas pratique 2 : Le risque de marché du portefeuille d'une caisse de retraite

**Contexte.** Une caisse de retraite gère 500 millions de FCFA, répartis en trois classes d'actifs. Le comité d'investissement veut connaître la perte potentielle à 10 jours ouvrés, savoir quelle classe porte le risque, et tester la résistance du portefeuille à un krach.

| Classe d'actifs | Poids | Volatilité annuelle |
| --- | --- | --- |
| Actions cotées sur la BRVM | 50 % | 20 % |
| Obligations souveraines | 35 % | 5 % |
| Fonds monétaire | 15 % | 1 % |

Corrélations : actions et obligations 0,2 ; actions et monétaire 0 ; obligations et monétaire 0,3. On néglige les rendements moyens sur 10 jours, et on compte 250 jours ouvrés par an.

**Questions.**

1. Écrivez la matrice de covariance annuelle et calculez la volatilité annuelle du portefeuille.
2. Déduisez la volatilité à 10 jours, puis la VaR à 99 % et l'Expected Shortfall à 97,5 % à 10 jours, en FCFA, sous hypothèse normale.
3. Calculez la part de chaque classe dans la variance du portefeuille.
4. Calculez la VaR à 99 % de chaque classe prise séparément, leur somme, et le gain de diversification.
5. Les rendements quotidiens des actions régionales ont souvent des queues lourdes. Recalculez la VaR et l'ES avec une loi de Student à 4 degrés de liberté de même volatilité. *Indication : une Student à ν degrés a une variance ν/(ν − 2) ; on la ramène à une variance de 1 en la multipliant par √((ν − 2)/ν).*
6. Stress test : les actions perdent 20 % et les obligations 3 % en quelques jours. Calculez la perte et comparez-la aux mesures précédentes.
7. Rédigez la conclusion pour le comité.

#### Corrigé du cas 2

**1. Matrice de covariance.** Σ\_ij = ρ~ij~ σ~i~ σ~j~. Par exemple, Cov(actions, obligations) = 0,2 × 0,20 × 0,05 = 0,002.

|  | Actions | Obligations | Monétaire |
| --- | --- | --- | --- |
| Actions | 0,0400 | 0,0020 | 0 |
| Obligations | 0,0020 | 0,0025 | 0,00015 |
| Monétaire | 0 | 0,00015 | 0,0001 |

La variance du portefeuille est wᵀ Σ w (chapitre 6) :

$$
\sigma_p^2 = 0{,}5^2 \times 0{,}04 + 0{,}35^2 \times 0{,}0025 + 0{,}15^2 \times 0{,}0001 + 2 \times 0{,}5 \times 0{,}35 \times 0{,}002 + 2 \times 0{,}35 \times 0{,}15 \times 0{,}00015 \approx 0{,}011024
$$

Donc σ~p~ ≈ **10,50 % par an**.

**2. Horizon 10 jours.** Si les jours sont indépendants, la variance est proportionnelle au temps : σ~10j~ = 10,50 % × √(10/250) = 10,50 % × 0,2 ≈ **2,10 %**.

$$
\mathrm{VaR}_{0{,}99} = 2{,}3263 \times 0{,}0210 \times 500 \text{ M} \approx 24{,}43 \text{ M} \qquad \mathrm{ES}_{0{,}975} = 2{,}3378 \times 0{,}0210 \times 500 \text{ M} \approx 24{,}55 \text{ M}
$$

**Interprétation** : une période de 10 jours sur 100, la caisse peut perdre plus de 24,4 millions de FCFA, soit 4,9 % de ses actifs.

**3. Contributions au risque.** La part de la classe i dans la variance vaut w~i~ (Σw)~i~/σ~p~². Pour les actions : (Σw)~actions~ = 0,04 × 0,5 + 0,002 × 0,35 = 0,0207, et 0,5 × 0,0207/0,011024 ≈ 93,9 %.

| Classe | Poids dans le portefeuille | Part du risque |
| --- | --- | --- |
| Actions | 50 % | ≈ 93,9 % |
| Obligations | 35 % | ≈ 6,0 % |
| Monétaire | 15 % | ≈ 0,1 % |

**Interprétation** : la moitié du capital produit presque tout le risque. Le poids d'une classe ne dit rien de son risque.

**4. Gain de diversification.** VaR de chaque classe seule (z × σ~i~ × √(10/250) × montant) : actions ≈ 23,26 M ; obligations ≈ 4,07 M ; monétaire ≈ 0,35 M. Somme ≈ **27,68 M**, contre 24,43 M pour le portefeuille : le gain de diversification vaut environ **3,26 millions** (12 %).

**5. Queues lourdes.** Pour une Student à 4 degrés ramenée à une variance de 1, le quantile à 99 % vaut t~0,99~(4) × √(2/4) ≈ 3,747 × 0,707 ≈ 2,649, contre 2,326 pour la normale.

| Mesure à 10 jours | Normale | Student (4) | Écart |
| --- | --- | --- | --- |
| VaR à 99 % | 24,43 M | 27,82 M | +14 % |
| ES à 97,5 % | 24,55 M | 29,65 M | +21 % |

**Interprétation** : avec des queues lourdes, la VaR augmente de 14 % et l'ES de 21 %. L'ES réagit davantage, car elle mesure la taille des pertes extrêmes (chapitre 9).

**6. Stress test.** Perte = 0,5 × 500 M × 20 % + 0,35 × 500 M × 3 % = 50 + 5,25 = **55,25 millions**, plus du double de la VaR. Ce n'est pas une contradiction : la VaR décrit les fluctuations « ordinaires » d'un modèle statistique, alors que le stress test décrit un scénario de crise qu'on choisit délibérément sévère.

**7. Conclusion pour le comité.** « La perte potentielle à 10 jours est d'environ 24 millions (VaR à 99 %), et pourrait atteindre 28 à 30 millions si les marchés ont des queues lourdes. Le risque vient à 94 % de la poche actions. Un krach de 20 % coûterait 55 millions. Si ce montant dépasse la perte acceptable par la caisse, il faut réduire la poche actions ou fixer une limite de perte qui déclenche une réduction automatique de l'exposition. »

**Pièges à éviter.** Multiplier la volatilité annuelle par 10/250 au lieu de √(10/250). Additionner les VaR des classes comme s'il n'y avait aucune diversification. Juger le risque d'une classe d'après son poids. Utiliser directement les quantiles d'une Student sans la ramener à la bonne variance.

### Cas pratique 3 : Une assurance indicielle sécheresse pour une coopérative de cacao

**Contexte.** Une coopérative regroupe 2 000 hectares de cacao. Un assureur propose une assurance **indicielle** : il ne vérifie pas les pertes de chaque planteur, il paie automatiquement selon la pluviométrie annuelle R mesurée à la station météo de la zone. L'indemnité par hectare vaut 500 FCFA par millimètre de pluie manquant sous 1 100 mm, plafonnée à 300 000 FCFA :

$$
I(R) = \min\big(500 \times (1100 - R)_+ \,;\; 300\,000\big) \text{ FCFA par hectare}
$$

L'historique de la station suggère une loi normale de moyenne 1 400 mm et d'écart-type 250 mm.

**Questions.**

1. Quelle est la probabilité qu'une indemnité soit versée une année donnée ? Et que le plafond soit atteint ?
2. Calculez l'indemnité moyenne par hectare, puis la prime pure de la coopérative. *Indication : pour R normale N(μ, σ²) et un seuil c, E\[(c − R)₊\] = (c − μ) Φ(d) + σ φ(d), avec d = (c − μ)/σ.*
3. Calculez la VaR à 99 % de l'indemnité totale versée à la coopérative. *Indication : I est une fonction décroissante de R.*
4. Comparez avec ce que serait la VaR si les 2 000 hectares avaient chacun leur propre pluviométrie, indépendante des autres. Expliquez la différence.
5. L'assureur exige une rémunération de 10 % sur un capital égal à VaR − prime pure. Quelle prime doit-il demander par hectare ?
6. Citez les avantages et les limites de l'assurance indicielle, et dites comment l'assureur peut supporter ce risque.

#### Corrigé du cas 3

**1. Probabilités.** Une indemnité est versée si R < 1 100 : P = Φ((1 100 − 1 400)/250) = Φ(−1,2) ≈ **11,5 %**, soit environ une année sur neuf. Le plafond est atteint si le déficit dépasse 600 mm, c'est-à-dire si R < 500 : P = Φ(−3,6) ≈ **0,016 %**, presque jamais.

**2. Indemnité moyenne.** Comme le plafond n'est presque jamais atteint, on calcule d'abord sans plafond. Avec c = 1 100, μ = 1 400, σ = 250, on a d = −1,2 :

$$
E[(1100 - R)_+] = (-300) \times \Phi(-1{,}2) + 250 \times \varphi(-1{,}2) = -300 \times 0{,}1151 + 250 \times 0{,}1942 \approx 14{,}03 \text{ mm}
$$

Soit 500 × 14,03 ≈ 7 013 FCFA par hectare. Le plafond retire quelques FCFA : le calcul exact (intégration numérique) donne **7 008 FCFA par hectare**. La prime pure de la coopérative vaut 2 000 × 7 008 ≈ **14,0 millions de FCFA**.

**Pourquoi cette formule ?** Elle vient de l'intégrale de (c − r) f(r) pour r < c, calculée après le changement de variable z = (r − μ)/σ. Le terme σ φ(d) traduit le fait que, lorsqu'il manque de la pluie, il en manque en moyenne plus que l'écart au seuil.

**3. VaR à 99 %.** Toutes les parcelles dépendent de la **même** pluviométrie, donc l'indemnité totale vaut 2 000 × I(R). Comme I est décroissante en R, la pire situation à 99 % correspond au quantile à 1 % de la pluie (propriété des quantiles, chapitre 3) :

$$
R_{0{,}01} = 1400 - 2{,}3263 \times 250 \approx 818{,}4 \text{ mm} \qquad \mathrm{VaR}_{0{,}99} = 2000 \times 500 \times (1100 - 818{,}4) \approx 281{,}6 \text{ millions}
$$

La TVaR à 99 %, calculée en Python, vaut environ **365 millions**.

**4. Le rôle de la dépendance.** Si chaque hectare avait sa propre pluie indépendante, la charge totale serait une somme de 2 000 risques indépendants d'écart-type ≈ 26 338 FCFA. Par le TCL, sa VaR à 99 % vaudrait environ 14,0 M + 2,3263 × √2 000 × 26 338 ≈ **16,8 millions**. La vraie VaR est **17 fois plus grande** (281,6 millions). Avec un indice unique, les 2 000 hectares sont touchés ensemble : il n'y a **aucune mutualisation** à l'intérieur de la coopérative. C'est un risque systémique, au sens du chapitre 7.

**5. Prime commerciale.** Capital = 281,6 − 14,0 ≈ 267,6 millions. Coût du capital : 10 % × 267,6 ≈ 26,8 millions. Prime totale ≈ 14,0 + 26,8 = 40,8 millions, soit environ **20 400 FCFA par hectare**, presque trois fois la prime pure.

**Interprétation** : c'est le coût du capital, et non la perte moyenne, qui rend l'assurance agricole chère pour un assureur local qui porte seul le risque.

**6. Avantages, limites et solutions.** Avantages : indemnisation rapide, pas d'expertise sur le terrain, pas de fraude possible sur les pertes déclarées. Limites : le **risque de base**, c'est-à-dire l'écart entre la pluie à la station et la perte réelle d'un planteur (une parcelle peut souffrir sans que l'indice se déclenche, ou l'inverse), et un risque très concentré pour l'assureur. Solutions : réassurer le risque auprès d'un réassureur international, qui le mutualise avec d'autres régions du monde ; regrouper des coopératives de zones climatiques différentes ; et, souvent, faire subventionner une partie de la prime par les pouvoirs publics ou des partenaires au développement.

**Pièges à éviter.** Traiter les 2 000 hectares comme indépendants. Prendre le quantile à 99 % de la pluie au lieu du quantile à 1 % : comme l'indemnité décroît avec la pluie, la pire situation correspond aux faibles pluies. Oublier que l'indemnité est nulle dans 88,5 % des années : sa loi a une grande masse en 0.

### Cas pratique 4 : Le risque opérationnel de fraude interne dans une banque

**Contexte.** Une banque commerciale étudie son risque de fraude interne (détournements par des employés). Son historique et les données de place conduisent au modèle suivant : le nombre annuel de fraudes suit une loi de Poisson de paramètre 4, et le montant d'une fraude suit une lognormale de paramètres μ = 15 et σ = 1,5 (en FCFA). Le capital est mesuré par VaR − perte attendue, au niveau de 99,9 % à un an, niveau historiquement utilisé par les modèles internes bancaires pour le risque opérationnel. Les actionnaires exigent 12 % par an sur ce capital.

La direction hésite entre deux mesures :

- **Mesure A (prévention)** : double validation des virements et rotation des postes sensibles, pour un coût de 5 millions par an. Elle réduit le nombre de fraudes de 25 %.
- **Mesure B (transfert)** : une assurance qui prend en charge, pour chaque fraude, la part au-delà de 100 millions. L'assureur facture 130 % de sa charge moyenne.

**Questions.**

1. Calculez la médiane et la moyenne du montant d'une fraude, et la probabilité qu'une fraude dépasse 100 millions.
2. Calculez la perte annuelle attendue (EL).
3. Par simulation d'un million d'années, estimez la VaR et la TVaR à 99,9 %, puis le capital.
4. Recommencez avec la mesure A.
5. Pour la mesure B, calculez la charge moyenne de l'assureur par fraude et la prime annuelle. *Indication : pour une lognormale, E\[(X − d)₊\] = e^μ\ +\ σ²/2^ Φ((μ + σ² − ln d)/σ) − d Φ((μ − ln d)/σ).* Puis estimez la nouvelle VaR et le nouveau capital par simulation.
6. Comparez le coût économique annuel (perte attendue conservée + coût de la mesure + 12 % du capital) de quatre options : rien, A, B, A et B ensemble. Que recommandez-vous ?
7. Quelles sont les limites de cette analyse ?

#### Corrigé du cas 4

**1. Le montant d'une fraude.** Médiane = e¹⁵ ≈ **3,27 millions** ; moyenne = e^15\ +\ 1,125^ ≈ **10,07 millions**. La moyenne est trois fois la médiane : quelques très grosses fraudes dominent. P(X > 100 M) = 1 − Φ((ln 10⁸ − 15)/1,5) = 1 − Φ(2,28) ≈ **1,13 %**.

**2. Perte attendue.** Modèle collectif (chapitre 11) : EL = λ E\[X\] = 4 × 10,07 ≈ **40,3 millions par an**.

**3. Situation actuelle.** La simulation (1 000 000 d'années, notebook 09) donne VaR~99,9~ % ≈ **659 millions** et TVaR~99,9~ % ≈ **997 millions**. Capital ≈ 659 − 40 = **619 millions**.

**Interprétation** : le capital vaut plus de 15 fois la perte moyenne. C'est typique du risque opérationnel : la moyenne est modeste, mais une seule fraude exceptionnelle peut coûter des centaines de millions. La TVaR, 50 % au-dessus de la VaR, confirme une queue très lourde. Avec un million d'années simulées, seules 1 000 dépassent la VaR : ces chiffres sont précis à quelques pour cent près (chapitre 12).

**4. Mesure A.** Avec λ = 3 : EL ≈ **30,2 millions**, VaR ≈ **583 millions**, capital ≈ **553 millions**. La prévention réduit bien la perte attendue (−25 %) mais beaucoup moins le capital (−11 %) : elle réduit le nombre de fraudes, pas leur gravité.

**5. Mesure B.** Avec d = 100 millions, ln d ≈ 18,42 :

$$
E[(X - d)_+] = 10{,}07 \text{ M} \times \Phi\Big(\frac{17{,}25 - 18{,}42}{1{,}5}\Big) - 100 \text{ M} \times \Phi\Big(\frac{15 - 18{,}42}{1{,}5}\Big) \approx 2{,}19 \text{ M} - 1{,}13 \text{ M} \approx 1{,}06 \text{ M}
$$

Charge annuelle de l'assureur : 4 × 1,06 ≈ 4,25 millions ; prime : 1,3 × 4,25 ≈ **5,52 millions**. Après assurance, la banque conserve min(X ; 100 M) par fraude. La simulation donne EL conservée ≈ **36,0 millions**, VaR ≈ **232 millions**, capital ≈ **196 millions**.

**Interprétation** : en cédant les fraudes extrêmes, la banque divise son capital par plus de trois, pour une prime d'environ 5,5 millions.

**6. Comparaison économique.**

| Option | EL conservée | Coût de la mesure | Capital | 12 % du capital | Coût total annuel |
| --- | --- | --- | --- | --- | --- |
| Rien | 40,3 | 0 | 619 | 74,3 | **114,5** |
| A seule | 30,2 | 5,0 | 553 | 66,3 | **101,5** |
| B seule | 36,0 | 5,5 | 196 | 23,5 | **65,0** |
| A et B | 27,0 | 9,1 | 181 | 21,7 | **57,8** |

(montants en millions de FCFA ; pour A et B ensemble, la prime d'assurance baisse à 4,1 millions car il y a moins de fraudes.)

**Recommandation** : combiner les deux mesures. L'assurance réduit le capital et donc le coût total de moitié ; la prévention ajoute un gain réel et réduit aussi la prime d'assurance. Si une seule mesure est possible, l'assurance est la plus efficace sur le capital, la prévention sur la perte attendue.

**7. Limites.** Les paramètres de la lognormale sont estimés sur peu de grosses fraudes : la VaR à 99,9 % est très sensible à σ (refaites le calcul avec σ = 1,8 pour le constater). L'assurance comporte un risque de contrepartie (l'assureur doit pouvoir payer) et des exclusions de garantie. Enfin, la réglementation peut limiter la réduction de capital accordée pour une assurance.

**Pièges à éviter.** Utiliser l'approximation normale pour une charge aussi asymétrique. Juger une mesure sur la seule perte attendue en oubliant le capital. Oublier le chargement de l'assureur dans le coût de la mesure B.

### Cas pratique 5 : Le contrat de prévoyance décès d'une entreprise

**Contexte.** Une entreprise de 1 000 salariés souscrit un contrat de prévoyance : en cas de décès d'un salarié dans l'année, l'assureur verse un capital de 5 millions de FCFA à sa famille. La probabilité annuelle de décès d'un salarié est de 0,4 %, et les décès sont supposés indépendants. L'assureur fixe son capital au niveau de 99,5 % (VaR − perte attendue) et exige une rémunération de 10 % de ce capital.

**Questions.**

1. Donnez la loi du nombre N de décès, son espérance, et la prime pure annuelle (totale et par salarié).
2. Calculez le quantile à 99,5 % de N exactement, puis avec l'approximation normale et l'approximation de Poisson. Laquelle est la plus fiable ici ?
3. Calculez la VaR et la TVaR à 99,5 % de la charge, puis le capital.
4. L'actuaire ajoute un scénario d'épidémie : une année sur cinquante, la probabilité de décès triple (1,2 %). Recalculez l'espérance de N, son quantile à 99,5 %, la VaR, la TVaR et le capital.
5. Comparez P(N ≥ 10) dans les deux modèles.
6. Calculez la prime commerciale hors frais (prime pure + 10 % du capital) dans les deux modèles, par salarié.
7. Qu'en concluez-vous sur l'utilité des scénarios extrêmes ?

#### Corrigé du cas 5

**1. Loi de N et prime pure.** 1 000 salariés indépendants, même probabilité : N suit une binomiale (1 000 ; 0,004). E\[N\] = **4 décès par an**. Prime pure = 4 × 5 millions = **20 millions**, soit **20 000 FCFA par salarié**.

**2. Le quantile à 99,5 %.**

| Méthode | Calcul | Quantile de N |
| --- | --- | --- |
| Exact (binomiale) | première valeur où F(k) ≥ 0,995 | **10** |
| Approximation normale | 4 + 2,5758 × √(1 000 × 0,004 × 0,996) ≈ 9,14 | 9,14 (soit 10 en arrondissant vers le haut) |
| Approximation de Poisson (λ = 4) | première valeur où F(k) ≥ 0,995 | **10** |

La Poisson est la plus fiable, car n est grand et p très petit (événements rares, chapitre 5). L'approximation normale fonctionne mal quand l'espérance est aussi faible (4) : la loi est très asymétrique.

**3. VaR, TVaR et capital.** VaR~99,5~ % = 10 × 5 M = **50 millions** ; TVaR~99,5~ % ≈ **54,0 millions** (formule 3 sur la loi binomiale) ; capital = 50 − 20 = **30 millions**.

**4. Scénario d'épidémie.** N suit maintenant un mélange : 0,98 × binomiale (1 000 ; 0,004) + 0,02 × binomiale (1 000 ; 0,012).

$$
E[N] = 0{,}98 \times 4 + 0{,}02 \times 12 = 4{,}16
$$

Le quantile à 99,5 % du mélange, calculé en Python, vaut **14 décès**.

| Mesure | Sans épidémie | Avec épidémie | Variation |
| --- | --- | --- | --- |
| Prime pure | 20,0 M | 20,8 M | +4 % |
| VaR à 99,5 % | 50 M | 70 M | +40 % |
| TVaR à 99,5 % | 54,0 M | 82,5 M | +53 % |
| Capital | 30 M | 49,2 M | +64 % |

**5. Probabilité d'au moins 10 décès.** Sans épidémie : **0,80 %**. Avec : **2,30 %**, presque trois fois plus.

**6. Prime commerciale par salarié.** Sans épidémie : (20 + 0,10 × 30)/1 000 = **23 000 FCFA**. Avec : (20,8 + 0,10 × 49,2)/1 000 ≈ **25 720 FCFA**, soit 12 % de plus.

**7. Conclusion.** Un scénario rare (une année sur cinquante) ne change presque pas la moyenne (+4 %), mais il change beaucoup le capital (+64 %), car le capital dépend de la queue de la distribution. Un assureur qui ne raisonne qu'en moyenne ne voit pas ce risque. C'est la raison d'être des scénarios de stress et des mesures de queue comme la VaR et la TVaR. Pour aller plus loin, l'assureur peut aussi limiter ce risque par la réassurance d'un excédent de décès ou en diversifiant ses contrats entre entreprises et régions.

**Pièges à éviter.** Arrondir le quantile normal vers le bas (9 au lieu de 10). Moyenner les deux quantiles (10 et 22) avec les poids 0,98 et 0,02, ce qui donnerait environ 10,2 au lieu de 14 : le quantile d'un mélange n'est pas le mélange des quantiles, il faut calculer la fonction de répartition du mélange. Oublier que l'épidémie touche tous les salariés en même temps : c'est justement ce qui rend les décès des salariés dépendants entre eux.

### Cas pratique 6 : Le bonus-malus d'un assureur automobile

**Contexte.** Un assureur automobile sait que ses assurés n'ont pas tous le même risque, mais il ne peut pas observer directement la fréquence de sinistres λ de chacun. Il modélise cette hétérogénéité ainsi : pour un assuré donné, le nombre annuel de sinistres suit une Poisson de paramètre λ ; dans le portefeuille, λ suit une loi Gamma de forme α = 2 et de taux β = 20 (densité proportionnelle à λ^α−1^ e^−βλ^, moyenne α/β = 0,1). Le coût moyen d'un sinistre est de 400 000 FCFA. L'assureur veut un barème de bonus-malus qui fasse payer à chacun une prime proche de son vrai risque.

**Questions.**

1. Calculez la prime pure d'un nouvel assuré, dont on ne sait rien.
2. Pour un assuré pris au hasard, le nombre de sinistres suit une binomiale négative de paramètres r = α = 2 et p = β/(β + 1) = 20/21. Calculez E\[N\], Var(N), P(N = 0) et P(N ≥ 2). Comparez avec une Poisson de paramètre 0,1.
3. Un assuré a eu k sinistres en t années. Montrez avec la formule de Bayes que la loi de son λ, sachant cette information, est une Gamma de paramètres α + k et β + t. Déduisez-en sa fréquence estimée.
4. Calculez la prime pure et le coefficient de bonus-malus (prime/prime de base) pour : 0 sinistre en 3 ans ; 1 sinistre en 3 ans ; 2 sinistres en 3 ans ; 0 sinistre en 10 ans.
5. Montrez que la fréquence estimée s'écrit Z × (k/t) + (1 − Z) × 0,1, avec Z = t/(β + t). Interprétez Z.
6. Un concurrent applique une règle simple : −5 % par année sans sinistre, +25 % par sinistre. Comparez avec votre barème pour 0 sinistre en 3 ans et pour 2 sinistres en 3 ans.
7. Concluez.

#### Corrigé du cas 6

**1. Prime pure a priori.** Fréquence moyenne 0,1 sinistre par an, coût moyen 400 000 : prime pure = **40 000 FCFA**.

**2. Binomiale négative.** E\[N\] = r(1 − p)/p = 2 × (1/21)/(20/21) = **0,1** et Var(N) = r(1 − p)/p² = **0,105** : la variance dépasse la moyenne (surdispersion due à l'hétérogénéité, chapitre 5).

| Probabilité | Binomiale négative | Poisson (0,1) |
| --- | --- | --- |
| P(N = 0) | (20/21)² ≈ 0,9070 | e^−0,1^ ≈ 0,9048 |
| P(N ≥ 2) | ≈ 0,00659 | ≈ 0,00468 |

Le modèle hétérogène donne un peu plus d'assurés sans sinistre, et **41 % de plus** d'assurés avec au moins deux sinistres : les mauvais conducteurs concentrent les sinistres multiples.

**3. Mise à jour bayésienne.** La densité a posteriori est proportionnelle à la densité a priori multipliée par la vraisemblance des observations. Sur t années, le nombre total de sinistres suit une Poisson de paramètre λt :

$$
f(\lambda \mid k) \propto \underbrace{\lambda^{\alpha - 1} e^{-\beta\lambda}}_{\text{a priori}} \times \underbrace{e^{-\lambda t}\,\lambda^{k}}_{\text{vraisemblance}} = \lambda^{(\alpha + k) - 1}\,e^{-(\beta + t)\lambda}
$$

On reconnaît une Gamma de paramètres α + k et β + t, de moyenne :

$$
\hat{\lambda} = E[\lambda \mid k \text{ sinistres en } t \text{ ans}] = \frac{\alpha + k}{\beta + t}
$$

**4. Barème.**

| Historique | Fréquence estimée | Prime pure | Coefficient |
| --- | --- | --- | --- |
| 0 sinistre en 3 ans | 2/23 ≈ 0,0870 | ≈ 34 783 | **0,87** (bonus de 13 %) |
| 1 sinistre en 3 ans | 3/23 ≈ 0,1304 | ≈ 52 174 | **1,30** (malus de 30 %) |
| 2 sinistres en 3 ans | 4/23 ≈ 0,1739 | ≈ 69 565 | **1,74** (malus de 74 %) |
| 0 sinistre en 10 ans | 2/30 ≈ 0,0667 | ≈ 26 667 | **0,67** (bonus de 33 %) |

**5. Crédibilité.** On écrit (α + k)/(β + t) = \[t/(β + t)\] × (k/t) + \[β/(β + t)\] × (α/β). C'est bien Z × (expérience de l'assuré) + (1 − Z) × (moyenne du portefeuille), avec Z = t/(β + t). Pour 3 ans, Z = 3/23 ≈ **0,13** ; pour 10 ans, Z = 1/3. **Interprétation** : Z est le poids accordé à l'historique personnel. Trois ans d'observation ne pèsent que 13 %, car les sinistres sont rares et un historique court est peu informatif. Plus on observe longtemps un assuré, plus on se fie à son propre historique.

**6. Comparaison avec la règle simple.** 0 sinistre en 3 ans : 0,95³ ≈ 0,857, proche de 0,87. 2 sinistres en 3 ans (une année sans sinistre, deux années avec un sinistre) : 0,95 × 1,25² ≈ 1,48, contre 1,74 dans le modèle. La règle simple est raisonnable pour les bons conducteurs, mais **trop clémente** pour les mauvais : l'assureur qui l'applique attire et garde des conducteurs à risque sous-tarifés.

**7. Conclusion.** Le bonus-malus n'est pas une punition : c'est une application directe de la formule de Bayes. Le modèle Poisson-Gamma donne un barème justifié, qui pèse correctement l'historique selon sa durée. Il faut toutefois estimer α et β sur les données du portefeuille (moyenne et variance des nombres de sinistres), et vérifier que le barème reste commercialement acceptable.

**Pièges à éviter.** Estimer la fréquence d'un assuré par k/t seul : avec 0 sinistre en 3 ans, on lui donnerait une prime nulle. Confondre le taux β et l'échelle 1/β de la loi Gamma (SciPy utilise l'échelle). Oublier que la binomiale négative de SciPy compte des échecs, avec p = β/(β + 1).

### Cas pratique 7 : Le risque de change d'un importateur

**Contexte.** Un importateur de riz doit payer à son fournisseur 1 000 000 de dollars dans 3 mois. Le dollar vaut aujourd'hui 600 FCFA. Le directeur financier modélise le cours S~T~ dans 3 mois (T = 0,25 an) par une lognormale sans tendance, de volatilité annuelle 8 % :

$$
\ln\Big(\frac{S_T}{600}\Big) \sim \mathcal{N}\Big(-\frac{\sigma^2 T}{2},\; \sigma^2 T\Big) \qquad \sigma = 0{,}08, \; T = 0{,}25 \qquad \Rightarrow E[S_T] = 600
$$

Sa banque lui propose deux couvertures : (1) un **contrat à terme** qui fixe dès aujourd'hui le cours à 602 FCFA ; (2) une **option d'achat** de dollars au prix d'exercice de 610 FCFA : si le dollar dépasse 610, l'importateur achète à 610 ; sinon, il achète au cours du marché. La prime de l'option est égale à son espérance de gain, E\[(S~T~ − 610)₊\] par dollar (taux d'intérêt négligés).

**Questions.**

1. Exprimez le coût C de la facture en FCFA sans couverture. Calculez E\[C\].
2. Calculez les quantiles à 95 % et 99 % de S~T~, puis la VaR à 95 % et 99 % du surcoût C − 600 millions. *Indication : le quantile d'une lognormale est l'exponentielle du quantile de la normale.*
3. Calculez la probabilité que le dollar dépasse 610 FCFA.
4. Calculez la prime de l'option avec la formule E\[(S~T~ − K)₊\] = S₀ Φ(d₁) − K Φ(d₂), où d₁ = (ln(S₀/K) + σ²T/2)/(σ√T) et d₂ = d₁ − σ√T.
5. Comparez les trois stratégies (aucune couverture, contrat à terme, option) selon le coût moyen et le coût dans le pire 5 % des cas.
6. Que recommandez-vous à l'importateur, dont la marge sur cette cargaison est de 30 millions de FCFA ?

#### Corrigé du cas 7

**1. Coût sans couverture.** C = 1 000 000 × S~T~ FCFA. Comme E\[S~T~\] = 600, E\[C\] = **600 millions**. Le choix de la moyenne −σ²T/2 pour ln(S~T~) sert justement à obtenir E\[S~T~\] = S₀, car E\[e^Y^\] = e^m\ +\ v/2^ pour Y normale (chapitre 5).

**2. Quantiles et VaR.** σ√T = 0,08 × 0,5 = 0,04 et −σ²T/2 = −0,0008. Le quantile d'une fonction croissante est la fonction du quantile (chapitre 3) :

$$
q_{0{,}95}(S_T) = 600 \times e^{-0{,}0008 + 0{,}04 \times 1{,}6449} \approx 640{,}3 \qquad q_{0{,}99}(S_T) = 600 \times e^{-0{,}0008 + 0{,}04 \times 2{,}3263} \approx 658{,}0
$$

VaR du surcoût : à 95 %, (640,3 − 600) × 1 000 000 ≈ **40,3 millions** ; à 99 %, ≈ **58,0 millions**. La TVaR à 95 % du surcoût vaut environ **51,2 millions**.

**3. Probabilité de dépasser 610.** P(S~T~ > 610) = 1 − Φ((ln(610/600) + 0,0008)/0,04) = 1 − Φ(0,433) ≈ **33 %**.

**4. Prime de l'option.** d₁ = (ln(600/610) + 0,0008)/0,04 ≈ (−0,01653 + 0,0008)/0,04 ≈ −0,393 et d₂ ≈ −0,433.

$$
E[(S_T - 610)_+] = 600 \times \Phi(-0{,}393) - 610 \times \Phi(-0{,}433) \approx 600 \times 0{,}347 - 610 \times 0{,}332 \approx 5{,}47 \text{ FCFA par dollar}
$$

Prime totale ≈ **5,47 millions**. On reconnaît la formule de la prime stop-loss d'une lognormale (cas pratique 4) : une option d'achat est une assurance contre la hausse du dollar, avec une franchise de 610.

**5. Comparaison des stratégies** (millions de FCFA) :

| Stratégie | Coût moyen | Coût au quantile à 95 % | Coût maximal |
| --- | --- | --- | --- |
| Aucune couverture | 600,0 | 640,3 | sans limite |
| Contrat à terme | 602,0 | 602,0 | 602,0 |
| Option (prix d'exercice 610) | 600,0 | 615,5 | 615,5 |

Pour l'option : le coût vaut min(S~T~ ; 610) millions plus la prime. Son espérance vaut E\[S~T~\] − E\[(S~T~ − 610)₊\] + prime = 600 − 5,47 + 5,47 = **600 millions** : avec une prime « juste », l'option ne coûte rien en moyenne. Dans les mauvais cas, le coût est plafonné à 610 + 5,47 = 615,5 millions.

**6. Recommandation.** Sans couverture, une hausse du dollar coûterait plus de 40 millions une fois sur vingt, ce qui effacerait la marge de 30 millions : l'importateur doit se couvrir. Le contrat à terme supprime tout risque pour un coût certain de 2 millions, et convient s'il veut un prix de revient connu dès aujourd'hui. L'option limite la perte à 15,5 millions tout en gardant le gain si le dollar baisse ; en pratique, la banque ajoutera une marge à la prime. Pour un importateur dont la marge est faible, le contrat à terme est en général le choix le plus simple.

**Pièges à éviter.** Modéliser S~T~ par une loi normale, qui autorise des cours négatifs et sous-estime la queue des hausses. Oublier le terme −σ²T/2, ce qui donne E\[S~T~\] ≠ 600. Juger l'option sur son coût moyen seul : son intérêt est de supprimer la queue de distribution du coût.

### Cas pratique 8 : Le risque de taux d'un portefeuille obligataire

**Contexte.** La trésorerie d'une banque détient 1 milliard de FCFA d'obligations souveraines de la région. Quand les taux d'intérêt montent, le prix des obligations baisse. Pour une petite variation Δy du taux, la variation relative de valeur est approchée par la duration modifiée D et la convexité C :

$$
\frac{\Delta P}{P} \approx -D\,\Delta y + \frac{1}{2}\,C\,(\Delta y)^2 \qquad D = 4{,}5, \quad C = 25
$$

La variation quotidienne du taux est supposée normale, de moyenne nulle et d'écart-type 6 points de base (1 point de base = 0,01 % = 0,0001), indépendante d'un jour à l'autre.

**Questions.**

1. En négligeant la convexité, exprimez la perte quotidienne L en fonction de Δy et donnez sa loi.
2. Calculez la VaR à 99 % à un jour et à 10 jours, puis l'Expected Shortfall à 97,5 % à 10 jours.
3. Stress test réglementaire : les taux montent brutalement de 200 points de base. Calculez la perte avec la duration seule, puis avec la convexité. Faites de même pour une baisse de 200 points de base. Que constatez-vous ?
4. Comparez la perte du stress test à la VaR à 10 jours.
5. Le comité actif-passif veut ramener la VaR à 10 jours sous 12 millions. Quelle duration maximale le portefeuille doit-il avoir ?
6. Quelles sont les limites de ce calcul ?

#### Corrigé du cas 8

**1. Loi de la perte.** Sans convexité, ΔP ≈ −D Δy P, donc la perte vaut L = −ΔP ≈ D P Δy = 4,5 × 10⁹ × Δy. C'est une transformation linéaire d'une normale (chapitre 3) : L est normale de moyenne 0 et d'écart-type 4,5 × 10⁹ × 0,0006 = **2,7 millions** par jour.

**2. VaR et ES.**

| Mesure | Calcul | Résultat |
| --- | --- | --- |
| VaR à 99 %, 1 jour | 2,3263 × 2,7 M | ≈ 6,28 millions |
| Écart-type à 10 jours | 2,7 M × √10 | ≈ 8,54 millions |
| VaR à 99 %, 10 jours | 2,3263 × 8,54 M | ≈ 19,86 millions |
| ES à 97,5 %, 10 jours | 2,3378 × 8,54 M | ≈ 19,96 millions |

Le passage à 10 jours multiplie la VaR par √10 ≈ 3,16, car les variances de variables indépendantes s'additionnent (chapitre 7).

**3. Stress test de 200 points de base** (Δy = ±0,02) :

| Scénario | Duration seule | Duration et convexité |
| --- | --- | --- |
| Hausse de 200 pb | −4,5 × 0,02 = −9,0 %, soit **−90 millions** | −9,0 % + 0,5 × 25 × 0,0004 = −8,5 %, soit **−85 millions** |
| Baisse de 200 pb | +90 millions | +9,0 % + 0,5 % = +9,5 %, soit **+95 millions** |

**Interprétation** : la convexité est favorable au porteur d'obligations. Elle amortit les pertes quand les taux montent et amplifie les gains quand ils baissent. Pour les petites variations quotidiennes (6 points de base), son effet est négligeable ; pour un choc de 200 points, il vaut 5 millions.

**4. Stress test et VaR.** La perte de stress (85 millions) vaut environ **4,3 fois** la VaR à 10 jours. Un choc de 200 points correspond à plus de 10 écarts-types à 10 jours (200/19 ≈ 10,5) : selon le modèle normal, il serait pratiquement impossible. Pourtant, des hausses de taux de cette ampleur se produisent lors de crises de dette ou de changements brutaux de politique monétaire. C'est pourquoi les régulateurs imposent des stress tests en plus de la VaR.

**5. Duration maximale.** La VaR est proportionnelle à D. Il faut D × (19,86/4,5) ≤ 12, soit D ≤ 12 × 4,5/19,86 ≈ **2,7**. La trésorerie doit vendre une partie de ses obligations longues ou les remplacer par des titres plus courts.

**6. Limites.** L'approximation par la duration et la convexité n'est valable que pour des variations modérées. Les variations de taux ont souvent des queues plus lourdes qu'une normale et ne sont pas indépendantes d'un jour à l'autre en période de crise. Toute la courbe des taux ne bouge pas forcément de la même quantité. Enfin, sur un marché obligataire peu liquide, vendre rapidement peut coûter plus que la baisse de prix théorique.

**Pièges à éviter.** Confondre 6 points de base (0,0006) et 6 % (0,06). Oublier le signe : c'est la **hausse** des taux qui fait perdre le porteur d'obligations. Multiplier la VaR à un jour par 10 au lieu de √10.

### Cas pratique 9 : Le risque de concentration d'un portefeuille de crédits aux entreprises

**Contexte.** Une banque régionale a prêté à trois entreprises. Les défauts sont supposés indépendants. Le régulateur et le comité des risques s'inquiètent de la taille du prêt à l'entreprise A.

| Entreprise | Montant prêté (EAD) | Probabilité de défaut (PD) | Perte en cas de défaut (LGD) |
| --- | --- | --- | --- |
| A (industrie) | 500 millions | 1 % | 45 % |
| B (commerce) | 200 millions | 3 % | 60 % |
| C (transport) | 100 millions | 5 % | 70 % |

**Questions.**

1. Calculez la perte en cas de défaut et la perte attendue de chaque entreprise, puis du portefeuille.
2. Énumérez les 8 scénarios possibles (chaque entreprise fait défaut ou non), avec leur perte et leur probabilité. Déduisez-en la loi de la perte totale.
3. Calculez la VaR et la TVaR à 95 % et à 99,5 %.
4. Expliquez ce qui détermine la VaR à 99,5 %.
5. La banque envisage de remplacer le prêt de 500 millions à A par cinq prêts de 100 millions à cinq entreprises indépendantes de même profil (PD 1 %, LGD 45 %). Recalculez la perte attendue, la VaR et la TVaR à 99,5 % (en Python), et le capital (VaR − perte attendue).
6. Formulez vos recommandations.

#### Corrigé du cas 9

**1. Pertes.** Perte en cas de défaut = EAD × LGD : A 225 millions, B 120 millions, C 70 millions. Perte attendue = PD × EAD × LGD : A 2,25 ; B 3,6 ; C 3,5 ; total **9,35 millions**. L'entreprise A a la plus petite perte attendue, mais la plus grosse perte possible.

**2. Les 8 scénarios.** Par indépendance, la probabilité d'un scénario est le produit des probabilités. Par exemple, « seule C fait défaut » : 0,99 × 0,97 × 0,05 = 0,048015.

| Défauts | Perte (millions) | Probabilité | F (cumulée) |
| --- | --- | --- | --- |
| Aucun | 0 | 0,912285 | 0,912285 |
| C | 70 | 0,048015 | 0,960300 |
| B | 120 | 0,028215 | 0,988515 |
| B et C | 190 | 0,001485 | 0,990000 |
| A | 225 | 0,009215 | 0,999215 |
| A et C | 295 | 0,000485 | 0,999700 |
| A et B | 345 | 0,000285 | 0,999985 |
| A, B et C | 415 | 0,000015 | 1 |

**3. VaR et TVaR** (formule 3 du chapitre 9) :

| Niveau | VaR | TVaR |
| --- | --- | --- |
| 95 % | 70 millions (F(0) < 0,95 ≤ F(70)) | ≈ 134,2 millions |
| 99,5 % | 225 millions (F(190) < 0,995 ≤ F(225)) | ≈ 239,2 millions |

**4. Ce qui détermine la VaR à 99,5 %.** La perte dépasse 190 millions si et seulement si A fait défaut, ce qui arrive avec probabilité 1 %, plus que les 0,5 % tolérés. La VaR à 99,5 % est donc entièrement déterminée par le défaut d'un seul emprunteur : c'est le **risque de concentration**. Remarque : on voit aussi que F(190) = P(A ne fait pas défaut) = 0,99 exactement, ce qui rend la VaR à 99 % très sensible aux arrondis.

**5. Diviser le prêt à A.** La perte attendue ne change pas : 5 × 0,01 × 100 × 0,45 = 2,25 millions pour les cinq prêts, soit toujours **9,35 millions** au total. Mais la loi de la perte change (calcul par énumération des 128 scénarios dans le notebook 10) :

| Mesure | Un prêt de 500 M à A | Cinq prêts de 100 M |
| --- | --- | --- |
| Perte attendue | 9,35 M | 9,35 M |
| VaR à 99,5 % | 225 M | 120 M |
| TVaR à 99,5 % | ≈ 239,2 M | ≈ 154,9 M |
| Capital (VaR − perte attendue) | ≈ 215,7 M | ≈ 110,7 M |

Le capital baisse de **49 %**, sans changer ni le montant total prêté ni la qualité des emprunteurs. C'est l'effet de la diversification : cinq défauts indépendants ont très peu de chances de survenir ensemble, alors que le défaut d'un gros emprunteur unique est « tout ou rien ».

**6. Recommandations.** (a) Fixer une limite d'exposition par emprunteur, par exemple en pourcentage des fonds propres, comme le prévoient les règles de division des risques des régulateurs bancaires. (b) Pour les gros clients, partager le prêt avec d'autres banques (crédit syndiqué) ou obtenir des garanties qui réduisent la LGD. (c) Suivre le risque de concentration avec la VaR et la TVaR à haut niveau, que la perte attendue ne voit pas.

**Pièges à éviter.** Juger le risque d'un prêt par sa seule perte attendue. Oublier un scénario dans l'énumération (vérifier que les probabilités somment à 1). Supposer les défauts indépendants si les entreprises appartiennent au même secteur ou au même groupe : la concentration serait alors encore plus forte.

### Cas pratique 10 : Catastrophe naturelle et réassurance d'un portefeuille habitation

**Contexte.** Un assureur couvre des habitations à Abidjan. Sa charge annuelle a deux composantes indépendantes :

- les sinistres **courants** (dégâts des eaux, incendies, vols) : un nombre de Poisson de paramètre 200, et un coût lognormal de paramètres μ = 13 et σ = 1 (FCFA) ;
- une **inondation majeure** : elle survient une année sur vingt (probabilité 5 %), et son coût total pour l'assureur suit alors une Pareto de paramètres x~m~ = 200 millions et a = 2.

L'assureur fixe son capital au niveau de 99,5 % (VaR − perte attendue). Un réassureur propose une tranche « 700 millions en excédent de 300 millions » : pour l'inondation, il paie la part du coût comprise entre 300 millions et 1 milliard.

**Questions.**

1. Sinistres courants : calculez la charge moyenne, son écart-type et une VaR à 99,5 % par approximation normale.
2. Inondation : calculez la charge moyenne annuelle, et la probabilité annuelle que l'inondation coûte plus de 300 millions, puis plus d'un milliard.
3. Par simulation d'un million d'années, estimez la VaR et la TVaR à 99,5 % des sinistres courants seuls, de l'inondation seule, puis du total. Calculez les capitaux. Quelle composante détermine le capital ?
4. Calculez la prime pure de la tranche de réassurance.
5. Recalculez la VaR, la TVaR et le capital après réassurance.
6. Le réassureur facture deux fois la prime pure et le capital coûte 10 % par an. La réassurance est-elle rentable ? Que pensez-vous de la TVaR après réassurance ?

#### Corrigé du cas 10

**1. Sinistres courants.** E\[X\] = e^13\ +\ 0,5^ ≈ 729 416 FCFA et E\[X²\] = e^26\ +\ 2^ = e²⁸. Par les formules de la Poisson composée (chapitre 11) :

$$
E[S_c] = 200 \times 729\,416 \approx 145{,}9 \text{ M} \qquad \sigma_{S_c} = \sqrt{200 \times e^{28}} \approx 17{,}0 \text{ M} \qquad \mathrm{VaR}_{0{,}995} \approx 145{,}9 + 2{,}5758 \times 17{,}0 \approx 189{,}7 \text{ M}
$$

**2. Inondation.** Une Pareto de paramètres 200 millions et a = 2 a pour moyenne 2 × 200/1 = 400 millions. Charge moyenne annuelle : 0,05 × 400 = **20 millions**. P(coût > 300 M) = 0,05 × (200/300)² ≈ **2,2 %** ; P(coût > 1 milliard) = 0,05 × (200/1 000)² = **0,2 %**.

**3. Simulation** (millions de FCFA) :

| Composante | Perte moyenne | VaR à 99,5 % | TVaR à 99,5 % | Capital |
| --- | --- | --- | --- | --- |
| Sinistres courants seuls | 145,9 | 195,1 | 202,9 | 49,2 |
| Inondation seule | 20,2 | 634,1 | 1 287,6 | 614,0 |
| Total | 166,0 | 780,5 | 1 433,5 | 614,4 |

**Interprétation** : l'inondation ne représente que 12 % de la perte moyenne, mais elle détermine presque tout le capital. Remarquez aussi que la VaR simulée des sinistres courants (195,1) dépasse l'approximation normale (189,7) : la queue lognormale rend la charge asymétrique.

**4. Prime pure de la tranche.** Le réassureur paie min((Y − 300)₊ ; 700) si l'inondation survient. Son espérance est l'intégrale de la fonction de survie de Y entre 300 et 1 000 (chapitre 4), avec S(y) = (200/y)² :

$$
0{,}05 \times \int_{300}^{1000} \Big(\frac{200}{y}\Big)^2 dy = 0{,}05 \times 200^2 \times \Big(\frac{1}{300} - \frac{1}{1000}\Big) \approx 0{,}05 \times 93{,}3 \approx 4{,}67 \text{ millions}
$$

**5. Après réassurance.** Perte moyenne conservée ≈ **161,3 millions** ; VaR à 99,5 % ≈ **464,3 millions** ; TVaR à 99,5 % ≈ **882 millions** ; capital ≈ **302,9 millions**, soit −51 %.

**6. Rentabilité et limites.** Coût de la réassurance : 2 × 4,67 ≈ 9,3 millions par an. Économie de capital : 614,4 − 302,9 ≈ 311 millions, qui coûtaient 10 % × 311 ≈ 31 millions par an. Gain net ≈ 31 − 9,3 + 4,7 (pertes cédées en moyenne) ≈ **26 millions par an** : la réassurance est très rentable. En revanche, la TVaR reste élevée (882 millions) : dans les pires années, l'inondation dépasse 1 milliard et la partie au-delà n'est pas couverte. L'assureur devrait étudier une seconde tranche au-dessus d'un milliard, et limiter son exposition dans les quartiers les plus inondables.

**Pièges à éviter.** Mélanger sinistres courants et catastrophes dans une seule loi : leurs comportements sont très différents et se modélisent séparément. Utiliser l'approximation normale pour le total : la Pareto avec a = 2 a une variance infinie. Juger la réassurance sur sa prime seulement, sans compter le capital libéré.

## Suite du cours

Les Parties 0 à 4, la banque de 40 exercices corrigés et les 10 cas pratiques complets sont terminés. Les annexes (formulaire complet, fiches des lois, glossaire français-anglais) suivront.

| Partie | Chapitres | Ce que vous saurez faire |
| --- | --- | --- |
| 2. Les outils | 5. Lois usuelles, dont les lois à queue lourde (Pareto, lognormale, Weibull) ; 6. Couples de variables, covariance, corrélation, sommes ; 7. Loi des grands nombres et TCL | Choisir une loi pour modéliser des sinistres, et comprendre pourquoi la mutualisation fonctionne |
| 3. Le risque | 8. Pourquoi mesurer le risque ; 9. VaR, TVaR, Expected Shortfall ; 10. Propriétés des mesures de risque ; 11. Modèle collectif fréquence × sévérité | Calculer une VaR et une TVaR à partir de F ou de S, en continu, en discret et sur données |
| 4. Aller plus loin | 12. Monte Carlo, estimation sur données, backtesting | Vérifier et valider une mesure de risque sur des données réelles |
| Annexes | Formulaire complet, fiche de chaque loi, glossaire français-anglais | Tout retrouver en un coup d'œil |

### Notebooks disponibles

| Fichier | Contenu |
| --- | --- |
| 00\_Partie0\_Rappels\_et\_Python.ipynb | SymPy, NumPy, SciPy, simulation, exercices 0.1 à 0.7 |
| 01\_Chapitre1\_Espace\_probabilise\_denombrement.ipynb | Dénombrement, audit de dossiers, anniversaires, exercices 1.1 à 1.5 |
| 02\_Chapitre2\_Conditionnement\_Bayes.ipynb | Test médical, bonus-malus, indépendance, exercices 2.1 à 2.5 |
| 03\_Chapitre3\_Variables\_aleatoires\_F\_S\_quantile.ipynb | Lecture graphique du quantile, inverse généralisé, perte mixte, exercices 3.1 à 3.5 |
| 04\_Chapitre4\_Esperance\_variance\_moments.ipynb | Prime pure, franchise, E\[X ∣ X > c\], exercices 4.1 à 4.6 |
| 05\_Partie2\_Lois\_couples\_theoremes\_limites.ipynb | Lois usuelles et queues, couples et covariance, portefeuille, LGN, TCL, mutualisation, exercices 5.1 à 7.4 |
| 06\_Partie3\_VaR\_TVaR\_modele\_collectif.ipynb | VaR et TVaR (continu, discret, données), queue lourde, sous-additivité, modèle collectif, Panjer, exercices 8.1 à 11.3 |
| 07\_Partie4\_Monte\_Carlo\_estimation\_backtesting.ipynb | Précision Monte Carlo et bootstrap, ajustement de lois, QQ-plot, backtesting (Kupiec, Bâle), étude de cas, exercices 12.1 à 14.2 |
| 08\_Banque\_exercices\_verification.ipynb | Vérification numérique des 40 exercices de la banque d'exercices |
| 09\_Cas\_pratiques.ipynb | Calculs et simulations des 5 cas pratiques complets |
| 10\_Cas\_pratiques\_6\_a\_10.ipynb | Calculs et simulations des cas pratiques 6 à 10 |
