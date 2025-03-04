# DEFT2013 Tâche 2 : Marie&Clara

POINT Marie - SACHOT Clara

## Description de la tâche

Le corpus donné contient 7 colonnes : 
- **doc_id** : un identifiant unique à chaque recette
- **titre** : le nom du plat
- **type** : si il s'agit d'une entre, d'un plat ou d'un dessert
- **difficulté** : classé sur une achelle de facile à difficile
- **cout** : classé entre bon marché, moyen et assez cher
-  **ingrédients** : la liste des ingrédients
- **recette** : la recette du plat


| doc_id             | titre | type | f1 difficulté | cout | f1 ingrédients |  recette | 
| ------------------ | ----- |------| --------------| -----| ---------------| -------- |
| recette_84191.xml     | Roulé à la confiture de lait | Dessert | Moyennement difficile | Bon marché | - Pour la garniture: - 1 boîte de lait concent... |    La veille, préparer de la confiture de lait en... |


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

Un arbre de décision est un modèle de prédiction qui utilise une structure arborescente pour prendre des décisions basées sur les valeurs des caractéristiques d'entrée. Chaque nœud interne représente une caractéristique, chaque branche représente une règle de décision, et chaque feuille représente un résultat. POur aviter le phénomène d'overfitting, nous avons décidé de fixer la profondeur maximale de l'arbre à 10.

### Run3: SVM  (Support Vector Machine)

Un SVM est un classifieur qui trouve l'hyperplan optimal séparant les différentes classes dans un espace de caractéristiques. Il utilise des vecteurs de support pour maximiser la marge entre les classes.

### Run4: Random forest

Une forêt aléatoire est un ensemble de nombreux arbres de décision entraînés sur des sous-ensembles aléatoires des données d'entraînement. Les prédictions sont faites en agrégeant les prédictions de tous les arbres. 

### Run5: Naive Bayes

Le classifieur Naive Bayes est basé sur le théorème de Bayes avec une hypothèse d'indépendance naïve entre les caractéristiques. Il calcule la probabilité qu'une instance appartienne à chaque classe et choisit la classe avec la probabilité la plus élevée. On peut noter que c'est celui avec le pire rappel pour les entrées. Incidement, la précision des plats est donc aussi très mauvaise. 

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

Il n'y a aucun document entre les scores de 0.0 et 0.4, et 689 documents entre 0.9 et 1.0, qui est l'interval le plus rempli. On compte 134 documents entre 0.4 et 0.5. On voit que bien que la profondeur soit de 10, l'arbre reste déja très confiant sur ses prédictions, avec une majorité des scores entre 0.9 et 1.0. Pour aurant, le modèle n'est pas si bon : il casse beaucoup d'entrées en plat et vice versa. 

#### SVM

Il n'y a aucun document entre les scores de 0.0 et 0.4, et seulement 8 enytre 0.4 et 0.5. La catégorie la plus remplie est entre 0.9 et 1.0, avec 834 documents. Le modèle a l'air très confiant confiant sur ses prédictions, puisque les probabilités entre 0.5 et 0.9 ne dépassent pas les 190/intervalles. Par ailleurs, on voit c=que ce modèle propose une bonne classification des recettes : il a une f-mesure pondérée de 0.87 et ne commet relativement que peu d'erreurs de classements entre les entrées et les plats. 

#### Random forest

Il n'y a aucun document entre les scores de 0.0 et 0.3, et 93 documents entre 0.9 et 1.0. L'interval le plus rempli est entre 0.8 et 0.9 avec 362 documents. Ici, le modèle a  l'air moyennement confiant sur la façon dont il a différencé les catégories, puisque les brobabilités sont assez bien réparties entre 0.5 et 0.9. Par ailleurs, les résultats ne sont pas très bons pour la classe 'Entrée', qui est beaucoup confondue avec la classe 'Plat principal'. 

#### Naive Bayes

Encore une fois, aucun document a un score inférieur à 0.3. Nous avons 32 documents entre 0.4 et 0.5, 79 documents entre 0.5 et 0.6. Le score est très évelé pour la grande majorité des documents : pour un score entre 0.9 et 1 nous avons 775 documents. La répartition est exponentielle. Le modèle à l'air très sur de ses prédictions mais il a prédit presque toutes les entrées en plat. 

#### Observations générales

Globalement, la catégorie dessert se différencie très bien des autres, et est très peu sujette à des erreurs. En revanche, sur presque tous les modèles (sauf SVM), les catégories entrées et plats sont très mal différenciées, et sont donc souvent mal classées. Cela vient surement du fait que les deux catégories présentent des plats salés avec beaucoup d'ingrédients en commun. On peut noter que c'est souvent les entres qui sont classées en plat, plu sque l'inverse. Cela peut etre lié au fait que le corpus contient plus de recettes de plats que d'entrées. 

On peut sans trop de difficultés estimer que le modèle qui fonctionne le mieux pour cette tâche est SVM. 
