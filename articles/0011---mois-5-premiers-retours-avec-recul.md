Bonjour !

Après ce long silence de quelques mois, me revoici. Mon mutisme n'était pas dû à un abandon de mon projet de me mettre à Ruby, puisque bien au contraire c'est le rythme trépidant de mon premier projet Rails qui m'a fait délaisser le récit de mon retour d'expérience.  
Cinq mois après avoir commencé mon apprentissage, et après avoir travaillé à temps plein sur Rails pendant tout ce temps, je peux à présent faire un retour plus complet et peut-être moins naïf de l'expérience.

### Travailler avec Ruby

Certes, les premiers pas avec Ruby n'ont pas forcément été des plus aisés. C'est que le bougre est tout de même assez différent des languages que j'avais principalement pratiqués jusqu'alors, qui hormis TCL/Tk avaient tous plus ou moins les mêmes règles syntaxiques.  
Ruby ne constitue pas une rupture totale avec les langages "à la C" que sont PHP, JavaScript ou ActionScript, comment peuvent l'être des langages comme Lisp, mais il y a quand même un temps d'adptation à prévoir.

Qui plus est, l'API de base du langage est extrêmement fournie ! Là où JavaScript fournit quelque chose comme une trentaine de méthodes sur les chaînes de caractères, Ruby nous en propose pas loin d'une centaine (je n'ai pas compté, c'est mon estimation à la grosse louche :-). Ajoutez à cela une bibliothèque standard très fournie, et vous êtes bons pour plusieurs mois de pratiques avant de commencer à avoir une connaissance raisonnable de l'API de Ruby.

Concernant la syntaxe, j'ai pu m'adapter sans trop de peine grâce à mon utilisation antérieure de CoffeeScript - celui-ci étant lourdement inspiré par Ruby, cela aide grandement.


#### Ruby après 5 mois de travail

So... Après 5 mois passés à coder en Ruby à longueur de journée, après plus d'une dizaine d'années de développement PHP, où en suis-je de ma perception de ce langage ?
  
Eh bien pour ma part, je suis totalement conquis ! ♥  
J'ai justement retouché à du code PHP il y a quelques jours, et j'ai bien eu du mal à me remettre à la logique "langage orienté objet mais à l'ADN fonctionnel" de PHP !  
Comme en JavaScript, il me paraît tellement plus naturel de faire un <code class="language-ruby">maChaine.downcase</code> pour mettre une chaîne de caractères en minuscules que de devoir recourir à un <code class="language-php">strtolower($maChaine)</code> !

De même, j'ai passé de nombreuses années à sagement mettre des points-virgules à la fin de chacune de mes lignes de code - y compris en JavaScript, où ceux-ci sont facultatifs dans l'immense majorité des cas. Mais après 5 mois de développement passés sur Ruby pour le back-end et CoffeeScript pour le front-end, j'ai trouvé soudain si lourd de devoir utiliser partout ces points-virgules en revenant sur du code PHP !

La syntaxe de Ruby, son API et ses bonnes pratiques sont vraiment un grand plaisir à utiliser au quotidien. Utiliser ce langage de pair avec CoffeeScript est pour moi une réelle joie. Ceci est tout-à-fait subjectif, et d'autres personnes pourront continuer à préférer coder en PHP, mais pour ma part je suis pleinement séduit. ❀  
Et pourtant, avant de m'y mettre CoffeeScript comme Ruby me paraissaient foncièrement hideux lorsque je tombais sur du code écrit dans ces langages. Mais je suis bien content aujourd'hui d'avoir pu dépasser ces premières impressions :-)

### Travailler avec Rails

Je vous parlais de la richesse de l'API Ruby, mais celle-ci n'est que peu de choses comparée à celle de Ruby on Rails ! Le framework est immense, et la portée du champ fonctionnel qu'il couvre est démultipliée par les nombreuses bibliothèques Ruby (les Gems) que l'on ne manque pas de lui associer.
 
J'avoue avoir passé énormément de temps sur StackOverflow et consorts les 2 premiers mois, tellement j'étais largué. Les guides Ruby on Rails sont excellents, à mon humble avis largement du même niveau que des modèles du genre comme ceux de Symfony 2 ou Zend Framework. Mais le framework est tellement riche que conjugué à l'apprentissage simultané de Ruby les débuts ne sont vraiment pas faciles.

#### Contrôleurs, Modèles et Vues : _piece of cake!_ 

