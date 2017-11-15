# Weekly report 23/10 :

## Réunion 23/10 avec Vincent:

* Contour administratif : remplissage de la convention de formation
* Radial : possiblité d'avoir de nouveaux foies directement à Strasbourg
* Discussion à propos de l'anapath

## Pré-requis école doctorale :

### Formation scientifique :

Sur toute la durée de la thèse :

* 2 enseignements de niveau master \(40 à 50h\); N'importe quel cours de M2 ou un cours de M1 dans la formation d'origine
* 6 conférences chaque année; 4 au moins organisées par l'école doctorale

### Formation transversale :

_"Communication skills in Scientific English"_ \(obligatoire en deuxième année\) + 1 ou plusieurs modules \(20 à 25h\) dont _"Charte de déontologie des métiers de la recherche"_ \(obligatoire\)

## Travail personnel :

### Méthodes de minimisation de coût : Descente de gradient

* _Batch gradient descent_ : Utilise l'ensemble des données de test à chaque itération pour mettre à jour le modèle
* _Stochastic gradient descent_ : n'utilise qu'une instance choisie aléatoirement

### Bag of words :

Le "texte" est souvent utilisé comme variable explicative dans les modèles, les bags of words sont des multisets qui encodent les mots apparaissant dans un texte.

But: classification de documents, l'idée de base est que les documents contenant les même mots sont souvent similaires.

Vocabulaire :
* Corpus : ensemble de documents
* Vocabulaire : liste de mots contenus dans les documents du corpus
* Dimension des vecteurs de caractéristiques : Taille du vocabulaire

Comparaison de deux documents : Calcul de la distance (souvent distance Euclidienne, appelée aussi _**norme L2**_)

Problèmes dans la classification de documents : notion de _**Sparse vector**_ (vecteurs creux : avec beaucoup de zéros), il faut beaucoup de données d'entrainement pour avoir un modèle suffisamment généralisable : _**Hughes effect**_.

Réduction de dimensionalité : Ne pas considérer la casse, supprimer les mots simples (notion de _**stop words**_) et le mots propre au corpus ( _**corpus-specified stop words**_), regrouper les différentes versions d'un même mot (stemming & lemmatization)...

### Standardisation des données :

Idéalement : Une moyenne à zéro ( _**zero mean**_ ) et une variance unitaire ( _**unit variance**_, un vecteur de caractéristiques a une variance unitaire quand la variance des caractéristiques est dans le même ordre de magnitude, par exemple on considère un modèle avec deux variables explicatives, la première aura des valeurs entre 0 et 1, la seconde entre 0 et 100.000, une mise à l'échelle devra être faite pur qu'elles soient entre 0 et 1 -> dans le cas contraire, la seconde variable pourrait avoir plus d'influence sur le modèle que la première dans certains cas)

### Régression logistique :

La régression logistique est un type de régression qui est utilisée en clasification,

On essaye, à partir de différents variables explicatives de déterminer la classe de sortie.

La variable de sortie n'est pas directement une valeur comme en régression linéaire, mais il s'agit d'une probabilité d'appartenir à une certaine classe.

On utilise pour cela une fonction logistique :

$$
F(t) = \frac{1}{1+exp(-t)}
$$

cette fonction donne une valeur entre 0 et 1 et elle décrit une sigmoïde.

Le terme t est alors une combinaison linéaire des différentes variables explicatives, dans le cas d'une seule variable on aura 

$$
t = B_0 + (B_1 * x)
$$

avec x étant la variable explicative et $B_0$, $B_1$ les différents poids minimisant l'erreur

### Métriques de performance :

* La mesure F1 (appelé aussi _**F-score**_ ) est une combinaison des scores de précision et de recall (moyenne pondérée des scores de precision et de recall)

  $$
  F1 = 2 * \frac{PR}{P+R}
  $$

  Cette mesure pénalise les classifieurs avec des scores de précision et de recall désequilibrés (comme c'est le cas pour un classifieur qui prédit toujours la classe positive)

* ROC, AUC : Notion de _**fall-out**_ (ou taux de faux-positifs), $F = \frac{FP}{VN + FP}$, _**Courbe ROC**_ = recall en fonction du fall-out. L' _**AUC**_ représente l'aire sous la courbe -> réduit la coubre ROC à une seule variable


### Grid Search :


_**Hyperparamètres**_ : paramètres du modèle qui ne sont pas appris, par exemple, dans un classifieur de spams, les hyperparamètres sont les paramètres de régularisation, ou encore les seuils utilisés pour supprimer du dictionnaire les mots qui apparaissent trop ou pas assez souvent.

_**Grid Search**_ = méthodes qui simule,t les différents modèles possibles en modifiant ces hyperparamètres et qui renvoient la meuilleure des combinaisons possibles -> Couteux mais très bien parallélisable

### Classification multi-classe :

Le but est de prédire une classe parmi plusieurs pour une instance donnée, par exemple déterminer la note d'un film (de 0:très mauvais à 4:très bon) en fonction de sa critique.

Stratégie _**one-vs-all**_  : Un classifieur binaire pour chaque classe possible.

Métriques de performance : comme pour la classification binaire, la précision, le recall et le score F1 peuvent être calculés pour chaque classe, ainsi que l'accuracy pour toutes les prédictions


### Classification multi-label :

Le but de ce type de classification est de déterminer à quel sous-ensemble de classes une instance appartient.

Par exemple, on cherche à déterminer les tags d'un message posté sur un forum, ou encore on cherche à déterminer les objets présents sur une image.

Une des méthodes consiste en la tranformation du problème, qui existe sous deux formes, que nous allons illustrer sur un classifieur d'article de journaux (déterminer les catégories auxquelles appartient l'article):

* La première méthode consiste à remplacer les sorties des différentes instances du jeu d'entrainement en **combinant les classes de sorties** :

    Exemple : On a dans le jeu d'entrainement les différentes classes possibles : "Local" , "France" et "Sport", un article ayant était classé comme relatant des catégories "Local" et "France" n'aura plus que l'étiquette "Local ET France" (_ET_ logique).

    -> On transforme donc le problème en une classification multi-classe (avec label unique).

    **PROBLEMES** : L'un des problèmes de cette méthode est que l'augmentation du nombre de classe rend la complexité ingérable, l'autre est qu'il ne sera possible d'obtenir que les différentes combinaisons se trouvant dans le jeu d'entrainement.

* La seconde méthode consiste à créer un modèle de **classification binaire pour chacune des classes dans l'ensemble des possibles** : 

  Au lieu d'avoir les classes "Local", "France" et "Sport", on aura 3 classifieurs: un déteminera si l'article parle du thème "Local" ou "non Local", etc 

  La solution sera une union des 3 classifieurs.

  **PROBLEMES** : Le principal problème avec cette méthode est qu'elle ne prend pas en compte la relation inter-classe lors de la classification. 

Métriques de performances : _**Hamming loss**_ (fraction moyenne des labels incorrects) _**Jaccard similarity**_ (taille de l'intersection entre les labels prédis et la vérité, divisiée par la taille de l'union ente les labels prédis et la vérité)



