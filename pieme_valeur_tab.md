---
title: Algorithmique -- Recherche de la $p$-ième valeur d'un tableau
author: Romain Gille
date: 06/04/2016
geometry: margin=1in
...

\newpage

*Suite du TD3, Exercice 2, calcul de la $p$-ième valeur*

* Trier $T$
* Retourner $T[p]$

Solution en $\Theta(n\log{n})$

Il faut être capable de calculer la valeur médiane : $p = \dfrac{n}{2}$.

On a $T[0:k_1] < T[k_1:k_2] < T[k_2:n]$ et $T[k_1] = T[k_1+1] = ... = T[k_2-1]$

* $p \in [k_1:k_2] \rightarrow$ retourner $T[k_1]$.
* $p \in [0:k_1] \rightarrow$ retourner la $p$-ième valeur de $T[0:k_1]$.
* $p \in [k_2:n] \rightarrow$ retourner la $(p - k_2)$-ième valeur de $[k_2:n]$.

```java
int pieme(int[] T, int i, int j, int p){
  segmenter(T[i:j]); // T[i:k1]<T[k1:k2]<T[k2:j]
  if(k1 <= p + i <= k2) return T[k1];
  else if(i <= p + i <= k1) return pieme(T, i, k1, p);
  else return pieme(T, k2, j, (p-k2)); //k2 <= p + i <= j
}
```

**Comment segmenter le tableau en trois parties ?**

On veut $T[0:k_1] < T[k_1:k_2] < T[k_2:n]$
et $T[k_1] = T[k_1+1] = ... = T[k_2-1]$

**$I(k_1, k_2, k_3) : T[i:k_1] < T[k_1:k_2] < T[k_3:j]$**

**Condition d'arrêt :** $k_2 = k_3$

**Initialisation :** $k_1 = i, k_2 = i+1, k_3 = j$

**Progression :**

$I(k_1, k_2, k_3) \text{ et }$

  * $T[k_3 -1] > T[k_1] \Rightarrow I(k_1, k_2, k_3 - 1)$
  * $T[k_3 -1] = T[k_1] \text{ et permutées } (T[k_3 - 1], T[k_2])
    \Rightarrow I(k_1, k_2 + 1, k_3)$
  * $T[k_3 -1] < T[k_1] \text{ et permutées } (T[k_3 - 1], T[k_2])
    \text{ et permutées } (T[k_1], T[k_2])
    \Rightarrow I(k_1 + 1, k_2 + 1, k_3)$
