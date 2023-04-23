# Feedback 2 - Apprenant 2 - Triple Triad Deck Builder

**Résumé :**

Le projet répond dans l'ensemble à la demande ! Bravo pour ça ! Cependant, je te conseille de prendre le temps de relire ton code afin d'éviter des coquilles. Tu peux également commencer à te renseigner sur l'optimisation de code et les packages présents dans le projet comme `express-session` ou `dotenv`.

| Étapes | Commentaires                                                                                                                                    |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | L'implémentation est bonne, mais nécessite plus de vérifications au niveau des retours de la base de données.                                   |
| 2      | Ta méthode `getSearchResults` est correctement appelée, mais il manque un cas fonctionnel : celui du "Et si je veux les cartes sans éléments ?" |
| 3      | La présence de `express-session` est top ! Mais, par manque de temps, des erreurs sont présentes.                                               |

> **- Etape 1**

Pour cette feature, nous t'avons demandé de créer un moyen de voir les détails de la carte sélectionnée. Visuellement, nous t'avons demandé la disposition de ton choix, mais le but était de te faire travailler la récupération d'informations via une base de données et de passer des éléments dynamiques plutôt que le style en lui-même.

Dans le router, on retrouve bien la route que tu as définie, à laquelle tu passes également un :id et une fonction.

    // Card
    router.get('/card/:id' , cardsController.item);

C'est exactement comme ça qu'il faut faire ! Cependant sache que tu peux réduire le texte présent encore afin de maintenir encore plus le code.

Tu peux transformer les constantes :

    const  mainController  =  require('./controllers/mainController');
    const  searchController  =  require('./controllers/searchController');
    const  cardsController  =  require('./controllers/cardsController');
    const  deckController  =  require('./controllers/deckController');

en :

    const { homePage } =  require('./controllers/mainController');
    const { searchPage, getSearchResults } =  require('./controllers/searchController');
    const { item } =  require('./controllers/cardsController');
    const { deckPage, addDeck } =  require('./controllers/deckController');

Cette action s'appel la "déstructuration", en JavaScript est une syntaxe qui permet d'extraire des valeurs d'un objet ou d'un tableau et de les affecter à des variables distinctes de manière concise et lisible.

ainsi tout tes routes peuvent etre transformer en :

    router.get('/', homePage);
    router.get('/search', searchPage);
    router.get('/searchResults', getSearchResults);
    router.get('/card/:id', item);
    router.get("/deck", deckPage);
    router.get("/deck/add/:id", addDeck);

Pour la fonction `item`, je te conseil de mieux la nommer, car dans un code dense avec beaucoup d'information juste le mot item est assez confus. Ce que nous voulons faire dans cette fonction c'est récupérer les informations d'une carte, nous pouvons par exemple l'appeler `renderOneCard` afin d'être plus clair (et ca serai raccord avec ta fonction `getOneCard`).

Tu as utiliser la fonction `Number` autour de `req.params.id`, c'est également une bonne pratique dans le cadre d'une verification que la valeur est bien de type number. Ca marche très bien dans ce cas la, car les id que tu passes sont tous des entiers, mais tu peux avoir des problème si tu souhaite convertir des donner avec la fonction `Number`. Celle ci est utilisée pour convertir une valeur en un nombre à virgule flottante, dans certains cas, la fonction [`parseInt`](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/parseInt) peut mieux convenir.

Pour la fonction `getOneCard`, je te propose juste un petit optimisation qui te permettra de proteger une erreur éventuel du retour de ton result.

Tout le bloc `if` suivant peut etre simplifier :

    if (cardResults.rowCount  ===  1){
        return  cardResults.rows[0];
    } else {
        return  null;
    }

en :

    return cardResults.rows[0] || null;

Si `cardResults` ou `rows` sont en erreur, alors cela renverra `null` et ne générera pas d'erreur.

Concernant ta vue `card.ejs`, elle est correcte... mais uniquement dans le cas où tout va bien !

Je te propose de tester une chose :

