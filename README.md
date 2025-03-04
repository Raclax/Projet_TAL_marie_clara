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

### Le prétraitement
Nous avons décidé d'appliquer du prétraitement sur la base de donnée avant de commencer l'analyse. 

**Création de la colonne 'text'**
Après analyse du dataset, nous en avons déduit que les colonnes les plus intéressentes pour une analyse textuelle serait les colonnes 'titre' et 'recette'. Nous es avons donc concaténées dans une seule colonne, 'text', pour pouvoir faciliter l'analyse. NOus avons décide de ne pas inclure la colonne 'ingrédients' d'abord car elle contient beaucoup de chiffres ce qui aurait pu poser problèmes sans être plus pour une analyse, et de plus les ingrédients sont tous re-cités dans la recette, docn ce n'est même pas très utile. 

**Stopwords et mots en minuscule**
Un autre élément pour faciliter l'analyse, nous avons également retiré les stop-words et chngé toutes les majuscules en minuscules. Grâce à cela, nous avons donc un text qui sera plus facile a analyser pour es différents modèles.

### Run1: baseline (méthode de référence)

- Le descripteur utilisé est une colonne 'text' qui est une concaténation des colonnes 'titre' et 'recette'. Nous avons jugés que c'étaient les colonnes les plus intéressantes pour mener une étude textuelle. C'est ce descripteur qui est utilisé pour tous les autres modèles également.

- Le classifieur utilisé est donc de prédire aléatoirement la classe de chaque élément lu dans la colonne 'text"

### Run2: Arbre de décision

Un arbre de décision est un modèle de prédiction qui utilise une structure arborescente pour prendre des décisions basées sur les valeurs des caractéristiques d'entrée. Chaque nœud interne représente une caractéristique, chaque branche représente une règle de décision, et chaque feuille représente un résultat.

### Run3: SVM  (Support Vector Machine)

Un SVM est un classifieur qui trouve l'hyperplan optimal séparant les différentes classes dans un espace de caractéristiques. Il utilise des vecteurs de support pour maximiser la marge entre les classes.

### Run4: Random forest

Une forêt aléatoire est un ensemble de nombreux arbres de décision entraînés sur des sous-ensembles aléatoires des données d'entraînement. Les prédictions sont faites en agrégeant les prédictions de tous les arbres. 

### Run5: Naive Bayes

Le classifieur Naive Bayes est basé sur le théorème de Bayes avec une hypothèse d'indépendance naïve entre les caractéristiques. Il calcule la probabilité qu'une instance appartienne à chaque classe et choisit la classe avec la probabilité la plus élevée.

## Résultats

| Run                | f1 Score |
| ------------------ | --------:|
| baseline           |   0.33   |
| Arbre de décision  |   0.77   |
| SVM                |   0.87   |
| Random forest      |   0.79   |
| Naive Bayes        |   0.69   |

### Analyse de résultats
	
#### Arbre de décision

Il n'y a aucun document avec un score de 0, et 72 documents entre 0.9 et 1.0. On compte 246 documents entre 0.5 et 0.6 mais l'interval le plus remplis est entre 0.7 et 0.8 (276 documents). Globalement le modèle a l'air assez sur de la façon dont il a identifié les différentes catégories, même si les résultats ne sont pas très bons pour les classes 'Plat principal' et 'Entrée'. 

#### SVM



#### Random forest



#### Naive Bayes




	Pistes d'analyse:
	* Combien de documents ont un score de 0 ? de 0.5 ? de 1 ? (Courbe ROC)
	* Y-a-t-il des régularités dans les document bien/mal classifiés ?
	* Où est-ce que l'approche se trompe ? (matrice de confusion)
	* Si votre méthode le permet: quels sont les descripteurs les plus décisifs ?
