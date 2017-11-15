# Weekly report 06/11/2017:

## Travail personnel:

###PCA:

Le but de la **PCA** (Principal Component analysis) est de visualiser un ensemble de données étant dans un repère à grande dimension ($n$) dans un repère de moindre dimension ($p$).

On note pour la suite $X$ l'ensemble des données dans un espace à $n$-dimensions.

Pour cela il est nécessaire d'effectuer plusieurs traitements :

* Ajustement des données :

  Cela est rendu possible en ayant une moyenne à zéro pour chacune des dimensions dans l'ensemble données :

  Rappel : $$X_{adj} = \{x_{i,j} - \overline{x_i}\ | i,j  \in \{0,\dots,n\}\}$$ : On soustrait la moyenne (sur chaque dimension) à chaque valeur de l'ensemble de départ.

* Calcul de la matrice de covariance, notée $A$

  Rappel : $$Cov(x,y) = \frac{\sum_{i=1}^n{(x_i-\overline{x}) (y_i - \overline{y})}}{n-1}$$

* Calcul des **vecteurs propres** (*eigenvectors*), **valeurs propres** (*eigenvalues*) de la matrice de covariance $A$

  Pour cela, il est nécessaire de rappeler qu'un couple vecteur propre - valeur propre respecte la relation suivante avec une matrice M donnée (nécessairement **carrée**) :

  $$M\vec{v} = \lambda\vec{v}$$, avec $$\vec{v}$$ un vecteur propre, et $$\lambda$$ sa valeur propre associée.

  _Attention_ : il est important de normaliser les vecteurs.

  Chacun des vecteurs propres est appelée **composante principale** de la matrice.

  Le vecteur propre avec la valeur propre associée la plus grande sera la première composante principale.


###Réduction de dimensionalité:

Une fois les vecteurs propres - valeurs propres trouvés, il est possible de tranformer les données pour en réduire leur dimensionalité. 

Pour cela on choisit $p$ vecteurs propres (avec les valeurs propres associées les plus élevées) qui nous serviront à exprimer le dataset. 

Les données obtenues ne seront plus exprimées qu'en $p$-dimensions.

Notion de $FeatureVector = (eig_1, eig_2,\dots, eig_n)$, qui est une matrice de vecteurs propres, **triés en fonction de leur valeur propre associée**.

Pour réduire la dimensionnalité des données, il suffit de n'utiliser qu'une partie des vecteurs propres (de $eig_1$ à $eig_p$), les données seront donc obtenues dans un sous-espace de dimension $p$.

On note alors $$FeatureVectorReducted = (eig_1, eig_2,\dots, eig_p)$$, les vecteurs propres qui seront utilisés pour la réduction de dimensionalité.

La transformation de données se fait à l'aide de la formule suivante : 

$$FinalData  = FeatureVectorReducted^\intercal \times (X_{adj})^\intercal$$ , à noter qu'on utilise la transposée de $FeatureVector$ pour avoir une dimension par ligne.

### Sklearn:

Un exemple brut de calcul de la PCA a été effectué sans l'aide de _sklearn_ pour comparer les résultats avec ceux donnés obtenus par la librairie.

D'autres tests ont été effectués à l'aide de $sklearn$ pour la classification du modèle connu d'Iris.

Dans la base de donnée d'Iris, les fleurs sont déterminés par 4 caractéristiques, et sont classées en 3 catégories de fleurs différentes.

La base a été divisée en 2 parties (60% pour l'entraînement et 40% pour les tests).

Pour commencer j'ai analysé la matrice de covariance de l'ensemble ainsi que ses vecteurs propres - valeurs propres pour ne garder qu'un seul vecteur propre qui avait une valeur propre associée significativement plus  importante que les autres. Le sous-espace correspondant n'était plus alors qu'une ligne ($1d$).

Une projection a été effectuée sur ce sous-espace (pour ne plus avoir que des points).

La solution la plus simple pour la classification est de déterminer trois "centres" (un par catégorie) en moyennant les différents points pour chaque catégorie.

Pour tester une nouvelle donnée, on commence par la projeter sur la droite avant de calculer la distance avec chaque centre afin de déterminer le centre le plus proche, et donc la classe d'appartenance.

Les résultats obtenus avec cette méthode sont plus que correct :

|             | précision | recall | f1-score | support |
| ----------- | :-------: | :----: | :------: | :-----: |
| 1           |   1.00    |  1.00  |   1.00   |   33    |
| 2           |   0.89    |  0.94  |   0.91   |   34    |
| 3           |   0.93    |  0.86  |   0.89   |   29    |
| avg / total |   0.94    |  0.94  |   0.94   |   96    |



L'autre solution que j'avais envisagé été de garder la PCA pour la réduction de dimensionalité mais d'effectuer la classification à l'aide d'une regression logistique (sur les données "réduites").

Cette nouvelle méthode _PCA-regression logistique_ donna des résultats un peu moins bons que ceux obtenus avec la méthode précédente, les voici : 

|             | précision | recall | f1-score | support |
| ----------- | :-------: | :----: | :------: | :-----: |
| 1           |   0.92    |  1.00  |   0.96   |   33    |
| 2           |   1.00    |  0.59  |   0.74   |   34    |
| 3           |   0.72    |  1.00  |   0.84   |   29    |
| avg / total |   0.89    |  0.85  |   0.85   |   96    |



Cependant lorsque la regression logistique est effectué sur des données non réduites (pas de PCA), les résultats semblent un peu meilleurs.

La seule hypothèse qui semble capable d'expliquer ces résultats est le fait que les données ne soient pas très conséquentes (150 éléments uniquement dans la base de données) : lorsque les tests sont effectués à plusieurs reprises parfois les scores de la méthode _PCA-regression logistique_ sont meilleurs que la méthode _regression logisitique seule_ ou que la méthode _PCA-moyenne des points_ ... 