1. Démarre ton serveur avec `npm run dev`.
2. Va sur une carte, n'importe laquelle.
3. Dans la barre d'adresse, remplace l'ID par un ID qui n'existe pas, par exemple 999 : `http://127.0.0.1:1234/card/999`.

Tu constateras qu'il y a des erreurs de rendu, car ta ligne `dataMapper.getOneCard(id)` peut être vide et tu ne fais pas de vérification qu'il y a des données.

Il y a deux solutions pour résoudre ce problème :

1. Mettre une vérification en place lors du retour de la ligne `dataMapper.getOneCard(id)` dans ton fichier `cardsController.js` afin de vérifier que nous avons bien des éléments.
2. Mettre une condition dans ton rendu qui vérifie que `card` est bien alimenté dans la vue `card.ejs`.

Pour être propre, je te conseille d'utiliser la première solution. Je te laisse essayer de la mettre en place. Si tu as besoin d'informations, n'hésite pas à nous demander !

> **- Etape 2**

Dans la fonction `getSearchResults`, tu as fait appel à `dataMapper.getElements()`, chose intelligente pour le coup, mais malheureusement cela ne fonctionne pas dans ton cas.

Ta fonction `getElements()` va renvoyer uniquement des cartes qui ont un élément, car tu utilises la construction `IS NOT NULL` dans la requête. Donc nous ne pourrons pas connaître les cartes qui n'ont pas d'élément. En règle générale, il est préférable d'optimiser les requêtes SQL que tu peux optimiser afin de gagner en performance. Ta fonction `getElements()` récupère l'ensemble des cartes du projet dont l'élément est renseigné, si nous avions plusieurs centaines de millions de cartes cela aurait peut-être été lourd à récupérer.

Dans ton cas, tu aurais pu créer une nouvelle fonction pour lui passer le paramètre élément, et cela t'aurait évité d'utiliser `filter`. Mais c'est très bien d'avoir pensé à la méthode `filter` !

Mais attention, dans tous les cas, il vaut mieux contrôler le retour de tes requêtes pour être sûr que les différentes constantes que tu as remplies contiennent des valeurs.

> **- Etape 3**

Voici une version corrigée du texte :

Tu as bien implémenté le package `express-session`, c'est très bien ! Je te donne quand même 2 informations importantes pour le futur :

- En règle générale, les `require` doivent être placés en haut de ton fichier afin d'avoir directement toutes les informations concernant les imports.
- Le projet utilise dotenv, qui permet de mettre en variable d'environnement des données de déploiement mais également des données sensibles comme ton secret qui n'est pas courant ! En utilisant cela, tu peux sécuriser ton projet lors d'une publication sur Github par exemple.

Remplace le code suivant :

    secret: 'dadp kdfpkazdf kaf a*fka kfafâlfaflafelfafv lefa',

par :

    secret: process.env.SECRET,

Dans ta fonction `addDeck`, tu as pensé à initialiser la session avec le tableau `deck`, c'est bien ! Maintenant, nous avons besoin de ce tableau un peu partout dans notre projet. Tu aurais pu le mettre directement dans un middleware dans l'`index.js` (ou segmenter dans un autre fichier, à toi de voir !).

J'ai remarqué une coquille dans le code de ta vue. Il y a un `$` qui se promène quand tu as une carte dans ton deck. En regardant le code de plus près, tu as écrit `<p  class="item-subtotal">$<%= card.price%></p>`. Où as-tu trouvé ce code ? Dans le projet, on ne parle pas de prix ou de total. Je pense que tu as voulu te servir d'un template existant que tu as trouvé sur internet ou dans un autre cours pour gagner du temps et que tu n'as peut-être pas eu le temps de le modifier. Quoi qu'il en soit, il y a un petit souci !

Pour le reste, nous sommes tout bons ! Si tu veux reprendre les suggestions que je t'ai faites, n'hésite pas. Et même si tu veux essayer de faire les `bonus` du projet, ça peut être également intéressant pour approfondir tes connaissances.
