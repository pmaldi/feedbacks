# Feedback 1 - Apprenant 1 - Triple Triad Deck Builder

**Résumé :**

Le projet fonctionne très bien et répond aux demandes, des optimisations de code sont possible mais ca viendra avec l'expérience !
Attention malgré tout a supprimer les marques de développement comme les commentaires **non nécessaires** et les **console.log** qui n'ont pas de valeur ajoutée en production.

| Etapes | Commentaires                                                                                                               |
| ------ | -------------------------------------------------------------------------------------------------------------------------- |
| 1      | Les fonctionnalités sont présentes, le code est propre et des optimisations sont possibles. Mais l'implémentation est la ! |
| 2      | A part une erreur plutot basique, rien a dire sur cette partie ! Bien joué !                                               |
| 3      | L'organisation est pensée, une implémentation pas fausse mais a déplacer pour etre "standard".                             |
| Bonus  | Très bien implémenté. Bravo !                                                                                              |

> **- Etape 1**

Avant toutes choses, félicitation la feature fonctionne correctement !
Lorsque je sélectionne une carte je tombe bien sur le détail de la carte avec les informations nécessaires (Elément, Niveau, Valeurs NSEW).
Visuellement, nous t'avons demander la disposition de ton choix mais de manière basique, le but était de te faire travailler la récupération d'information via une base de donnée et de passer des éléments dynamique et non le style en lui même.

Dans le router on retrouve bien la route que tu as défini, tu lui passe également un :id et une fonction.

    // Card
    router.get('/card/:id', cardController.renderOneCard);

La méthode est la bonne ! Cependant sache que tu peux réduire le texte présent encore afin de maintenir encore plus le code.

Tu peux transformer les constantes :

    // Controllers
    const  mainController  =  require('./controllers/mainController');
    const  searchController  =  require('./controllers/searchController');
    const  cardController  =  require('./controllers/cardController');

en :

    // Controllers
    const { homePage, notFound } =  require('./controllers/mainController');
    const { searchByElement, searchByName, searchByValues, searchByLevel, searchPage } =  require('./controllers/searchController');
    const { renderOneCard, removeCard, deckPage, addCard } =  require('./controllers/cardController');

Cette action s'appel la "déstructuration", en JavaScript est une syntaxe qui permet d'extraire des valeurs d'un objet ou d'un tableau et de les affecter à des variables distinctes de manière concise et lisible.

ainsi tout tes routes peuvent etre transformer en :

    // Card
    router.get('/card/:id', renderOneCard);

Pour la fonction `renderOneCard`, c'est top ca fonctionne également très bien, c'est un bon reflexe de mettre des appels instables (comme des appels API ou BDD) dans des blocs "try catch", ca te permettra de reduire le taux d'erreur non couverte dans ton projet.

Tu as utiliser la fonction `Number` autour de `req.params.id`, c'est également une bonne pratique dans le cadre d'une verification que la valeur est bien de type number. Ca marche très bien dans ce cas la, car les id que tu passes sont tous des entiers, mais tu peux avoir des problème si tu souhaite convertir des donner avec la fonction `Number`. Celle ci est utilisée pour convertir une valeur en un nombre à virgule flottante, dans certains cas, la fonction [`parseInt`](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/parseInt) peut mieux convenir.

Pour la fonction `getOneCard`, je te propose juste un petit optimisation qui te permettra de proteger une erreur éventuel du retour de ton result.

Tout le bloc `if` suivant peut etre simplifier :

    if (result.rowCount  ===  1) {
        return  result.rows[0];
    }

en :

    return result.rows[0] || null;

Si, `result` ou `rows` est en erreur, alors ca renverra `null` et ne génèrera pas d'erreur.

Pour ta view `card-item.ejs` en soit elle est bonne également, mais tu peux mettre l'ensemble de tes balises `<p>` dans la condition `card.element` que tu as crée. vu que si nous n'avons pas de carte, alors nous n'aurons pas non plus les différents attributs nécessaires :

    <div  class="cardsContainer">
        <div>
    	    <img  src="/visuals/<%= card.visual_name %>"  alt="Carte <%= card.name %>" />
    	    <% if(card.element){ %>
    		    <p>Element: <%= card.element %></p>
    		    <p>Level: <%= card.level %></p>
    		    <p>Value North: <%= card.value_north %></p>
    		    <p>Value East: <%= card.value_east %></p>
    		    <p>Value South: <%= card.value_south %></p>
    		    <p>Value West: <%= card.value_west %></p>
    	    <% } else { %>
    		    <p>Element: empty</p>
    	    <% } %>
        </div>
    </div>

> **- Etape 2**

Dans la fonction `searchByElement`, je vois que tu as essayer de faire une vérification en plus, mais tu n'as du la mettre en place dans la logique. Tu as crée un bloc `if(searchedElement != '')` tu peux juste vérifier la présence de `searchedElement`

    if (searchedElement) {
        searchedResults = await dataMapper.searchByElement(searchedElement);
    }

autre remarque avec le champs `title` de ton render, tu peux mettre du templating afin de rentre plus agreable le code en lecture :

    title: `Résultat de la recherche : ${searchedElement ? searchedElement : 'sans élément'}`,

L'avantage c'est que ca evitera que tu te perd avec les `"+"` de concaténation. Ce qui dans cette étape te crée un soucis :
![Erreur NaN](https://i.imgur.com/BExNCFO.png)

> **- Etape 3**

Pour l'utilisation du Middleware c'est bien et c'est encore mieux de l'avoir isolé dans un dossier a part !

Cependant j'aurai personnellement mis le Middleware au niveau de l'index en "global" de ton application, actuellement c'est un peu ~~"[(~~triché~~)]"~~ de l'avoir mis dans le router, puisque ton tableau ce crée dès la première route consultée et non lors du lancement de l'application ce qui sur d'autre projet peux créer des soucis.

L'ajout au deck fonctionne également très bien, si je peux ajouter une remarque, toujours prévenir l'utilisateur que son action a eu une conséquence.... ou pas !
Nous pouvons regretter l'absence de message d'information pour prévenir l'utilisateur que la carte a bien été rajouter au deck ou que non, elle n'a pas été ajoutée au deck a cause de ...

Dans `deckPage`, nous pouvons faire une petite optimisation également, pour nous passer du bloc `if` :

    deckPage: (req, res) => {
    	const cards = req.session.cards || [];
    	res.render('card/deck', { cards });
    },

**_Remarque :_** Dans la fonction `removeCard`, c'est très bien d'avoir utiliser la fonction `filter` !

    const  deck  =  req.session.cards.filter(card  =>  !(card.id  ===  cardId));

> **- Bonus**

Bravo d'avoir mis en place les bonus ! L'implémentation est bonne une petite remarque cependant pour aller plus loin dans ton apprentissage.

Pour le `searchByValues`, tu peux optimiser le `if` afin de gerer le cas ou `searchedDirection` et `searchedValue` est manquant afin de reduire un appel a la base de donnée, tu l'as implémenter mais je pense que nous pouvons aller plus loin :

    if (!searchedDirection  ||  !searchedValue) {
        return  res.status(400).send('Missing search parameters');
    }

Pour le reste nous sommes tout bon !
