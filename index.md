# Veille technologique

## IA et jeux de stratégie combinatoires abstraits

### Introduction

> Qu'est-ce qu'un jeu de stratégie combinatoire abstrait ?

Un jeu de stratégie combinatoire abstrait doit remplir quatres conditions différentes :

- Premièrement, seulement deux entités peuvent y participer (une entité peut être un joueur ou une équipe de joueurs), et ils doivent être antagonistes.
- Deuxièmement, le jeu doit avoir lieu au tour par tour.
- Troisièmement, toute l'information doit être disponible aux deux joueurs. Par exemple, un jeu de cartes, où l'on cache ses cartes à son adversaire, ne peut remplir cette condition.
- Quatrièmement, le jeu ne doit pas contenir de hasard. Cela reviendrait à flouter de l'information aux joueurs.

Cette définition peut sembler compliqué, mais tout le monde à déja joué à ce type de jeu, que ce soit sous la forme d'une partie de puissance quatre, d'échecs, ou même de Go.

> Où en est l'intelligence artificielle ?

Cela fait déjà maintenant quelques dizaines d'années que de nombreuses personnes se posent la question de la performance des intelligences artificielles dans ce type de jeu. Dès 1958, on observe déjà des algorithmes capables de battre des humains lambda aux échecs, c'est à dire avec très peu de connaisances du jeu, à travers le NSS Chess Program, développé par trois chercheurs de Carnegie Mellon University. Les échecs ont toujours été l'un des jeux le plus fréquemment utilisé pour démontrer la puissance de ces algorithmes, grâce à sa popularité dans le monde.
Cependant, le vrai point de rupture eut lieu en 1997, avec la défaite de Garry Kasparov, à ce moment champion du monde d'échecs, par Deep Blue, un super ordinateur créé par IBM. Depuis ce jour, les algorithmes n'ont cessé d'évoluer, on est capables de développer ce type d'algorithmes pour la majorité des jeux de stratégie combinatoires abstraits, et ils n'ont aucun mal à battre les meilleurs joueurs humains.

### Etat de l'art

> Quels sont les algorithmes à l'état de l'art actuellement ?

Aujourd'hui, on peut facilement trouver des informations sur les algorithmes les plus performants et sur leur fonctionnement. La plupart d'entre eux fonctionnent avec des principes d'apprentissage par renforcement, et ils existent pour pratiquement tous les jeux. Par exemple, pour le shogi, il y a Elmo, les échecs ont Stockfish, et le jeu de Go a AlphaGo. Mais il existe un algorithme encore plus récent capables de battre tous ces algorithmes spécialisés, et ceci en étant de capable de jouer à une multitude de jeux différents : AlphaZero. Il a été développé par DeepMind a été révélé le 5 Décembre 2017. L'algorithme a été entrainé uniquement en jouant contre lui même, et avec seulement 8 heures d'entrainement, il était déjà capable de battre la concurrence existante. Aujourd'hui, même cinq après, il reste un algorithme extrêmement performant qui a continué d'être amélioré.

### Fonctionnement des algorithmes

> Comment programmer le jeu ?

La première partie de la création d'un algorithme qui peut jouer à un jeu de stratégie combinatoire abstrait, est tout simplement de coder ce jeu. Cela peut parait trivial, cependant, il existe de nombreuses petites optimisations pendant ce processus qui sont nécessaires de mettre en place pour faciliter l'entrainement. Un des points les plus importants est l'utilisation de bitboards. Quasimment tous les jeux de stratégie combinatoire abstrait utilisent un plateau, ou du moins quelque chose qui fait office de plateau. Naïvement, on pourrait penser qu'il suffit de le traduire par une matrice, et d'y ajouter les composants au fil du jeu comme on le souhaite, par exemple sous forme d'objets qui représentent leurs caractéristiques au cours de la partie. Cependant, il se trouve que l'on peut employer une méthode beaucoup plus efficace qui va grandement accélérer la phase d'entrainement des algorithmes d'apprentissage par renforcement. A la place de stocker précisément ce qui se trouve dans chaque case du plateau, on va simplement stocker l'information sous sa forme la plus simple : une chaîne de bits. Cela permettra de réduire non seulement la complexité spatiale, en construisant des tableaux biens moins riches en informations, mais surtout d'aller plus vite lors des calculs.

> Comment choisir un coup ?

Une fois que l'on est capable de faire en sorte de jouer des parties, il faut créer un agent intelligent et l'entraîner. Cet entraînement se fait généralement grâce à un algorithme d'apprentissage par renforcement, où l'algorithme va jouer contre lui même ou contre d'autres algorithmes existants afin d'optimiser ses paramètres. Mais que faut-il optimiser ?

La première partie est l'optimisation de la fonction d'évaluation. La fonction d'évaluation est un moyen pour l'algorithme d'évaluer si un état lui plait ou non d'un point de vue stratégique. Evidemment, il ne peut pas la connaître dès le départ, à moins qu'il soit dôté d'une logique simple. On peut donc lui faire optimiser ses décisions grâce à l'apprentissage par renforcement, et le voir développer son raisonnement sur plusieurs paramètres. Il apprendra à identifier certains paternes, et surtout comment répondre de manière optimale à ces derniers.

