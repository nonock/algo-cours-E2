---
title: Algorithmique -- Tri par tas
author: Romain Gille
date: 30/03/2016
geometry: margin=1in
...

*Suite du heapsort*

### Faire tas

*On cherche à augmenter le tas : le $k$ préfixe est un tas $\rightarrow$ le
$k + 1 préfixe est un tas*.

$I(k) : T[0:k]$ est un tas

```java
  void faireTas(int[] T){
    int n = T.length();
    int k = 1; // I(k)
    while(k != n){
      augmenterTas(T, k); // I(k + 1)
      k++
    } // I(n) donc T[0:n] est un tas
  }
```
`faireTas` est en $\Theta(n \log n)$ car `augmenterTas` est en $\Theta(\log k)$
et le `while` est en $O(\log n)$.

\newpage

#### Augmenter tas

$I(j) : i \in [0:k + 1]$ \\ $\{a(j)\}$

**Initialisation :** $j = k$

**Arrêt :** $a(j)$ domine. Alors $i \in [0:k + 1] \Rightarrow i$ est dominant.
donc $T[0:k + 1] est un tas$.

**Progression :** $I(j)$ et $a(j)$ non dominant et permutées($j, a(j)$)
$\rightarrow I(a(j))$.

```java
  void augmenterTas(int[] T, int k){
    int j = k; // I(j)
    while(! domine(a(j))){ // I(j) et a(j) non dominant
      permuter(j, a(j)); // I(a(j))
      j = a(j); // I(j)
    } // I(j) et a(j) domine => T[0:k + 1] est un tas
  }
```
`augmenterTas` est en $\Theta(\log k)$ car `permuter` est en $\Theta(1)$
et le `while` est en $O(\log k)$.

\newpage
