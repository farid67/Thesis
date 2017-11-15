# PCA + Réduction de dimensionalité

## PCA :

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

  _Attetion_ : il est important de normaliser les vecteurs 

  Chacun des vecteurs propres est appelée **composante principale** de la matrice.

  Le vecteur propre avec la valeur propre associée la plus grande sera la première composante principale 



## Réduction de dimensionalité :

Une fois les vecteurs propres - valeurs propres trouvés, il est possible de tranformer les données pour en réduire leur dimensionalité. 

Pour cela on choisit $p$ vecteurs propres (avec les valeurs propres associées les plus élevées) qui nous serviront à exprimer le dataset. 

Les données obtenues ne seront plus exprimées qu'en $p$-dimensions.

Notion de $FeatureVector = (eig_1, eig_2,\dots, eig_n)$, qui est une matrice de vecteurs propres, **triés en fonction de leur valeur propre associée**.

La transformation de données se fait à l'aide de la formule suivante : 

$$FinalData  = FeatureVector^\intercal \times (X_{adj})^\intercal$$ , à noter qu'on utilise la transposée de $FeatureVector$ pour avoir une dimension par ligne.

Pour réduire la dimensionnalité des données, il suffit de n'utiliser qu'une partie des vecteurs propres (de $eig_1$ à $eig_p$), les données seront donc obtenues dans un sous-espace de dimension $p$.