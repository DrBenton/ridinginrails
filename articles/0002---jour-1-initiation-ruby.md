Bonjour !

Comme je l'expliquais dans [mon introduction](http://ridinginrails.tumblr.com/post/48532076563/intro-motivations-pour-passer-de-php-a-ruby-on-rails) à ce blog, me voici donc décidé à investir le temps qu'il faudra pour me mettre à [Ruby on Rails](http://rubyonrails.org/).
Première étape : apprendre le langage [Ruby](http://www.ruby-lang.org/fr/) !


### Apprendre Ruby quand on vient de PHP et JavaScript

Ruby peut paraître un bien étrange animal lorsqu'on l'approche pour la première fois et qu'on a été comme moi principalement habitué à des langages dont la syntaxe ressemble à celle du C - comme PHP, JavaScript, Java... L'oeil s'est habitué à se repérer aux accolades, aux parenthèses et aux points-virgules.  
En Ruby, rien de tout cela n'est présent ! On a bien la possibilité d'utiliser les parenthèses pour définir les méthodes et les fonctions, mais elles sont facultatives lorsqu'on les appelle. Au début, c'est assez perturbant :-)

#### Surmonter l'appréhension des blocs

Autre point qui m'a fortement rebuté au départ : les blocs. Cette étrange construction syntaxique, commençant par un <code>do</code>, se poursuivant avec un ou plusieurs noms de variables entre _pipes_ <code>|</code> et finissant par <code>end</code> me paraissait bien mystérieuse, jusqu'à ce que je finisse par comprendre que c'est plus ou moins un manière d'écrire une fonction anonyme - une _Closure_.  
Concept qui nous est en réalité très familier lorsqu'on fait du JavaScript ou du PHP 5.3+.

Ainsi, ce bloc en Ruby :
<pre><code class="language-ruby">[1, 2, 3].each do |i|
  puts i
end</code></pre>
est tout simplement équivalent au code JavaScript suivant :
<pre><code class="language-ruby">[1, 2, 3].forEach( function(i) {
  console.log(i);
} );</code></pre>

#### Tutoriaux Ruby

Pour se mettre en douceur mais avec efficacité à Ruby, j'ai utilisé les ressources suivantes, à peu près dans cet ordre :

- [Ruby en vingt minutes](http://www.ruby-lang.org/fr/documentation/quickstart/), sur le site officiel de Ruby

- sur ce même site, les pages [De PHP à Ruby](http://www.ruby-lang.org/fr/documentation/ruby-from-other-languages/to-ruby-from-php/) et [De Java à Ruby](http://www.ruby-lang.org/fr/documentation/ruby-from-other-languages/to-ruby-from-java/) valent également la peine d'être consultées

- une fois ce rapide tour du propriétaire fait, il est temps de commencer à taper du code Ruby. Et avant d'avoir à l'installer, quoi de mieux que de jouer avec dans le navigateur ?  
  Je ne peux que chaudement recommander le site [Try Ruby](http://tryruby.org/), qui permet de bien comprendre les bases du langages en une petite demie-heure. Il existe d'autres sites dans le même genre, comme _Ruby Monk_, mais j'ai trouvé que ce dernier par exemple franchissait les paliers de difficulté de manière un peu abrupte parfois.  
  Pour la petite histoire, _Try Ruby_ est à la base l'oeuvre d'un développeur Ruby qui a apparemment fait figure d'ovni il y a quelques années, connu uniquement sous le nom "_why". Cet article à son sujet est très intéressant, je vous invite à le lire si vous avez un peu temps : [Ruby: Où est le programmeur _why?](http://www.slate.fr/story/60473/ou-est-why-ruby)

- on peut également s'amuser avec la console interactive de Ruby via le navigateur, grâce au projet [Repl.it](http://repl.it/languages/Ruby), qui a "porté" pas mal de langages de script (dont Ruby, donc) en JavaScript grâce à [emscripten](http://emscripten.org/).

Une fois cette petite heure consacrée à une rapide approche du langage, on peut enfin passer à l'installation.

### Installer Ruby

Si vous êtes sous Linux ou Mac, pas de problème, votre machine est vraisemblablement déjà équipée de Ruby. Vous pouvez vous en assurer en tapant <code>ruby -v</code> en ligne de commande.

Sous Windows, c'est déjà plus problématique.  
Il existe bien le site [Ruby Installer](http://rubyinstaller.org/), qui nous fournit un environnement Ruby prêt à l'emploi pour Windows. Mais les problèmes en ce qui me concerne ont commencé lorsque j'ai voulu faire communiquer ma première application _Rails_ avec mon serveur MySQL : Ruby en est tout à fait capable, bien sûr, mais il lui faut pour cela compiler une DLL de la bibliothèque MySQL.  
Procéder à des compilations n'est qu'une formalité sous Linux, mais sous Windows nous n'avons pas accès si facilement aux outils de développement et aux _headers_ des bibliothèques tierces, et cela peut se révéler bien galère.

Il y a des solutions (comme [celle-ci](http://blog.mmediasys.com/2011/07/07/installing-mysql-on-windows-7-x64-and-using-ruby-with-it/) par exemple), mais j'avais envie de me mettre rapidement dans le bain et non de passer une heure à installer ou compiler une bête DLL d'accès à MySQL.

**EDIT** : on me souffle [à l'oreillette](http://ridinginrails.tumblr.com/post/48605023304/jour-1-sinitier-a-ruby#comment-872560082) que [Rails Installer](http://railsinstaller.org/) et quant à lui livré avec vraiment tout ce qu'il faut pour le développement Rails sous Windows.  
Même si pour ma part je pense a priori rester sur mon *workflow* à base de Vagrant (que j'ai adopté car je ne connaissais pas Rails Installer), cet installeur "tout en un" pourra effectivement peut-être faire gagner du temps à certains lecteurs :-)

#### Vagrant

J'ai finalement opté pour... [Vagrant](http://www.vagrantup.com/). Cet outil, pour peu que vous ayez au préalable installé le gestionnaire de machines virtuelles [VirtualBox](https://www.virtualbox.org/), permet de gérer en ligne de commande une collection de machines virtuelles pré-configurées. Vagrant est codé en Ruby, on reste dans le thème ^_^

J'ai donc installé Vagrant (qui est livré sous Windows avec son propre interpréteur Ruby), puis j'ai téléchargé la machine virtuelle "precise32" (tout ceci est très bien expliqué sur [le site de Vagrant](http://www.vagrantup.com/)).  
Une fois celle-ci téléchargée, je me suis connecté en SSH à cette Ubuntu Precise et j'ai pu commencer à paramétrer mon environnement Ruby proprement.

**Même si vous êtes sous Linux ou Mac, et que vous avez déjà votre propre environnement Ruby prêt à l'emploi, il peut se révéler judicieux d'utiliser Vagrant**. Vous avez ainsi une machine virtuelle complètement dédiée au développement Ruby, et si vous bossez en équipe cela permet à vos confrères de travailler avec exactement le même environnement que vous.

Si vous installez Vagrant sous Windows vous trouverez d'ailleurs des documentations Ruby qui peuvent être bien utiles, aux formats PDF et CHM, dans le répertoire <code>C:\vagrant\vagrant\embedded\doc</code>.

Par la suite je me suis d'ailleurs fait ma propre "box" Vagrant avec RVM, Rails et le client MySQL pré-installés, afin d'avoir à l'avenir sous la main une VM "Ruby" prête à l'emploi ; mais... c'est une autre histoire :-)


### Configurer proprement son environnement  Ruby

Les pros du Ruby me contrediront peut-être, mais d'après ce que j'en ai compris à ce stade-là il est intéressant de ne pas forcément utiliser la version de Ruby "brute" installée sur votre OS (ou votre machine virtuelle, si vous utilisez Vagrant).

En effet, en Ruby on va installer tout un tas de [Gems](https://rubygems.org/), qui sont l'équivalent des packages de [Packagist](https://packagist.org) / [Composer](http://getcomposer.org/) en PHP ou des modules de [NPM](http://npmjs.org) en Node.js - c'est d'ailleurs bien de ce système de Gems que les deux systèmes pré-cités se sont inspirés :-)

Ces Gems, contrairement à Composer ou NPM, ne sont pas installées dans le répertoire local de votre application Ruby mais à un niveau global (comme cela se faisait avec [Pear](http://pear.php.net/) en PHP). Du coup, si sur une de vos applis Ruby vous avez besoin d'utiliser la version 3.2 d'une Gem quelconque et que sur une autre de vos applis vous devez absolument utiliser la version 2.8 ce cette même bibliothèque, vous allez avoir un soucis.
Et si une application Ruby nécessite la version 1.8 de Ruby alors que l'autre a besoin de la 2.0, c'est encore pire bien entendu.

#### Installer RVM

Pour résoudre ce problème, Ruby nous propose [RVM](https://rvm.io/), le Ruby Version Manager !  
Avec RVM vous allez pouvoir installer et gérer plusieurs versions de Ruby sur votre système, chacune étant dotée de sa propre collection de Gems. Et si vous en avez le besoin, chaque version de Ruby peut également être dotée de plusieurs ensembles de Gems, les "[Gem Sets](https://rvm.io/gemsets/basics/)".  
Si vous avez déjà utilisé le [n](https://github.com/visionmedia/n) du "Node.js Guru" TJ Holowaychuk pour Node.js, vous connaissez déjà le principe.

Qui plus est, RVM installe les différentes versions de Ruby dans votre répertoire utilisateur. Vous pouvez donc gérer vos Gems en toute tranquillité sans avoir à passer par le compte _root_ pour la moindre opération.

Plutôt que de mettre à jour la version de Ruby de mon système avec <code>apt-get</code>, j'installe donc RVM (cf. [cette page](https://rvm.io/rvm/install/), qui après m'avoir fait télécharger tout un tas de packages va chercher les sources de la dernière version de Ruby et la compile. Quelques minutes plus tard, un <code>ruby -v</code> et un <code>rvm info</code> me confirment que j'utilise bien à présent une version de Ruby propre à mon utilisateur, et non celle du système.

En tapant <code>irb</code> je peux jouer avec la console interactive de Ruby, cette fois sur mon système. Ca y est, Ruby est fin prêt !

**EDIT** : un lecteur m'a signalé que l'on pouvait avoir plusieurs versions d'une même Gem sur une seule install Ruby, sans avoir forcément recours aux "Gem Sets" de RVM. Merci à lui pour ce conseil de Rubyiste ! :-)  
> http://ridinginrails.tumblr.com/post/48532076563/intro-motivations-pour-passer-de-php-a-ruby-on-rails#comment-872182153

**EDIT BIS** : je me suis finalement tourné vers [rbenv](https://github.com/sstephenson/rbenv/), qui semble être un meilleur choix que RVM pour gérer ses différentes versions de Ruby.   L'installation de _rbenv_, que j'ai détaillée [ici](http://ridinginrails.tumblr.com/post/49375087294/jour-3-faire-tourner-ruby-on-rails#rbenv), est cependant un chouilla plus complexe, alors si comme moi vous voulez avoir une gestionnaire de versions de Ruby mais que vous ne voulez pas passer trop de temps pour le moment à l'installer, vous pouvez rester avec RVM.

### Premiers pas avec Ruby

A ce stade-là vous pouvez, avec les différentes documentations Ruby sous le coude commencer à créer vos propres fichiers "Hello world", créer des scripts Ruby qui lisent et modifient des fichiers, faire vos premières classes Ruby.

Pour travailler en Ruby vous avez une foultitude d'éditeurs à votre disposition. Pour ma part je suis un grand fan des produits de Jetbrains, aussi me suis-je tourné vers [RubyMine](http://www.jetbrains.com/ruby/index.html) - payant, certes, mais peu onéreux.  
Mais que vous soyez plutôt [Sublime Text](http://www.sublimetext.com/), [Netbeans](http://netbeans.org) (avec [le plugin ad hoc](http://plugins.netbeans.org/plugin/38549/ruby-and-rails)), [Vim](http://www.synbioz.com/blog/l_utilisation_de_vim) ou autre il est fort probable que le logiciel que vous utilisez pour coder en PHP et JavaScript supporte également Ruby :-)

Une fois dans le code, en ce qui me concerne les points suivants m'ont posé quelques soucis de compréhension au début.

#### Les fonctions globales

En Ruby absolument tout n'est qu'objet, nous dit-on.  
Mais il existe quand même des "fonctions globales", comme <code>print</code>, que vous pouvez utiliser sans objet. M'aurait-on menti sur la nature "tout objet" de Ruby ? :-)  
Après recherche sur le sujet, il se trouve que toutes les méthodes de la classe [Kernel](http://ruby-doc.org/core-2.0/Kernel.html) sont affectées à absolument tous les objets, et sont accessibles dans n'importe quel contexte - ce qui revient à les avoir à disposition depuis n'importe où, comme des fonctions globales effectivement.

#### Les _mixins_

En regardant divers tutoriaux j'ai vu passer pas mal d' <code class="language-ruby">include MyModule</code> dans les classes Ruby.
Il se trouve qu'en Ruby on utilise visiblement beaucoup ce qu'en PHP 5.4 on appelle les [Traits](http://php.net/manual/fr/language.oop5.traits.php), et ce qu'en JavaScript on appelle plutôt les [mixins](http://javascriptweblog.wordpress.com/2011/05/31/a-fresh-look-at-javascript-mixins/) - que vous avez peut-être déjà utilisé avec [Underscore.js](http://underscorejs.org/#mixin) ou [Ext JS](http://docs.sencha.com/extjs/4.0.7/#!/api/Ext.Class-cfg-mixins) par exemple.

Ce système de "mixins par les modules" est expliqué en détail dans pas mal d'articles sur le Web, comme [ici](http://www.ruby-doc.org/docs/ProgrammingRuby/html/tut_modules.html) par exemple.

#### Extension de classes existantes

Avec Ruby vous pouvez à tout moment ajouter des méthodes à des classes existantes, comme vous le feriez en JavaScript en ajoutant des fonctions au prototype d'un objet. La syntaxe pour ce faire peut paraître un peu étrange, puisqu'on "ré-ouvre" la classe en question, mais on s'y fait.

Vous pouvez voir tout cela expliqué [là](http://juixe.com/techknow/index.php/2007/01/17/reopening-ruby-classes-2/) par exemple.

#### Les blocs et les lambdas

Pour moi - comme pour beaucoup de développeurs venant des langages de programmation de la famille du C j'imagine - les blocs Ruby m'ont paru très étranges au premier abord.  
Mais comme je le disais au début de cet article, il ne s'agit en réalité que d'une syntaxe permettant de définir des fonctions anonymes. Il y a d'ailleurs trois façon différentes de gérer ces _closures_ en Ruby.

Pour davantage d'information à ce sujet je ne peux que vous inviter à aller voir [cette présentation](http://matthieusegret.com/presentation-ruby-block-proc-lambda/) , qui nous explique tout cela en détail.

### Bilan de mes premiers pas avec Ruby

Après cette première journée passée à installer un environnement Ruby à peu près propre et à jouer avec les différentes possibilités du langage, je ne peux qu'être réjoui.

La syntaxe du langage, qui me paraissait affreuse il y a encore pas si longtemps (même si mon utilisation de [CoffeeScript](http://coffeescript.org/) depuis quelques mois m'avait préparé à celle de Ruby), me paraît à présent bien pratique.  
On ne s'encombre pas d'autant de signes "décoratifs" qu'avec PHP ou JavaScript, puisqu'on a très rarement besoin d'utiliser les parenthèses, les accolades et les points-virgules - même si j'avoue avoir continué à utiliser les parenthèses lors de l'appel d'une méthode, au début :-) Du coup, avec l'habitude le code apparaît plus clairement.  
L'utilisation du mot-clef <code class="language-ruby">end</code> pour délimiter les fins de méthodes, de classes et de blocs m'a paru bien peu sexy dans un premier temps (javais l'impression de faire du BASIC), mais je m'y suis fait et cela ne me choque plus.

Je ne vais pas faire le tour complet du langage dès maintenant (celui-ci est très riche !), mais après avoir vu ses principales caractéristiques je me sens à présent prêt pour découvrir __Ruby on Rails__.

Mais ceci fera l'objet d'un autre article ! :-)