---
title: Algorithmique -- MergeSort
author: Romain Gille
date: 30/03/2016
geometry: margin=1in
...

\newpage

# Dernier tri de tableau : MergeSort (tri fusion)

* Tri optimal (non stochastique) : $\Theta(n \log n)$
* Copie du tableau (ne trie pas "sur place")
* Approche "Diviser pour régner" (idem QuickSort)

```java
  void mergeSort(int[] T){
    int n = T.length;
    ms(T, 0, n);
  }
  void ms(int[] T, i, j){
    int n = j - i;
    if(n >= 2){
      int[] T1 = Arrays.copyOfRange(T, i, n/2),
            T2 = Arrays.copyOfRange(T, n/2, j);
      mergeSort(T1);
      mergeSort(T2);
      merge(T1, T2, T, i, j);
    }
  }
```

$I(k, k1, k2) : T[i:k] \text{ suivi de } T[k1:n1] \text{ fusionné avec }
T2[k2:n2] = T1[0:n1] \text{ fusionné avec } T2[0:n2]$

**Initialisation :** $k1 = 0, k2 = 0, k = i$

**Arrêt :** $k1 = n1 \text{ ou } k2 = n2$ *il reste à faire après ça*

**Progression :** $I(k, k1, k2)$ et $\overline{\text{arrêt}}$ et $T1[k1] \leq
T2[k2]$ et $T[k] = T1[k1] \Rightarrow I(k + 1, k1 + 1, k2)$
$I(k, k1, k2)$ et $\underline{\text{arrêt}}$ et $T1[k1] \leq T2[k2]$ et
$T[k] = T2[k2] \Rightarrow I(k + 1, k1, k2 + 1)$

```java
  void merge(int[] T1, int[] T2, T, i, j){
    int n1 = T1.length, n2 = T2.length, k1 = 0, k2 = 0, k = i; // I(k, k1, k2)
    while(k1 != n1 && k2 != n2){
      if(T1[k1] <= T2[k2]){
        T[k] = T1[k1];
        k ++;
        k1 ++;
      } else{
        T[k] = T2[k2];
        k ++;
        k2 ++;
      }
    }
    while(k1 != n1){
      T[k] = T1[k1];
      k ++;
      k1 ++;
    }
    while(k2 != n2){
      T[k] = T2[k2];
      k ++;
      k2 ++;
    }
  }
```
`merge()` est de complexité $\Theta(n)$.
