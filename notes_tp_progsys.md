# TP1
pid terminal courant: `ps`
Afficher sous arbre du processus (pid) : `pstree -p 'pid'`
Quand on mobilise un coeur de processeur avec process qui fait beaucoup de calcul et qu'on exécute ce processus dans 3 autre sterminaux, les 4 coeurs du processeur sont mobilisés à 100% (commande htop)
wait(&status) après avoir défini int &status; alors wait() renvoie le result de wait qui peut être une erreur (par ex si le processus n'a pas de fils).
Orphelin : le père se termine avant
Zombie : se termine avant son père donc avant d'être acquitté par wait(NULL) dans code du père
Fork bomb : plein de fils créés sans être acquittés -> saturation espace mémoire

On peut faire du recouvrement de code avec exec

# TP3 - Signaux
typedef void (*sighandler_t)(int);
sighandler_t return_value;
void gestionnaire(int signal){
    if (signal == SIGINT){
        printf("stop\n");
    }
}

int main(int argc, char * argv[]){
    return_value = signal(SIGINT, gestionnaire);
    if (return_value == SIG_ERR){
        printf("erreur\n");
        exit(1);
    }
    int i = 0;
    while (i<30){
        printf("%d\n",i);
        sleep(1);
        i+=1;
    }
}

Pour envoyer signal à pid: kill -SIGINT pid

# TP4 - Tubes
Utilisation d'un tube pipe dans un même programme
int main (void){
    int tube[2]; int returnValue; // used to store the pipe and the return value of its creation
    char buffer; // used to store a char read from the pipe. We will read char by char in the pipe
    pid_t pid_fils;  

    /* Creation du tube */
    returnValue = pipe(tube); if (returnValue != 0) { perror("Echec creation tube"); return EXIT_FAILURE; }

    /* Création du fils */
    pid_fils = fork(); if (pid_fils == -1) { perror("Echec fork\n"); return EXIT_FAILURE;}

    if (pid_fils != 0) { /* Code affecté au pere */
        sleep(10);
        close (tube[0]); /* Fermeture du descripteur en lecture puisque le pere veut écrire */
        /* Ecriture dans le tube */
        char * test = "Test du texte"; // (pointeur vers chaine de caractères constante)
        printf("(père) j'écris \"Test du texte\"\n");
        write(tube[1], test, strlen(test)); // string value without '\0' as we will retrieve char by char
        close (tube[1]);
        wait(NULL); /* Attente de la terminaison du processus fils*/
        return EXIT_SUCCESS;
    }
    else { /* Code affecté au fils */
        close (tube[1]); /* Fermeture du descripteur en ecriture puisque le fils veut lire */
        /* Lecture du tube dans le tube, caractère par caractère avec écriture sur la sortie standard (STDOUT_FILENO est le descripteur de fichier associé à stdout) */
        while (read(tube[0], &buffer, 1) > 0) {printf("(fils) j'ai lu %c\n", buffer);}
        close (tube[0]);
        exit(EXIT_SUCCESS);
    }
}
// En faisant un sleep(10) dans le père le fils attend aussi 10sec pour afficher
// car avant cela il n'y a rien dans le tube

Utilisation d'un tube nommé "fifo" dans deux processus différents
**Processus écrivain**
int main (void){
    unlink("tube");
    int return_value = mkfifo("tube", 0600);
    if (return_value ==-1){printf("tube existant : on va travailler avec le tube existant\n");} else {printf("tube cree dans le processus ecrivain\n");}
    /* Ouverture du tube pour ecriture */
    int tub_nomme = open("tube", O_WRONLY);
    if(tub_nomme == -1){printf("ouverture tube impossible dans le processus ecrivain\n");return EXIT_FAILURE;}
    char * texte1 = "Ils ont des chapeaux ronds, vive la Bretagne\n"; // (pointeur vers chaine de caractères constante)
    char * texte2 = "Ils ont des tonneaux ronds, vive les bretons\n";
    /* Ecriture dans le tube */
    printf("ecriture 1\n"); write(tub_nomme, texte1, strlen(texte1));
    sleep(4);
    printf("ecriture 2\n"); write(tub_nomme, texte2, strlen(texte2));
    /* Fermeture du tube */
    close(tub_nomme);

    /* Suppression du tube */
    /* unlink = demande de suppression du tube, si on le désire (sinon reste
        sur le système mais ne contient pas de données (point de rencontre) ) */
    unlink("tube");
    return EXIT_SUCCESS;
}

**Processus lecteur**
int main (void){
    char buffer[5];
    /* Ouverture du tube pour lecture */
    int tub_nomme = open("tube", O_RDONLY);
    if(tub_nomme == -1){perror("ouverture tube impossible dans le processus lecteur\n");return EXIT_FAILURE;}

    /* Lecture dans le tube, par paquets de 5 octets */
    buffer[5]='\0'; // Ajout caractère de fin de chaîne en position 5 du buffer (pour affichages).
    while (read(tub_nomme, buffer, 5)){printf("Je viens de lire : %s\n",buffer); sleep(1);}

    /* Fermeture du tube */
    close(tub_nomme);
    exit(EXIT_SUCCESS);
}

# TP5 - Mémoire partagée et sémaphores
