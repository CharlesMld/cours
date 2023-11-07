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
![image](https://github.com/CharlesMld/cours/assets/64355512/72a7cbd6-e1a8-42d7-bad3-5997c0042916)













