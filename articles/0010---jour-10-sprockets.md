Bonjour !

Aujourd'hui je m'attaque à la découverte et à la personnalisation de [Sprockets](https://github.com/sstephenson/sprockets), le gestionnaire d'_assets_ de Ruby on Rails : celui-ci permet d'ajouter des traitements automatisés aux fichiers CSS et JavaScript de nos application Rails. C'est la "[Asset Pipeline](http://guides.rubyonrails.org/asset_pipeline.html)".

Si vous avez déjà travaillé avec Symfony 2 vous vous êtes déjà très probablement frotté à [Assetic](https://github.com/kriswallsmith/assetic), son équivalent PHP. Je ne suis pour ma part pas un grand fan à la base de ce genre de solutions, puisque je préfère généralement maîtriser la chaîne des _assets_ de A à Z.  
Mais puisque Rails 3 est livré par défaut avec Sprockets activé (ce ne sera apparemment plus le cas pour Rails 4, où Sprockets sera devenu une entité optionnelle à part entière) et que j'ai l'intention de découvrir dans les détails les rouages de RoR, je me suis prêté au jeu.

### CoffeeScript, SASS et consorts "hands-in-ze-pockets"

Le premier avantage de Sprockets est de pouvoir utiliser très facilement des surcouches à JavaScript et CSS, sans avoir à configurer quoi que ce soit.  

Concernant JavaScript, il y a encore 2 écoles, et celle de la "surcouche", qu'elle se nomme [CoffeeScript](http://coffeescript.org), [TypeScript](http://www.typescriptlang.org/Playground/), [Dart](http://www.dartlang.org/) ou [que sais-je encore](https://github.com/jashkenas/coffee-script/wiki/List-of-languages-that-compile-to-JS) est loin d'avoir gagné la bataille.  
Libre à chacun d'utiliser le "vrai" langage JavaScript ou l'une de ses surcouches. Pour ma part, je suis passé à CoffeeScript il y a un peu plus d'un an, et je trouve que ma productivité y a vraiment gagné.  

Pour les feuilles de style en revanche, il semblerait que plus personne aujourd'hui n'utilise encore le langage CSS tel quel. Le succès stratosphérique du kit de démarrage Twitter Boostrap et son utilisation de [LESS](http://lesscss.org/) y sont probablement pour beaucoup.  
Jusqu'ici j'utilisais LESS, mais puisque [SASS](http://sass-lang.com/) est le choix privilégié des Rubyistes (il est programmé en Ruby :-), qu'il n'est [pas dénué d'avantages](http://www.hongkiat.com/blog/sass-vs-less/) et qu'il est doté de sa propre surcouche sympathique ([Compass](http://compass-style.org/)) je vas donc opter pour SASS.

#### Tout ce ptit monde dans "app/assets"

Avec Sprockets on gère ses _assets_ (fichiers JavaScripts, feuilles de styles et images) dans le dossier "app/assets" et ses 3 sous-dossiers "images/", "javascripts/" et "stylesheets/". Ces 3 dossiers sont transparents : ils ne sont là que pour nous permettre d'organiser proprement nos assets, et ne seront pas visibles dans l'URL.  
Ainsi, un fichier "_app/assets/javascripts/config.js_" sera accessible à l'URL "_/assets/config.js_", et une feuille de style "_app/assets/stylesheets/common/footer.css_" à l'URL "_/assets/common/footer.css_".  

On écrit pas ces URLs en dur, mais on utilise à la place les _helpers_ de Vue que Rails met à notre disposition pour cela, comme <code class="language-ruby">stylesheet_link_tag</code>, <code class="language-ruby">javascript_include_tag</code> ou <code class="language-ruby">image_tag</code>. L'intérêt de la manoeuvre, comme avec Assetic en PHP, est d'avoir chaque asset appelé séparément en mode "development" et de les voir compactés en un seul fichier lorsqu'on est en mode "production" sans avoir à changer la moindre ligne de code.  
Tout ceci est expliqué en détail sur [le guide RoR correspondant](http://guides.rubyonrails.org/asset_pipeline.html).

#### Aparté : consulter les guides Rails sans connexion à Internet

Ho, j'en profite pour signaler une astuce très intéressante que j'ai découverte récemment. Il se trouve que je bosse en ce moment depuis l'étranger, et que j'aime me plonger dans l'atmosphère locale en allant bosser dans un café.   
Ce café dispose d'une connexion WiFi mais celle-ci est de médiocre qualité, et a une fâcheuse tendance à se couper de manière impromptue au moment où j'avais un besoin crucial de la documentation Rails.  
Grâce à RubyMine je peux circuler avec fluidité dans les classes RoR, à coups de "Ctrl+clic" sur les différents méthodes qui m'intéressent, et me servir de l'auto-documentation des fonctionnalités dont j'ai besoin. Mais la synthèse des Guides Rails est parfois tout de même bien utile.  

Alors que je m'étonnais qu'il n'existe pas de version téléchargeable de ces guides (hormis au format e-book), j'ai compris grâce à l'irremplaçable duo Google/StackOverflow qu'on pouvait tout simplement les générer en local !  
Pour cela, Rails a besoin de la gem [RedCloth](http://redcloth.org/). J'ajoute donc celle-ci à la section "development" de mon fichier Gemfile, je l'installe avec la commande <code class="language-bash">bundle install</code>, et je n'ai plus qu'à demander la génération des Guides Rails dans le dossier "_doc/guides_" de mon application :
<pre><code class="language-bash">rake doc:guides
</code></pre>
Yee-ah!

### Les moteurs Sprockets

Tout ceci est fort pratique, mais n'est que la base de Sprockets , qui permet donc d'utiliser des surcouches comme SASS ou CoffeeScript de manière très simples, grâces à différents _engines_.  
Chacun va en gros lire le fichier demandé, l'interpréter et renvoyer le résultat afin qu'il soit traité par l'_engine_ suivant si nécessaire.  

Chaque _engine_ est déclenché automatiquement par Sprockets lorsque telle ou telle extension de fichier est trouvée. Ainsi, pour faire du CoffeeScript on nommera notre fichier "_my_widget.js.coffee_" : cela indique à Sprockets qu'on souhaite qu'il déclenche le moteur CoffeeScript pour ce fichier avant de le livrer au navigateur. De même, une feuille de style "_my_widget.css.scss_" sera automatiquement passé à la moulinette SASS.

Et on peut même combiner différents moteurs sur un fichier ! Un fichier "config.js.coffee.erb" verra ainsi son contenu traité par ERB, ce qui permet de faire une passerelle avec Ruby, puis le résultat est transféré au moteur CoffeeScript.  
L'URL de l'asset, bien sûr, ne change pas : mon fichier "_app/assets/javascripts/app/config.js.coffee.erb_" sera toujours accessible via l'URL "_/assets/app/config.js_".

#### Un petit bug, en passant

Bien sûr, utiliser ERB partout dans ses fichiers Javascript ou CSS n'est pas une bonne idée, et il vaut mieux utiliser le mécanisme avec parcimonie.  
Pour ma part j'ai ajouté l'extension ".erb" à 2 fichiers seulement : un "_app/config.js_" qui me permet de passer certaines variables de config Ruby à JavaScript (je gère ces variables via la gem [Gaston](github.com/chatgris/gaston), simple et efficace), et un "_app/i18n.js_" afin de passer certains textes à JavaScript.

J'ai malheureusement rencontré un bug assez ennuyeux avec ce dernier, qui m'a tenu en haleine pendant pas loin de 2 heures.
J'ai donc un fichier "_app/assets/javascripts/amd-modules/app/admin/i18n.js.coffee.erb_", avec le contenu suivant :
<pre><code>define [], () ->
             
    i18n = {}
    
    i18n.title = &lt;%= I18n.t('title').to_json %&gt;
    
    i18n.app = {}
    i18n.app.admin = &lt;%= I18n.t('emailing.admin').to_json %&gt;
    
    i18n
             
</code></pre>

Lorsque je l'appelle via son URL "_/assets/amd-modules/app/admin/i18n.js_", j'ai malheureusement le vilain résultat suivant au lieu du contenu de mon fichier :
<pre><code>
throw Error("Encoding::InvalidByteSequenceError: \"\\xC3\" on US-ASCII ...
</code></pre>

Je ne sais qui d'ERB, CoffeeScript ou de Sprockets est responsable de cela, mais d'après ce qu'en comprends il y a un soucis avec le fait que mon fichier CoffeeScript n'utilise aucun caractère accentué (il est donc visiblement considéré comme encodé en "US-ASCII") alors qu'avec ERB j'injecte dedans des chaînes de caractères qui elles vont être encodées en UTF-8 comme il se doit.

La solution que j'ai trouvée après deux heures d'investigation manque de panache, mais elle est simple et résout le problème : ajouter une chaîne de caractère accentuée au fichier CoffeeScript, afin de forcer l'encodage du fichier lui-même en UTF-8 :
<pre><code>define [], () ->
             
    # forces UTF-8 encoding of the file (2 hours spent on this damned bug!!!)
    # hé !        
         
    i18n = {}
    
    # ... ici le même code que précédemment
    
    i18n
             
</code></pre>

La chaîne de caractères "hé" est un commentaire CoffeeScript, et n'apparaîtra même pas en commentaire dans le fichier JavaScript envoyé au navigateur. J'y retrouve bien à présent mes textes au format JSON.


### Ajouter son propre moteur

La mode est aux frameworks JavaScript MVC lourds comme Angular, mais je suis déjà en train d'ingurgiter beaucoup de choses avec mon apprentissage de Ruby et de Rails et je ne vais pas m'y mettre tout de suite. Je reste donc pour l'instant sur ce que je pratique depuis quelque temps, à savoir la combinaison [Backbone.js](http://backbonejs.org/) pour le MVC et [RequireJS](http://requirejs.org/) pour la modularité et la gestion automatisée des dépendances.

#### Une approche simple : des modules RequireJS chargés à la demande

J'ai en ce qui me concerne l'habitude d'utiliser un schéma simple basé sur RequireJS : certains de mes éléments HTML sont dotés d'un attribut "**data-amd-widget**", dont la valeur est le chemin d'un module RequireJS.  
Pour chacun de ces éléments HTML, le module correspondant est chargé, puis une méthode "createWidget()" de ce module est lancée, avec en paramètre l'élément HTML en question, enveloppé par jQuery.

J'avais découvert cette approche via [ce billet de blog](http://paceyourself.net/2011/05/14/managing-client-side-javascript-with-requirejs/), et j'en suis assez fan : c'est simple, modulaire, et cela permet le _lazy loading_ des fonctionnalités d'une page HTML.  
Concrètement j'ai donc mon module RequireJS principal, chargé en fin de page, qui va déclencher la "widgets factory" :
<pre><code class="language-coffeescript">
# fichier "app/assets/javascripts/amd-modules/app/main.js.coffee"

define (require, exports, module) ->
  
  $ = require "jquery"
  logger = require "util/logger"
  
  myDebug = !false
  
  myDebug && logger.debug "#{module.id} ::", "On the bridge, captain!"
  moduleActivator = require('util/widgets-factory')
  
  $(document).ready () ->
    moduleActivator.execute()
    
  exports

</code></pre>

Et la "widgets factory", donc :
<pre><code class="language-coffeescript">
# fichier "app/assets/javascripts/amd-modules/util/widgets-factory.js.coffee"

##
# @see http://paceyourself.net/2011/05/14/managing-client-side-javascript-with-requirejs/
##

define (require, exports, module) ->
  
  $ = require "jquery"
  logger = require "util/logger"
  
  myDebug = !false
  
  myDebug && logger.debug "#{module.id} ::", "on the bridge, captain!"
  
  loadModule = ($jqElement) ->
    moduleName = $jqElement.data("amd-widget")
    
    require [moduleName], (module) ->
      module.createWidget($jqElement)
  
  exports.execute = ($widgetsContainer) ->
    $widgetsContainer = $widgetsContainer || $("html")
    $dataModules = $("[data-amd-widget]", $widgetsContainer)
    nbDataModules = $dataModules.length
    
    myDebug && logger.debug "#{module.id} ::", "#{nbDataModules} widgets found."
    
    i = 0
    while i &lt; nbDataModules
      loadModule( $dataModules.eq(i) )
      i++
  
  exports

</code></pre>

Chaque module de "widget" peut ensuite à sa guise implémenter sa fonction "createWidget()" sous la forme d'une _Factory_ de Vue Backbone, ou bien de code JavaScript brut, etc. C'est une approche qui se veut pragmatique.

#### Le problème : utilisation des modules RequireJS en production

J'ai tout de même un soucis avec cette approche : comme vous le voyez j'utilise [la syntaxe "Common JS"](http://requirejs.org/docs/api.html#cjsmodule) des modules RequireJS, et l'identité de chaque module est déterminée automatiquement par RequireJS au _runtime_, selon le chemin du module. Cette manière de faire comporte 2 inconvénients :

* Les dépendances de chaque module, au lieu d'être clairement exposées avec la syntaxe RequireJS classique, doivent être déterminées par RequireJS via un "toString()" sur la fonction puis un _parsing_ à grands coups d'expression régulières : ça coûte du temps coté client.
* L'identifiant de chaque module étant déterminé au _runtime_ par le chemin du fichier JavaScript (e.g. l'identité du fichier "_app/assets/javascripts/amd-modules/util/widgets-factory.js.coffee_" est "_util/widgets-factory_"), tout s'écroule dès qu'on compacte ces fichiers en un seul pour le mode "production" avec la commande <code class="language-bash">rake assets:precompile</code>.

Pour y remédier, et également dans l'optique de mieux comprendre les rouages de Sprockets, j'ai donc créé mon propre moteur d'assets Sprockets.

#### Création du moteur, et liaison à une extension d'asset

Donner ici le code source du moteur lui-même n'a que peu d'intêret, et ce d'autant plus que le code n'est pas très propre. Si j'en ai le temps plus tard, je le nettoierai et l'empaqueterai sous forme de Gem.  
Mon propos est plutôt ici de montrer la démarche telle que je l'ai comprise à travers différentes sources sur le Net, afin de faire gagner du temps à ceux nouveau venus sur Rails comme moi qui voudraient aux aussi pouvoir utiliser ce type de mécanisme :

* Je crée le moteur en question, de préférence en le faisant hériter la classe "Titl::Template". Sa méthode "evaluate" sera automatiquement déclenchée par Sprockets le moment venu, avec en paramètre des objets me permettant d'effectuer mon traitement sur le fichier :
<pre><code class="language-ruby">
# fichier "lib/acme/sprockets/requirejs_template.rb"

require 'tilt'

module Acme
  
  module Sprockets
    
    class RequireJSTemplate &lt; ::Tilt::Template

      self.default_mime_type = 'application/javascript'

      CJS_DEFINE_REGEXP = /define\(\s*function\s*\(require,\s*exports,\s*module\s*\)\s*\{/
      CJS_REQUIRE_REGEXP = /[^.]\s*require\s*\(\s*["']([^'"\s]+)["']\s*\)/ # from r.js

      # Check to see if RequireJS env is loaded
      def self.engine_initialized?
        @initialized
      end

      # Setup RequireJS library. 
      def initialize_engine
        @initialized = true
      end

      def prepare
      end

      # Converts a  "CommonJS-style" RequireJS module into a flat one
      #
      # Something like
      #
      #   define(function(require, exports, module) {
      #     var config = require("app/config");
      #     var _ = require("vendor/lodash");
      #     var logger = require("app/logger");
      #     // ...
      #   }
      #
      # is converted into
      #
      #   define('my/module/path', ['require', 'exports', 'module' , 'config', 'underscore', 'logger'], function(require, exports, module) {
      #     // same function body
      #   }
      #
      def evaluate(scope, locals, &block)
        # Ici mon traitement, lui aussi à grands coups d'expressions régulières.
        # L'intêret est que chaque module va être agrémenté de son identifiant, même lorsque tout
        # le monde est compacté en un unique fichier JavaScript pour la production, et que les dépendances
        # sont exposées, ce qui évite à RequireJS de les déterminer au runtime.
        #
        # Ce traitement est effecué à chaque requête en mode "development", mais ne l'est pas
        # en mode "production" puisque chaque asset y est pré-généré via la commande
        # "rake assets:precompile".
        #
        # Pour le traitement on a accès au fichier ciblé via des variables comme "scope.logical_path" ou
        # "scope.pathname"
        #
      end
      
    end
    
  end
  
end

</code></pre>
* J'associe ensuite ce nouveau moteur à une extension de fichier donnée, ici ".reqjs" :
<pre><code class="language-ruby">
# fichier "config/initializers/sprockets.rb"

require 'sprockets'
require 'acme/sprockets/requirejs_template'

AcmeRails::Application.assets.register_engine '.reqjs',  ::Acme::Sprockets::RequireJSTemplate
</code></pre>

Et voilà ! Je n'ai plus qu'à ajouter l'extension en question à chacun de mes modules RequireJS, dans le dossier "_app/assets/javascripts/amd-modules/_". Cela donne des fichiers ".js.reqjs.coffee",  et le traitement sera effectué automatiquement pour chacun d'eux.  
Yeah!

### Ajouter ses fonctionnalités SASS

Pour finir, quelque chose d'un peu plus trivial mais qui peut servir. SASS étant codé en Ruby, il est très facile d'y ajouter ses propres fonctionnalités depuis son application Rails.

Dans mon cas, j'ai besoin de faire référence à une URL externe, celle de l'application PHP avec laquelle je dois interagir, et chez qui je vais piocher certains assets. Plutôt que de mettre cette URL en dur dans mes fichiers CSS, je vais donc créer une nouvelle fonction SASS, reliée à une méthode Ruby :

* Je crée dans mes _initializers_ RoR une classe contenant mes nouveaux _helpers_ SASS :
<pre><code class="language-ruby">
# fichier "config/initializers/acme_sass_helpers.rb"

module Sass
  module Rails
    module Helpers
      
      def typolight_admin_asset_url(path)
        Sass::Script::String.new("url(#{Gaston.acme.typolight_admin_assets_path}#{path.value})")
      end
      
      def rails_asset_url(path)
        Sass::Script::String.new("url(#{Gaston.acme.rails_assets_path}#{path.value})")
      end
      
    end
  end
end
</code></pre>
* Puis, dans mes fichiers SASS, j'ai juste à utiliser des fonctions dont le nom est celui des méthodes que j'ai définies dans ma classe d'_Helpers_, en remplaçant les underscores par des tirets :
<pre><code class="language-css">
// ...
header {
  border-bottom: 1px solid #ccced0;
  background: #fff typolight-admin-asset-url("images/bgBody.gif") repeat-x;
}
// ...
.save-bt {
  background-image: rails-asset-url("common/icons/document-save.png");
}
// ...
</code></pre>

C'est simple et bigrement pratique :-)

### Ma conclusion sur Sprockets

J'ai vu ces fonctionnalités fortement décriées ça et là, ce qui a je pense eu pour conséquence le retrait de Sprockets de la future version 4 de Ruby on Rails : il sera toujours possible de l'utiliser, bien entendu, mais elle ne sera pas incluse et activée par défaut comme c'est le cas avec la version 3.

Pour ma part, j'avoue que conceptuellement je trouve qu'il n'est peut-être pas des plus élégant de trop lier ainsi des fonctionnalités ou des données d'assets à des fonctionnalités Ruby, mais en pratique force est de reconnaître qu'il est bien commode d'avoir un tel couteau suisse sous la main.  

Mes premiers assets ont d'ailleurs été trop chargés en fonctionnalités ERB ça et là, et je me suis vite aperçu qu'il n'était pas forcément judicieux de coller de l'ERB partout : c'est certes bien tentant, mais il faut résister à l'envie d'en mettre partout afin de conserver une séparation des responsabilités au sein de l'application.  
Je me suis donc limité à l'utilisation d'ERB dans mes assets pour 2 fichiers JavaScript seulement (la config et l'internationalisation), et pour le reste j'utilise les moteurs Sprockets et la possibilité d'ajouter des fonctionnalités SASS.

Même si des gems existent pout utiliser des fonctionnalités comme [Yeoman](http://yeoman.io/) dans Rails, j'ai le sentiment qu'il est préférable de choisir entre un outil comme Yeoman **ou** Sprockets, mais que combiner les 2 peut donner naissance à quelque chose de bien compliqué.  

Dans ce cadre, je ne sais si je continuerai à utiliser Sprockets à l'avenir, mais aujourd'hui il résout bien mes problématiques et c'est tout ce que je demande à un tel outil.  
Je continuerai donc allègrement de lui confier mes assets pour ce premier projet Ruby on Rails ! :-)



