# Feedback 3 - Apprenant 3 - Triple Triad Deck Builder

**Résumé :**

Pour ce projet, je pense qu'avec un peu plus de temps, tu aurais pu corriger tes erreurs. Dans l'ensemble, ce sont des problèmes d'implémentation et non de compréhension. Prends bien le temps de refaire cet exercice en t'aidant de la correction comme support. Essaye de comprendre pourquoi nous l'avons fait de cette façon et essaie de structurer ton développement.

Le fait d'avoir une erreur majeure à l'étape 1, mais que tu aies quand même essayé d'implémenter l'étape 2, me conforte dans l'idée que c'était peut-être de dernière minute. En tout cas, ne te décourage pas, tu vas y arriver !

| Étapes | Commentaires                                                                                                                                                                           |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Il y a un décalage entre 2 paramètres qui fait que l'étape est en erreur et non fonctionnelle. Dommage, l'implémentation est en bonne voie !                                           |
| 2      | Je pense que tu t'es perdu en cours de route avec l'ensemble des mots-clés "elements", "card", etc. Prends 5 minutes de pause pour relire tranquillement la consigne pour te rassurer. |
| 3      | L'implémentation est présente ! C'est bien, il ne reste plus qu'à approfondir tout ça et continuer l'exercice !                                                                        |

> **- Etape 1**

Aie ! Malheureusement ton code est en erreur, quand je click sur une carte.

![Erreur Etape 1](https://i.imgur.com/x4JOBwn.png)

Je vais t'expliquer pourquoi.

Dans ta fonction `cardPage`, tu récupères les informations depuis ta base de données à travers la méthode `getCard`. Jusque-là, il n'y a pas de problème. Cependant, après, tu essaies de faire un rendu de ta page `cardDetails` en passant le paramètre `oneCard`, donc la variable que tu veux récupérer dans ta vue, et la variable `oneCard`.

Dans ton cas, tu essaies de faire une itération sur la variable `card` afin d'afficher les informations. Il y a plusieurs problèmes à faire ça :

Premièrement, pour l'information de ton détail de carte, il ne peut y avoir qu'une seule carte. Ça n'a pas de sens de faire une itération si tu n'as qu'une seule carte. Donc actuellement, même si tu corriges la variable, tu auras une erreur te disant que tu ne peux pas itérer sur la variable `oneCard`, car ce n'est pas un tableau.

Maintenant, si on retire la boucle d'itération, et quand on transforme en `oneCard`, ton code fonctionne presque complètement. Il reste des petits soucis d'affichage comme la présence du suffixe `.jpg` pour l'image, mais au moins, nous avons les informations de la carte.

Dans la structure de ta vue, tu as mis en place une balise `<a>` qui te ramène sur la page où tu te trouves. Ça n'a pas de valeur ajoutée, donc tu peux t'en séparer.

Tu as également quelques coquilles de caractères comme `">`.

Je t'invite à regarder la correction du projet pour comprendre les erreurs que tu as faites. Mais rassure-toi ! Rien de dramatique !

> **- Etape 2**

Pour cette étape, nous souhaitions que tu implémentes une fonction de recherche qui récupère l'ensemble des cartes selon l'élément auquel elles appartiennent.

Pour cela, il fallait suivre les étapes suivantes :

- Créer une fonction dans le `dataMapper` qui a pour but de récupérer l'ensemble des cartes selon l'élément que l'on passe en paramètre. Dans ton cas, tu as créé la fonction `getCardByElement` qui fait cela, cependant la construction de ta requête avec `IS NOT NULL` ne fonctionnera pas pour récupérer les cartes qui n'ont pas d'élément. Il faudrait plutôt utiliser `IS NULL` pour cela.

- Créer un nouveau contrôleur et appeler la fonction que l'on a définie à l'étape précédente. N'oublie pas d'importer le fichier `dataMapper` afin de pouvoir utiliser la fonction `getCardByElement`.

```
const dataMapper = require('../dataMapper');

function cardsByElementController(req, res) {
  // Récupération de l'élément dans les paramètres de la requête
  const { element } = req.params;

  // Appel de la fonction getCardByElement du dataMapper
  dataMapper.getCardByElement(element, (error, cards) => {
    if (error) {
      console.error(error);
      res.sendStatus(500);
      return;
    }

    // Affichage de la vue cardsByElement avec les cartes récupérées
    res.render('cardsByElement', { cards });
  });
}

module.exports = cardsByElementController;
```

- Enfin, créer une vue avec une boucle `for` qui va itérer pour chaque carte qui se trouve dans le tableau `cards`. Dans ton cas, la structure HTML n'était pas correcte, mais la boucle `for` était présente avec une itération. Cependant, attention, nous voulons la liste des cartes selon l'élément recherché. Si tu laisses la boucle itérer avec `element.element`, tu vas toujours avoir la même valeur, à savoir le nom de l'élément que tu cherches.

Je t'invite à reprendre cette partie avec la correction pour bien comprendre le cheminement et maîtriser complètement cette fonctionnalité. Si tu as besoin d'un support complémentaire, n'hésite pas à revenir vers moi afin que nous puissions y aller étape par étape !

> **- Etape 3**

Tu as mis en place le package `express-session`, c'est bien ! Je te donne quand même deux informations importantes pour le futur :

- En règle générale, les `require` sont toujours placés en haut du fichier afin d'avoir directement toutes les informations concernant les imports.

- Le projet utilise dotenv, qui permet de mettre en variable d'environnement des données de déploiement, mais également des données sensibles comme ton secret qui est... pas courant ! L'utilisation de dotenv te permettra de sécuriser ton projet lors d'une publication sur Github, par exemple.

```
secret: 'shqhshjsqh suiuui siofioq',
```

devrait être remplacé par :

```
secret: process.env.SECRET,
```

Dans le fichier `index.js`, tu as écrit des lignes qui ne sont pas utiles pour le projet, comme par exemple :

```
app.use(mainController.cardPage);
app.use(mainController.notFound);
```

Généralement, lorsque tu utilises `app.use`, c'est que tu veux faire appel à un _middleware_. Dans ton cas, tu lui passes des routes, mais tu n'as pas besoin de le faire car tu les as déjà définies dans le fichier `router.js`.

Tu peux donc nettoyer ces deux lignes qui ne sont pas utiles pour ton projet.

Pour la suite de l'étape, je te conseille de reprendre l'exercice afin de pouvoir essayer de mettre les cartes dans la session. Profite de la correction afin de t'auto-corriger. Je pense que tu en es capable !
