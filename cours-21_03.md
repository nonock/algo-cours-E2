---
title: Algorithmique -- écriture de programmes efficaces et sûrs
author: Romain Gille
date: 21/03/2016
geometry: margin=1in
...

# Retour sur le TP

## Exercice 2

Calcul en $\Theta(n_1 + n_2)$ de $E = E_1[0:n_1] \text{inter} E_2[0:n_2]$

**Propriété :** $I(k, k_1, k_2)$
$$E[0:k] \text{union} E_1[k_1:n_1] \text{inter} E_2[k_2:n_2] = E_1 \text{inter} E_2$$

**Arrêt :** $k_1 = n_1 \text{ ou } k_2 = n_2$  
`while(!(k1 == n1 || k2 == n2)) ...`  
`while(k1 != n1 && k2 != n2) ...`  

## Exercice 3

On cherche $a \times b$.

**Cas d'arrêts :**

* $b = 0$ : $a \times b = 0$

**Cas généraux :** *$b > 0$*

* $b$ pair : $a \times b = (2a)(b/2)$
* $b$ impair : $a \times b = a + (2a)(b/2)$

```java
  int mD(int a, int b){
    if(b == 0) return 0;
    if(b % 2 == 0){
      return mD(a << 1, b >> 1);
    } else{ // b impair
      return a + mD(a << 1, b >> 1);
    }
  }
```

# Complexité d'un programme

> Forme du temps de calcul, dans le pire cas, en fonction de la taille n du
  problème, pour de grandes valeurs de n.

Une fonction $f$ est en $\Omega(g_1) \Rightarrow$ $f$ est minorée par $g_1$.  
Une fonction $g_2$ est en $\Omega(f) \Rightarrow$ $g_2$ domine $f$.  
Si $g_1 = g_2 = g$, $f$ est en $\Theta (g)$.

$f$ est en $\Omega(g_1)$ :

$$\text{il existe } n_1, c_1; n > n_1 \Rightarrow f(n) \geq c_1 g_1(n)$$

$f$ est en $O(g_2)$ :

$$\text{il existe } n_2, c_2; n \geq n_2 \Rightarrow f(n) \geq c_2 g_2(n)$$

$f$ est en $\Theta(g)$ :

$$f \text{ est en } \Omega(g) \text{ et } f \text{ est en } O(g)$$.

## Complexités croissantes

$$\Theta(1) \text{ (temps constant) } \rightarrow \Theta(\log{\log{n}})
\rightarrow \Theta(\log{n}) \rightarrow \Theta(\log^2{n}) \rightarrow \Theta(n)
\rightarrow \Theta(n \log{n}) \rightarrow \Theta(n^2) \rightarrow \Theta(n^3)
\rightarrow \Theta(2^n)$$

* **Problèmes faciles :**  
  Un problème est dit facile s'il existe un algo polynomiale pour le résoudre.

* **Problèmes difficiles :**  
  On ne connaît pas d'algorithme polynomiale. Les algos sont en $\Omega(2^n)$.

* **Problèmes $NP-$complets :**  
  Problèmes difficiles, équivalents pour cette propriété.

# Optimalité des algorithmes de tri comparatifs en $O(n log(n))$

## Mesure du temps de calcul

Le nombre de comparaisons $T[x] \leq T[y] ?$

* Soient n valeurs à trier.  
  Il existe $ n!$ permutations de ces valeurs.  
  **Remarques :** le nombre d'exécutions possibles du programme est $\geq n!$.  
  Par l'absurde... Sinon il existe deux permutations différentes, conduisant à
  la même exécution du programme.  
  $\Rightarrow$ au moins l'une des deux ne sera pas triée.

* Soit $p$ le nombre de comparaisons du pire cas d'exécution du programme.  
  Alors $2^p$ majore le nombre d'exécution du programme.  
  $$n! \leq 2^p$$
  $$n! = n . (n-1) . (n-2) . ... . ({n \over 2}) . ... . 1$$
  $$({n \over 2})^{n \over 2} \leq n . (n-1) . (n-2) . ... ({n \over 2}) \leq n!$$
  $$({n \over 2})^{n \over 2} \leq n! \leq 2^p$$
  $$({n \over 2})^{n \over 2} \leq  2^p$$
  $$({n \over 2}) log {n \over 2} \leq p$$
