Bonjour !

Dernière étape aujourd'hui avant d'attaquer le code proprement dit, et puis j'enchaîne sur mes premiers Contrôleurs, et la gestion du cache du code avec Ruby on Rails. C'est qu'après toutes ces ingestions de tutoriels Ruby/Rails et ces installations diverses de logiciels sur ma machine, je suis impatient à présent de faire enfin mes premières armes concrètes avec RoR !


### Une dernière installation de logiciel pour la route : Phusion Passenger

La [précédente journée](http://ridinginrails.tumblr.com/post/49375087294/jour-3-faire-tourner-ruby-on-rails) j'ai préparé mon environnement Ruy on Rails sur une machine virtuelle (gérée via [Vagrant](http://www.vagrantup.com/)), et je l'ai connecté à mon serveur MySQL.  
J'ai donné vie une première fois à mon application avec le serveur Web intégré à Ruby on Rails, **WEBrick**, qui m'a permis de voir en local sur le port 3000 la page d'accueil par défaut proposée par RoR, et un affichage d'infos dynamiques relatives à mon installation Rails (via une requête Ajax proposée par cette même page). Bien.  

#### WEBrick != production

Mais WEBrick n'est destiné à être utilisé qu'à des fins de tests en local, et il me faudra bien trouver une autre manière de faire vivre RoR sur un site Web réel, en production.  
Par ailleurs, il se trouve que la réalisation de ma première vraie application RoR va se dérouler dans un contexte un peu particulier, puisque qu'elle devra s'intégrer à un site Web existant, développé en PHP et connecté à une base de données MySQL déjà bien remplie, avec déjà plus de 200 tables au compteur.

#### Unicorn à la rescousse ?

Dans ce contexte, je ne vais a priori pas me baser sur le serveur Rails qui m'a semblé être le plus utilisé, d'après ce que j'ai pu voir sur le Net (et comme un lecteur me l'a confirmé [dans les commentaires](http://ridinginrails.tumblr.com/post/49375087294/jour-3-faire-tourner-ruby-on-rails#comment-881573945) :-) : **Unicorn**.  
Ce serveur applicatif est notamment celui utilisé par GitHub, qui en détaille ses bénéfices dans [cet article](https://github.com/blog/517-unicorn), ou encore par Twitter, qui là aussi chantait en 2010 [ses louanges](http://engineering.twitter.com/2010/03/unicorn-power.html). Il semble être un bon choix pour qui veut des performances et de la souplesse dans les mises à jour (entre autres choses, Unicorn permet apparemment de redémarrer son appli Rails à chaud, sans perdre aucune des requêtes clients qui était en cours de traitement au moment du redémarrage).

#### Parenthèse : pourquoi un serveur applicatif, et pas seulement un serveur Web comme avec PHP ?

En PHP le processus de mise à jour est assez simple : on modifie un fichier PHP, et la modification sera automatiquement effective pour toutes les requêtes HTTP qui vont suivre.  
_(Je parle là du principe de base de PHP, de celui dont l'extrême simplicité a fait le succès. C'est bien sûr un peu plus intello que cela dès qu'on a des solutions de cache qui rentrent en piste, que ce soit sur l'opcode PHP avec [APC](http://www.php.net/manual/fr/book.apc.php) ou avec la gestion des [ETags](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers#contentbased) avec des logiciels comme [Varnish](https://www.varnish-cache.org/).)_

Mais le principe fondateur de PHP reste que pour chaque requête HTTP le serveur Web va grosso-modo créer un nouveau thread dédié à ce client, qu'il va regarder quel fichier PHP est demandé et qu'il va passer la main (via _mod_php_ par exemple) au moteur PHP pour ré-analyser et ré-exécuter le code PHP du fichier en question - ainsi que toutes ses éventuelles dépendances.
Lorsqu'on a une modification à apporter, on remplace le fichier concerné et la modification sera effective dès la prochaine créeation de script PHP par le serveur Web. C'est simple et c'est assez efficace.

Avec Ruby on Rails, le processus est nettement différent. D'après ce que j'ai compris, on va lancer une bonne fois pour toute une (ou plusieurs, en production) instance de l'application Rails, puis pour chaque requête le serveur applicatif déclenchera une instruction précise de ce processus, avec en paramètre les différents informations attentante à la requête HTTP. Ce serveur attendra en retour que cette instruction, une fois déclenchée, retourne un corps de réponse HTTP et des entêtes HTTP (ou, a minima, un code de retour HTTP).

Cela est formalisé par l'interface [Rack](http://fr.slideshare.net/judofyr/introducing-rack), volontairement très simple d'approche.  Cette interface, inspirée du monde du Python (avec [WSGI](http://en.wikipedia.org/wiki/Wsgi)), vous est peut-être familière si vous avez déjà travaillé avec le micro-framework PHP [Slim](http://www.slimframework.com/).  
En effet, son auteur a eu la bonne idée de reprendre à son tour ce principe de WSGI/Rack, et de le transposer en PHP. Vous pouvez le voir décrit en PHP [sur cette page](http://docs.slimframework.com/#Middleware-Overview) de la documentation de Slim.

On a donc une application Rails qui s'initialise, se lance, puis attend qu'on lui transmette des demandes de requêtes HTTP auxquelles elle devra répondre, suivant le protocole Rack. Mais puisque l'application est lancée une bonne fois pour toutes, lorsqu'on a une modification à faire et qu'on met à à jour un fichier Ruby sur le serveur cela ne lui fera ni chaud ni froid. Elle aura déjà analysé ce fichier Ruby, dont elle garde une copie compilée en mémoire vive, et elle ne se soucie plus des éventuelles modifications du fichier sur le disque dur.

Si vous avez déjà travaillé avec Node.js, cela doit vous être familier puisqu'on retrouve exactement le même fonctionnement.

Je détaillerai plus bas comment on peut dans ce contexte mettre à jour sans que ce soit trop pénible notre application chaque fois qu'on a une modification à y apporter (ce qui, pendant le développement, se produit à peu près 200 fois par jour).

#### Choix d'un serveur applicatif

Comme je le disais avant cette parenthèse sur le pourquoi des serveur applicatifs en Ruby, pour faire le passe-plat entre les requêtes HTTP brutes et notre applicatif en Rack/Rails on dispose d'un choix assez varié de serveurs applicatifs Ruby. Parmi ceux-ci, Unicorn semble être l'un des plus utilisés, mais je ne vais a priori pas pouvoir faire appel à lui en ce qui me concerne.  
En effet, mon appli Rails devra être intégrée comme je l'expliquais à un site PHP existant, tournant sous Apache, et il serait bougrement pratique, pour ne pas rajouter à la complexité d'avoir 2 langages de programmation totalement différents sur un unique site Web, de pouvoir greffer mon application RoR à Apache, de la même manière qu'on y greffe PHP via _mod_php_.

Eh bien ça tombe bien, un à-peu-près-équivalent de _mod_php_ existe pour Rack (et donc pour Rails, qui se base sur l'interface Rack) : il s'agit de [Phusion Passenger](https://www.phusionpassenger.com/) (site sur lequel vous tomberez également si vous tapez "http://modrails.com" :-).  
Celui-ci ne semble pas avoir que des avantages, comparé à des concurrents comme Unicorn, mais il a au moins pour lui la simplicité de sa mise en place, et sa bonne intégration à Apache et Nginx.

#### Installation de Phusion Passenger

On a juste à appliquer les 2 étapes expliquées [ici](https://www.phusionpassenger.com/download/#open_source) :
<pre><code class="language-bash">gem install passenger
passenger-install-apache2-module
</code></pre>(ici sans utiliser _sudo_, puisque j'utilise [rbenv](http://ridinginrails.tumblr.com/post/49375087294/jour-3-faire-tourner-ruby-on-rails#rbenv) )

Cette dernière commande nous indique éventuellement quels packages on doit ajouter via <code class="language-bash">apt-get install</code> pour mener à bien l'installation de Phusion Passenger. On les installe, puis on relance cette commande.    
Dans mon cas par exemple, sur une Ubuntu fraîchement installée il a fallu ceci : <pre><code class="language-bash">sudo apt-get install build-essential libcurl4-openssl-dev zlib1g-dev apache2-prefork-dev libapr1-dev libapr1-dev</code></pre>

Lorsque tout est fini, il n'y a plus qu'à ajouter les 3 lignes de conf Apache que nous dicte le script. Soit on y va comme un bourrin et on le colle dans _/etc/apache2/apache2.conf_, soit on la joue plus fine et on suit les recommandations de [ce site](http://www.web-l.nl/posts/5-setting-up-a-rails-production-server-part-1) :

1. on se rend dans le répertoire _/etc/apache2/mods-available/_

2. on y crée le fichier **passenger.load**, et on y colle la directive "LoadModule" que nous a donné le script d'install de Passenger - dans mon cas : <pre><code class="language-apache2">LoadModule passenger_module /home/vagrant/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/passenger-3.0.19/ext/apache2/mod_passenger.so</code></pre>

3. on y crée également le fichier **passenger.conf**, avec cette fois les 2 autres directives - dans mon cas : <pre><code class="language-apache2">PassengerRoot /home/vagrant/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/passenger-3.0.19
PassengerRuby /home/vagrant/.rbenv/versions/1.9.3-p392/bin/ruby</code></pre>

Avec ça on a un beau module Apache, qu'on a plus qu'à activer proprement :
<pre><code class="language-bash">sudo a2enmod passenger
sudo service apache2 restart</code></pre>

Pour le reste, tout est très bien expliqué dans [la documentation de Phusion Passenger](http://www.modrails.com/documentation/Users%20guide%20Apache.html).  
Dans mon cas par exemple le site Ruby on Rails sera accessible depuis une "sous-URL" du site PHP, ce qui me donne une addition à mon VHost Apache de ce genre :
<pre><code class="language-apache2">    RackBaseURI /rails 
    &lt;Directory /vagrant_php_app/rails&gt;
        RackEnv development # en local je force le mode "development" de Ruby on Rails
        Options -MultiViews
    &lt;/Directory&gt;
</code></pre>
Le répertoire "/vagrant/rails" est un lien symbolique vers le dossier "public/" de mon application Ruby on Rails, comme précisé dans la doc de Passenger sur le "[déploiement vers des sous-URI](http://www.modrails.com/documentation/Users%20guide%20Apache.html#deploying_rack_to_sub_uri)" :
<pre><code class="language-bash">ln -s /vagrant_rails_app/public /vagrant_php_app/rails
</code></pre>

### Quand redémarrer le serveur ?

Comme je l'expliquais plus haut, contrairement à PHP on a avec Rails une application qui se lance, qui analyse (pour simplifier) tous les scripts Ruby de notre appli Rails une bonne fois pour toute, puis qui attend qu'on lui envoie des requêtes HTTP à traiter. Dans ce contexte, comment gérer la mise à jour des fichiers, puisqu'une fois l'instance Rails lancée elle ne se souciera plus a priori de nos éventuelle modifications sur nos fichier Ruby  ? Doit-on se taper le redémarrage du serveur WEBrick/Apache/Unicorn/whatever à chaque fois qu'on modifie le moindre fichier ??

Heureusement non, car lorsque l'application est en mode **development** (ce qui est le cas par défaut avec WEBrick, et ce que j'ai forcé manuellement dans la config de mon VHost Apache avec Passenger) Ruby on Rails va recharger toutes les classes de Contrôleurs, Modèles et Vues de l'application à chaque requête HTTP.  
Chouette alors, grâce à ça on aura donc jamais besoin de redémarrer le serveur pour qu'il prenne en compte nos modifications !

#### Parfois malheureusement...

Eh bien si, dans certains cas il le faudra pourtant bien. Déjà, lorsqu'on touche à un composant applicatif qui ne relève ni des Contrôleurs, ni des Modèles, ni de la Vue. Par exemple, dès qu'on modifie un initialiseur ou une directive de configuration (et il y a de quoi faire en la matière, comme détaillé [ici](http://guides.rubyonrails.org/configuring.html) par exemple).  

Mais aussi dans certains cas que je n'ai pas tout-à-fait saisi. Par exemple, dans un de mes Contrôleurs j'ai un code précédant ma classe, qui fait appel à un composant logiciel que j'ai placé dans le répertoire "lib/" de mon application Rails :
<pre><code class="language-ruby">
require 'acme/php_bridge.rb'

class MyController &lt;  ApplicationController

    include Acme::PhpBridge
    before_filter :check_php_bridge

    # .. le code de mon Contrôleur ...
    
end</code></pre>

Si vous avez lu comme moi les dfférents tutos Rails dont je vous parlais, vous voyez qu'ici je fais appel à un module de mon crû, "PhpBridge", qui se trouve dans un "namespace" (en Ruby, un module) - nommé pour l'exemple "Acme" afin de respecter la vie privée de mon client :-)  
J'inclus ce module à mon Contrôleur en tant que _mixin_, comme on le ferait en PHP 5.4+ avec un _Trait_, puis je demande à Rails de lancer la fonction "check_php_bridge" de mon mixin à chaque fois que le Contrôleur doit être déclenché.  
La première requête HTTP que je lance avec ce code fonctionne très bien, que ce soit avec WEBrick ou Passenger, mais à partir de la deuxième je récolte une vilaine erreur me disant que la route (!) "Acme::PhpBridge" n'a pas été trouvée. Je n'ai pas d'autre choix alors que de redémarrer le serveur.

Je n'ai pas vraiment compris le pourquoi de ce problème, je l'avoue - même si je me doute que cela doit avoir un rapport avec le fait que RoR va recharger le code de ma classe à chaque requête, puisque je suis en mode _development_, et qu'à partir de ce moment-là le code qui est en-dehors de la classe elle-même (mon "require") n'est plus déclenché comme il faut.

Pour remédier à cela, ne sachant trop que faire puisqu'étant un total néophyte en Rails, j'ai pris le parti de mettre mon "require" dans un _initializer_ Rails, tel que je l'ai vu faire dans le code de LinuxFr, [ici](https://github.com/nono/linuxfr.org/blob/master/config/initializers/require_libs.rb).  
(je savais bien que ça me servirait rapidement, d'avoir gardé le code source de ces quelques applis Rails existantes sous le coude :-) .

Seulement maintenant, mon code n'est plus dans aucune des 3 composantes MVC que Rails recharge automatiquement en mode _development_ ! A chaque fois que je modifie mon module "Acme::PhpBridge", donc, je dois redémarrer le serveur.  
Sous WEBrick, je stoppe le serveur avec Ctrl+C puis je le relance avec la commande abrégée <code class="language-bash">rails s</code>, et sous Passenger je fais comme on me l'a dit dans la doc :
<pre><code class="language-bash">touch tmp/restart.txt #redémarre Passenger 
</code></pre>

Ce n'est pas très long à faire, heureusement, mais c'est tout de même un peu pénible de devoir faire cela à chaque fois que je vais ajouter ou modifier une classe qui ne fera pas partie des Contrôleurs, du Modèle ou de la Vue.  
Je pourrai survivre avec cela, mais **si un pro du Ruby a un conseil à me donner sur ce point précis je suis preneur** :-)

### Cette fois c'est bel et bien terminé pour l'installation

Voilà, j'ai un environnement Ruby et Ruby on Rails prêt à l'emploi, un Passenger qui va me servir mon appli via Apache, et je crois avoir à peu près compris quand il allait falloir que je redémarre le "serveur Rack" manuellement.  

A partir du prochain jour, je ne devrais plus avoir à faire qu'à du code Ruby on Rails proprement dit ! Je tâcherai de vous remonter les points qui m'auront surpris, ravi ou posé problème, en tant que développeur néophyte Ruby/Rails venant du monde PHP / Javascript :-)

A bientôt !



