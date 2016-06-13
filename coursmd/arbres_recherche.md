
---
title: Algorithmique -- Structures de données liste et arbres binaires
author: Romain Gille
date: 02/05/2016
geometry: margin=1in
...

# Arbres binaires de recherche, de coût moyen de recherche minimum

## Coût moyen d'un arbre

$$C(A) = \sum\limits_{i \in X} p_i c(i)$$

où $p_i$ est la probabilité d'avoir à chercher $i$ dans $A$ et $c(i)$ est le
nombre de comparaisons pour atteindre $i$.


## Données du problème

$$V = {v_0, v_1, ..., v_{n-1}} \text{ triées croissantes}$$
$$P = {p_0, p_1, ..., p_{n-1}}$$


## Problème

Calculer l'arbre binaire contenant les valeurs $V[0:n]$ de coût moyen de
recherche minimum.


## Notation

$m(i, j)$ est le coût d'un arbre $A_{ij}^*$ (Arbre de recherche optimal
contenant $V[i:j]$).

taille $t = j - i$


## Équation de récurrence des valeurs $m(i, j)$

**Base** : les problèmes de taille $t = 0$ : $m(i, i) = 0$

**Cas général** : les problèmes de taille $1 \leq t \leq n$

* $t = 1$       : $m(i, i+1), ..., m(n-1, n)$
* $t = 2$       : $m(i, i+2), ..., m(n-2, n)$
* $1 \leq t \leq n$   : $m(i, i+t), ..., m(n-t, n)$
* $t = n$       : $m(0, n)$

$m^{(k)}(i, j)$ est le coût minimum sachant que $v_k$ est la racine.

On a donc $m(i, j) = \text{ min } m^{(k)}(i, j)$

D'après BELLMAN, l'arbre est de coût minimum si ses deux sous arbres (gauche et
droite) sont minimum.

Donc $m^{(k)}(i, j) = m(i, k) + m(k+1, j) + \sum\limits_{i \leq x \leq j} p_x$

Donc $m(i, j) = \text{ min } m^{(k)}(i, j) = m(i, k) + m(k+1, j) +
\sum\limits_{i \leq x \leq j} p_x$

### Rappel

$$\text{min }_{y \in Y} f(y) + c = (\text{min }_{y \in Y} f(y)) + c$$

Avec $c$ une constante.

\newpage

### Pour résumer

**Base** : $t = 0$, $m(i, i) = 0$, $\forall i$ : $0 \leq i \leq n$

**Cas général** :$1 \leq t \leq n$
$m(i, j) = \text{ min } m^{(k)}(i, j) = m(i, k) + m(k+1, j) +
\sum\limits_{i \leq x \leq j} p_x$

Nous calculons $M[0:n+1][0:n+1]$ de terme général $M[i][j] = m(i, j)$

```java
int[][] calculerM(int[] V, int[] P){
    int n       = V.length;
    int[][] M   = new int[n+1][n+1];
    // t = 0 : m(i, i)
    for(int i = 0; i <= n; i++) M[i][i] = 0;
    for(int t = 1; t <= n; t++)
        for(int i = 0; i <= n - t; i++){
            int j = i + t;
            M[i][j] = Integer.MAX_VALUE;
            for(int k = i; k < j; k++){
                int mijk = M[i][k] + M[k+1][j];
                if(mijk < M[i][j]){
                    M[i][j] = mijk;
                    M[j][i] = k; // rangement de l'argument
                }
            }
            int S = 0;
            for(int x = i; x < j; x++) S = S + P[x];
            M[i][j] = M[i][j] + S;
        }
    return M;
}

ArbreBinaire abro(int[][] M, int[] V){
    int n = V.length;
    return abro(M, V, 0, n);
}

ArbreBinaire abro(int[][] M, int[] V, int i, int j){
    // retourne A*ij
    if(i == j) return null;
    int kStar = M[j][i]
    return new ArbreBinaire(V[kStar], abro(M, V, i, kStar),
                            abro(M, V, kStar+1, j)
                            );
}
```
