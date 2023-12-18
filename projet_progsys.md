But du projet: trier un fichier composé de pleins d'entiers par différentes méthodes

Pour cela on verra que le tri dans un seul fichier est plus rapide dans le cas où le fichier de base tient en mémoire.
S'il ne tient pas en mémoire, le répartir dans plein de fichiers et les trier en parallèle peut s'avérer plus efficace.

Les fork sont utiles mais ils ne fonctionnent pas en mémoire partagée alors que les threads oui.

Les appels system sont pas ouf il vaut mieux utiliser exec (voir cours progsys)
execlp("")

cmilliau@ross-a-16:~/Sort$ sort -g /tmp/test.sort.txt | diff -q - /tmp/test.txt 
Les fichiers - et /tmp/test.txt sont différents 
Ceci est normal car on sort le fichier test.sort.txt qui est déjà trié et on le compare à test.txt qui n'est pas trié donc les deux sont bien différents

cmilliau@ross-a-16:~/Sort$ sort -g /tmp/test.txt | diff -q - /tmp/test.sort.txt 
Cette commande ne renvoit rien ce qui est normal car test.txt est maintenant trié par la fonction unix sort puis passé en pipe (correspond au -) à la fonction diff
