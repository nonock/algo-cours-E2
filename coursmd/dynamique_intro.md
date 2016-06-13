---
title: Algorithmique -- Programmation dynamique
author: Romain Gille
date: 06/04/2016
geometry: margin=1in
...

\newpage

# Principe d'optimalité de Richard Bellman (début des années 50)

> Toute politique optimale est construite sur des sous-politiques optimale.

# Problème du sac à dos

$n$ objets $[0:n]$
$\forall i$, objet $i$ de taille $t_i$ et valeur $v_i$
sac de capacité $C$

**Problème :** Remplir le sac avec un sous-ensemble d'objets de somme de tailles
$\leq C$ et de somme de valeurs maximum.

**Algorithme recherche exhaustive :** $\Omega(2^n)$
On ne peux donc pas traiter ce problème.


valeur du $n$-ième objet $(v_{n-1})$.
valeur max d'un sac $C - t_{n-1}$ rempli d'un sous ensemble des $n-1$ premiers
objets.
$\Rightarrow$ valeur max d'un sac $C$ rempli avec un sous-ensemble des $n$
premier objets **sachant que le $n$-ième objet est dans le sac**.
Si le $n$-ième objet n'est pas dans le sac, la valeur du sac $C$ est la valeur
max du sac $C$ rempli avec un sous-ensemble des $n-1$ premiers objets.

## Notations

$m(k, c)$ : valeur max d'un sac de capacité $c$, $0 \leq c \leq C$, rempli avec
un sous-ensemble des $k$ premiers objets.

On veut calculer $m(n, C)$.

## Base de la récurrence

$k = 0, m(0, c) = 0, \forall c : 0 \leq c \leq C$

## Cas général

$1 \leq k \leq n$
$m(k, c) = $

* On ne prend pas le $k$-ième objet : $m(k - 1, c)$
* On prend le $k$-ième objet : $m(k - 1, c - t_{k-1}) + v_{k-1}$

donc $m(k, c) = MAX{m(k - 1, c), m(k - 1, c - t_{k-1}) + v_{k-1}}$
$\forall c, 0 \leq c \leq C$

On calcule $M[0:n + 1][0:C + 1]$ de terme général $M[k][c] = m(k, c)$.

\newpage

```java
int[][] calculerM(int[] T, int[] V, int C){
  int n = T.length;
  int[][] M = new int[n + 1][C + 1];
  //Base
  for(int c = 0; c <= C; c++) M[0][c] = 0;
  //Cas général par k croissant
  for(int k = 1; k <= n; k++){
    for(int c = 0; c <= C; c++){
      if(T[k - 1] > c) M[k][c] = M[k - 1][c]
      else{
        int mk = M[k - 1][c - T[k - 1]] + V[k - 1]; //max si le k-ième est pris
        M[k][c] = Math.max(mk, M[k - 1][c]);
      }
    }
  }
  return M;
}
```

Pour ce problème, on est en $\Theta((n + 1)* (c + 1)) = \Theta(nC)$

# Généralisation du problème du sac à dos

**Équation de récurrence :** $m(0, c) = 0$ $\forall c, 0 \leq c \leq C$

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

\newpage

## Affichage pour le sac à dos de base

On construit $M[0:K+1][0:C+1]$ :

$M[k][c] = m(k, c)$

```java
void asm(int[][] M, int[] t, int k, int c){ // affichage sac max
    if(k >= 1){
        if(M[k][c] == M[k-1][c]){ // le kième objet n'est pas dans le sac
            asm(M, T, k-1, c);
        }
        else{ // kième objet dans le sac
            System.out.print(k-1 + ", ");
            asm(M, T, k-1, c-t[k-1]);
        }
    }
}
```

## Affichage pour le sac à dos généralisé

On calcule "à la volée" la matrice M et une matrice $A[0:K+1][0:C+1]$ de terme
général $A[k][c] = \text{arg} m(k, c)$, quantité du $k^{e}$ item dans un sac
$m(k, c)$.

**CODE DANS LE MAIL**

```java
void asmg(int[][] A, int[] t, int k, int c){
    asmg(A, t, K, C); // appel principal
    if(k >= 1){
        System.out.print((k-1) + ", " + A[k][c] + "\t");
        asmg(A, t, k-1, c - A[k][c] * t[k-1]);
    }
}
```
