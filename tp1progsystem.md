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

