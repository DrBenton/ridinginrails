Bonjour !

Aujourd'hui pas de palpitantes (?) découvertes dans mon apprentissage de Rails à raconter ici, puisque je suis revenu aux fondamentaux avec.... une formation Ruby donnée par des pros, j'ai nommé [Human Coders](http://formations.humancoders.com/) !

Cette première journée était consacrée aux bases du langage, que j'avais déjà pas mal potassées en ce qui me concerne. Aussi connaissais-je déjà sur le papier la plupart des points que nous avons vu ensemble pour ce premier jour.  

Cependant à la lumière de cette première séance j'ai pu constater qu'à travers mon auto-formation à grand renfort de tutos et de Stack Overflow il m'a tout de même manqué une utilisation immédiatement mise en pratique des bases du langage. J'étais en effet tellement impatient de me mettre à Ruby on Rails que je m'étais contenté d'effleurer le langage Ruby lui-même.

### A la découverte des trésors cachés de l'API Ruby

Après cette première journée je vois que j'étais passé à coté de certains choses, que l'API de Ruby contient des méthodes super pratiques un petit peu partout que je ne connaissais pas par exemple - lorsqu'on s'y met tout seul on a tendance à seulement rechercher dans la doc de l'API les équivalents des méthodes qu'on connaît dans d'autres langages, ce qui m'avait fait passer à coté de chouettes trucs comme la méthode [sample](http://ruby-doc.org/core-2.0/Array.html#method-i-sample) de la classe Array.  
De même, comment aurais-je pu soupçonner que Ruby comportât une méthode [Math.hypot](http://ruby-doc.org/core-2.0/Math.html#method-c-hypot) pour calculer une hypoténuse ? Cette fonction, essentielle la plupart du temps lorsqu'on programme un jeu par exemple, n'est pas présente même dans un langage quasiment dédié au jeu comme ActionScript... mais elle existe bel et bien en Ruby :-)

### Ruby est décidément un langage pragmatique

A la lumière du bien humble savoir qu'est le mien aujourd'hui sur Ruby, mon impression d'un langage orienté vers le pragmatisme s'est renforcée.  
Là où PHP par exemple essaie depuis quelques années de se rapprocher du Java avec des _design patterns_ dans tous les sens, des imports par dizaines dans chaque classe et la programmation par annotations (que je critique absolument pas, pour ma part j'aime bien), j'ai le sentiment que Ruby ne s'embarrasse pas des concepts les plus abstraits de la Programmation Orientée Objet.  

Pas de classes abstraites, pas d'Interfaces, pas ou peu d'utilisation de l'injection de dépendances... Ruby se concentre sur une API riche et orientée vers des méthodes qui rendent service au quotidien, et se contente pour la POO de l'Héritage et de l'utilisation des Mixins - qui sont, ai-je d'ailleurs appris aujourd'hui à travers la formation, moins simplistes qu'ils semblent l'être.  

Une classe peut ainsi se retrouver bardée de fonctionnalités uniquement via l'utilisation de Mixins (ce qu'on constate dans Rails, d'ailleurs), et le développeur est invité à ne pas de prendre la tête sur des questions comme "Vais-je utiliser une classe asbtraite ou une interface pour cette fonctionnalité ?", pour se concentrer sur la réutilisation simple et maximale de fonctionnalités avec les Mixins.  
- tout ceci n'est bien sûr que mon impression en tant que débutant.

### Lire et retenir c'est bien, appliquer c'est encore mieux

Au-delà de ces petites découvertes de l'API, la formation est bien progressive et portée par des exercices pratiques ludiques, qui m'ont permis de mettre en pratique tout ce que j'avais découvrir sur le Net.  Lire et retenir est une chose, mettre en application correctement en est une autre.  

[Matthieu](https://twitter.com/MatthieuSegret) est à l'écoute des questions, et a pu répondre à mes nombreuses (j'ai dû être lourd à la longue, je vais tâcher de moins l'interrompre demain :-) questions sur "Ruby en pratique" : en pratique, qu'utilise un "Rubyiste" lorsqu'il doit implémenter telle ou telle fonctionnalité, alors que Ruby propose tel moyen d'y parvenir mais aussi tel autre ? Tout un pan que je pouvais acquérir avec de l'auto-formation. J'ai même pu caser quelques questions sur Rails !  
...et puis un homme qui vous accueille par un petit déjeuner avec viennoiseries et jus d'orange avant de débuter la journée de formation ne peut assurément pas être un mauvais bougre :-)


Bref, je suis impatient de voir ce que les journées suivantes me réservent. Pour ma part j'attends particulièrement la partie "Créer ses propres Gems", ainsi que de celle sur "Créer son propre DSL". Wait and see...

A bientôt !


