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

J'ai lancé ton projet mais malheureusement de mon côté il se passe rien j'avais peur que le projet soit complètement vierge mais au final je vois qu'il y a quand même des choses donc je vais corriger des différentes parties que tu as fait.

Pour rappel pour cette étape nous voulions que quand tu cliques sur une carte on accède aux informations de la carte que tu as sélectionné avec les informations telles que le niveau de la carte et les points de chaque direction sans oublier l'élément de la carte qui est majeur pour l'ensemble du projet.

Je vois que dans le projet tu as créé un contrôleur `mainController`, ou à l'intérieur de celui-ci une fonction `cardPage` est présente, avant toute chose je peux te dire que tu ne passes pas l'information card dans le render de cette fonction donc quoi qu'il arrive il ne s'affichera rien, vu ce que tu as essayé de mettre sache que ce que tu dois passer c'est la constante card

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

Si, `queryResult` ou `rows` est en erreur, alors ca renverra `null` et ne génèrera pas d'erreur.

D'ailleurs tu remarqueras qu'il y a un souci dans ton code entre le mot `queryResult` et `resultQuery`, les deux sont censés être les mêmes et non pas de variables complètement différentes donc `resultQuery` devrait être remplacé par `queryResult` et j'ai également utiliser la constante `targetId` que tu avais commenter.

Une fois que ça c'est fait tu peux modifier tu as vu afin de rajouter le lien qui t'amènera au détail de chaque carte pour ça c'est dans la partie `cardList.ejs`

Tu peux remplacer la balise `<a href="#">` par `<a href="/card/<%= card.id %>">`

Ah et puis en regardant ton router.js j'ai remarquer que tu ne lui passé pas non plus le parametre id sur ta route card.
Pour ce faire 4 petits caractère a rajouter !

    router.get('/card/:id', mainController.cardPage);

À partir de tout ça tu devrais voir le détail de ta carte, attention cependant je vois que dans ta vue card.ejs que tu fais un forEach de cards.
Ensuite aux modifications que nous avons fait nous n'allons pas lui passer de tableau mais une seule carte avec l'ensemble des détails accessible tu n'as donc pas besoin de faire une itération je te laisse corriger le code ou le continuer par rapport aux remarques que je t'ai fait si tu es en difficulté n'hésite pas à nous prévenir !

> **- Etape 2 et Etape 3**

Je pense que tu as perdu beaucoup de temps sur la première partie et donc tu n'as pas eu le temps de continuer le projet si c'est le cas je t'encourage à reprendre le projet en espérant que mes explications ont débloqué ta situation et qui te le permettront d'avancer plus sur le projet, si ce n'est pas le cas n'hésite pas à m'expliquer ce que tu n'as pas compris dans les consignes que l'on t'a donné ça nous permettra de te débloquer et aussi d'améliorer nos explications pour le futur.
