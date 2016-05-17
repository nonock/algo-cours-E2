---
title: Algorithmique -- Tri par tas
author: Romain Gille
date: 23/03/2016
geometry: margin=1in
...

\newpage

# Tri par tas ($\Theta(n \log n)$)

## Un tas ?

> Un tas est un arbre binaire presque parfait, dont tous les sommets sont
  dominants.

*arbre binaire :* chaque n\oe ud a deux descendants.
*presque parfait :* parfait jusqu'à l'avant dernier niveau et le dernier
  niveau entièrement à gauche, sans trous.
*parfait :* chaque ligne a $2^p$ valeurs.
*sommets :* n\oe uds de l'arbre (indicés de haut en bas et de gauche à droite).
*dominants :* chaque indice est supérieur ou égal à chacun de ses descendants.

**Remarque :** $T[0]$ est la valeur max du tas.

### Gauche

$g(i) = 2 \times (i + 1) - 1$

### Droite

$d(i) = 2 \times (i + 1)$

### Ascendant (immédiat)

$a(i) = \dfrac{(i - 1)}{2}$

Cas particulier, $a(0) = 0$

### Degré ($0, 1 \text{ ou } 2$)

$\text{deg}(i) = \text{ si } g(i) > n - 1, \text{ alors } 0$
$\text{sinon, si } g(i) = n - 1, \text{ alors } 1$
$\text{sinon } 2$

```java
int deg(int i, int n){
  if(g(i) > n-1) return 0;
  else if(g(i) == n - 1) return 1;
  else return 2;
}
```

### Dominance

$\text{deg}(i) = 0 \text{ ou } \text{deg}(i) = 1 \text{ et } T[i] \geq T[g(i)]$
ou $\deg(i) = 2 \text{ et } T[i] \geq T[g(i)] \text{ et } T[i] \geq T[d(i)]$

```java
boolean domine(int[] T, int i){
  int n = T.length;
  int deg = deg(i, n);
  return deg == 0 ||
         deg == 1 && T[i] >= T[g(i)] ||
         deg == 2 && T[i] >= T[g(i)] && T[i] >= T[d(i)];
}
```

## Tri par sélection (Rappel)

$$I(k) : T[0:k] \text{ croissant et } T[0:k] \leq T[k:n]$$

**Init :** $k = 0$
**Arrêt :** $k = n (\text{ ou } k = n - 1)$
**Progression :** $I(k)$ et $k \neq n$ et $m =$ minimum de $T[k:n]$ et
  permutées($T[m], T[k]$) $\Rightarrow I(k + 1)$
**Complexité :** $\Theta(n^2)$

## Tri par tas

$$I(k) : T[k:n] \text{ croissant et } T[0:k] \leq T[k:n] \text{ et } T[0:k]
  \text{ est un tas}$$

**Init :** $k = n$ et $T[0:n]$ est un tas
**Arrêt :** $k = 0$ ou $k = 1$
**Progression :** $I(k)$ et $k \neq 0$ et permutées($T[k - 1], T[0]$)
$\rightarrow I(k - 1)$ et $T[0:k - 1]$ est un tas.

```java
void heapSort(int[] T){
  int n = T.length();
  faireTas(T);
  int k = n; // I(k)
  while(k != 1){
    permut(T, 0, k - 1);
    retablirTas(T, k - 1); // I(k - 1)
    k--; // I(k)
  }
}
```
**Complexité :** Le corps de boucle est en $\Theta(\log k) = O(\log n)$
(majoration). On le répète $n$ fois donc le temps de calcul est
majoré par $O(n \log n)$.
De plus, *heapsort* est en $\Omega(n \log n)$ (minoration) car c'est un
tris comparatifs. Donc *heapsort* est en $\Theta(n \log n)$.

\newpage

### Rétablir Tas

**Entrée :** le $k$-préfixe est "presque un tas" : $i \in [0:k] \\ {0}
\Rightarrow \text{ dominant}(i)$.
**Sortie :** $i \in [0:k] \Rightarrow \text{ dominant}(i)$, donc $T[0:k]$
est un tas.

$$I(j) : i \in [O:k] \\ {j} \Rightarrow \text{ dominant}(i)$$

**Init :** $j = 0$
**Arrêt :** $\text{dominant}(j)$

```java
void retablirTas(int[] T, int k){
  int j = 0;
  while(!domine(j)){
    if(deg(j, k) == 1){
      permut(T, j, T[g(j)]);
      j = g(j);
    } // I(j)
    else{
      if(T[g(j)] >= T[d(j)]){
        permut(T, j, g(j));
        j = g(j);
      } // I(j)
      else{
        permut(T, j, d(j));
        j = d(j);
      } // I(j)
    }
  }
}
```

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
