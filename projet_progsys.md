But du projet: trier un fichier composé de pleins d'entiers par différentes méthodes

Pour cela on verra que le tri dans un seul fichier est plus rapide dans le cas où le fichier de base tient en mémoire.
S'il ne tient pas en mémoire, le répartir dans plein de fichiers et les trier en parallèle peut s'avérer plus efficace

Les fork sont utiles mais ils ne fonctionnent pas en mémoire partagée alors que les threads oui.
