Bonjour !

Eh bien me voici à présent au pied du mur. Après avoir [mûrement réfléchi](http://ridinginrails.tumblr.com/post/48532076563/intro-motivations-pour-passer-de-php-a-ruby-on-rails) et décidé de tenter de passer du développement PHP au développement Ruby On Rails, et après avoir passé [une journée](http://ridinginrails.tumblr.com/post/48605023304/jour-1-sinitier-a-ruby) à installer, configurer et tester les bases du langage Ruby, me voici prêt à me retrousser les manches pour enfin découvrir [Ruby On Rails](http://rubyonrails.org).


### Installation de Ruby on Rails

Première étape, donc : installer Rails sur ma machine.
De ce coté c'est très simple. Si vous êtes sous Mac ou Linux (ou si vous passez comme moi par une VM, via Vagrant par exemple), installer Ruby on Rails est à peu près aussi long que taper
<pre><code class="language-bash">gem install rails</code></pre>
Vous l'avez compris si vous avez suivi [le précédent épisode](http://ridinginrails.tumblr.com/post/48605023304/jour-1-sinitier-a-ruby), cela demande tout bêtement au gestionnaire de packages Ruby, Gem, de télécharger et d'installer le package Rails.

Si vous êtes sous Windows avec un environnement Ruby bien configuré, vous pouvez tenter également cette ligne de commande, ça devrait fonctionner j'imagine. Et si vous avez été plus malin que moi et que vous avez installé Ruby via [Rails Installer](http://railsinstaller.org/), comme [on me l'a conseillé dans les commentaires](http://ridinginrails.tumblr.com/post/48605023304/jour-1-sinitier-a-ruby#comment-872560082), vous avez déjà le framework en votre possession.

### Créer sa première application Rails

Vous souvenez-vous de ce qu'était le développement PHP il y a encore quelques années ? Pour chaque projet on repartait de zéro, installant si besoin est quelques bibliothèques Pear, et on copiait-collait ça et là quelques fichiers PHP qui nous servaient de bibliothèques utilitaires génériques.

C'était rigolo, c'était les joies de l'artisanat, et c'est peut-être comme ça qu'on apprend le mieux à mettre les mains dans le cambouis, lorsqu'on débute.  
Mais si vous avez travaillé avec Zend Framework, Symfony, Laravel, Silex ou n'importe quel autre surcouche PHP "majeure" dernièrement, ce temps de l'artisanat doit comme moi vous sembler bien lointain. En effet, aujourd'hui on ne démarre plus guère de projet PHP _from scratch_, et on utilise bien souvent quelque chose dans ce goût-là pour initialiser un projet :
<pre><code class="language-bash">composer create-project symfony/framework-standard-edition path/to/install 2.2.0
</code></pre>

Je ne suis pas spécialiste de l'histoire des nouvelles technologies, et encore moins de celle de Ruby, mais sauf erreur de ma part ces processus-là, très carrés et très sérieux - "industriels", comme on dit aujourd'hui - nous viennent encore une fois de Ruby on Rails.
C'est à mon humble connaissance le premier framework Web à avoir eu recours à des utilitaires en ligne de commande pour l'initialisation, la maintenance et le déploiement de sites Internet.  
De là, la brèche était ouverte et nombreux sont ceux à s'y être engouffrés - et Fabien Potencier parmi les premiers, avec Symfony 1.

#### Génération automatique de squelette applicatif avec Rails

Mais foin de blabla, il était question ici de créer ma première application Rails !  
Eh bien une fois la Gem Rails installée et bien campée sur ses positions, à l’affût de mes demandes, je n'ai plus qu'à me rendre dans le répertoire dans lequel je gère gère mes projets Web et à y lancer la commande (ici mon projet va s'appeler "blog") _rails new blog_.

En ce qui me concerne je n'ai aucun doute sur les qualités intrinsèques de SQLite, et surtout pas dans le cadre d'un développement en local, mais en tant que vieux croûton habitué à MySQL j'ai préféré tout de suite indiquer à Rails que j'allais me connecter à mon bon vieux serveur MySQL, géré via mon fidèle camarade [HeidiSQL](http://www.heidisql.com/), plutôt que de créer une base de données SQLite. Ca donne donc quelque chose dans ce genre :
<pre><code class="language-bash">cd work/www
rails new blog --database=mysql</code></pre>

#### _Convention over Configuration_

On voit alors la gem Rails créer tout un tas de répertoires et de fichiers. Ayant déjà travaillé avec des frameworks comme Symfony ou Zend Framework cela ne m'a pas trop choqué, mais si vous n'êtes pas déjà passé par cette case ça pourrait - à juste titre - vous effrayer.  
Mais pas de panique, cela n'est que l'expression fort noble d'un concept cher à Rails, "_Convention over Configuration_" : plutôt que de laisser chaque développeur personnaliser son usage du framework comme il l'entend, RoR nous prend par la main avec douceur mais fermeté, et entend nous faire utiliser ses propres conventions plutôt que de nous laisser partir en _free style_.

Si vous n'êtes pas habitué à ce principe, cela vous gênera peut-être, mais en ce qui me concerne j'adhère plutôt pas mal à ce principe.  
Il permet de retrouver ses marques instantanément dans n'importe quel projet utilisant cette même technologie, et nous laisse tout de même toute la logique métier de notre application pour épancher notre soif insatiable de liberté.

Oui, moi aussi j'aime parcourir les plaines et sentir le vent dans mes cheveux, sans avoir l'impression d'être restreint par un dogme technologique.  
Mais j'accepte cette "_Convention over Configuration_" initiale parce que pour avoir vu des dizaines de projets Web architecturés au petit bonheur et pas toujours d'une bonne manière, je la pense judicieuse, et que je sais que ma créativité de codeur va pouvoir s'en donner à coeur joie là où est vraiment le vif du sujet - à savoir tout mettre en oeuvre pour réaliser une belle application correspondant aux besoins du client.

### Comprendre Rails

A ce stade-là j'ai ma première application "blog" automatiquement générée par Rails, et puisque je suis curieux il va bien me falloir décortiquer un peu toute cette structure avant de commencer bêtement à appliquer tout le bazar MVC.  
Si vous avez déjà utilisé des technos comme Symfony ou Zend Framework, la structure de fichiers de RoR doit vous rappeler quelque chose, mais elle comporte tout de même quelques spécificités propres.

Je pourrais ici reproduire une énième copie de l'explication de l'arborescence de Rails, afin de vous faire partager mes recherches sur le sujet, mais je ne m'en abstiendrai car beaucoup l'ont fait avant moi, et d'une bien meilleure façon que je ne le ferais.

Ils ne se sont pas arrêté là, d'ailleurs, les bougres. Non, car tout dans l'esprit de partage qui caractérise les développeurs Ruby et qui...  
Ah oui, j'ai oublié de parler de ça. Ruby on Rails a l'air d'être assez ardu à assimiler, même quand on vient de frameworks à peu près équivalents en PHP. Heureusement, pour m'aider à franchir le cap  j'ai vraiment eu l'impression pour le moment que la communauté Ruby était ouverte et plutôt prompte à aider son prochain. Ce qui est toujours agréable lorsqu'on est nouveau venu, et me rassure en cas de pépin si j'ai besoin d'aide. Cela ne veut pas dire pour autant que j'ai été reçu comme un chien galeux dans la communauté PHP ou Node.js, hein :-) Mais celle de Ruby a l'air plutôt sympatoche en tout cas.  
Où en étais-je ? Ah oui, l'esprit-de-partage-de-la-communauté-Ruby, donc. Eh bien oui, il existe un grand nombre de guides et tutoriels pour s'initier à Ruby on Rails, ce qui est plutôt bath.

#### Rails for Zombies

Pour ma part, l'un des éléments qui m'a motivé à tenter cette transition de PHP vers RoR n'est rien d'autre qu'un tutoriel vidéo interactif gratuit. Il n'est disponible qu'en anglais, mais on peut afficher des sous-titres (anglais) pour aider à la compréhension, si besoin est.  
Ce tutoriel est vraiment excellemment fait : on visionne une courte vidéo, puis on nous pose une série de cinq questions, auxquelles on répond par une ou plusieurs lignes de code Ruby. A travers ces vidéos on entraperçoit vraiment le potentiel de Ruby on Rails, et c'est vraiment après avoir fait ce tutoriel "Rails for Zombies" que je me suis dit qu'il **fallait** que je m'y mette :-)  
> http://railsforzombies.org/levels/1

#### Guides officiels

Une fois ce tutoriel vidéo fait, il m'a bien fallu aller chercher quelque chose de plus substantiel. Une première recherche sur le site officiel de Rails m'a donné ainsi accès aux "guides Rails", en anglais, avec notamment cette mise en jambe qui aborde rapidement les concepts clefs du framework : [Getting started with Rails](http://guides.rubyonrails.org/getting_started.html)

Mais puisqu'il y a moult à lire (RoR est **vraiment** un gros framework), et même si je lis pas trop mal l'anglais après toutes ces années à lire des documentations IT, mon cerveau a tout de même une préférence naturelle pour le texte en français lorsqu'il doit ingurgiter une aussi grande quantité d'informations.

Si votre cerveau fait ce qu'il peut donc, comme le mien, vous aurez peut-être avantage à plutôt vous diriger vers le lien suivant. C'est une traduction en français des mêmes "guides Rails", et même s'ils ne portent pas sur la toute dernière version du framework (la traduction a dû prendre un peu de retard) j'imagine que la majorité des principes qui y sont détaillés continuent de s'appliquer : [Débuter avec Rails](http://railsdebutant.org/fr_guides/getting_started.html)

J'en profite pour citer au passage ce même site [railsdebutant.org](railsdebutant.org) : "_Nous parlons d'un framework conçu par un Danois dans un langage inventé par un Japonais et utilisé majoritairement aux Etats-Unis et en Angleterre !_". Je me sens vraiment citoyen du monde.

#### La mine d'or du débutant

Mais tout cela n'était que peu de chose en réalité comparé à ce que j'ai découvert par la suite, après de plus amples recherches ! Un livre en ligne qui donne une vision d'ensemble de Ruby on Rails, avec des exemples pratiques, des références dans tous les sens et un contenu exhaustif, traduit en plusieurs langues dont le français, véritable mine d'or de connaissances sur RoR :  
[http://french.railstutorial.org/](http://french.railstutorial.org/chapters/beginning)

Je ne sais pas si cet ouvrage fait autorité ou non parmi les développeurs Ruby on Rails, et je ne sais même pas qui s'est donné la peine de traduire un aussi grand volume de tutoriel, mais c'est tout bonnement épatant.  
_(prenez garde par contre à la licence de ce livre, qui comporte un piège caché : tout ce contenu est sous la terrible licence "[BEERWARE](http://french.railstutorial.org/chapters/beginning#license)", qui vous demande de payer une bière à l'auteur du livre si vous le croisez un jour et que vous pensez que son travail en vaut la peine. A vous de voir si vous voulez risquer le coup. Vous êtes prévenu.)_

**EDIT** : j'ai finalement trouvé [le dépôt GitHub des traductions de ce livre](https://github.com/mhartl/rails_tutorial_translation/tree/french). D'après ce que j'ai vu des commits tout ce contenu a été traduit en français par [Philippe Perret](https://github.com/PhilippePerret) - un grand merci à lui ! :-)

#### A quoi ressemble du code Ruby on Rails dans la vraie vie ?

En ce qui vous concerne vous êtes peut-être quelqu'un de bien peu attaché aux cours magistraux, et préférez aller vous frotter tout de suite à du code réel. Le confort douillet de la cheminée en lisant un tutoriel, un verre de bourbon à la main, très peu pour vous. Vous êtes _hot like lava_, et avez envie d'aller voir tout de suite ce que donne Rails utilisé sur des sites en production avec des milliers de pages vues.

Je ne reprendrai pas là la liste des principaux "grands sites" basés sur Rails, que vous pourrez trouver un peu partout, mais si vous voulez voir le code source d'applis Rails "de la vraie vie" je ne peux que vous recommander ces quelques dépôts d'applis Rails Open Source sur GitHub, qui devraient satisfaire votre curiosité :

- le code source de RedMine, excellente application de gestion de projet "à la Basecamp" : [https://github.com/redmine/redmine](https://github.com/redmine/redmine)
- le code source du site LinuxFr, réalisé par Bruno Michel, directeur technique d'AF83 : [https://github.com/nono/linuxfr.org](https://github.com/nono/linuxfr.org)
- le code source du site Web de Git : [https://github.com/github/gitscm-next](https://github.com/github/gitscm-next)
- le code source du site RailsFrance : [https://github.com/railsfrance/railsfrance.org](https://github.com/railsfrance/railsfrance.org)
- le code source de l'application TeaBook Open Reader : [https://github.com/TEA-ebook/teabook-open-reader](https://github.com/TEA-ebook/teabook-open-reader)

Je les ai pas mal parcouru, pour ma part. Et même si je n'ai pas encore tout saisi, je trouve intéressant de voir, au-delà des blogs que proposent les tutoriels, à quoi ressemble du "vrai" code Ruby on Rails.  
Qui plus est, je me dis que je pourrai toujours me reporter à ces codes sources d'applis matures, développées par des pros du RoR, si je me trouve bloqué dans une impasse.

### Aller plus loin avec Rails

Eh bien en fait non, je ne suis pas allé plus loin pour ce deuxième jour.  
J'ai pensé qu'il était nécessaire de vraiment commencer à assimiler les premières bases du framework.  
Rails est **vraiment** un énorme ensemble logiciel : il doit être tout-à-fait possible de commencer à coder une appli RoR avec pour seul bagage la lecture du guide "Getting started", mais pour ma part j'ai pensé que ça n'était pas forcément une bonne idée de partir à l'aventure sans équipement.

Aussi, et même si j'avais bien sûr hâte de coder mes premiers Contrôleurs, Modèles et Vues dans mon application "blog", cette deuxième journée d'initiation Rails a été principalement consacrée à la lecture de [ce gigantesque tutoriel Ruby on Rails](http://french.railstutorial.org/chapters/beginning).  
J'avoue ne pas avoir reproduit toutes les manipulations indiquées par le livre, mais je l'ai lu à peu près en entier (même si c'était en diagonale, pour certaines parties) et j'ai l'impression à présent de mieux savoir où je vais.

Voilà qui clôt ma deuxième journée d'apprentissage de Rails. Je vous retrouve prochainement pour la suite de mon récit, avec notamment l'utilisation de Bundler, puis ma première application Rails, en immersion immédiate dans un projet client réel.  
 - oh mais quel teasing ! :-)
