Bonjour !

Eh bien voici [Ruby](http://ridinginrails.tumblr.com/post/48605023304/jour-1-sinitier-a-ruby) et [Rails](http://ridinginrails.tumblr.com/post/48693826792/jour-2-sinitier-a-rails) installés. Je me suis auto-formé rapidement sur les bases de Ruby et sur celles de Rails, il n'y a plus qu'à lancer la bête !

### Préambule : remplacement de RVM par rbenv

Au premier jour de mon auto-initiation j'avais cru comprendre en parcourant différentes ressources sur Internet qu'il était de bon ton d'installer Ruby via [RVM](https://rvm.io/). Ce que je fis.
Entre-temps des lecteurs avertis m'ont cependant révélé que RVM c'était bien-mais-pas-top. Notamment parce qu'une fois installé, la commande <code>cd</code> (celle-là même qui permet de changer de répertoire) de l'utilisateur est remplacée par une version "RVM-ienne", qui se charge de changer automatiquement la version de Ruby utilisée si on débarque dans un dossier où se trouve un fichier ".rvmrc", comme expliqué [ici](https://rvm.io/workflow/rvmrc/#project). L'idée ne semble pas mauvaise, mais sa mise en pratique via le remplacement d'une commande système aussi essentielle est effectivement sujette à débat.

Et j'avoue par ailleurs - à titre tout-à-fait subjectif - que si cela peut me permettre de ne plus avoir à aller sur le site Web de RVM, dont le design m'arrache une larme chaque fois que j'ai besoin d'aller vérifier un point de la documentation, je signe tout de suite :-)

#### _chruby_, _ry_, _rbenv_...

Il existe plusieurs solutions alternatives assurant peu ou prou la même fonction que RVM, comme les [chruby](https://github.com/postmodern/chruby) et [ry](https://github.com/jayferd/ry) qu'un lecteur m'a conseillés, mais je me suis finalement tourné vers [rbenv](https://github.com/sstephenson/rbenv/).
Pourquoi celui-ci plutôt qu'un autre, me direz-vous ? Parce qu'il fallait bien faire un choix, et parce que son auteur, Sam Stephenson, bosse chez [37signals](http://37signals.com/) (chez qui Rails a été créé), et qu'il est notamment l'auteur de [Prototype.js](https://github.com/sstephenson/prototype#readme), [eco](https://github.com/sstephenson/eco#readme) ou [Sprockets](https://github.com/sstephenson/sprockets#readme) (l'équivalent _railesque_ d'[Assetic](https://github.com/kriswallsmith/assetic#readme), si vou avez bossé avec Symfony 2), et que je me dis qu'un auteur aussi prolifique et semble-t-il talentueux sait a priori ce qu'il fait.
Sam m'a l'air de quelqu'un à qui on peut faire confiance, et étant donné que je débute avec Ruby et Rails ça me fait plaisir d'être rassuré.

<a id="rbenv"></a>
#### Installation de _rbenv_

Bref, je désinstalle RVM et je suis sagement les consignes d'installation de _rbenv_ (entièrement manuelle, mais au moins je sais ce qui se passe sur ma machine contrairement à RVM), et me voici quelques minutes plus tard avec un Ruby géré par _rbenv_.
<pre><code>
# désintallation de RVM
rvm implode

# téléchargement de rbenv
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv

# sous Ubuntu, remplacer "~/.bash_profile" par "~/.profile"
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' &gt;&gt; ~/.bash_profile
# idem
echo 'eval "$(rbenv init -)"' &gt;&gt; ~/.bash_profile

# recharge le shell avec rbenv
exec $SHELL -l

# installe le plugin rbenv "ruby-build"
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

# installe une version récente de Ruby 1.9.3
rbenv install 1.9.3-p392
# ...
# ça télécharge et ça compile. On a le temps de se faire un café...
# ...

# après la compilation, sélectionne cette version de Ruby par défaut sur la machine
rbenv global 1.9.3-p392
</code></pre>

Désolé pour la coloration syntaxique bugguée, je n'ai pas réussi à obtenir mieux avec [google-code-prettify](https://code.google.com/p/google-code-prettify/)...

#### Ruby 1.9.3 à la place de la 2.0

Vous noterez peut-être au passage que j'ai installé une version **1.9.3** de Ruby, et non la 2.0 que j'avais installée initialement avec RVM.
C'est que l'on se trouve en ce moment dans une période charnière. Ruby 2.0 est sorti il y a quelques mois, pour les 20 ans du langage. C'est à cette occasion d'ailleurs que Yukihiro Matsumoto, dit "Matz", le créateur de Ruby, nous a révélé une nouvelle approche du l'ingénierie logicielle : le _Birthday Driven Development_ . Je suis impatient d'essayer cette nouvelle méthode :-)

Ruby 2 est sorti, donc, mais tout l'écosystème Ruby n'est pas encore à jour avec les quelques incompatibilités que cette nouvelle mouture apporte par rapport à la précédente. J'ai notamment rencontré avec cette 2.0 un soucis lors de l'installation de Passenger (que je détaillerai  dans le prochain article).

J'ai donc préféré ne pas faire le chien fou, et rester sur une version moins avant-gardiste de Ruby. J'aurai bien le temps de jouer avec les nouveautés de la 2.0 plus tard, si tout se passe bien et que j'arrive à perdurer dans la "Voie du Ruby".

### Installation des Gems du projet

Ruby 1.9.3 est proprement installé, j'ai réinstallé Rails avec un petit "_gem install rails_", il est temps de lancer la bête !
Je me rends donc dans le répertoire de mon appli Rails (générée la veille avec "_rails new blog --database=mysql_"), et je m’apprête à enfin démarrer le moteur.

Dans un instant de lucidité je me rappelle néanmoins d’une étape que j’ai vue décrite dans tous les tutoriels que j’ai parcourus, que j’allais oublier : l’installation des Gems nécessaire à ma première application Rails !

#### Gems et Gemfile

Les Gems sont des bibliothèques logicielles Ruby, équivalentes à peu près aux packages PHP que l’on peut trouver sur [Packagist](https://packagist.org/). En Ruby ces bibliothèques sont donc des Gems, et elles sont regroupées sur le bien-nommé site [RubyGems.org](http://rubygems.org/).
Pour les installer d'après les descriptions contenues dans un fichier texte, on a accès à un outil qui lui s’appelle [Bundler](http://gembundler.com/).

Allez, un petit tableau récapitulatif :

| PHP | Node.js  | Ruby  |
| ------------- | ------------- | ------------- |
| package | module | gem |
| Composer | npm | Bundler |
| composer.json | package.json | Gemfile |


On peut tout-à-fait installer des Gems avec uniquement l'outil en ligne de commande [gem](http://guides.rubygems.org/command-reference/), qui va à la demande télécharger la Gem que vous lui demandez et l'installer sur votre machine.

Mais dès lors que l'on souhaite télécharger et installer plusieurs Gems en une seule fois d'après les indications contenues dans un fichier texte, Bundler (qui est lui-même une Gem) est nécessaire.
Ce fichier texte, à la racine du projet qui exprime ses dépendances, doit se nommer "**Gemfile**" (équivalent donc de "composer.json" en PHP ou de "package.json" en Node.js).

#### Installation de Bundler

Si vous avez comme moi installé Ruby avec _rbenv_, Bundler n'est malheureusement pas encore présent, et il va nous falloir l'installer. Heureusement c'est très facile, l'utilitaire <code>gem</code> va le faire pour nous :
<pre><code>gem install bundler
</code></pre>

Si vous n'avez pas installé le plugin _rbenv_ [rbenv-gem-rehash](https://github.com/sstephenson/rbenv-gem-rehash), il faut ensuite :

* soit relancer une session, ce qui est moyennement pratique
* soit demander explicitement à _rbenv_ de ré-analyser les Gems installées et de créer si besoin est les raccourcis vers les exécutables liées aux différentes Gems. Ceci afin qu'elles soient utilisables en ligne de commande directement : <pre><code>rbenv rehash</code></pre> Il n'est cependant pas forcément très commode de devoir lancer cette commande après chaque installation de Gem, aussi vais-je plutôt installer [rbenv-gem-rehash](https://github.com/sstephenson/rbenv-gem-rehash).

Une fois cela fait, Bundler est disponible sur notre système, sous la forme d'une commande "<code>bundle</code>".

#### Lancement de Bundler

Je me rends donc dans le répertoire où est installé mon appli RoR, et je demande à Bundler d'aller me télécharger et de m'installer toutes les Gems dont va dépendre mon application Ruby on Rails, telles que décrites dans le fichier Gemfile qui avait été automatiquement créé lors de la génération avec "_rails new [nom du projet]_" :
<pre><code>cd /home/oliv/_WORK/www/blog/
bundle install
</code></pre>

Hop, ni une ni deux, voilà Bundler qui se connecte au dépôt central de Gems, et commence à télécharger et installer les différentes Gems qui figurent dans le fichier "Gemfile", ainsi que les Gems dont elles dépendent.

#### Cas particulier de la compilation de la Gem "mysql2"

Malheureusement la magie s'interrompt brusquement, avec un vilain message d'erreur concernant la Gem [mysql2](http://rubygems.org/gems/mysql2).
Eh oui, j'ai fait mon malin en choisissant d'utiliser MySQL au lieu du SQLite que me proposait Rails par défaut, et j'avais donc remplacé la Gem "sqlite3" par "mysql2" dans mon "Gemfile" :
<pre><code>#gem 'sqlite3' # au revoir SQLite...
gem 'mysql2' # ...bienvenue MySQL</code></pre>

Jusque-là rien de bien litigieux. Mais le soucis, c'est que la Gem "mysql2" qui permet à Ruby de communiquer avec un serveur MySQL nécessite lors de son installation de compiler une partie de son code écrit en C. Et pour que ce code puisse être compilé, il est nécessaire d'avoir sur sa machine le code source C qui permet cette communication avec MySQL.
Sous Ubuntu, donc, j'ai dû installer le package ad hoc :
<pre><code>sudo apt-get install libmysqld-dev
</code></pre>

Une fois cela fait, je relance <code>bundle install</code>, et tout se déroule cette fois comme sur des roulettes.

### Premier démarrage du moteur

Pfwheee ! Eh bien, je l'aurais mérité mon premier lancement de Ruby on Rails ! :-)
Bon, rien de bien méchant non plus : une fois les manips connues c'est en fait assez rapide à reproduire, et j'ai pu les refaire en 5 petites minutes sur une nouvelle machine virtuelle.

Allez, je commence tranquillou, avec le serveur Web intégré à Ruby on Rails. Ce serveur, WEBrick, n’a absolument pas vocation à être utilisé en production, mais il est semble-t-il bien utile pour tester son application Rails rapidement.
Je me rends donc dans le répertoire de mon appli Rails (générée la veille avec "_rails new blog --database=mysql_"), et je lance la commande suivante :
<pre><code>rails server
</code></pre>

#### Installation d'un moteur JavaScript

Dans mon cas le premier lancement fut un échec cuisant, avec un message d’erreur de 3km. Mais je suis motivé, j'en ai vu d'autres et je ne m'arrête pas là.
Alors alors, qu'est-ce qu'il y a qui va pas cette fois-ci ? Voyons... Le message d'erreur me parle de la nécessité d'installer... **un moteur JavaScript**.
JavaScript, vous ici ? Diable ! J'aime beaucoup JavaScript, mais je ne pensais pas l'avoir invité sur mon appli Ruby on Rails.

En y regardant de plus près, je repère le coupable : il s'agit de [CoffeeScript](http://coffeescript.org/). Ce bien chouette langage, géré de base par RoR, a originellement été écrit par son auteur en Ruby. Mais aujourd'hui le "transcompilateur" CoffeeScript est lui-même écrit en CoffeeScript, et compilé il nécessite un moteur JavaScript pour mener sa mission à bien.

Pour lancer automatiquement les compilations de mes futurs fichiers CoffeeScript en JavaScript, Ruby va donc demander à un moteur JavaScript de faire le boulot. Pour ce faire c'est la Gem "ExecJS" qui va être appelée à la rescousse, et elle est capable de communiquer avec 5 moteurs JavaScript différents - listé [ici](https://github.com/sstephenson/execjs#readme).
En ce qui me concerne je ne me suis pas pris la tête, et j'ai tout simplement installé Node.js sur la machine :
<pre><code>sudo apt-get install nodejs
</code></pre>

#### It's alive!

Une fois cela fait, tout se passe bien cette fois, ExecJS a automatiquement détecté Node.js et pourra faire affaire avec lui le temps venu.
Pour peu que j'ai correctement indiqué à RoR les identifiants de connexion à MySQL (_c.f. ma config, en annexe_), mon application Rails prend enfin vie !

Je me rends en HTTP sur ma propre machine, sur le port 3000, et la page d'accueil par défaut de Ruby on Rails s'affiche enfin dans toute sa splendeur.

### A suivre...

Mais le temps passe, ma foi, et je vais clôturer ici-même cet article. Je n'ai pas démérité après tout, avec ma première application Ruby on Rails opérationnelle. Dans le prochain article j'installerai notamment un serveur Web un peu plus balèze que WEBrick, Phusion Passenger.

Je commencerai également, si tout se passe bien, à plancher sur mon premier projet Ruby on Rails.
Car oui, je suis bien content et bien chanceux. A peine ai-je commencé à me mettre à Rails que j'ai pu trouver une mission me permettant de mettre les mains dans le cambouis de manière furieusement concrète. Je suis impatient de voir ce que donne en conditions réelle le développement Ruby on Rails !

A bientôt !


### Annexe

#### Configuration de la base de données

Mon fichier "config/database.yml" :
<pre><code>
development:
  adapter: mysql2
  encoding: utf8
  host: localhost
  database: blog_test
  username: blog_test
  password: blog_test
  pool: 5
  timeout: 5000
</code></pre>

Dans le cas de l'utilisation de [Vagrant](http://www.vagrantup.com/) : pour que le Ruby on Rails installé sur ma machine virtuelle VirtualBox puisse communiquer avec le serveur MySQL installé sur ma machine réelle, j'ai dû indiquer l'adresse "10.0.2.2" au lieu de "localhost".

#### Latence réseau avec Vagrant

Si comme moi vous utilisez [Vagrant](http://www.vagrantup.com/) pour gérer votre environnement Ruby, vous constaterez peut-être que la machine virtuelle rame fort à chaque fois qu'elle va chercher du contenu sur Internet. C'est bien dommage, puisqu'entre les installations des packages Debian et celles des différentes Gem, j'en ai bien besoin d'Internet.

J'ai trouvé sur le Web les lignes suivantes, à ajouter au fichier "**Vagrantfile**", qui peuvent aider :
<pre><code>
  config.vm.provider :virtualbox do |vb|
    vb.auto_nat_dns_proxy = false
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end
</code></pre>

Qui plus est, dès qu'on lance WEBrick la communication se fait extrêmement lentement, avec plusieurs secondes de latence entre la demande HTTP et la réponse.
Cela est apparemment dû à un problème avec VirtualBox, et pour y remédier, j'ai trouvé la solution suivante (précisément [ici](http://stackoverflow.com/questions/1156759/webrick-is-very-slow-to-respond-how-to-speed-it-up#answer-3465134)), qui fonctionne rudement bien : il suffit de passer à <code>true</code> la valeur de la directive <code>:DoNotReverseLookup</code> du fichier de config de WEBrick.
(chez moi à l'emplacement _/home/vagrant/.rbenv/versions/1.9.3-p392/lib/ruby/1.9.1/webrick/config.rb_)