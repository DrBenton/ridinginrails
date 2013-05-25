Bonjour !

J'ai à présent les 2 mains dans le cambouis, développant ma première application Ruby on Rails pour mon client 8 heures par jour. Les débuts ont été assez hésitants, et j'ai passé beaucoup de temps à faire des recherches sur Internet. Heureusement il existe de très nombreuses ressources concernant Ruby et Ruby on Rails, et j'arrive toujours à me débloquer d'une manière assez honorable.  
Je m'en voudrais de "saloper" ma première réalisation Rails, aussi j'essaie de faire les choses proprement ! :-)

Après avoir un peu avancé sur l'intégration HTML/CSS du projet, je me suis cependant vite retrouvé devant un dilemne à résoudre : quel moteur de template choisir ?

### ERB, la syntaxe "_PHP-like_" de Ruby

Le moteur de template livré par défaut avec Rails est ERB, dont le fonctionnement est détaillé [ici](http://guides.rubyonrails.org/layouts_and_rendering.html).  
Il n'y a pas grand-chose à reprocher à ERB, il fait le job comme il faut : on bénéficie de la souplesse et de la beauté du langage Ruby dans les Vues, afin de structurer la logique d'affichage dans des balises <code>&lt;% .... %&gt;</code>, et on affiche le contenu de variables avec les balises <code>&lt;%= .... %&gt;</code>.  
Si ça vous rappelle furieusement les balises <code>&lt;?php .... %&gt;</code> et <code>&lt;?= .... %&gt;</code> de PHP, c'est normal, c'est exactement le même principe.  

On dispose dans ERB de tout un tas de _helpers_ mis à notre disposition par le framework Ruby on Rails, qui permettent de simplifier l'affichage HTML d'images, de liens, de feuilles de styles... La syntaxe ERB est très simple, facile à apprendre, et on est instantanément à l'aise avec sa syntaxe lorsqu'on vient de PHP.  
On dispose du système de "layout" et de "blocs" que l'on peut avoir avec Twig par exemple, ici appellé des _[yielding regions](http://guides.rubyonrails.org/layouts_and_rendering.html#understanding-yield)_, ce qui permet beaucoup de souplesse.

Mais alors, que lui reproche-t-on ? S'il est le moteur de template par défaut de RoR et qu'on se pose la question de choisir un autre moteur, c'est a priori que quelque chose cloche.  
D'après ce que j'ai pu comprendre, le principal reproche que lui font les _rubyistes_ est qu'avec ERB on reste dans la logique verbeuse du HTML : on ouvre un <code>&lt;div&gt;</code>, on ferme le <code>&lt;/div&gt;</code>, on doit ajouter des commentaires HTML sur certaines fermetures de balises pour retrouver quel élément elles ferment, etc. Ruby est un langage qui permet une extrême concision, et j'ai l'impression que ses utiliateurs aimeraient rester dans cet esprit lorsqu'ils font l'intégration de leurs pages Web.

A titre personnel je suis habitué à de l'HTML "classique" depuis 15 ans, aussi je me suis dit que j'aurais peut-être un résistance au changement trop importante pour passer à un autre système. Et puis je suis déjà en train d'apprendre un nouveau langage de programmation et un nouveau framework, c'est déjà pas mal pour mon pauvre cerveau. Aussi m'étais-je résolu à rester sur ERB, malgré le fait que ce système de templating soit visiblement très peu utilisé actuellement par les _rubyistes_.

Seulement voilà, même en PHP on n'utilise plus guère la syntaxe PHP "pure" pour le templating (même si c'est à la base la grand force du langage : le système de templating est inhérent au langage), et j'ai pris mes petites habitudes avec Twig.  
Même lorsque j'ai fait du Node.js, je suis resté sur un système de template qui colle au plus près de l'HTML, avec la version de TJ Holowaychuck (un des _gurus_ de Node.js) de EJS : https://github.com/visionmedia/ejs  
Celle-ci est plus ou moins un portage d'ERB (et donc de la syntaxe PHP) en Javascript, mais elle y ajoute un bien chouette système, les [filtres](https://github.com/visionmedia/ejs#filters). Avec les filtres la syntaxe devient moins verbeuse, plus fluide, et leur absence m'a vite manqué en pratiquant ERB.

