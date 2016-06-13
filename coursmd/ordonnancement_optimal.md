---
title: Ordonnancement optimal
author: Romain Gille
date: 08/06/2016
...

# Ordonnancement de projet

**Base : ** $t(0) = 0$
**Cas général :** $t(j) = Max{t(i) + d(i)}$

```java
int[][] calculerTetA(Liste[] P, int[] D){
    int n       = P.length;
    Liste[] Pp  = symetrique(P);
    int[] T     = new int[n], A = new int[n];
    T[0] = 0;
    A[0] = -1;
    for(int j = 1; j < n; j++){
        T[j] = Integer.MIN_VALUE;
        for(Liste l = Pp[j]; !vide(l); l = reste(l)){
            int i = premier(l);
            int m = T[i] + D[i];
            if(m > T[j]){
                T[j] = m;
                A[j] = i;
            }
        }
    }
    int[][] TA = {T, A};
    return TA;
}

static class Liste{
    int j;
    Liste reste;

    Liste(int j, Liste reste){
        this.j      = j;
        this.reste  = reste;
    }
}

static int premier(Liste l){return l.j;}
static boolean vide(Liste l){return l == null;}
static Liste reste(Liste l){return l.reste;}

Liste[] symetrique(Liste[] P){
    int n = P.length;
    Liste[] Pp = new Liste[n];
    for(int i = 0; i < n; i++){
        for(Liste l = P[i]; !vide(l); l = reste(l)){
            Pp[premier(l)] = new Liste(i, Pp[premier(l)]);
        }
    }
    return Pp;
}
```
