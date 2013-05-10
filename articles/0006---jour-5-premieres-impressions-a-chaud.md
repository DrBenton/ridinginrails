Bonjour !

Maintenant que tout ce qui concerne l'installation et la configuration d'un environnement Ruby on Rails est terminée, je peux m'adonner à plein temps au vif du sujet, l'utilisation de Rails.  
Après une journée passée à débuter ma première application RoR, voici à chaud (et un peu en vrac) mes premières impressions sur l'utilisation du langage Ruby et du framework Rails.

### Lisibilité du code

Je commence par la surface : la lisibilité du code. Pour ma part je n'ai jamais fait de C ou de C++, mais je suis cependant habitué à des langages qui en ont la syntaxe : PHP, JavaScript, ActionScript, Perl, Java... Hormis quelques applications que j'ai réalisées en TCL/Tk par-ci par-là, je ne me suis guère aventuré en territoire inexploré de ce coté-là.

Par conséquent, j'ai encore un peu de mal à lire du code Ruby : mon oeil continue de chercher ses familières accolades ouvrantes et fermantes et ses parenthèses. Lors de l'écriture du code Ruby, en revanche, pas de problème : il est même très plaisant d'utiliser un langage débarrassé de toutes ces décorations, et on se concentre vraiment sur les méthodes elles-mêmes.  
Cela étant, je pense que c'est uniquement une question d'habitude (j'ai bien réussi à être presque totalement à l'aise avec CoffeeScript), et j'imagine qu'avec le temps la lecture de code Ruby me sera aussi naturelle que celle des autres langages auxquels je me suis habitué.

Une autre "prise" manque encore pour accrocher mon regard dans la compréhension du code que j'ai sous les yeux : les <code class="language-ruby">return</code>. Le mot-clef est présent en Ruby, mais on l'utilise beaucoup moins qu'ailleurs. En effet, une méthode retourne **toujours** la dernière expression évaluée en son sein. En conséquence de cela, on omet bien souvent le _return_ lors des retours de méthodes "normaux", et on ne l'utilise généralement que pour mettre fin au déroulement d'une méthode de manière anticipée. Par exemple :
<pre><code class="language-ruby">
  def groups
    return @groups if defined? @groups

    if serialized_groups.nil? #no group, stop right here dude!
      @groups = []
      return @groups
    end

    logger.debug "*************** let's PHP-unserialize the user groups..."
    @groups = ::PHP.unserialize serialized_groups
    @groups.map!(&:to_i) #we only want integers in our groups IDs :-)
  end
</code></pre>
Cette méthode, directement copiée-collée d'un de mes premiers Modèles Rails (soyez indulgents, amis Rubyistes, s'il vous paraît peu adapté aux us et coutumes du langage :-), utilise le mot-clef _return_ deux fois pour interrompre le déroulé de la méthode _groups_.  
Dans le cas où on exécute tout le code de la méthode, en revanche, vous pouvez voir qu'il n'y a pas de _return_ final : c'est donc la valeur de la variable d'instance <code class="language-ruby">@groups</code> qui va être retournée. en même temps que celle-ci est modifiée par la méthode <code class="language-ruby">map!</code> (par convention, le nom de la plupart des méthodes qui changent la valeur de l'objet sur lequel elles sont utilisées finit par un point d'exclamation ; ça fait un peu bizarre au début, mais à l'usage c'est vraiment super).  

En relisant mon code à l'occasion de la rédaction de cet article, je m'aperçois d'ailleurs que j'ai encore laissé traîner des _return_ par-ci par-là en fin de méthodes : ah, on ne change pas ses habitudes en un jour :-)

