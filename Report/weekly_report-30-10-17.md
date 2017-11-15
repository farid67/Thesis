# Weekly report 30/10/2017 :

## Réunion IHU Vincent 30/10:

Rencontre avec un fellow côté IHU pour un test de récupération de données sur les serveurs IHU.

Le logiciel présente un suivi patient depuis sa prise en charge avec les différents examens (Imagerie, biopsie, chimio...).

Certains patients semblent correspondre à nos attentes (aucuns traitements avant la première acquisition d'images, càd pas de chimio notamment)

Problème concernant la récupération des images : sur le logiciel utilisé pour visionner les données, elles semblaient disponibles uniquement une par une, alors qu'il devrait être possible de récupérer directement des séries Dicom (voir avec le fellow).

# Travail personnel:

### Arbres de décision :

Arbres dont les branches correspondent aux séquences les plus courtes de variables explicatives permettant de déterminer une sortie.

Nœud de l'arbre : état d'une variable explicative.

Feuille : Valeur de sortie.

Ils sont généralement créés en divisant successivement l'ensemble des données d'entraînement.

Un des algorithmes classiquement utilisé pour la création de l'arbre est l'**ID3** (pour Iterative Dichotomiser 3)

Exemple d'utilisation : Déterminer si un animal est un chat ou un chien avec comme données uniquement : sa nourriture favorite (croquettes pour chat, croquettes pour chien, bâcon), le fait qu'il soit grincheux? (oui ou non).

Pour cet exemple, on se limitera à un arbre de décision binaire, avec deux fils à chaque noeuds qui ne seront que "oui" ou "non". Il faut donc séparer la variable explicatives "nourriture favorite" en 3, (aime manger de la nourriture pour chat?, aime manger du bâcon?, etc)

Sélection des différentes questions : la meilleure question est celle qui partitionne le mieux les différentes classes, il faut donc utiliser l'**entropie** qui est grosso-modo **une mesure de l'incertude**, et qui se calcule avec la formule suivante :

$$H(X) = -\sum^{n}_{i=1}P(x_i)\log_b(P(xi))$$

Pour le lancer d'une pièce par exemple, l'entropie vaudra 1.0 

L'unité de cette valeur est le bit, elle correspondra au nombre de bits nécessaire pour encoder l'ensemble des états possibles (1 bit = 2 valeurs : pile ou face).

_Choix du nœud suivant_: celui qui diminue le plus l'entropie, quantifiable à l'aide de plusieurs métriques : 

​	**Le gain d'information**

​		$$IG(T,a) = H(T) - \sum_{v\in vals(a)} \frac{\left\lVert x\in T | x_a = v\right \lVert} {\left\lVert T\right\lVert} H({x\in T | x_a=v})$$

​		avec $H(t)$ l'entropie du nœud parent, $T$ l'ensemble des instances, et $a$ la variable que l'on teste

​	**Coefficient de Gini (Gini impurty)**

​		Permet de mesurer la proportion de classes dans un ensemble.

​		$$Gini(T) = 1 - \sum^j_{i=1} P(i|T)^2$$

​		avec $T$ l'ensemble des instances pour le nœud courant, $j$ le nombre de classes et $P(i|t)$ la probabilité de choisir un élement de la classe $i$ dans le sous-ensembe d'instances.

​		Coefficient nul quand toutes les instances d'un sous-ensemble appartiennent à la même classe, augmente quand les classes sont réparties aléatoirement.

**Apprentissage ensembliste**: Méthode qui combine un ensemble de modèle pour produire un estimateur qui a une meilleure performance prédictive que les composantes individuelles séparées.

Une **forêt aléatoire (random forest)** est un ensemble d'arbres de décision entraînés sur des sous-ensembles choisis aléatoirement. Le résultat est généralement obtenu en combinant la moyenne des prédictions sur les différents arbres. L'utilisation d'une forêt aléatoire prévient généralement du sur-apprentissage.

_Avantages et inconvénient des arbres de décision :_ 

* Facile à utiliser : ne réquière pas de normalisation  (moyenne à zéro ou variance unitaire), tolère les données manquantes dans les variables explicatives.
* **Plus sensibles au surapprentissage que les autres modèles** => utilisation de techniques comme le pruning (qui supprimes les nœuds les plus lourds ainsi que les feuilles), ou encore en limitant la profondeur de l'arbre ou en autorisant la création de fils uniquement quand le nombre d'instances dépasse un certain seuil..
* Les algorithmes efficaces de création d'abres de décision comme ID3 sont _gloutons_: ils apprennent efficacement en faisant des optimisations locales mais **ne garantissent pas d'avoir un arbre globalement optimal**.



### Clustering avec K-means:

Rappel : Clustering = type d'apprentissage non supervisé pour lequel le but est de trouver des groupes d'observations similaires dans un ensemble de données non-étiquettées.

#### K-means :

Processus itératif de déplacement du centres des clusters appelés **centroïds**.

**K** nombre de clusters que l'on veut découvrir, l'algorithme ne peut pas trouver le K en question.

Les paramètres à optimiser sont :

* la position des centroïds 
* les observations attribuées à chaque cluster

Fonction de coût **J** :

$$J = \sum^K_{k=1}\sum_{i\in C_k} \left\lVert x_i - \mu_k\right\lVert ^2$$ , avec $\mu_k$ étant la position du centroïd pour le cluster $k$

La position initiale du centroïd est choisie aléatoirement, une des méthodes consiste à prendre une instance aléatoire par centroïd initial.

**Optimum local** : l'algorithme peut atteindre un optimum local en fonction des centroïds initialement choisis.

**Méthode de elbow** : Méthode permettant de déterminer le nombre optimal **K** de clusters (en fonction d'un rapport entre le coup et la distortion moyenne du système, autrement dit de la distance inter-classe)

**Silouhette coefficient** : Mesure de la concentration et de la séparation des clusters, calculée par instance : 

​	$$s = \frac{ba}{\max(a,b)}$$, avec $a$ = distance moyenne entre les instances du cluster et $b$ la distance moyenne entre l'instance et le instances du cluster le plus proche.

​	Valeur élevée = bonne qualité lors de la création des clusters.

Exemple d'utilisation de K-means : 

* Lors de la compression d'une image (pour la réduction du nombre de bits utilisés pour coder la couleur)