Une fois que l'algorithme est capable d'identifier un état, il faut qu'il soit capable d'aller plus loin que ce dernier. Pour cela, il faut mettre en place un parcours d'arbre. En effet, on peut représenter une partie de jeu combinatoire abstrait sous forme d'arbre. A chaque état, le joueur a plusieurs choix possibles pour jouer. L'état actuel est la racine de l'arbre, et ses feuilles sont les états suivants. De plus, on peut associer la valeur de l'état à chaque noeud pour se rendre compte de la pertinence d'un chemin. De manière récursive, il est donc théoriquement possible de construire un arbre qui englobe toutes les parties existantes. Cependant, à moins de jouer au Morpion, ces arbres sont gigantesques, et il va donc falloir faire des choix sur quelle partie de l'arbre on veut représenter. On va donc limiter la profondeur de l'arbre, et optimiser le parcours d'arbre pour que l'algorithme fasse ses choix de manière rapide et efficace. Pour cela, il existe plusieurs méthodes. La plus utilisée aujourd'hui est la méthode de Monte Carlo, qui calcule une sortie possible pour chaque feuille de l'arbre, et en remontant à travers l'arbre, donne le meilleur chemin vers la victoire. D'autres existent, comme l'élagage Alpha-Beta, qui permet de supprimer les branches inintéressantes.

Il existe aussi des astuces que l'on peut utiliser pour améliorer les performances de ces algorithmes. L'une des plus connues est l'utilisation d'arbres d'ouverture. Le principe est simple : au début de la partie, le nombre de combinaisons possibles, et surtout de combinaisons viables, est assez faible, et pour les jeux les plus connus, elles ont même déjà été étudiées par des humains. On peut donc simplement donner ces informations à l'algorithme sous forme de table qu'il va utiliser en début de partie pour ne pas gâcher de puissance de calcul, et surtout ne pas gâcher de temps. Ces tables existent aussi pour la fin de jeu, et elles peuvent donc être utiles pour aider un algorithme à reconnaitre des patternes gagnants vers la fin du jeu.

> Comment améliorer ces algorithmes ?

Bien évidemment, il reste toujours de quoi faire pour aller plus loin dans ce domaine. De nombreuses propositions sont encore faites, et j'ai choisi de parler de deux solutions qui me paraissaient intéressantes. La première repose sur un raccourci qui peut être fait sur la représentation théorique du jeu. En effet, on a choisi de représenter le jeu comme un arbre, mais cela est-il vraiment la meilleur de le représenter ? Il se trouve qu'un graphe pourrait être plus adapté, car l'arbre comporte des états dupliqués. Si l'on reprend l'exemple du morpion, imaginons une partie où le joueur avec les cercles commences. Il met un cercle au milieu, l'autre joueur met une croix en bas à gauche de la grille, et lui remet un cercle en bas de la grille. On aurait également pu arriver à ce même état si le joueur qui a les cercles avait commencé avec un cercle en bas, puis enchainé avec un cercle au milieu.
Une autre amélioration intéressante pourrait venir du population based training. C'est une méthode qui n'a encore été que trop peu utilisée, mais qui pourrait intervenir dans l'optimisation des hyperparamètres de l'algorithme, qui sont souvent optimisés avec des algorithmes simples voire à la main. Il consiste simplement à faire tourner plusieurs agent apprenants en même temps avec des hyperparamètres différentes, et de les faire communiquer entre eux sur quels hyperparamètres fonctionnent le mieux.

### Conclusion

Pour conclure, le champ de l'intelligence artificielle est très fortement lié à la théorie des jeux et aux jeux de stratégie combinatoire abstraits. C'est un domaine qui passione les gens et motivent les chercheurs, ce qui permet des avancées et ébahit souvent le public. C'est aussi un champ un expansion constante, et qui ne semble pas prêt de s'arrêter.

## Annexe

[Document sur la méthode de veille](https://raw.githubusercontent.com/Pntony/NTIC/gh-pages/PARODI_Rapport_methodologique.pdf)

### Sources

1. Bratko, I. ‘AlphaZero - What’s Missing?’, n.d., 6.
2. [‘CCC: Lc0 vs Dragon - Computer Chess Championship - Chess.Com’](https://www.chess.com/computer-chess-championship). Accessed 17 March 2022. .
3. Chen, Jim X. [‘The Evolution of Computing: AlphaGo’](https://doi.org/10.1109/MCSE.2016.74). Computing in Science Engineering 18, no. 4 (July 2016): 4–7. .
4. Granter, Scott R., Andrew H. Beck, and David J. Papke Jr. [‘AlphaGo, Deep Learning, and the Future of the Human Microscopist’](https://doi.org/10.5858/arpa.2016-0471-ED). Archives of Pathology & Laboratory Medicine 141, no. 5 (1 May 2017): 619–21.
5. [‘I Created an AI to Play Chess - YouTube’](https://www.youtube.com/watch?v=DZfv0YgLJ2Q). Accessed 17 March 2022.
6. Li, Fangxing, and Yan Du. [‘From AlphaGo to Power System AI: What Engineers Can Learn from Solving the Most Complex Board Game’](https://doi.org/10.1109/MPE.2017.2779554). IEEE Power and Energy Magazine 16, no. 2 (March 2018): 76–84.
7. Wang, Fei-Yue, Jun Jason Zhang, Xinhu Zheng, Xiao Wang, Yong Yuan, Xiaoxiao Dai, Jie Zhang, and Liuqing Yang. [‘Where Does AlphaGo Go: From Church-Turing Thesis to AlphaGo Thesis and Beyond’](https://doi.org/10.1109/JAS.2016.7471613). IEEE/CAA Journal of Automatica Sinica 3, no. 2 (April 2016): 113–20.
8. Wu, Ti-Rong, Ting-Han Wei, and I-Chen Wu. [‘Accelerating and Improving AlphaZero Using Population Based Training’](https://doi.org/10.1609/aaai.v34i01.5454). Proceedings of the AAAI Conference on Artificial Intelligence 34, no. 01 (3 April 2020): 1046–53.
