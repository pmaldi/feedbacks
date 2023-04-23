# Feedback 3 - Apprenant 3 - Triple Triad Deck Builder

**Résumé :**

Pour ce projet je pense qu'avec un peu plus de temps tu aurais corrigé tes erreurs, dans l'ensemble ce sont des problèmes d'implémentation et non de compréhension prends bien le temps de refaire cet exercice en t'aidant de la correction comme support essaie de comprendre pourquoi nous l'avons fait comme ça et essaie de structurer ton développement.
Le fait d'avoir une erreur majeure à l'étape 1 mais que tu aies essayé d'implémenter malgré tout l'étape 2 me conforte dans l'idée que c'est peut-être de dernière minute. En tout cas ne te décourage pas tu vas y arriver !

| Etapes | Commentaires                                                                                                                                                                         |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1      | Il y'a un décalage entre 2 parametres qui font que l'étape est en erreur et non fonctionnel, dommage l'implémentation est en bonne voie !                                            |
| 2      | Je pense que tu t'es perdu en cours de route avec l'ensemble des mots clés "elements" "card" etc.. Prend 5 minutes de pause pour relire tranquillement la consigne pour te rassurer. |
| 3      | L'implémentation est présente ! C'est bien, plus qu'a approfondir tout ça et a continuer l'exercice !                                                                                |

> **- Etape 1**

Aie ! Malheureusement ton code est en erreur, quand je click sur une carte.

![enter image description here](https://i.imgur.com/x4JOBwn.png)

Je vais t'expliquer pourquoi.
Dans ta fonction `cardPage` tu récupères les informations depuis ta base de données à travers la méthode `getCard`. Jusque-là il y a pas de problème, cependant après tu essayes de faire un rendu de ta page `cardDetails` en passant le paramètre `oneCard` donc la variable que tu veux récupérer dans ta vue et la variable `oneCard`.

Dans ton cas tu essayes de faire une itération sur la variable `card` afin d'afficher les informations. Il y a plusieurs problèmes à faire ça :

Premièrement sur l'information de ton détail de carte il ne peut y avoir qu'une seule carte ça n'a pas de sens de faire une itération si tu as qu'une seule carte. Donc actuellement même si tu corriges la variable tu auras une erreur te disant que tu ne peux pas itérer sur la variable `oneCard` car ce n'est pas un tableau.

Maintenant si on retire la boucle d'itération, et quand on transforme en `oneCard`, ton code fonctionne presque complètement, il reste des petits soucis d'affichage comme la présence du suffixe `.jpg` pour l'image mais au moins nous avons les informations de la carte.

Dans la structure de ta vue, tu as mis en place une balise `<a>` qui te ramène sur la page où tu te trouves ça n'a pas de valeur ajoutée donc tu peux t'en séparer.

Tu as également quelques coquilles de caractère comme `">` .

Je t'invite a regarder la correction du projet pour comprendre les erreurs que tu as fait. Mais rassure toi ! Rien de dramatique !

> **- Etape 2**

Pour cette étape nous voulions que tu implémente une fonction de recherche qui récupère l'ensemble des cartes selon l'élément auquel il appartient.

Pour ça le cheminement qu'il fallait suivre c'était :

- créer une fonction dans le `dataMapper` qui a pour but de récupérer l'ensemble des cartes selon l'élément que l'on passe en paramètre (dans ton cas tu as créé la fonction `getCardByElement` qui fait ça, attention cependant la construction de ta requête avec `IS NOT NULL` ne pourra pas fonctionner pour récupérer les cartes qui n'ont pas d'éléments.

- créer un nouveau contrôleur et d'appeler la fonction que l'on a défini à l'étape d'avant, attention à bien importer le fichier `dataMapper` afin de pouvoir utiliser la fonction `getCardByElement`

      const dataMapper = require('../dataMapper');

- et enfin créer une vue avec une boucle for qui va itérer pour chaque carte qui se trouve dans le tableau élément. Dans ton cas la structure HTML en elle-même n'est pas bonne mais la boucle `for` est présente avec une itération, par contre attention nous voulons la liste des cartes si tu laisses la boucle itérée avec `element.element` tu vas avoir toujours la même valeur à savoir le nom de l'élément que tu veux.

Je te laisse reprendre cette partie avec un support la correction afin de bien comprendre le cheminement et que tu puisses le maîtriser complètement, si tu as besoin de support complémentaire n'hésite pas à revenir vers moi afin qu'on prenne le temps d'y aller étape par étape !

> **- Etape 3**

Tu as mis en place le package `express-session` c'est bien ! Je te donne quand même 2 informations importantes pour le futur,

- En régle générale, les `require` on les met toujours en haut de ton fichier afin d'avoir directement toutes les informations concernant les imports.

- Le projet utilise dotenv, qui permet de mettre en variable d'environnement des données de déploiement mais également des données sensibles comme ton secret qui est... pas courant !

Le faite d'utiliser ça, ca te permettra de sécuriser ton projet lors d'une publication sur Github par exemple.

    secret: 'shqhshjsqh suiuui siofioq',

par

    secret: process.env.SECRET,

Egalement dans le fichier `index.js` tu as fait des lignes qui ne servent pas dans le projet comme par exemple :

    app.use(mainController.cardPage);
    app.use(mainController.notFound);

Généralement quand tu fais un `app.use` c'est que tu veux faire appel à un _middleware_ toi dans ton cas tu lui passes des routes mais tu n'as pas besoin de le faire car tu les as déjà défini dans le fichier `router.js`.

Tu peux donc nettoyer ses deux lignes qui ne sont pas utiles pour ton projet.

Pour la suite de l'étape, je te conseille de reprendre l'exercice afin de pouvoir essayer de mettre les cartes dans la session profite de la correction afin de t'auto-corriger, je pense que tu en es capable !
