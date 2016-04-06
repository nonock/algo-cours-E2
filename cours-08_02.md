---
title: Algorithmique -- écriture de programmes efficaces et sûrs
author: Romain Gille
date: 08/02/2016
geometry: margin=1in
...

Propriété de `I(a, k')` : a = `arg min T[k:k']`

**Initialisation :** a = k et k' = k + 1

**Condition d'arrêt :** k' = n

**Progression :**

`I(a, k')` et k' $\neq$ n et `T[k']`$\geq$ `T[a]` $\Rightarrow$ `I( a, k' + 1)`

`I(a, k')` et k' $\neq$ n et `T[k']`< `T[a]` $\Rightarrow$ `I( k', k' + 1)`

\newpage

```java
int indiceMin(int[] T, int k, int n){ // retourne a = arg min T[k:n]
  int a = k, kp = k + 1; // I(a, k')
  while(kp != n){ // I(a, k') et k' != n
    if(T[kp] >= T[a]){ // I(a, k' + 1)
      kp ++; // I(a, k')
    }
    else{ // I(k', k' + 1)
      a = kp; // I(a, k' + 1)
      kp ++; // I(a, k')
    } // I(a, n) donc a = arg min T[k:n]
  }
}
```

## Temps de calcul de l'appel `indiceMin(T, k, n)`

L'initialisation est en temps constant, $\alpha$.

Le corps de boucle s'exécute en temps compté, $\beta$.

Le corps de boucle est exécuté `n - k - 1` fois le test `kp != n` est en temps
constant, $\gamma$. Il évalué `n - k` fois.

**D'où**

$T_{indiceMin}(k) = \alpha + (n-k-1)\beta + (n-k)\gamma$

$= (\alpha - \beta ) + (n - k)(\beta + \gamma)$

$= A + B(n-k)$

$= \Theta(n-k)$

```java
void triSelection(int[] T){
  int n = T.length;                                 // temps constant : delta
  int k = 0; // I(k)                                // temps constant : delta
  while(k != n){ // I(k) et k != n                  // A' + B(n - k)
    int m = indiceMin(T, k, n);                     // A' + B(n - k)
    int x = T[k];                                   // A' + B(n - k)
    T[k] = T[m];                                    // A' + B(n - k)
    T[m] = x; // I(k + 1)                           // A' + B(n - k)
    k = k + 1; // I(k)                              // A' + B(n - k)
  } // I(n) donc T[0:n] trié dans l'ordre croissant // A' + B(n - k)
}
```

$T_{TS}(n) = \delta + n(A' + {{B} \over {2}}) + n^2 * {{B} \over {2}}$

\newpage
