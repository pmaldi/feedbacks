# Correction du MCD

**Rappel du projet :**

> Création d'une Web App plateforme e-commerce de Mug O'Clock.
> Un utilisateur peut créer sur son compte une ou plusieurs adresses de livraison.
> Il peut liker un produit ou plusieurs de la page de ce dernier.
> Il peut également passer une commande sur un produit.

**Pour mieux s'y retrouver, nous allons définir les entités suivantes :**

- Un Utilisateur :
  - A au moins une adresse de livraison.
  - Peut avoir plusieurs adresses.
  - Peut liker plusieurs produits.
  - Peut passer des commandes.
- Une Commande :
  - Ne peut appartenir qu'à un seul utilisateur.
  - Ne contient qu'un seul produit.
  - Peut être livrée à une seule adresse.
- Un Produit :
  - Peut être liké par plusieurs utilisateurs.
  - Peut appartenir à plusieurs commandes.
- Une Adresse :
  - Appartient à au moins un utilisateur.

**Correction des cardinalités :**

Entre `Adresse` et `Utilisateur`, tu as mis une cardinalité 1,1 entre `Adresse` et l'association `A` dans notre cas, une adresse peut appartenir à plusieurs personnes donc nous pouvons attendre une cardinalité de 1,n. Cependant, nous n'avons pas de précision si l'adresse est réellement unique, mais si nous avons 2 membres d'une même famille qui veulent commander, une cardinalité 1,1 sous-entend que nous ne pourrons pas avoir plusieurs personnes avec la même adresse.

Entre `Utilisateur` et `Commande`, nous sommes bons !

Entre `Commande` et `Produit`, du côté de `Commande`, nous avons une cardinalité de 1,1 et non 1,n. Si nous mettons 1,n, cela sous-entend que nous pouvons mettre plusieurs articles dans une commande, ce qui ne semble pas être le cas dans le projet.

Entre `Produit` et `Utilisateur`, tu as mis en place un `ManyToMany`, c'est top ! Effectivement, c'est le cas, en règle générale sur les `ManyToMany`, tu dois passer par une nouvelle entité afin de stocker les informations bidirectionnelles.

Il manque pour moi un lien entre la table `Commande` et `Adresse` pour valider la phrase "`Une commande peut être livrée qu'à une seule adresse`".

**Remarques :**

- À quoi correspond le `mug_type` dans la table `Produit` ? Il n'est dit nulle part que nous gérons plusieurs types de mug.

- Pour le `status` des commandes, j'aurais mis une nouvelle entité afin de gérer uniquement les informations.

- Le `amount` n'est pas pertinent comme mot, il vaut mieux mettre `price`.

- La table `Utilisateur` et/ou `Adresse` n'a pas de nom ou prénom. Il manque des informations importantes pour un bon fonctionnement.
