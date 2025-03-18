# DEFT2013 Tâche 2 : Marie&Clara

POINT Marie - SACHOT Clara

## Description de la tâche

Le corpus donné contient 7 colonnes : 
- **doc_id** : un identifiant unique à chaque recette
- **titre** : le nom de la recette
- **type** : s'il s'agit d'une entrée, d'un plat ou d'un dessert
- **difficulté** : classé sur une échelle de facile à difficile
- **coût** : classé entre bon marché, moyen et assez cher
-  **ingrédients** : la liste des ingrédients avec leur quantité
- **recette** : la recette complète

| doc_id             | titre | type |  difficulté | coût |  ingrédients |  recette | 
| ------------------ | ----- |------| --------------| -----| ---------------| -------- |
| recette_84191.xml     | Roulé à la confiture de lait | Dessert | Moyennement difficile | Bon marché | - Pour la garniture: - 1 boîte de lait concent... |    La veille, préparer de la confiture de lait en... |


La tâche va consister à prédire le **type** de recette ('Entrée', 'Plat principal', 'Dessert'). 

## Statistiques corpus

### Répartition des documents

Les données avaient été divisées entre *train* et *test* au préalable : le CSV 'train' contient 12 473 lignes et le CSV 'test' 1 388.
Le fichier 'train' a été subdivisé en 'train' et 'validation'. Nous avons choisi de garder 80% de train et de mettre 20% de validation. Cela nous amène donc à un ensemble d'entraînement qui contient 5 172 recettes et un ensemble de validation qui en contient 1293.

### Répartition des classes dans chaque échantillon : 

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
Nous avons décidé d'appliquer un prétraitement sur la base de donnée avant de commencer l'analyse. 

**Création de la colonne 'text' :**

Après analyse du dataset, nous en avons déduit que les colonnes les plus intéressentes pour une analyse textuelle seraient les colonnes 'titre' et 'recette'. Nous les avons donc concaténées dans une seule colonne, 'text', pour pouvoir faciliter l'analyse. Nous avons décidé de ne pas inclure la colonne 'ingrédients' d'abord car les ingrédients sont tous re-cités dans la recette, elle contient beaucoup de chiffres ce qui aurait pu poser problèmes sans ajouter d'information pour une analyse.

**Stopwords et mots en minuscule :**

Un autre élément pour faciliter l'analyse, nous avons également retiré les stop-words et changé toutes les majuscules en minuscules. Grâce à cela, nous avons donc un texte qui sera plus facile a analyser pour les différents modèles.

### Les descripteurs

Pour ce travail, nous avons décidé de comparer deux descripteurs : TF-IDF et Word2Vec. TF-IDF est plutôt axé sur le compte de mots pour pondérer leur importance mais Word2Vec permet de capturer les relations sémentiques entre les mots d'une phrase. 

#### Run1: baseline (méthode de référence)

- Le descripteur utilisé est une colonne 'text' qui est une concaténation des colonnes 'titre' et 'recette'. Nous avons jugés que ces colonnes étaient les plus intéressantes pour mener une étude textuelle. C'est ce descripteur qui est utilisé pour tous les autres modèles également.

- Le classifieur utilisé est donc de prédire aléatoirement la classe de chaque élément lu dans la colonne 'text"

#### Run2: Arbre de décision

Un arbre de décision est un modèle de prédiction qui utilise une structure arborescente pour prendre des décisions basées sur les valeurs des caractéristiques d'entrée. Chaque nœud interne représente une caractéristique, chaque branche représente une règle de décision, et chaque feuille représente un résultat. Pour éviter le phénomène d'overfitting, nous avons décidé de fixer la profondeur maximale de l'arbre à 10.

#### Run3: SVM  (Support Vector Machine)

Un SVM est un classifieur qui trouve l'hyperplan optimal séparant les différentes classes dans un espace de caractéristiques. Il utilise des vecteurs de support pour maximiser la marge entre les classes.

#### Run4: Random forest

Une forêt aléatoire est un ensemble de nombreux arbres de décision entraînés sur des sous-ensembles aléatoires des données d'entraînement. Les prédictions sont faites en agrégeant les prédictions de tous les arbres. 

#### Run5: Naive Bayes

Le classifieur Naive Bayes est basé sur le théorème de Bayes avec une hypothèse d'indépendance naïve entre les caractéristiques. Il calcule la probabilité qu'une instance appartienne à chaque classe et choisit la classe avec la probabilité la plus élevée.

## Résultats

| Run                | f1 Score |
| ------------------ | --------:|
| baseline           |   0.33   |


**TF-IDF**

