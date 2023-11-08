# Exercice 1
1. systemd de pid 1
2. Pid du shell : 5040
3. pstree 5040 -> bash───pstree
4. pstree -p 5040 -> bash(5040)───pstree(6009)
5. gcc -Wall -o concurrence concurrence.c
6. bash(5040)─┬─concurrence(8116)
              └─concurrence(8117)
7. pstree -p 5040 -> bash(5040)
8. Dans shell 1 on tape ./concurrence & puis ./concurrence &
   Dans shell 2 pstree -p 5040 donne bash(5040)─┬─concurrence(8116)
                                                └─concurrence(8117)
9. bash(5040)─┬─concurrence(17350)
              ├─concurrence(17383)
              └─concurrence(17396)
10. .
11. ./concurrence & ./concurrence on observe un entrelacement des processus
    (33379) 2994 km ..., 2994 km à pied, ca use les souliers.
    (33380) 2659 km ..., 2659 km à pied, ca use les souliers.

# Exercice 2
En lancant 3 exécutions du code en parallèle les 4 coeurs sont mobilisés à 100%

# Exercice 3
`man fork`
En-tête à inclure :
```
#include <sys/types.h>
#include <unistd.h>
```

# Exercice 4
1. D'après le manuel,
   ERRORS
       EAGAIN A system-imposed limit on the number of threads was encountered.  There
              are a number of limits that may trigger this error
       EAGAIN The caller is operating under the SCHED_DEADLINE scheduling policy  and
              does not have the reset-on-fork flag set.  See sched(7).

       ENOMEM fork()  failed to allocate the necessary kernel structures because mem‐
              ory is tight.

       ENOMEM An attempt was made to create a child process in a PID namespace  whose
              "init" process has terminated.  See pid_namespaces(7).

       ENOSYS fork() is not supported on this platform (for example, hardware without
              a Memory-Management Unit).

       ERESTARTNOINTR (since Linux 2.6.17)
              System call was interrupted by a signal and will be  restarted.   (This
              can be seen only during a trace.)
2. bash(5040)───appel_fork1(62753)───appel_fork1(62754)
3. non
4. gnome-terminal-(5022)─┬─bash(5040)───appel_fork1(63173)───+
                         ├─bash(7350)───pstree(63243)
                         ├─{gnome-terminal-}(5023)
                         ├─{gnome-terminal-}(5025)
                         ├─{gnome-terminal-}(5026)
                         └─{gnome-terminal-}(7146)

![Capture d’écran du 2023-11-07 18-13-49](https://github.com/CharlesMld/cours/assets/64355512/c47791fe-a6f8-4e8f-a430-f383b1dabf7c)
![image](https://github.com/CharlesMld/cours/assets/64355512/424321de-b54a-43fd-9d8f-9bee4b94e763)


# Exercice 5
![image](https://github.com/CharlesMld/cours/assets/64355512/bde0a146-8d83-48a3-88a2-6dbb198377ce)
![image](https://github.com/CharlesMld/cours/assets/64355512/8875bdd2-f1fb-4395-8f1f-6de0f5138339)

# Exercice 6
![image](https://github.com/CharlesMld/cours/assets/64355512/1c5cc62d-6dc2-436d-aeb3-f5bfca82dbad)












# Exercice 8

Dans un système à temps partagé de type Linux, la création de processus est un élément essentiel du fonctionnement du système d'exploitation. Voici comment ce processus fonctionne :

1. Demande de création : La création d'un processus commence par une demande de création émanant d'un autre processus existant, généralement via un appel système comme "fork" ou "exec". Cette demande peut provenir d'un utilisateur ou d'un programme.

2. Allocation d'un espace d'adressage : Une fois la demande de création reçue, le système d'exploitation alloue un espace d'adressage pour le nouveau processus. Cela inclut la création d'une copie du tableau des descripteurs de fichiers, de la table des signaux et d'autres structures de données nécessaires.

3. Duplication du processus parent : Le processus parent est copié pour créer le processus enfant. Cette copie comprend le code, les données et l'état du processus parent, et les deux processus partagent initialement la même mémoire physique. Cependant, grâce à la mémoire virtuelle, chaque processus perçoit sa propre copie privée de la mémoire.

4. Modification de l'espace d'adressage : Le processus enfant peut ensuite modifier son espace d'adressage, par exemple en chargeant un nouveau programme avec l'appel système "exec". Cela permet au processus enfant de s'exécuter avec un code différent de celui du processus parent.

5. Planification et exécution : Une fois le processus enfant créé et configuré, le planificateur du noyau décide du moment où le processus enfant et le processus parent seront exécutés. Le système Linux utilise un ordonnanceur pour attribuer du temps CPU aux processus en fonction de leur priorité et de leur utilisation. Cela permet un partage équitable des ressources entre les processus.

6. Communication : Les processus peuvent communiquer entre eux en utilisant des mécanismes tels que les tubes, les signaux, les sockets ou la mémoire partagée, ce qui permet une coopération et une coordination entre les processus.

7. Terminaison : Les processus peuvent se terminer volontairement en utilisant un appel système comme "exit", ou être arrêtés de manière involontaire, par exemple en raison d'une erreur. Une fois un processus terminé, le système d'exploitation libère les ressources associées.

Le système à temps partagé de type Linux permet une exécution simultanée de plusieurs processus, garantissant que chaque processus a sa part équitable de temps CPU. La création de processus permet de gérer de manière flexible les tâches et les applications, contribuant ainsi à la stabilité et à l'efficacité du système.











