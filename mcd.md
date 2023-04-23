# Correction du MCD

**Rappel du projet :**

> Création d'une Web App plateforme e-commerce de mug o'clock.
> Un utilisateur peut créer sur son compte une ou plusieurs adresses de livraison,
> Il peut liker un produit ou plusieurs de la page de ce dernier,
> Il peut aussi bien sur, passer une commande sur un produit

**Pour mieux si retrouver nous allons définir les entités**

- Un Utilisateur
  - a au moins une adresse de livraison
  - peut avoir plusieurs adresses
  - peut liker des plusieurs produits
  - peut passer des commandes
- Une Commande

  - ne peut appartenir qu'à un seul utilisateur
  - ne contient qu'un seul produit
  - peut être livrer qu'a une seul adresse

- Un Produit

  - peut être like par plusieurs utilisateurs
  - peut appartenir à plusieurs commandes

- Une Adresse
  - appartient à au moins un utilisateur

**Correction des cardinalités**

Entre `Address` et `User`, tu as mis une cardinalité 1,1 entre `ADDRESS` et l'association `HAS` dans notre cas une adresse peux appartenir a plusieurs personnes donc nous ponvons attendre une cardinalité 1,n.
Cependant nous n'avons pas de précision si l'adresse est réellement unique, mais si nous avons 2 membres d'une même famille qui veut commander, une cardinalité 1,1 sous entend que nous ne pourront pas avoir plusieurs personnes avec la même adresse.

Entre `User` et `Order`, Nous sommes bon !

Entre Order et Product, coté Order nous sommes a 1,1 et non 1,N. Si on met 1,N ca sous entend que nous pouvons mettre plusieurs articles dans une commande. ca ne semble pas etre le cas dans le projet.

Entre `Product` et `User`, tu as mis en place un `ManyToMany` c'est top ! effectivement c'est le cas, en régle générale sur les `ManyToMany` tu dois passer par une nouvelle entité afin de stocker les informations bidirectionnel.

Il manque pour moi un lien entre la table `Order` et `Address` pour valider la phrase "`Une commande peut etre livrer qu'a une seul adresse`"

**Remarques**

- A quoi correspond le `mug_type` dans la table `Product`, il est dit nul part que nous gérons plusieurs type de mug.

- Pour le `status` des commandes, j'aurai mis une nouvelle entité afin de gerer uniquement les informations.

- Le `amount` n'est pas pertinent comme mot, vaut mieux mettre `price`.

- La table `User` et/ou `Address` n'a pas de nom ou prénom. Il manque des informations importante de bon fonctionnement.

# Correction du MCD

**Rappel du projet :**

> Création d'une Web App plateforme e-commerce de mug o'clock.
> Un utilisateur peut créer sur son compte une ou plusieurs adresses de livraison,
> Il peut liker un produit ou plusieurs de la page de ce dernier,
> Il peut aussi bien sur, passer une commande sur un produit

**Pour mieux si retrouver nous allons définir les entités**

- Un Utilisateur
  - a au moins une adresse de livraison
  - peut avoir plusieurs adresses
  - peut liker des plusieurs produits
  - peut passer des commandes
- Une Commande

  - ne peut appartenir qu'à un seul utilisateur
  - ne contient qu'un seul produit
  - peut être livrer qu'a une seul adresse

- Un Produit

  - peut être like par plusieurs utilisateurs
  - peut appartenir à plusieurs commandes

- Une Adresse
  - appartient à au moins un utilisateur

**Correction des cardinalités**

Entre `Address` et `User`, tu as mis une cardinalité 1,1 entre `ADDRESS` et l'association `HAS` dans notre cas une adresse peux appartenir a plusieurs personnes donc nous ponvons attendre une cardinalité 1,n.
Cependant nous n'avons pas de précision si l'adresse est réellement unique, mais si nous avons 2 membres d'une même famille qui veut commander, une cardinalité 1,1 sous entend que nous ne pourront pas avoir plusieurs personnes avec la même adresse.

Entre `User` et `Order`, Nous sommes bon !

Entre Order et Product, coté Order nous sommes a 1,1 et non 1,N. Si on met 1,N ca sous entend que nous pouvons mettre plusieurs articles dans une commande. ca ne semble pas etre le cas dans le projet.

Entre `Product` et `User`, tu as mis en place un `ManyToMany` c'est top ! effectivement c'est le cas, en régle générale sur les `ManyToMany` tu dois passer par une nouvelle entité afin de stocker les informations bidirectionnel.

Il manque pour moi un lien entre la table `Order` et `Address` pour valider la phrase "`Une commande peut etre livrer qu'a une seul adresse`"

**Remarques**

- A quoi correspond le `mug_type` dans la table `Product`, il est dit nul part que nous gérons plusieurs type de mug.

- Pour le `status` des commandes, j'aurai mis une nouvelle entité afin de gerer uniquement les informations.

- Le `amount` n'est pas pertinent comme mot, vaut mieux mettre `price`.

- La table `User` et/ou `Address` n'a pas de nom ou prénom. Il manque des informations importante de bon fonctionnement.