| Run                | f1 Score |
| ------------------ | --------:|
| Arbre de décision  |   0.77   |
| SVM                |   0.87   |
| Random forest      |   0.79   |
| Naive Bayes        |   0.69   |


**Word2Vec**

| Run                | f1 Score |
| ------------------ | --------:|
| Arbre de décision  |   0.72   |
| SVM                |   0.81   |
| Random forest      |   0.79   |
| Naive Bayes        |   0.64   |




## Analyse de résultats

### Modèles
	
Pour ce qui s'agit de la comparaison des modèles, nous allon la baser sur les résultats obtenus avec TF-IDF, car c'est le descripteur qui donne les meilleurs f-mesures et donc les meilleurs résultats. 

**Arbre de décision**

Il n'y a aucun document entre les scores de 0.0 et 0.4, et 689 documents entre 0.9 et 1.0, qui est l'intervalle le plus rempli. On compte 134 documents entre 0.4 et 0.5. Bien que la profondeur soit de 10, l'arbre reste déja très confiant sur ses prédictions, avec une majorité des scores entre 0.9 et 1.0. Pour autant, le modèle n'est pas si bon : il classe beaucoup d'entrées en plat et vice versa. 

**SVM**

Il n'y a aucun document entre les scores de 0.0 et 0.4, et seulement 8 entre 0.4 et 0.5. La catégorie la plus remplie est entre 0.9 et 1.0, avec 834 documents. Le modèle a l'air très confiant sur ses prédictions, puisque les probabilités entre 0.5 et 0.9 ne dépassent pas les intervalles 0.9 à 1. Par ailleurs, on voit que ce modèle propose une bonne classification des recettes : il a une f-mesure pondérée de 0.87 et ne commet relativement que peu d'erreurs de classements entre les entrées et les plats. En revanche, avec TF-IDF comme avec Word2Vec, ce modèle est le plus long à lancer.
**Random forest**

Il n'y a aucun document entre les scores de 0.0 et 0.3, et 93 documents entre 0.9 et 1.0. L'intervalle le plus rempli est entre 0.8 et 0.9 avec 362 documents. Ici, le modèle a  l'air moyennement confiant sur la façon dont il a différencé les catégories, puisque les probabilités sont assez bien réparties entre 0.5 et 0.9. Par ailleurs, les résultats ne sont pas très bons pour la classe 'Entrée', qui est beaucoup confondue avec la classe 'Plat principal'. 

**Naive Bayes**

Encore une fois, aucun document a un score inférieur à 0.3. Nous avons 32 documents entre 0.4 et 0.5 et 79 documents entre 0.5 et 0.6. Le score est très évelé pour la grande majorité des documents : pour un score entre 0.9 et 1 nous avons 775 documents. La répartition est exponentielle. Le modèle à l'air très sûr de ses prédictions mais il a prédit presque toutes les entrées en plat. On peut noter que c'est celui avec le pire rappel pour les entrées. Incidement, la précision des plats est donc aussi très mauvaise. 



### Descripteurs

Les comparaisons des f-mesures montre bien que TF-IDF donne de meilleurs résultats que Word2Vec : à part avec random forest ou les valeurs sont les mêmes, pour les mêmes modèles les f-mesures seront meilleurs avec TF-IDF qu'avec Word2Vec. 

Globalement, lorsqu'on observe les matrices de confusion et les courbes ROC, on observe les mêmes tedendences d'erreurs entre les deux descripteurs, c'est à dire à confondre entrée et plat, avec principalement la classification de d'entrées en plats. Les proportions d'erreurs sont assez similaires même si, comme l'ndique la f-mesure, elle est souvent un peu plus importante avec TF-IDF. 

### Observations générales

Globalement, la catégorie dessert se différencie très bien des autres, et est très peu sujette à des erreurs. En revanche, sur presque tous les modèles (sauf SVM), les catégories entrées et plats sont très mal différenciées, et sont donc souvent mal classées. Cela vient sûrement du fait que les deux catégories présentent des plats salés avec beaucoup d'ingrédients en commun. On peut aussi noter que c'est souvent les entrées qui sont classées en plat, plutôt que l'inverse. Cela peut être lié au fait que le corpus contient plus de recettes de plats que d'entrées (environ deux fois plus de plats).

On peut sans trop de difficultés estimer que le modèle qui fonctionne le mieux pour cette tâche est SVM. On peut quand même nuancer ces résultats avec le fait que ce soit aussi el modèle le plus long à faire tourner. 

Pour ce qui est des descripteurs, c'est avec IT-IFD qu'on obtient les meilleurs résultats. Cela est surement lié au fait que notre jeu de données d'entrainement ne compte que 13 000 lignes, ce qui n'est pas assez pour word2vec pour bien apprendre les relations entre les mots. 
