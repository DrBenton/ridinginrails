Bonjour !

Bienvenue sur ce blog, sur lequel je tâcherai de détailler mon parcours, en tant que développeur PHP / JavaScript, dans l'apprentissage de *[Ruby on Rails](http://rubyonrails.org)*.
Pourquoi raconter ce parcours de transition ? Parce que je pense ne pas être le seul développeur PHP à m'intéresser à _Rails_, et que mon humble témoignage pourra peut-être se révéler utile pour d'autres personnes voulant elles aussi s'y initier.


### Le contexte


Je vais commencer par me présenter rapidement, afin de poser le cadre qui va être le sujet de cet espace.
Je suis développeur Web depuis 2000, depuis quelques années en freelance, et même si j'ai eu l'occasion de travailler sur pas mal de technologies différentes mon "coeur de métier" reste les langages PHP, JavaScript et ActionScript.
(en ce qui concerne ce dernier, même si la technologie Flash n'est pas morte et a encore de beaux jours devant elle notamment dans le domaine du jeu, le temps de sites Web _Full Flash_ est bel et bien révolu, et malgré toutes les qualités d'ActionScript 3 cette compétence dans le contexte "Web" sera malheureusement bientôt à mettre au placard)

Au cours de ma formation en DUT S.R.C. de 1998 à 2000 j'ai pu apprendre les langages JavaScript, Perl et Java.
Lorsque j'ai commencé à travailler en agence Web, en juillet 2000, Perl en mode CGI commençait cependant à ne plus être très utilisé dans le Web, remplacé progressivement mais rapidement par PHP4.
C'est donc avec PHP que j'ai programmé quasiment toutes les parties _back-end_ de tous les sites Internet et intranets sur lesquels j'ai pu travailler depuis 13 ans.


#### PHP


PHP est un superbe langage pour débuter la programmation Web, et j'ai adoré l'utiliser.
Aujourd'hui, avec les versions [5.3](http://php.net/manual/fr/migration53.new-features.php) et [5.4](http://php.net/manual/fr/migration54.new-features.php) le langage commence enfin à être "sérieux", industrialisé - tout en restant toujours aussi simple d'accès pour les néophytes, ce qui est une belle prouesse.
Le développeur PHP a accès aujourd'hui aux typages pour les arguments de méthodes, aux _namespaces_, aux _Closures_, aux _Traits_, et à toutes sortes de joyeusetés qui permettent au programmeur averti d'aller plus loin que les simples _includes_ et affichages de données de formulaires :-)

L'écosystème PHP n'a jamais été aussi actif : on dispose enfin avec [Packagist](https://packagist.org/) et Composer d'un gestionnaire de packages simple d'accès et ouvert (ce que n'était pas Pear), et avec des outils comme Zend Framework, Symfony 2, Laravel ou Silex on commence à atteindre un état de l'art tout-à-fait satisfaisant pour qui aime les belles architectures logicielles.

Mais en ce qui me concerne, après toutes ces années passées sur PHP je suis aujourd'hui lassé par certaines caractéristiques du langage. Je ne m'étendrai pas spécialement là-dessus, et ne souhaite en aucun cas lancer un appel à troll :-)
Disons simplement je me retrouve de plus en plus souvent touché par la "[PHP sadness](http://phpsadness.com/)", et que lorsque je reviens de langages "tout objet" comme JavaScript pour revenir vers PHP, qui essaie de tendre dans cette direction mais qui ne l'atteindra jamais étant donné son ADN, j'ai bien vite envie de revenir au premier !
Pouvoir faire, par exemple :

<pre><code class="language-javascript">'Hello World'.split(' ').reverse().join(' ').toUpperCase()</code></pre>

et non :

<pre><code class="language-php">strtoupper(implode(' ',  array_reverse(explode(' ', 'Hello World')))) </code></pre>


#### Node.js


