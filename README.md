# DEFT2013 Tâche 2 : Marie&Clara

POINT Marie - SACHOT Clara

## Description de la tâche

	1 ou 2 exemples de documents (avec leur identifiant)

## Statistiques corpus

### Répartition des documents

Les données avaient été divisées entre train et test au préalable : le CSV 'train' contient 12 473 lignes et le CSV 'test' 1 388.
Le fichier 'train' a été subdivisé en 'train' et 'validation'. Nous avons choisi de garder 80% de train et de mettre 20% de validation. Cela nous amène donc à un ensemble d'entrainement qui contient 5 172 et un ensemble de validation qui en contient 1293.

### Répartition : 

**Train :**
- Plat principal    : 46%
- Dessert           : 31%
- Entrée            : 23%

**Test :**
- Plat principal    : 46%
- Dessert           : 29%
- Entrée            : 25%

**Validation :**
- Plat principal    : 48%
- Dessert           : 29%
- Entrée            : 23%


## Méthodes proposées

### Run1: baseline (méthode de référence)

- Le descripteur utilisé est une colonne 'text' qui est une concaténation des colonnes 'titre' et 'recette'. Nous avons jugés que c'étaient les colonnes les plus intéressantes pour mener une étude textuelle. 

- Le classifieur utilisé est donc de prédire aléatoirement la classe de chaque élément lu dans la colonne 'text"

### Run2: Arbre de décision
### Run3: SVM
### Run4: Random forest
### Run5: Naive Bayes

## Résultats

| Run                | f1 Score |
| ------------------ | --------:|
| baseline           |   0.33   |
| Arbre de décision  |   0.77   |
| SVM                |   0.86   |
| Random forest      |   0.78   |
| Naive Bayes        |   0.67   |

### Analyse de résultats
	
	Pistes d'analyse:
	* Combien de documents ont un score de 0 ? de 0.5 ? de 1 ? (Courbe ROC)
	* Y-a-t-il des régularités dans les document bien/mal classifiés ?
	* Où est-ce que l'approche se trompe ? (matrice de confusion)
	* Si votre méthode le permet: quels sont les descripteurs les plus décisifs ?
