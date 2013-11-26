Bonjour !

Il y a quelques jours a eu lieu la première [faille critique de Ruby](https://www.ruby-lang.org/en/news/2013/11/22/heap-overflow-in-floating-point-parsing-cve-2013-4164/) (quel produit informatique n'en a pas ?) publiée alors que je disposais d'un serveur faisant tourner une appli Ruby on Rails en production. Il m'a donc fallu apprendre rapidement comment mettre à jour Ruby sur le dit serveur.  
Chouette chouette chouette...

Pour ceux qui comme moi se trouveraient face à cette situation pour la première fois, je me suis dit qu'il pourrait être utile de partager le détail de la manoeuvre que j'ai faite sur mon serveur.  
Il existe certainement de meilleures procédures, notamment à base de Chef/Puppet/Docker ou je ne sais quoi d'autre, mais je préfère encore faire les choses de manière artisanale pour le moment - et puis, je ne suis pas encore un expert du Ruby :-)

Plutôt que de lister bêtement les commandes à taper, j'explique un peu à quoi sert chacune ; mais si on est pressé on peut juste taper les commandes l'une après l'autre, en adaptant seulement le numéro de patch de Ruby 2.0 :-)

Le tout est à faire avec l'utilisateur avec lequel on a installé [rbenv](https://github.com/sstephenson/rbenv) (ici "admin"). On aura besoin de "sudo" uniquement à la fin, pour éditer 2 fichiers de conf d'Apache puis le redémarrer.

* Au lieu d'utiliser la version de Ruby proposée par les dépôts Debian, qui ne sont pas très à jour et qui peuvent poser problème si on a plusieurs applis Ruby ayant des dépendances différentes, j'ai utilisé un des 2 standards de Ruby en la matière : [rbenv](https://github.com/sstephenson/rbenv).  
Rbenv sert à pouvoir installer plusieurs versions de Ruby sur une seule machine, chacune isolée dans son coin avec ses propres libs etc.
Il est également muni d'un plugin "[ruby-build](https://github.com/sstephenson/ruby-build)" qui sert à télécharger, compiler et installer différentes versions de Ruby.

* Si une nouvelle faille critique est découverte, donc.  
A priori le correctif de Ruby proposé via le plugin "ruby-build" sera disponible très vite (le correctif de cette faille-ci, par exemple, a été disponible pour installation via ruby-build dans les heures qui ont suivies la publication de la faille).   
La première chose à faire est de donc mettre à jour ce plugin "ruby-build", ce qui aura aussi pour effet de mettre à jour sa liste des versions de Ruby dispos :  
<code class="language-bash">admin@prod:$ cd ~/.rbenv/plugins/ruby-build/
admin@prod:$ git pull</code>  

* On vérifie que la version corrigeant la faille de sécurité est disponible :  
<code class="language-bash">admin@prod:$ rbenv install -l</code>  
Cette commande affiche toutes les versions dispos (y en a pas mal). La version de Ruby qui permet de faire fonctionner mon application actuelle est soit une 1.9.x, soit une 2.0. Cette dernière étant plus performante, on va rester sur une "2.0.".  
Pour la 2.0, donc, le correctif d'aujourd'hui est par exemple le patch 353, comme expliqué dans la note de [ruby-lang.org](http://ruby-lang.org/) révélant la faille de sécurité.  
On vérifie donc qu'on a bien "2.0.0-p353" qui s'affiche dans la liste.

* C'est le cas ? Super, alors on va pouvoir demander à "ruby-build" de télécharger son code source, de la compiler et de l'installer :  
<code class="language-bash">admin@prod:$ rbenv install 2.0.0-p353</code>  
On va se faire un café, un thé, on règle la facture d'électricité dont on procrastinait le paiement depuis une semaine, ou l'on vaque à toute autre occupation prenant le temps d'une compilation (sur mon serveur prod ça a pris 6-7 minutes).

* On vérifie que cette nouvelle version est bel et bien disponible en local à présent :  
<code class="language-bash">admin@prod:$ rbenv versions</code>  
Et là normalement on voit notre nouvelle mouture de Ruby parmi la liste des versions installées en local. La petite astérisque indiquant la version active par défaut est toujours devant la version de Ruby précédente.
Ainsi, si on demande à Ruby d'afficher sa version, on a toujours la précédente qui s'affiche pour le moment :  
<code class="language-bash">admin@prod:$ ruby -v
ruby 2.0.0p247 (2013-06-27 revision 41674) [i686-linux]</code>  

* On se rend dans le répertoire de l'appli Ruby on Rails.  
<code class="language-bash">admin@prod:$ cd ~/my-wonderful-rails-website---prod</code>  
On dit à rbenv qu'on souhaite toujours que ce soit cette version qui soit active dès lors qu'on est dans ce répertoire :  
<code class="language-bash">admin@prod:$ rbenv local 2.0.0-p353</code>  
Cela crée un fichier ".ruby-version" dans le répertoire, qui est détecté par "rbenv". Si on redemande la version de Ruby alors qu'on se trouve dans ce répertoire, on a alors bien la nouvelle version qui s'affiche :  
<code class="language-bash">admin@prod:$ ruby -v
ruby 2.0.0p353 (2013-11-22 revision 43784) [i686-linux]</code>

* On va à présent installer les bibliothèques Ruby requises par l'appli Rails. Pour cela on commence par installer la gem (bibliothèque Ruby) "bundler", qui permet d'installer d'autres gems d'après une liste de gems spécifiées dans un fichier "Gemfile" (un peu l'équivalent de Composer et de son fichier "composer.json" en PHP) :  
<code class="language-bash">admin@prod:$ gem install bundler</code>  
Puis on demande à Bundler d'installer les gems de notre appli Ruby on Rails, d'après le contenu du fichier Gemfile qui se trouve dans notre répertoire "admin-rails-prod" :  
<code class="language-bash">admin@prod:$ bundle install</code>  
Ces gems étaient bien sûr déjà présentes sur le système, mais... uniquement pour notre version précédente de Ruby ! Grâce à "rbenv" les différentes versions sont bien cloisonnées, et elle ont chacune leurs propres gems. Il faut donc réinstaller les gems dont dépend l'appli Rails chaque fois qu'on installe une nouvelle version de Ruby.  
Là normalement on voit plein de messages "installing XXX" défiler, puis le shell nous rend la main : toutes nos gems sont de nouveau installées sur notre environnement Ruby flambant neuf !

* Tiens tant qu'on y est, on va dire à "rbenv" que la version par défaut de Ruby activée où qu'on se trouve sur le serveur sera notre nouvel opus :  
<code class="language-bash">admin@prod:$ rbenv global 2.0.0-p353</code>  

* C'est bientôt fini. Il faut encore mettre à jour Phusion Passenger, a.k.a. "mod_rails". C'est lui qui me permet de faire tourner l'appli Rails directement depuis Apache, sans avoir à utiliser un serveur d'application spécifique.  
Mon premier projet Rails devant s'interfacer avec un site existant tournant en environnement LAMP, il fallait que je garde Apache comme base. Et même si j'ai vu Phusion Passenger décrié ça et là, en ce qui me concerne je suis très satisfait par le service rendu - je l'ai d'ailleurs utilisé pour des petits projets Node.js et Django en parallèle, depuis que je l'ai découvert grâce à Ruby on Rails :-)  
On a mis à jour Ruby sur la machine, c'est super, mais Passenger continue donc lui de tourner avec l'ancienne version. Damn!

* On suit donc la procédure d'install de Passenger, qu'on réapplique dans notre nouvel environnement Ruby. On commence par aller chercher le code source de Passenger :  
<code class="language-bash">admin@prod:$ gem install passenger</code>  
Puis on effectue la compilation du module pour notre nouveau Ruby :  
<code class="language-bash">admin@prod:$ passenger-install-apache2-module</code>  
On appuie sur "Entrée" pour confirmer, et attend la fin du bouzin. C'est beau comme une compilation de programme C/C++ (hem...). Si vous avez envie d'un deuxième café, c'est maintenant.  
Normalement toutes les dépendances de Passenger sont déjà installées sur le serveur, ça devrait rouler. Dans le cas contraire, il nous signale quels packages Debian on doit installer avant de relancer cette commande.

* Quand il a fini, il nous donne 3 lignes à copier-coller dans nos fichier de conf Apache. Là par exemple, ça donne :
> The Apache 2 module was successfully installed.
> 
> Please edit your Apache configuration file, and add these lines:
>    LoadModule passenger_module /home/admin/.rbenv/versions/2.0.0-p353/lib/ruby/gems/2.0.0/gems/passenger-4.0.25/buildout/apache2/mod_passenger.so
>    PassengerRoot /home/admin/.rbenv/versions/2.0.0-p353/lib/ruby/gems/2.0.0/gems/passenger-4.0.25
>    PassengerDefaultRuby /home/admin/.rbenv/versions/2.0.0-p353/bin/ruby
>
> After you restart Apache, you are ready to deploy any number of Ruby on Rails applications on Apache, without any further Ruby on Rails-specific configuration!

* On édite donc nos 2 fichier de conf Apache:  
<code class="language-bash">admin@prod:$ sudo vi /etc/apache2/mods-available/passenger.load</code>  
On y colle la première ligne que nous avait donnée Passenger, celle commençant par "LoadModule".  
On réitère avec l'autre fichier :  
<code class="language-bash">admin@prod:$ sudo vi /etc/apache2/mods-available/passenger.load</code>  
Et on y met les 2 lignes suivantes, commençant par "PassengerRoot" et "PassengerDefaultRuby".

* Et voilà, y a plus qu'à croiser les doigts et redémarrer Apache :  
<code class="language-bash">admin@prod: sudo service apache2 restart</code>

Ca paraît un peu long comme ça, mais en réalité c'est assez vite fait ; le plus long c'est la compilation de Ruby puis celle de Passenger :-)


Hop! this helps!