Mais si tu aimes tant JavaScript pourquoi ne pas quitter PHP pour passer à JavaScript, me direz-vous ? Avec des technologies comme [Node.js](http://nodejs.org/), on peut aujourd'hui faire sérieusement du JavaScript pour coder la partie _back-end_ d'un site Web.
C'est exactement ce que je me suis dit, il y a un peu plus d'un an ! :-) Je me suis donc mis à Node.js, j'ai créé mes premiers modules _Open Source_ Node.js, publiés sur [NPM](https://npmjs.org/) (le dépôt central de modules Node.js), et j'ai développé quelques protos de sites Web avec le chouette micro-framework [Express](http://expressjs.com/guide.html).

C'était une très bonne expérience, cependant je me suis heurté à 2 contraintes qui ne sont pas négligeables :

- Une des forces de Node.js est la programmation "tout asynchrone", qui permet de ne pas bloquer le serveur pendant une opération d'entrée/sortie (par exemple lorsqu'on communique avec une base de données, où qu'on lit ou écrit un fichier).
  Malheureusement, cet aspect peut rapidement se transformer en "[Callback Hell](http://callbackhell.com/)". Il existe certes des techniques permettant de rendre la gestion de la programmation asynchrone moins gênante, comme le plébiscité module [async](https://github.com/caolan/async), mais ça reste assez pénible à gérer lorsque vous avez plusieurs opérations asynchrones liées.
  A la longue, j'ai trouvé que la gestion de la multiplicité des opérations asynchrones prenait trop de place dans mon code.

- Fort de mes expériences avec Node.js, et pourvu de code que je pouvais montrer à un employeur, je me suis mis à la recherche d'une mission Node.js.
  Las ! En France on trouve bien quelques startups qui utilisent cette technologie, mais pour le moment on ne peut pas dire que la mayonnaise prenne. En une année j'ai dû tomber sur 4 ou 5 offres de missions Node.js ; on ne peut pas vraiment parler de marché porteur pour le moment.. :-/


### Après Node.js : retour à PHP, en demie-teinte


Après avoir essayé Node.js, et ne trouvant pas de mission dans ce domaine, je suis revenu à PHP. J'ai eu l'occasion de travailler sur des projets Symfony 2 et Silex, j'ai fait encore un peu de Zend Framework.

Tous sont de bien beaux outils, mais après quelques mois la "PHP sadness" m'a malheureusement de nouveau touché de son aile.
PHP est une belle technologie, qui n'a jamais été aussi intéressante à utiliser qu'aujourd'hui, mais force est de constater qu'après toutes ces années à l'utiliser, pour ma part la lassitude est bel et bien là.

Il faudrait que je puisse trouver une techno ayant les caractéristiques suivantes :

- **Open Source** : j'y tiens :-)
- **orientée objet**
- **plaisante à utiliser**, sans trappe cachée comme le _callback hell_
- et, *last but not least*, **suffisamment utilisée** pour pouvoir vivre de l'usage de cette technologie

Et là, il a bien fallu que je me rende à l'évidence. Il n'y avait pas vingt milles candidats pouvant répondre à ces critères, mais un seul a priori : **Ruby on Rails**.


### Ruby on Rails

Ruby on Rails, c'est d'abord le langage Ruby. Ca fait longtemps qu'on se croise sans vraiment se parler, lui et moi ! :-)
Que ce soit lorsque j'utilise l'excellent [JSDuck](https://github.com/senchalabs/jsduck) pour générer les documentations de mes projets JavaScript, lorsque j'installe un [RedMine](http://www.redmine.org/) chez un client, lorsque j'utilise [Capistrano](www.capistranorb.com) ou [Vagrant](http://www.vagrantup.com/), il me faut à chaque fois installer Ruby et des Gems.

Mais au-delà de ces applications, une ombre de Ruby bien plus puissante rôdait depuis quelques années autour des différents frameworks PHP que j'utilisais. En effet, que ce soit Zend Framework, Symfony ou Silex, les technologies PHP les plus en pointe sont toutes inspirées de frameworks Ruby !
Les Cake PHP, Code Ignitier, Zend Framewok, Symfony et consorts sont initialement des retranscriptions presque littérales de [Ruby on Rails](rubyonrails.org), et Silex est à la base un "portage" en PHP du micro-framework [Sinatra](http://www.sinatrarb.com/).

Parce que j'aime savoir comment fonctionnent les outils que j'utilise, j'ai notamment pas mal décortiqué le code source de Zend Framework 1. Je me souviens non sans émotion de cette bien intrigante mention de Ruby on Rails dans les commentaires d'en-têtes d'un des composants fondamentaux du framework, la Route : https://github.com/mridgway/Zend-Framework-1.x-Mirror/blob/master/library/Zend/Controller/Router/Route.php#L33

Aujourd'hui ces frameworks ont évolué et ont pris chacun leur propre chemin par rapport à Ruby on Rails, mais leur génèse reste quoi qu'il en soit directement liée à "Rails".

Après toutes ces années où je n'ai pas voulu aller voir à quoi ressemblait ce fameux _Rails_ qui avait donné naissance à tous les frameworks MVC PHP que j'utilisais, je vais donc tenter l'aventure.

#### La syntaxe de Ruby

J'ai longtemps été rebuté par la syntaxe de Ruby, qui s'éloignait tellement de ce que je connaissais qu'elle me paraissait fort laide.
Le concept de blocs, notamment, me paraissait bien obscur -  jusqu'à ce que je comprenne que c'était simplement là une manière d'écrire des _closures_, comme on le fait à longueur de journée en JavaScript.

Depuis quelques temps j'ai pas mal travaillé avec [CoffeeScript](http://coffeescript.org/) également. Après quelques réticences devant cette syntaxe étrange, force est de constater que c'est un très chouette langage - qui inspire d'ailleurs fortement les spécifications des futures versions de JavaScript.
La syntaxe de CoffeeScript étant à dessein très proche de celle du Ruby, il m'est à présent plus facile de lire du code Ruby.

#### Vers l'inconnu et au-delà

Me voilà donc prêt à plonger dans le grand bain ! :-)

Je ne sais ce qu'il adviendra, et je m'empresserai peut-être de fuir Ruby et Ruby on Rails une fois que j'aurai mis les mains dedans. Mais je pense que le jeu en vaut la chandelle, même si cela va être un gros investissement temporel de m'y mettre.

Si comme moi vous avez envie de diversifier un peu votre travail de développeur Web passionné, peut-être ce blog pourra-t-il vous être utile.

Je vous retrouve très prochainement pour mes premiers pas en Ruby !