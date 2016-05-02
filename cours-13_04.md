---
title: Algorithmique -- Programmation Dynamique
author: Romain Gille
date: 13/04/2016
geometry: margin=1in
...

# Généralisation du problème du sac à dos

**Équation de récurence :** $m(0, c) = 0$ $\forall c, 0 \leq c \leq C$

**Cas général :** $1 \leq k \leq n$ $\forall c, 0 \leq c \leq C$


$m(k, c) :$
$$[0;k - 1] = \{0, 1, ..., k - 2\}$$
$$[0, k] = \{0, 1, ..., k - 2, k - 1\}$$

On a donc : $m(k, c) = max\{m(k - 1, c), v(k - 1) + m(k - 1, c t (k - 1))\}$

```java
int[][] M = int[n + 1][0 : C + 1];
// Base : k = 0
for(int c = 0; c <= C; c++) M[0][c] = 0;
// cas général : 1 <= k <= n, par taille k croissant
for(int k = 1; k <= n; k++){
  for(int c = 0; c <= C; c++){
    if(c - T[k - 1] < 0) M[k][c] = M[k - 1][c];
    else M[k][c] = Math.max(M[k - 1][c], V[k - 1] + M[k - 1][c] - T[k - 1]);
  }
}
```

En particulier : $M[n][C]$ est le sac de valeur max.

```java
void afficherSacValMax(int[][] M, int[] T, int k, int c){
  if(k > 0){
    if(M[k][c] == M[k - 1][c]) afficherSacValMax(M, T, k - 1, c);
    else{
      System.out.print((k - 1) + " ");
      afficherSacValMax(M, T, k - 1, c - T[k - 1]);
    }
  }
}
```

## Méthode alternative

```java
int m(int[] V, int[] T, int k, int c){
  // appel principal : m(v, T, n, C)
  if(k == 0) return 0;
  if(c - T[k - 1] < 0) return m(V, T, k - 1, c);
  return Math.max(m(V, T, k - 1, c), m(V, T, k - 1, c - T[k - 1]));
}
```

Cette méthode est en $\Omega(2^n)$ donc pas bon.

\newpage

# Sac à dos généralisé (TD 5)

* $n$ items
* item $i$ disponible en $q(i)$ exemplaires (objets)
* chaque objet de l'item $i$ est de taille $t(i)$
* un sac de capacité $C$

**Problème :** Calculer un sous-ensemble d'objet (pris dans les $n$ items), de
somme max et de somme de tailles $\leq C$.

## Différence avec sac à dos "de base"

Valeur d'un lot de $q$ exemplaires de l'item $i$.

$v(i, q) :$

* $v(i, 0) = 0$
* $v(i, q \geq 1) \leq q . v(i, 1)$
* $v(i, q)$ est non décroissante en $q$.

$m(k, c) :$ valeur max d'un sac $c$ rempli avec des objets puis dans le
sous-ensemble des $k$ premiers items.

**BASE :** $m(0, c) = 0, \forall c, 0 \leq c \leq C$

**CAS GÉNÉRAL :** $1 \leq k \leq n, 0 \leq c \leq C$

$m(k, c) = \max_{0 \leq q \leq Q(k - 1)}\{v(k - 1, q) + m(k - 1, c - q .
t(k - 1))\}$

```java
int[][] calculerM(int[][] V, int[] T, int C){
  int n = V.length;
  int[][] M = new int[n + 1][c + 1];
  for(int c = 0; c <= C; c++) M[0][c] = 0;
  for(int k = 1; k <= n; k++)
    for(int c = 0; c <= C; c++){
      M[k][c] = M[k - 1][c];
      int qK = V[k - 1].length; // nombre d'exemplaire du k-ième item
      for(int q = 1; q <= qK; q++)
        if(c - q * T[k - 1] >= 0){
          int mQ = V[k - 1][q] + M[k - 1][c - q * T[k - 1]];
          if(mQ > M[k][c]) M[k][c] = mQ;
        }
    }
  return M;
}
```