Créer ses premiers Contrôleurs, Modèles et Vues est pourtant aisé, tant cela est abondamment expliqué sur des ressources comme les [Guides Rails](http://guides.rubyonrails.org/getting_started.html) ou le [Rails Tutorial](http://french.railstutorial.org/).  
De plus, si comme moi vous êtes déjà habitués à la logique MVC de frameworks comme Symfony, Laravel ou Zend Framework (tous largement inspirés dans leurs premières versions par Rails, rappelons-le ;-), cela ne devrait pas vous poser trop de problème.  

La partie la plus difficile à maîtriser est le Modèle, puisque si vous avez une base de données fournie (ce qui était mon cas, puisque mon premier projet Rails devait inteagir avec un projet PHP existant comptant plus d'une centaine de tables MySQL) il va falloir éplucher les documentations pour découvrir comment faire telle ou telle jointure spécifique avec l'ORM de Rails.
  
Mention spéciale à ce dernier, en passant : j'ai beau avoir pratiqué des ORM PHP comme Doctrine ou celui de Zend Framework, j'ai trouvé l'ActiveRecord de Ruby on Rails bien plus agréable et concis dans on utilisation. Cela est sûrement dû en grande partie aux mécanismes spécifiques permis par Ruby en termes de méta-programmation et de concision du code, qui ont permis aux concepteurs de Ruby on Rails de créer un outil puissant et très agréable à utiliser une fois qu'on commence à s'y retrouver.


#### Pardonne-moi HAML, je t'avais mal jugé
  
Je profite également de ce débriefing "5 mois plus tard" pour revenir sur l'avis fort mitigé que j'avais émis au sujet du système de rendu HTML [HAML](http://haml.info/), et je lui adresse aujourd'hui mes publiques excuses.  
A l'usage, il est réellement très bien foutu, et permet une productivité démentielle par rapport à l'écriture de code HTML classique.  

J'avais à l'origine cherché un système de templating proche de [Twig](http://twig.sensiolabs.org/), étant habitué à celui-ci dans mes derniers projets Silex et Symfony, et je m'étais finalement rabattu sur HAML par dépit.  
Aujourd'hui pourtant, il me paraît tellement poussif de devoir gérer le code HTML comme on le fait dans des moteurs comme Twig ! Je ne jette pas la pierre à Twig, attention ; je pense toujours que celui-ci est un très bon outil de templating, plein de bonnes idées.  
Je fais du HTML depuis 1998, et je ne pensais pas dire cela un jour, mais aujourd'hui je ne me vois plus à présent devoir rédiger à la main mon code HTML !  

Avec des moteurs comme HAML, [Slim](http://slim-lang.com/) ou [Jade](http://jade-lang.com/) en Node.js (dont il existe d'ailleurs [une implémentation PHP](https://github.com/everzet/jade.php)), finies l'écriture des chevrons "<" et ">", les balises de fermeture et tout le toutim. On a juste à écrire les ouvertures de balises, et leurs imbrications sont uniquement gérées par l'indentation.  

Une fois habitué, le code est bien plus lisible, et il est surtout bien plus souple ! Il suffit de déplacer des lignes et de changer leur indentation avec les raccourcis clavier de son IDE préféré pour ré-agencer sa mise en page, de manière bien plus rapide qu'on ne le ferait en HTML "traditionnel".  
Les débuts sont déconcertants, et j'étais le premier à être réfractaire à cette manière de faire de l'HTML, mais aujourd'hui je recommande chaudement ce système. L'essayer, c'est ne plus revenir en arrière !
  
#### Les bonnes pratiques de Rails
  
Tâcher de découvrir les bonnes pratiques pour tout le code autour de ces Contrôleurs, Modèles et Vues m'a donné pas mal de fil à retordre, et mille fois sur le métier j'ai remis mon ouvrage - dit plus prosaïquement, je me suis tapé pas mal de refactorisations :-)  
En Symfony 2 par exemple on a tout un système de Services, accessibles via un _Service Locator_ omniprésent. On a également un système d'annotations, et des fonctionnalités d'injections de dépendances.  

Avec Ruby on Rails, ces _design patterns_ ne sont pas utilisés, aussi faut-il apprendre à composer autrement. Heureusement, en lisant des ressouces comme les [Rails Casts](http://railscasts.com/) (certains screencasts sont payant, mais cela vaut vraiment le coup, et je ne regrette pas d'avoir suivi les recommendations des _Humans Coders_ Matthieu et Camille lors de ma [formation Ruby](http://formations.humancoders.com/formations/ruby) chez eux :-) et en épluchant le code source de nombreuses applications basées sur Ruby on Rails sur GitHub, on arrive à prendre ses marques.  

J'ai également complété mon apprentissage avec les fantastiques ressources de [Code School](http://www.codeschool.com/), dont le mantra est "learn by doing". Les premières leçons interactives sont gratuites, à l'instar de la fameuse [Rails for Zombies](http://railsforzombies.org/) qui m'avait convaincue de tenter l'aventure Ruby on Rails. Mais je ne regrette pas les quelques dizaines d'euros consacrés à un abonnement payant donnant accès à toutes les ressources de Code School. Elles permettent un solide apprentissage de bonnes pratiques.

#### Organisation du code de ce premier projet Rails

Je travaille seul sur ce premier projet Rails, aussi n'ai-je pas de recul ni l'avis d'un Rubyiste chevronné, mais je crois que j'ai malgré tout aujourd'hui une structure applicative assez propre :-)
  
* Mes Modules sont sagement rangés dans le répertoire "lib/[nom de mon client]/", et sont tous contenus dans un namespace au nom du client.  
En Ruby et dans Ruby on Rails, on utilise très massivement ce _design pattern_ de Modules, plus connu sous le nom de [Mixins](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#mixinpatternjavascript) en JavaScript ou de [Traits](http://php.net/manual/fr/language.oop5.traits.php) en PHP 5.4.
* Mes Services sont dans le répertoire "app/services/", et chacun est un [Singleton](http://ruby-doc.org/stdlib-1.9.3/libdoc/singleton/rdoc/Singleton.html), utilisant la fonctionnalité du même nom disponible dans la bibliothèque standard de Ruby.  
Merci là encore à la formation Ruby des _Human Coders_, sans laquelle j'aurais ré-implémenté cette fonctionnalité à la main sans savoir qu'elle était là prête à l'emploi.
* J'ai également quelques Factories par-ci par-là, accessibles sous forme de Services.
* Pour la gestion des nombreuses directives de configuration que j'ai créées pour les besoins de mon application (habitué que j'ai été à Zend Framework 1.x, je ne pouvais me passer de ce système), j'ai utilisé la petite Gem [Gaston](http://chatgris.github.io/Gaston/), qui me fournit tout cela de manière très simple, tout en me permettant d'avoir des valeurs de directives différentes selon que je suis en développement ou en production.  

Je ne prétends pas avoir fait le projet Ruby on Rails le plus propre du monde, et peut-être un vétéran Rails trouverait-il moult à redire à ce que j'ai fait, mais de mon humble point de vue je suis assez satisfait de l'organisation de mon application, et j'ai appris à ne plus architecturer mon code au moyen de _Services Locators_ omniprésents comme je m'étais habitué à le faire avec Silex ou Symfony 2. 

### Conclusion

Après ces 5 premiers mois passés à l'apprentissage de Ruby et de Rails, et même si mon premier projet Rails n'est pas encore terminé, je peux déjà dresser un premier bilan :

* Ruby est décidément un chic type, et travailler avec lui au jour le jour est un vrai plaisir. Sa concision et son pragamatisme permettent par ailleurs une super productivité.
* Rails n'est pas en reste. Son ORM est de toute beauté,et après avoir fait l'effort de se mettre à un moteur de templating comme HAML ou Slim on se demande pourquoi on a passé autant d'années à écrire un code HTMl aussi verbeux.
Hormis cela, si vous faites déjà du Symfony vous retrouverez peu ou prou les mêmes mécanismes.
* Mon langage de programmation favori reste encore aujourd'hui CoffeeScript, et lorsque je repasse sur du code back-end après avoir passé quelques jours à monter une chouette interface utilisateur en JavaScript/CSS je regrette de ne pouvoir délimiter mon code Ruby uniquement par l'indentation au lieu de devoir mettre des "end" partout.  
Cela étant, CoffeeScript et Ruby se ressemblent énormément, et hormis cette manière de gérer la hiérachisation du code, le passage de l'un à l'autre est presque transparent.  
Et depuis que je fais du Ruby, je suis plus à l'aise en CoffeeScript qu'avant ! J'étais réticent par exemple à utiliser des structures comme <code class="language-coffeescript">return false if user.notFound</code>, mais avec Ruby je me suis habitué à les utiliser et elles me semblent aujourd'hui tellement naturelles que je regretterai cruellement leur abscence dans des langages comme PHP ou JavaScript.

J'étais initialement très réticent à l'idée de me mettre à Ruby, notamment parce que sa syntaxe me rebutait, et j'ai tout de même passé pas mal de temps sur StackOverflow les premiers mois.  
Mais ce ce qui me concerne l'effort en valait réellement la peine, et je ne saurais que recommander chaudement à n'importe quel confrère développeur PHP de tenter l'aventure !