Le langage Ruby en lui-même est vraiment très agréable à utiliser, et assez souvent le "principe de moindre surprise" voulu par son créateur fonctionne assez bien : on a pas souvent besoin d'aller examiner la documentation de l'API du langage, tant les noms de méthodes sont logiques et cohérents (alors que, sans vouloir troller, après plus de 10 ans de PHP je ne me souviens pas toujours si c'est _utf8encode()_ ou _utf8_encode()_ :-)  

Il m'a fallu cependant passer au "snake_case" après des années de bons et loyaux services en "camelCase", mais ça n'est pas franchement traumatisant.  
A noter d'ailleurs : si Ruby utilise la "snake_case" pour les noms de fichiers et les noms de méthodes, on utilise bien la "camelCase" pour les noms de modules et de classes. C'est un peu étrange.


Je peux comprendre que le créateur de Ruby on Rails ait décidé de baser le projet Basecamp sur ce langage alors que rien ou presque n'existait alors en Ruby pour faire du Web : dans le livre [Getting Real](http://gettingreal.37signals.com/), il explique ce choix au chapitre "Choose tools that keep your team excited and motivated". Et en effet, Ruby me paraît être un bon choix pour avoir du fun en codant.

### _Design patterns_ : pas tout-à-fait les mêmes qu'en PHP

Une des premières choses qui m'a surpris dans l'utilisation de Ruby on Rails : les _design patterns_ utilisés ne sont pas les mêmes que ceux auxquels on a pu s'habituer lorsqu'on vient de frameworks comme Symfony 2, Silex, Zend Framework 2 ou Laravel.  
Les 3 frameworks susdits s'architecturent en effet beaucoup sur les _design patterns_ "Service Locator" et "Dependency Injection", et puisqu'ils sont à la base plus ou moins directement inspirés de Ruby on Rails je m'attendais à les retrouver dans ce dernier.
 
Eh bien pas du tout, d'après ce que j'ai pu voir jusqu'ici, "Service Locator" et "Dependency Injection" ne sont pas utilisés dans RoR.  
Au lieu de cela, j'ai l'impression que le framework est massivement basé sur une fonctionnalité offerte par Ruby, les _mixins_ (équivalents des _Traits_ en PHP 5.4+).  

Lorsque j'ai une fonctionnalité à ajouter à mon appli Rails, donc, je ne vais pas créer un "service" accessible via un "service locator", comme je le ferais avec un de ces frameworks PHP. Au lieu de cela, d'après ce que j'ai pu comprendre il y a 2 choix possibles :

* Créer un Gem pour ma fonctionnalité, et l'ajouter au fichier "Gemfile" de mon application. Cela revient à peu près en PHP à créer un module publié via Composer, et à l'ajouter au "composer.json" du projet. Cette approche nécessite cependant d'avoir pas mal de temps de temps devant soi, puisque le code va être assez indépendant de l'appli et qu'il faut "packager" sa Gem comme il faut, avec tests unitaires et tout le toutim. Ca permet certes d'être sûr que le code est propre, mais en phase de prototypage ou de livraison-à-rendre-pour-hier on a pas forcément ce luxe.  

* Créer sa fonctionnalité dans un module à part, placé dans le répertoire "lib/" de son appli Rails. Il faut alors activer ce code au démarrage de l'appli Rails, via un _initializer_. 

C'est cette dernière approche que j'ai pour le moment choisi. Pour la première fonctionnalité annexe que j'ai dû créer (un pont applicatif entre mon appli Rails et le code PHP pré-existant) , j'ai donc ajouté un fichier "lib/acme/php_bridge.rb", avec la structure suivante :
<pre><code class="language-ruby">
module Acme

  module PhpBridge

    def check_php_bridge
      logger.debug "check_php_bridge()" if Rails.env.development?
      # ici du code...
    end

    private

      # ici mes méthodes privées
      # ...
      # ...

  end
  
end
</code></pre>

Pour rendre cette fonctionnalité disponible dans mes Contrôleurs, j'ai ajouté un fichier "require_acme_libs.rb" dans le répertoire "config/initializers/", avec ce contenu :
<pre><code class="language-ruby">
require 'acme/php_bridge'
</code></pre>

Pour utiliser ma fonctionnalité "check_php_bridge", enfin, je n'ai plus qu'à ajouter le code suivant en début de mes Contrôleurs visés :
<pre><code class="language-ruby">
class MyController &lt; ApplicationController

  # Acme custom actions
  include Acme::PhpBridge
  before_filter :check_php_bridge
  
end
</code></pre>

Et d'après ce que j'ai pu comprendre, dans RoR tout fonctionne ainsi, utilisant au mieux la possibilité qu'offre le langage Ruby, à travers les _mixins_ et la possibilité qu'ont les classes de se modifier elles-mêmes au _runtime_ pendant leur définition même (comme on peut le faire avec JavaScript, d'ailleurs).

Pour le "PHPiste" que je suis, cependant, cela ne va pas sans me poser pour le moment un léger soucis de compréhension du code, je l'avoue. J'ai en effet le sentiment qu'il y a beaucoup de "magie" dans Rails.
 

### La magie dans le code Ruby on Rails

#### Préambule : la magie dans les frameworks PHP

Je me souviens de mes premiers pas avec Zend Framework (première édition) en 2009 :
> "Mais là, on appelle la méthode "$this->getUser()" dans le Contrôleur alors que cette méthode n'existe pas ?  
> - C'est normal, c'est le fonctionnement de ZF, elle va être résolue en PHP via les [méthode magique](http://php.net/manual/fr/language.oop5.magic.php), qui vont déléguer ça automatiquement à un Helper"

J'étais abasourdi. Les différents composants applicatifs étaient presque tous reliés par des méthodes magiques, et si vous n'aviez pas la documentation du Zend Framework en livre de chevet il était impossible de s'y retrouver.  

Depuis j'ai bossé sur de nombreux projets avec le ZF et je m'y suis habitué, ça va mieux merci :-)  
Avec Symfony 2 ily a également une part de magie, mais elle est assez facilement pistable. Lorsqu'on utilise <code class="language-php">$this->get('mailer')</code>, il n'est pas forcément évident de savoir de prime abord quelle instance de quelle classe va être retournée. Heureusement, la console Symfony 2 permet de voir le mapping de tout ce petit monde :
<pre><code class="language-bash">php app/console container:debug
</code></pre>

#### _Mixins_ partout = magie ?

Dans Ruby on Rails, en revanche, je n'ai pas trouvé d'outil analogue. La console Rails est fantastique (j'en parlerai plus bas), mais puisqu'il n'y a pas de "Service Container" centralisé elle est bien incapable d'après ce que j'ai pu voir de nous donner un listing équivalent.  

Pour moi qui aime bien savoir comment fonctionnent grosso-modo les outils que j'utilise, il n'est pas facile de savoir à quels éléments on se réfère lorsqu'on utilise toutes ces fonctionnalités issues des dizaines (centaines ?) de _mixins_ utilisés par Rails - auxquels viennent s'ajouter ceux des différentes Gem utilisées, ainsi que ceux développés par mes soins pour les besoins propres de l'application.

J'ai pour ma part fait l'achat d'un licence de RubyMine, qui est "Rails-aware" comme Jean-Claude, et qui me permet fort sympathiquement d'accéder au code source d'une méthode de _mixin_ par un simple "Ctrl+clic" sur la méthode en question.  
Mais sans RubyMine, je serais bien en galère je pense pour naviguer dans le code source de Rails et des Gems que j'utilise. Lorsque je vois la méthode "before_filter" par exemple, abondamment utilisée dans Ruby on Rails, comment savoir de quelle classe Ruby vient diable cette fonctionnalité ?

Vous me répondez peut-être par un lapidaire "RTFM", et vous n'aurez pas tort. Mais ça prend toujours un peu de temps, et même une fois l'animal localisé il n'est pas toujours évident de bien le comprendre : [http://apidock.com/rails/AbstractController/Callbacks/ClassMethods/before_filter](http://apidock.com/rails/AbstractController/Callbacks/ClassMethods/before_filter).  
Heureusement, les "Guide Rails", dont je parlais dans un précédent article, sont bien faits et assez exaustifs, ce qui pallie en partie au problème.  

Mais pour ma part, j'avoue que pour le moment j'ai tout de même une vision "wild wild west" du code Rails, dans la mesure où l'utilisation intensive des _mixins_ provenant de dizaines de classes rend difficile la tracabilité du code pour le développeur PHP que je suis.  
J'imagine qu'avec la connaissance du framework ça s'arrangera, tout comme je me suis fait (et que j'ai vite appris à aimer) à Zend Framework à l'usage, mais en attendant je suis bien content d'avoir RubyMine :-)  
_(et non, je n'ai pas d'actions chez JetBrains)_

### La documentation

Un petit mot également sur les documentation Ruby et Rails.  

[Celle de Ruby](http://ruby-doc.org/core-2.0/) est un peu austère à mon goût lorsqu'on débarque sur la page d'accueil, mais on est pas là pour s'amuser me direz-vous. Hormis cela, elle n'est pas aussi complète que celle de PHP (qui reste à mon goût une référence en la matière, et qui m'a redonné goût à la vie lorsque je l'ai découverte après avoir tant galéré sur celle de Perl :-) mais elle fait le taf.  

Ah tiens, tant que j'y suis, petit apparté : faites attention si comme moi vous pensez naïvement pouvoir faire un "chercher-remplacer" dans une chaîne de caractères avec la méthode [replace](http://ruby-doc.org/core-2.0/String.html#method-i-replace) : c'est un faux ami, et elle n'a rien à voir avec son homonyme JavaScript ou la "str_replace" de PHP.  
Pour faire cela en Ruby, d'après ce que j'ai pu voir on utilise soit <code class="language-ruby">my_string["hello"] = "world"</code> pour remplacer la première occurrence de "hello", ou <code class="language-ruby">my_string.gsub(/hello/, "world")</code> pour y remplacer toutes les occurrences.  
C'est le seul exemple de violation du "principe de moindre suprise" que j'ai constaté pour le moment.  
Fin de l'aparté.

Pour Rails, je me réfère plutôt aux "Guides Rails" qu'à l'API, trop confuse à mon goût (toujours à cause de la multiplication des _mixins_).  
Et pour tous les petits détails qui ne sont pas dans les guides, Stack Overflow - où les Rubyistes sont bien présents - est toujours notre ami.

Il reste malgré tout quelques zones d'ombres que je n'ai pas encore pu éclaircir. Par exemple, je n'ai pu trouver nulle part la liste des variables accessibles depuis les Vues Rails (au sein des fichiers de templates ERB par exemple, ou dans les Helpers de Vue). J'ai apparemment accès au Contrôleur via la variable/helper "controller", à son nom avec "controller_name", aux paramètres via "params", etc.  
Mais c'est en fouillant Stack Overflow que j'ai pu extraire ces quelques informations, et ça m'embête pas mal de devoir ainsi aller à la pêche aux données sans avoir une référence simple me détaillant ce à quoi j'ai accès depuis les Vues.

### La console Rails, un ami pour la vie

Je finirai ces premières impressions par une très bonne surprise : la console Rails !  
On lance la bête en tapant une simple commande :
<pre><code class="language-bash">rails console # ou "rails c", en version abrégée
</code></pre>

On se retrouve alors dans une console interactive Ruby, semblable en tout point à celle livrée par défaut avec Ruby, [irb](http://fr.wikipedia.org/wiki/Interactive_Ruby). Sauf qu'ici on a accès à nos classes Rails directement, ce qui permet de les tester à loisir.  
Ceci est notamment très utile pour le Modèle, afin de vérifier les requêtes SQL qui sont envoyées à la base de données lorsqu'on utilise les classes ActiveRecord de RoR : toutes les requêtes SQL sont en effet affichées en direct dans la console Rails chaque fois que l'une d'elle part vers la base.

Grâce à cette console j'ai pu vérifier très rapidement que la solution de cache que j'avais mise en place pour la table des "groupes d'utilisateurs" de mon application (dont le contenu n'a pas bougé d'un iota depuis des années dans l'application PHP à laquelle je vais greffer ma première application Ruby on Rails) fonctionnait bel et bien :
<pre><code class="language-ruby">
class UserGroup &lt; ActiveRecord::Base

  attr_accessible :name

  # le nom de la table a été préfixé dans l'appli PHP
  self.table_name = :tl_user_group
  # table non gérée par Rails ; on désactive (à regret) les "timestamps" automatiques
  self.record_timestamps = false

  # la table contient beaucoup d'infos superflues, dont je ne veux pas par défaut
  default_scope :select=> 'id, name' 

  # quelques constantes pour la logique applicative dans les autres Modèles
  GROUP_SUPER_ADMIN = 1
  GROUP_ADMIN = 2
  GROUP_USER = 3
  GROUP_USER_ADVANCED = 4

  # clef pour le cache des données
  USER_GROUPS_CACHE_KEY = :tl_user_group_cache_key
  # cette constante n'a pas vocation à être utilisée ailleurs que dans cette classe
  private_constant :USER_GROUPS_CACHE_KEY

  # ici, l'utilisation du cache
  # @see http://guides.rubyonrails.org/caching_with_rails.html
  def self.all
    @@all ||= Rails.cache.fetch(USER_GROUPS_CACHE_KEY) do
      logger.debug "*************** let's retrieve the users groups... (they will be stored in a cache afterwards)"
      super
    end
  end

end

</code></pre>

Ici je procède à un double cache des données : un géré par Rails, l'autre par le fait que je stocke le résultat du "ActiveRecord::Base#all()" dans une variable statique de la classe (indiqué par le préfixe "@@"). Lorsque les applis RoR sont sur le serveur de production les classes sont réutilisées d'un client HTTP à l'autre, contrairement à PHP par exemple.  
Avec la console Rails j'ai pu vérifier que le premier appel à "UserGroup.all" lançait la requête SQL, et que tous les appels suivants utilisaient le cache - y compris lorsque je supprime la valeur du cache Rails.

En réalité je pense supprimer à terme le cache applicatif via la variable statique de classe ; même si cette approche me semble plus performante (pas de passage par Redis), elle me paraît moins souple, car si exceptionnellement le contenu de ma table change, le seul moyen dont je disposerai pour réinitialiser ma variable statique sera de redémarrer l'application Rails.  
Mais il était didactiquement intéressant de mettre cela en place :-)

Et par ailleurs, j'aime beaucoup la possibilité offerte par cette syntaxe :
<pre><code class="language-ruby">
  def some_data
    @computed_data ||= compute_data
  end
</code></pre>
Grâce à elle, on peut gérer un cache de données le temps de la requête HTTP avec beaucoup de concision, tout en restant clair.  
Ici, la méthode "compute_data" ne sera exécutée que la première fois que la méthode "some_data" sera appellée. Pour tous les appels consécutifs, c'est la valeur de la variable d'instance "@computed_data" créée lors du premier appel qui sera directement retourné.
 
### That's all folks!

Voilà qui clôt cet article.  Pour résumer mes premiers pas avec Ruby on Rails, je dirais donc :
 
* le langage Ruby est vraiment très plaisant à utiliser, sa concision demande un temps d'adaptation mais rend son utilisation très agréable 
* le framework Ruby on Rails confirme pour le moment l'impression de "puissance" qu'il me donnait, même si pour le moment je reste un peu frustré de ne pas tout comprendre à son fonctionnement et que je trouve que l'utilisation intensive des _mixins_ ne facilite pas la compréhension du code
* la documentation du langage Ruby et celle du framework Rails sont plutôt bonnes (mention spéciale aux "Guides Rails"),même si je les trouve un peu en-deça de celle de PHP. La communauté Ruby est bien présente sur les Stack Overflow et consorts, c'est appréciable.
* le langage Ruby est **vraiment** très plaisant à utiliser
* Ruby on Rails est **vraiment** très puissant

Sur ce je vous laisse, je retourne faire joujou avec mes Modèles dans la console Rails :-)

A bientôt ! 




