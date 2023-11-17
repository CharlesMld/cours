# Exercice 1

## 1
"que vous lirez peut−être si vous avez le temps\n" s'affiche en premier car il y a un sleep(2) dans le code du fils
"Monsieur le Président, je vous fais une lettre\n" s'affiche ensuite

## 2
On utilise la primitive wait() avec l'argument NULL dans le else() du père 

## 3
La premiere question qui me vient en tete est pourquoi definir une var status ????!!!! car ici il semblerait...
Réponse du prof ! quand on donne le &status en arguemnt du wait celui-ci y écrit la valeur de retour ud wait ie
soit l'entier d'erreur ou de réussite
Ainsi le programme 2 fonctionne mais le wait du fils associe à la var status un entier d'erreur ca rle fils n'a pas de fils
Le programme 3 fonctionne et cette fois on sort du fils avant de faire le wait donc pas de var d'erreur dans status
Le prog 4 ne fonctionne pas car le père n'attend pas son fils avant de s'exécuter (lignes 17 et 18 inversées)

# Exercice 2

## 1
## 2
Le père devrait temriner en premier
## 3
Non car son père se termine avant lui ainsi, à un moment le fils affichera 
(fils, 10338) .. mon père est 10337
et ensuite, après la terminaison du père,
(fils, 10338) .. mon père est 2257
On rajoute un wait dans le père

# Exercice 3

## 1
printf asynchrone
int main(void)
{
	pid_t pid_fils = -1;	/* Pour récupérer la valeur de retour de l'execution du fork */
	printf("Je suis le père\n");
    pid_fils = fork();	/* Création du processus fils */
	if (pid_fils == 0) {
        sleep(2);
		printf("Je suis le fils\n");
		exit(EXIT_SUCCESS);
	} 
    else {
        wait(NULL);
		printf("Je suis le pere après avoir créé mon fils\n");
        exit(EXIT_SUCCESS);
	}
}

## 2
Simple

## 3
Ici, le père dort 5 sec et le fils dort 10 sec ainsi le père attend 5 sec de plus pour que son fils se termine puis s etermine
int main(void)
{
	pid_t pid_fils = -1;	/* Pour récupérer la valeur de retour de l'execution du fork */
    pid_fils = fork();	/* Création du processus fils */
	if (pid_fils == 0) {
        sleep(10);
		printf("Je suis le fils\n");
		exit(EXIT_SUCCESS);
	} 
    else {
        sleep(5);
        wait(NULL);
		printf("Je suis le pere\n");
        exit(EXIT_SUCCESS);
	}
}

## 4
Le processus fils est alors terminé mais dans un état zombie car le père n'a pas encore acquitté son fils

# Exercice 4
Tous les processus sont zombies jusqu'à ce que le père les acquitte avec le wait.
Dans cet exemple, beaucoup trop de processus zombies sans être acquittés => saturation

# Exercice 5

















