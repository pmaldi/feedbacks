# Feedback 4 - Apprenant 4 - Triple Triad Deck Builder

**Résumé :**

Le projet a été commencé mais quelques difficultés sont apparentes prends le temps de bien analyser les consignes et n'hésite pas à chercher les informations de tes implémentations.
Il est important de comprendre le fonctionnement ce que tu essaies de mettre en place j'espère que mes explications te seront utiles si ce n'est pas le cas n'hésite pas à me contacter ou à contacter un autre tuteur qui aura une autre approche peut-être à ton besoin.

Allez ! Tu vas y arriver !

| Etapes | Commentaires                                                                                                     |
| ------ | ---------------------------------------------------------------------------------------------------------------- |
| 1      | L'étape n'est pas finaliser mais le début de l'implémentation est pertinente malgré quelques erreurs, continue ! |
| 2      | /                                                                                                                |
| 3      | /                                                                                                                |

> **- Etape 1**

J'ai lancé ton projet mais malheureusement de mon côté il ne se passe rien. J'avais peur que le projet soit complètement vierge, mais finalement je vois qu'il y a quand même des éléments. Je vais donc te corriger sur les différentes parties que tu as réalisées.

Pour rappel, pour cette étape, nous voulions que lorsque tu cliques sur une carte, tu puisses accéder aux informations de la carte sélectionnée, telles que le niveau de la carte, les points de chaque direction et l'élément de la carte, qui est majeur pour l'ensemble du projet.

Je constate que dans le projet, tu as créé un contrôleur nommé `mainController`. À l'intérieur de celui-ci, une fonction `cardPage` est présente. Avant toute chose, je peux te dire que tu ne passes pas l'information "card" dans le render de cette fonction. Ainsi, quoi que tu fasses, rien ne s'affichera. Vu ce que tu as essayé de mettre, sache que ce que tu dois passer est la constante "card".

      res.render('card', {
        card
      });

Je vois que tu as créé une fonction `getCard` dans le `dataMapper`, c'est exactement ce qu'il fallait faire félicitations pour y avoir pensé ! Cependant dans cette fonction il y a quelques petits problèmes, la première c'est que dans ta query tu lui passe `$1`

    const queryResult = await client.query(
      `SELECT *
      FROM card
      WHERE card.id = $1;
      `);

Alors que tu ne renseignes à aucun moment la valeur que doit prendre `$1`, pour ça je te propose de remplacer cette valeur par un templating de l'id

      getCard: async (id) => {
        const targetId = Number(request.params.id);
        const queryResult = await client.query(
          `SELECT *
          FROM card
          WHERE card.id = ${targetId};
          `);
        if (resultQuery.rowCount === 1) {
          return queryResult.rows[0]
        }
        return null;
      },
    };

Tu peux également simplifier le bloc if :

    if (queryResult.rowCount  ===  1) {
        return  queryResult.rows[0];
    }

en :

    return queryResult.rows[0] || null;

Si `queryResult` ou `rows` génèrent une erreur, cela renverra `null` et ne générera pas d'erreur.

D'ailleurs, tu remarqueras qu'il y a un problème dans ton code entre les noms `queryResult` et `resultQuery`. Les deux doivent être identiques et ne doivent pas être deux variables complètement différentes. Par conséquent, `resultQuery` doit être remplacé par `queryResult`. J'ai également utilisé la constante `targetId` que tu avais commentée.

Une fois que cela est fait, tu peux modifier la partie `cardList.ejs` pour ajouter un lien qui te mènera au détail de chaque carte. Remplace la balise `<a href="#">` par `<a href="/card/<%= card.id %>">`.

En outre, en regardant ton fichier router.js, j'ai remarqué que tu ne passais pas non plus le paramètre id sur ta route card. Pour ce faire, il suffit d'ajouter 4 petits caractères :

    router.get('/card/:id', mainController.cardPage);

À partir de là, tu devrais pouvoir voir le détail de chaque carte. Cependant, je vois que dans ta vue `card.ejs`, tu fais une boucle `forEach` pour afficher les cartes. Cependant, après les modifications que nous avons apportées, nous ne passerons plus un tableau mais une seule carte avec l'ensemble des détails accessibles. Par conséquent, tu n'as pas besoin de faire une boucle `forEach`. Je te laisse corriger le code en conséquence. Si tu rencontres des difficultés, n'hésite pas à nous en informer !

> **- Etape 2 et Etape 3**

Je pense que tu as perdu beaucoup de temps sur la première partie et donc tu n'as pas eu le temps de continuer le projet si c'est le cas je t'encourage à reprendre le projet en espérant que mes explications ont débloqué ta situation et qui te le permettront d'avancer plus sur le projet, si ce n'est pas le cas n'hésite pas à m'expliquer ce que tu n'as pas compris dans les consignes que l'on t'a donné ça nous permettra de te débloquer et aussi d'améliorer nos explications pour le futur.
