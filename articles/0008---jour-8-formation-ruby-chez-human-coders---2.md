Bonjour !

C'est inéluctable, il n'y aura jamais d'article consacré au "jour 7", puisque je passe directement du sixième au huitième. Un peu comme l'album 5 de Gaston Lagaffe - toute proportion gardée dans la qualité des contenus respectifs.  
C'est qu'hier soir je me suis rendu avec nos 2 formateurs Ruby Matthieu et Camille à un _[Human Talk](http://humantalks.com/)_ dans les locaux de Viadeo - fort intéressant par ailleurs - , aussi n'ai-je pas eu le temps de coucher sur papier mes apprentissages Ruby de la journée.

### Construire une Gem

Nous avons commencé par étudier la structure d'une Gem (une bibliothèque Ruby), et nous avons enchaîné avec la création dans les règles de l'art de notre propre Gem.  
Ca tombe bien, car je me disais que ce serait rudement bien d'empaqueter sous forme de Gem certaines des fonctionnalités que je vais devoir créer pour ma première appli Rails.

C'est qu'en tant que développeur PHP, j'ai pris l'habitude avec Zend Framework et Symfony 2 d'utiliser certains outils que je n'ai pas tous retrouvés dans la profusion de fonctionnalités offertes par Ruby, sa librairie standard, Rails ou les Gems. Je vais donc m'atteler à les transposer en Ruby afin de pouvoir rester dans les "bonnes pratique Rails" tout en ajoutant quelques "PHP-iseries" auxquelles je me suis habitué, et qui me causeraient bien du chagrin si je ne pouvais plus les utiliser.  
Les empaqueter sous forme de Gem me permettrait de pouvoir réutiliser ces fonctionnalités facilement et proprement dans de futurs projets, et ça pourrait rendre service à d'autres puisque je les publierais en Open Source sur GitHub, comme je l'ai fait avec mon ptit module [Node-DBI](https://npmjs.org/package/node-dbi) pour Node.js.

Mais puisque j'allais devoir compenser mon manque d'expérience par un rythme soutenu sur ce premier projet, je m'étais dit que je n'aurais malheureusement pas le temps d'apprendre à créer à une Gem proprement. Eh bien grâce à cette formation la création de Gems m'a été démystifiée, et hormis les tests unitaires qui seront consommateurs de temps quoi qu'il arrive, je pourrai rapidement et sans appréhension m'atteler à ce chantier.  
Il me tarde de publier ma première Gem sur [RubyGems](http://rubygems.org/) ! :-)

### Metaprogrammation, à nous deux !

Certains des langages de programmation que j'ai pu pratiquer jusqu'ici, comme PHP ou ActionScript 3, permettent l'[introspection](http://fr.wikipedia.org/wiki/R%C3%A9flexion_(informatique)). Dans des frameworks comme Symfony 2, d'ailleurs, l'introspection a une grande importance - notamment avec la programmation par annotations. Je pense donc que j'aurais pu sans trop de mal trouver mes marques en la matière en Ruby si le besoin s'était fait sentir.

Pour l'**intercession**, en revanche, c'est une autre histoire. L'intercession (j'ignorais moi-même l'existence de ce mot jusqu'à ce matin :-), dixit Wikipédia, est _la capacité d'un programme à modifier son propre état d'exécution_ ; c'est qui permet par exemple à une classe de se modifier elle-même au _runtime_.  A part en TCL/Tk, à vrai dire, pour ma part je crois n'avoir à peu près jamais utilisé cette fameuse intercession.  
Certes, on a bien en PHP les [méthodes magiques](http://php.net/manual/fr/language.oop5.magic.php) qui permettent de s'en approcher, mais ça reste du bricolage à mon sens, puisque jamais on ne crée **réellement** de méthode au _runtime_ : on se contente de les émuler.  

Avec Ruby, de ce coté c'est la fête ! Le langage est vraiment fait pour ça, ce qui encourage à s'en servir même pour des choses triviales ne faisant pas appel à des _design patterns_ de haute voltige. Si j'avais continué seul sur _la Voie du Ruby &trade;_, je suis à peu près certain qu'il ne me serait jamais venu à l'idée d'utiliser cette forme de métaprogrammation, alors que Matthieu et Camille ont pu nous expliquer en quoi c'était une chose naturelle en Ruby, et dans quels types de contextes on pouvait l'utiliser.

Je pense que je serai vite confronté à une situation où utiliser la métaprogrammation en Ruby me fera gagner du temps et/ou de la simplicité dans le code de mon appli Rails. Je ne manquerai pas de le détailler ici, afin de partager cette connaissance en d'illustrant à quoi cela peut servir.

### Les DSL

Voilà un autre domaine dont j'avais pas mal entendu parler dans les différents tutoriaux Ruby et Rails que j'ai pu parcourir : les [DSL](http://fr.wikipedia.org/wiki/Domain-specific_programming_language).  
Alors de ce coté-là, c'est simple : je suis sûr à 100% que je n'aurais jamais utilisé ce truc de mon propre chef :-) J'en avais une vision approximative, je trouvais pas ça super utile et n'ayant jamais utilisé de langage de programmation rendant cela aussi fluide que Ruby, ça n'est absolument pas une façon de faire à laquelle j'aurais pensé pour résoudre un problème.

Eh bien force est de reconnaître que c'est bien utile, ces petites bêtes. Avec les fonctionnalités et la syntaxe de Ruby, où parenthèses et points-virgules sont optionnels, on peut très facilement créer un DSL - en gros, un langage de script simplifié, dédié à une usage précis, en surcouche de Ruby.  
Matthieu nous a montré quelques exemples de DSL utilisés dans le monde du Ruby, puis nous l'avons suivi pour créer notre propre DSL. En quelques minutes seulement nous pouvions configurer tous les paramètres de notre application de test avec un langage de script simplifié élaboré spécialement pour elle. C'est vraiment très chouette.

Voilà un autre domaine de Ruby que je n'aurais jamais utilisé par moi-même, et qui je pense me sera très vite utile en pratique. Là aussi je tâcherai de partager ce savoir fraîchement acquis, en détaillant ici ma première DSL lorsque le besoin d'en créer une se présentera.  
En attendant, je ne peux que vous inviter à aller trouver un tutoriel à ce sujet, si comme moi vous n'avez jamais utilisé de DSL et que vous ne voyez pas ce que ça peut vous apporter. Ca vaut le coup.

### Apprendre Ruby c'est se faire plaisir

La formation est déjà finie, ces 3 jours sont passés très vite.  
Hormis les grandes lignes que j'ai résumées ici, Matthieu et Camille ont pu répondre à mes (trop) nombreuses questions, et m'ont permis d'être aujourd'hui à l'aise avec Ruby, ce que je n'étais absolument pas jusqu'ici puisque j'étais passé directement à la case Rails sans m'attarder sur la case Départ.  

Ce qui est bien dommage, puisque je ne touchais pas 20.000 francs : je restais en surface du langage, me contentant d'utiliser les équivalents de ce à quoi j'étais habitué en PHP/JavaScript/ActionScript et reproduisant les mêmes schémas, alors que Ruby a une versatilité au _runtime_ très élevée, qui lui confère des possibilités et des _design patterns_ inédits.  

Si comme moi vous débutez en Rails, je ne peux que vous conseiller de faire tout le contraire de ce que j'avais entrepris initialement. Je n'avais fait que survoler les bases du langage, alors qu'il est tellement agréable à utiliser et à découvrir en détail que cela vaut réellement le coup de bien potasser Ruby avant de se lancer dans RoR, avec un exemple pratique histoire de pouvoir mettre tout de suite les mains dans le cambouis.  

Si vous n'avez pas d'inspiration pour créer un premier projet en Ruby à but didactique, vous pouvez par exemple vous monter rapidement un petit jeu simple, comme nous l'avons fait avec Matthieu, en vous appuyant sur la librairie Ruby [Gosu](https://github.com/jlnr/gosu/wiki). Elle n'a je pense pas vocation à être utilisée pour des cas "réels", mais pour pratiquer les bases de Ruby tout en s'amusant c'est un très bon outil - combiné à des ressources graphiques comme [celles-ci](http://untamed.wild-refuge.net/rmxpresources.php?characters), c'est un très bon moyen d'explorer le langage, puisqu'en une petite heure vous pouvez avoir votre personnage dirigé au clavier qui zigzague pour éviter les ennemis, tout en ayant potassé Ruby :-)

Je finirai cet article en ne manquant pas de remercier une nouvelle fois [Matthieu](https://twitter.com/MatthieuSegret) et [Camille](https://twitter.com/CamilleRoux) : [ces quelques jours de formation Ruby](http://formations.humancoders.com/formations/ruby) vont me faire gagner un temps fou. Au-delà même du simple temps, il y a certains domaines comme les DSL et la méta-programmation que je n'aurais probablement jamais exploré par moi-même, et ça c'est inestimable.  
Merci les gars !

Je tâcherai de publier ici mes usages de ce que j'ai pu apprendre lors de cette formation, afin que ceux qui comme moi n'auraient pas utilisé de leur propre chef ces possibilités de Ruby puissent eux aussi se faciliter la vie... et se faire plaisir.  
Car c'est là aussi l'un des enseignements que j'ai pu tirer de cette formation : coder avec Ruby c'est fun, **vraiment** fun !


A bientôt !


