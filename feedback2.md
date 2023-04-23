# Feedback 2 - Apprenant 2 - Triple Triad Deck Builder

**Résumé :**

Le projet répond dans l'ensemble a la demande ! Bravo pour ça ! Cependant je te conseil de prendre le temps de relire ton code afin d'éviter des coquilles.
Tu peux également commencer a te renseigner sur l'optimisation de code et les packages present dans le projet comme `express-session` ou `dotenv`.

| Etapes | Commentaires                                                                                                                                |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | L'implémentation est bonne mais nécessite plus de vérification au niveau des retours de la base de donnée.                                  |
| 2      | Ta méthode getSearchResults est correctement appeler, mais il manque un cas fonctionnel celui du "Et si je veux les cartes sans éléments ?" |
| 3      | La présence de express-session est top ! Mais par manque de temps des erreurs sont présentes.                                               |

> **- Etape 1**

Pour cette feature, nous t'avons demandé de créer un moyen de voir les détails de la carte sélectionner.
Visuellement, nous t'avons demander la disposition de ton choix mais de manière basique, le but était de te faire travailler la récupération d'information via une base de donnée et de passer des éléments dynamique et non le style en lui même.

Dans le router on retrouve bien la route que tu as défini, tu lui passe également un :id et une fonction.

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

Si, `cardResults` ou `rows` est en erreur, alors ca renverra `null` et ne génèrera pas d'erreur.

Pour ta view `card.ejs` est correct.... Mais uniquement dans le cas ou tout va bien !

Je te propose de tester une chose :

1.  Démarre ton serveur avec `npm run dev`
2.  va sur une carte, n'importe lequel.
3.  Dans la bar d'adresse, remplace l'id par un id qui n'existe pas par exemple 999. `http://127.0.0.1:1234/card/999`

Tu constatera qu'il ya des erreurs de rendu car ta ligne `dataMapper.getOneCard(id)` peut être vide, et tu ne fais pas de vérification qu'il y'a des données.

2 solutions pour ce problème :

1.  Met une vérification en place au retour de la ligne `dataMapper.getOneCard(id)` dans ton `cardsController.js` afin de verifier que nous avons bien des éléments.
2.  Met une condition dans ton rendu qui vérifie que `card` est bien alimenter dans la vue `card.ejs`

Pour faire propre, je te conseil la première solution. Je te laisse essayer de mettre en place cette solution si tu as besoin d'informations n'hésite pas a nous demander !

> **- Etape 2**

Dans la fonction `getSearchResults`, tu as fais appel a `dataMapper.getElements()` chose intelligente pour le coup, mais malheureusement ne fonctionne pas dans ton cas.

Ta fonction `getElements()`va renvoyer que des cartes qui ont un élément, car tu utilise la construction `IS NOT NULL` dans la requête. Donc nous ne pourront pas connaitre les cartes qui n'ont pas d'élément, autre chose en règle général, il vaut mieux optimiser les requêtes SQL que tu peux optimiser afin de gagner en performance. Ta fonction `getElements()` récupère l'ensemble des cartes du projet dont l'élément est renseigner, si nous avions plusieurs 100ene de millions de cartes ca aurai été peut-être lourd a récupérer.

Dans ton cas, tu aurai pu faire une nouvelle fonction afin de lui passer le paramètre élément, et ca t'aurai éviter de faire un `filter`. Mais c'est très bien d'avoir penser a la méthode `filter` !

Mais attention dans tout les cas, vaut mieux contrôler le retour de tes requêtes pour être sur que les différentes constante que tu as remplit contienne des valeurs.

> **- Etape 3**

Tu as bien implémenter le package `express-session` c'est très bien ! Je te donne quand même 2 informations importantes pour le futur,

- En régle générale, les `require` on les met toujours en haut de ton
  fichier afin d'avoir directement toutes les informations concernant
  les imports.
- Le projet utilise dotenv, qui permet de mettre en variable
  d'environnement des données de déploiement mais également des données
  sensibles comme ton secret qui est... pas courant !

le faite d'utiliser ça, ca te permettra de sécuriser ton projet lors d'une publication sur Github par exemple.

    secret: 'dadp kdfpkazdf kaf a*fka kfafâlfaflafelfafv lefa',

par

    secret: process.env.SECRET,

Dans ta fonction `addDeck` tu as penser a initialiser la session avec le tableau `deck` c'est bien !

Maintenant ce tableau nous en avons besoin un peu partout dans notre projet, tu aurai pu le mettre directement dans un _middleware_ directement dans l'`index.js` (ou segmenter dans un autre fichier a toi de voir !).

J'ai remarquer une coquille dans le code de ta view, il y'a un `$` qui ce promène quand tu as une carte dans ton deck. en regardant le code de plus près tu fais `<p  class="item-subtotal">$<%= card.price%></p>`. Ou as tu trouver ce code ? Dans le projet on ne parle pas de price ou de total, je pense que tu as voulu te servir d'un Template existant que tu as trouver sur internet ou dans un autre cours pour gagner du temps et que tu as peut-être pas eu le temps de le modifier, quoi qu'il en soit y'a un petit soucis !

Pour le reste nous sommes tout bon ! Si tu veux reprendre les suggestions que je t'ai fait hésite pas, et même si tu veux essayer de faire les `bonus` du projet ca peut-être également intéressant pour approfondir tes connaissances.