Me voici donc résolu à aller chercher un autre moteur de templating. Habitué au [Twig](http://twig.sensiolabs.org/) de Symfony 2, je me suis donc mis à la recherche d'un système analogue en Ruby. Après tout, Twig est à la base inspiré du système de templates du framework Python [Django](https://docs.djangoproject.com/en/1.5/topics/templates/), et je me suis dit que si des développeurs PHP avaient décidé de s'inspirer de ce système il y aurait de fortes chances que ce soit également le cas de développeurs Ruby.  
C'est ainsi que j'ai installé et essayé... [Liquid](http://wiki.shopify.com/Liquid) !

### Liquid, un syntaxe proche de celle de Twig

Liquid est donc un système de templating lui aussi inspiré de celui de Django, développé apparemment par Shopify, qui l'a publié en Open Source sur son espace [GitHub](https://github.com/Shopify/liquid/). Si vous êtes comme moi habitué à Twig et que vous parcourez la documentation de Liquid, vous devriez être en terrain connu : la syntaxe est peu ou prou la même et on y retrouve le même système de filtres.  
Chouette chouette chouette, me suis-je dit, je vais pouvoir rester dans un système proche de celui auquel je suis habitué, et qui a fait ses preuves. De plus, léquipe avec qui je travaille sur ce premier projet Rails bosse de son coté sur Symfony 2, ça simplifiera la mutualisation des ressources si besoin est.
 
 Malheureusement, je me suis retrouvé confronté à 2 obstacles majeurs :
 
 * Liquid a été pensé pour structurer des Vues simples, dans le système de personnalisation de boutique de Shopify, et ne gère absolument pas le système de "layouts" et de "blocs". On trouve bien quelques ressouces pour mettre le système de layout simple en place, comme [ici](https://gist.github.com/danshultz/2914625), mais on a toujours pas de blocs, ce qui est bien embêtant.
 * Bien plus ennuyeux : les Vues gérées par Liquid sont - à dessein - isolées du code Ruby de l'application. Cela est tout-à-fait justifié dans l'optique d'utilisation de Shopify, qui n'aimerait pas que ses utiisateurs aient accès au code Ruby de leur plateforme en manipulant leurs templates, mais c'est très moyennement commode lorsqu'on utilise Ruby on Rails.  
 En effet, ce dernier met à notre disposition tout un tas de _helpers_ bien commodes, sans lesquels on pourrait par exemple difficilement utiliser le système de gestion d'_assets_ [Sprockets](https://github.com/sstephenson/sprockets) (équivalent d'Assetic pour ceux qui ont utilisé Symfony 2). Les templates Liquid étant totalement isolés du code de l'application Ruby, nous n'avons donc pas accès à ces _helpers_. Il serait possible réaliser un _mapping_ de ces _helpers_ Rails, en créant pour chacun un [tag](https://github.com/Shopify/liquid/wiki/Liquid-for-Programmers#create-your-own-tags) Liquid, mais je n'ai pas le temps pour réaliser cela moi-même sur ce premier projet Rails.

Pour ces 2 raisons, je ne vais pas choisir Liquid pour ma part, même si sa ressemblance avec Twig aurait été bien commode.

### Les autres compétiteurs

Un tour dans la catégorie "template engines" de [Ruby Toolbox](https://www.ruby-toolbox.com/categories/template_engines), qui élabore des degrés de "popularités" de bibliothèques logicielles Ruby en fonction de plusieurs critères (dont le nombre de téléchargements depuis [RubyGems](http://rubygems.org/) ), permet de voir quels autres moteurs de template on a à notre disposition avec Ruby.

Le plus populaire de cette liste, et de loin, est "erubis". Cette bibliothèque est apparemment un ERB codé en C, avec amélioration des performances, mais malgré ce très haut indice de popularité sur Ruby Toolbox je n'ai vu aucun site Rails l'utiliser parmi ceux dont j'ai parcouru le code source. La dernière mise à jour de "erubis" date d'il y a 2 ans, ça sent le sapin.

Tilt est le deuxième de la liste, mais il n'est pas en lui-même un moteur de template : c'est plutôt, d'après ce que j'ai compris, une couche d'abstraction permettant avec la même API d'interragir avec plusieurs moteurs différents. Si vous avez travaillé avec Node.js, cela vous rappellera peut-être le projet [consolidate.js](https://github.com/visionmedia/consolidate.js) de TJ Holowaychuck (eh oui, encore lui :-)

On retrouve également [Mustache](http://mustache.github.io/) dans ces solutions de template. Celui-ci a l'avantage d'être simple, performant, et d'être décliné à l'identique dans plus de 20 langages de programmation (!). Mais sa logique "logic-less" est assez gênante lorsqu'on est habitué à des moteurs de template riches comme Smarty ou Twig, aussi ai-je passé mon tour en ce qui me concerne.

### HAML et Slim : structure de l'arborescence HTML par l'indentation

J'en viens donc au troisième énergumène de notre liste : [HAML](http://haml.info/). Je me doutais bien que je me retrouverais un jour coincé dans le même ascenceur que HAML, et j'avais juqu'ici tenté de l'éviter, mais me voici au pied du mur. En effet, hormis [RedMine](http://www.redmine.org/) (qui accuse aujourd'hui un certain âge, même si cela n'enlève rien à la qualité du produit) tous les sites Web basés sur Rails dont j'ai pu parcourir le code source ont basé leurs templates sur HAML. Et lorsqu'au cours de la [formation Ruby](http://formations.humancoders.com/formations/ruby) j'avais demandé à Matthieu Segret quel moteur de template était utilisé en ce moment par les _rubyistes_, il m'avait confirmé ce que je craignais : HAML règne sans partage.

Pour ma part, je n'étais absolument pas chaud pour m'y mettre : HAML s'éloigne fortement de la syntaxe HTML, et le code des templates qui en résulte est assez déboussolant lorsqu'on y est pas habitué.  

J'avais joué un peu avec le moteur de templates Node.js [Jade](https://github.com/visionmedia/jade) (dont je vous laisse deviner l'auteur), qui est un peu dans le même esprit (même si en Ruby une autre technologie, [Slim](http://slim-lang.com/), se revendique plus proche de Jade qu'HAML). Effectivement, les débuts avaient été difficiles puisqu'il m'avait fallu perdre les automatismes de l'HTML, mais après quelques heures j'étais finalement rentré dans la logique et je m'étais rendu compte qu'il est effectivement bien commode de pouvoir gérer son code sans fermeture de balise, uniquement avec une structure par indentation.

J'avais donc fait l'effort de me mettre à un moteur de template éloigné de la syntaxe HTML avec Jade lorsque je travaillais avec Node.js, mais par fainséantise j'avais voulu éviter de m'y replonger pour HAML. Las ! Il semble bel et bien que ce soit bien lui que la communauté Ruby on Rails plébiscite, et il va bien falloir que je m'y mettre ! 

### HAML

Malgré le fait que sa syntaxe ne m'enchante guère, et malgré certains retours d'expérience négatifs (notamment [celui de Brandon Keepers](http://opensoul.org/blog/archives/2011/11/30/haml-the-unforgivable-sin/), ingénieur chez GitHub) qui ne m'ont pas rassuré, j'ai donc converti les quelques templates ERB que j'avais réalisés jusqu'ici sur ma première application Rails en HAML.

La première demie-heure fut poussive, mais après ce temps d'adaptation la conversion a été assez rapide, et je me suis vite retrouvé avec tous mes templates raccourcis d'un facteur d'environ 50% par rapport à leur version "HTML/ERB". La concision d'HAML est bien réelle.

J'ai notamment trouvé particulièrement pratique de pouvoir modifier très facilement l'arborescence du document HTML simplement en faisant remonter ou redescendre un bloc puis en le réindentant correctement. Les "balises" HAML ne sont pas fermées comme en HTML, et de ce coté-là c'est un vrai plaisir de disposer d'une telle souplesse.

Là où j'ai clairement déchanté, en revanche, c'est sur la gestion de gros blocs de texte intégrés dans une page HTML. A terme, tous les textes de mon appli Rails seront bien sûr soit en base de données dans mes différents Modèles, soit dans les fichiers YAML d'internationalisation gérés par Rails (ce système d'[i18n](http://guides.rubyonrails.org/i18n.html) est fort simple et très pratique, d'ailleurs, et permet quelques techniques de ninja comme [celle-ci](http://opensoul.org/blog/archives/2012/11/05/abusing-rails-i18n-to-set-page-titles/) ).  

Mais puisque j'en suis aux premiers tests de mise en page, j'aurais aimé pouvoir coller tel quel mon pavé de texte dans mon template.  
Malheureusement HAML n'est vraiment pas fait pour ça, et [le système choisi](http://haml.info/docs/yardoc/file.REFERENCE.html#multiline) pour intégrer du texte multiligne n'est vraiment pas pratique - là où [Jade](http://jade-lang.com/) par exemple se contentait d'une simple indentation pour ce faire.  
On a certes le filtre ":plain" pour cet usage, mais dans mes tests une partie de l'indentation structurelle de mon texte se retrouvait appliquée dans le code source final de la page. Ce texte ayant vocation à être partiellement converti en Markdown, cette indentation me gêne assez.

J'ai finalement pu résoudre mon problème, en externalisant ce long texte ailleurs et en y faisant référence dans mon template HAML sous forme de variable - ce qui serait de toute façon arrivé tôt ou tard, puisque normalement aucun texte ne sera laisé tel quel dans les templates.  
Mais j'ai eu sur ce coup-là l'impression d'être davantage contraint que libéré par la syntaxe de HAML. Comme le dit Brandon Keepers dans [son article](http://opensoul.org/blog/archives/2011/11/30/haml-the-unforgivable-sin/), ce dernier permet effectivement de rendre le code source HTML plus concis, mais sans pour autant apporter de réelle valeur ajoutée, contrairement à ce que peuvent faire SASS et CoffeeScript pour les feuilles de style et le JavaScript.

Je vais continuer à utiliser HAML parce que c'est visiblement le moteur de template le plus utilisé avec Ruby on Rails (et de loin) et qu'il fait assez bien le boulot qu'on attend de lui, mais... j'avoue rester assez circonspect envers lui en ce qui me concerne. Le coup de coeur n'est clairement pas là.  
Tant pis. La route du Ruby on Rails ne peut pas non plus être couverte de jasmin tout au long du chemin.


Je vous retrouve bientôt pour la suite de ma découverte du développement RoR !