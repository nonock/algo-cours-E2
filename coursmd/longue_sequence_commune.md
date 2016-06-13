
---
title: Algorithmique -- Structures de données liste et arbres binaires
author: Romain Gille
date: 02/05/2016
geometry: margin=1in
...

# Calcul d'une plus longue séquence commune à deux séquences

Une séquence $X$ est de la forme $X = x_0, x_1, ..., x_{m-1}$ avec des symboles
ordonnés.
Soit $Y = y_0, y_1, ..., y_{m-1}$ et $X$.
$X'$ est une sous séquence de $X$ si elle a été obtenue en enlevant des
symboles de $X$.

* Si on les enlève tous, on obtient la séquence vide $\epsilon$
* Si on en enlève aucun, on obtient $X$ (une séquence est sa propre sous
    séquence)

$k$ préfixe de $X$, $X(k) = x_0, ..., x_{k-1}$

$Z$ est une séquence commune à $X$ et $Y$ si $Z$ est sous séquence de $X$ et de
$Y$.
$Z$ sous séquence de $X$ et de $Y$ est une `plsc`
(plus longue séquence commune) à $X$ et à $Y$ si $Z$ est de longueur max.

## Exemple

`plsc(`$\epsilon$`, "bonjour")` = $\epsilon$
`plsc("bonsoir", `$\epsilon$`) =` $\epsilon$
`plsc("bonjour", "bonsoir") = "bonor"`
`plsc("tiens, bonjour", "bonjour, ça va ?") = "bonjour"`

Prenons deux séquences $X$ et $Y$ se finissant toutes deux par `*` :

* $x_{m-1} = y_{n-1}$
    `plsc(X(m), Y(n)) = plsc(X(m-1), Y(n-1)) . *`
* $x_{m-1} \neq y_{n-1}$
    * `plsc(X(m), Y(n)) = plsc(X(m-1), Y(n-1))`
    * `plsc(X(m), Y(n)) = plsc(X(m), Y(n-1))` le dernier symbole de X est
        éventuellement dans la `plsc`
    * `plsc(X(m), Y(n)) = plsc(X(m-1), Y(n))` le dernier symbole de Y est
        éventuellement dans la `plsc`

`l(p, q)` longueur d'une `plsc` à `X(p)` et `Y(q)`.

En particulier `l(m, n) = plsc(X, Y)`

**Base :**

`l(0, q) = 0`, $\forall$ `q`, `0` $\leq$ `q` $\leq$ `n`
`l(p, 0) = 0`, $\forall$ `p`, `0` $\leq$ `p` $\leq$ `m`

**Cas général :** `1` $\leq$ `p` $\leq$ `m`, `1` $\leq$ `q` $\leq$ `n`

`l(p, q) =`

* si $x_{p-1} = y_{q-1}$ alors `1 + l(p-1, q-1)`
* sinon `l(p, q) = max{l(p, q-1); l(p-1, q)}`

On calcule `L[0 : m+1][0 : n+1]` de terme général `L[p][q] = l(p, q)`

En particulier :

* `L[m][n] = plsc(X, Y)`
* `L[0][q] = 0`
* `L[0][p] = 0`

```java

int[][] calculerL(String X, String Y){
    int m = X.length(), n = Y.length();
    int[][] L = new int[m+1][n+1];
    // Base
    for(int p = 0; p <= m; p++) L[p][0] = 0;
    for(int q = 0; q <= n; q++) L[q][0] = 0;
    // Cas général
    for(int p = 1; p <= m; p++)
        for(int q = 1; q <= n; q++){
            if(X.charAt(p-1) == Y.charAt(q-1))
                L[p][q] = 1 + L[p-1][q-1];
            else
                L[p][q] = Math.max(L[p][q-1], L[p-1][q]);
        }
    return L;
}
```
Calcul en $\Theta(m * n)$

## Version avec récurrence

```java

void afficherPlsc(int[][] L, String X, String Y, int p, int q){
    if(p > 0 && q > 0)
        if(X.charAt(p-1) == Y.charAt(q-1)){
            afficherPlsc(L, X, Y, p-1, q-1);
            System.out.print(X.charAt(p-1));
        }
        else{
            if(L[p][q-1] >= L[p-1][q]) afficherPlsc(L, X, Y, p, q-1);
            else afficherPlsc(L, X, Y, p-1, q);
        }
}
```


\newpage

## Version itérative

**Invariant :** `I(p, q, r) : Plsc(X, Y) = plsc(X(p), Y(q)) . Z[r : l(m, n)]`

**Initialisation :** `p = m, q = n, r = l(m, n)`

**Arrêt :** `p = 0` ou `q = 0`

**Progression :**

* `I(p, q, r)` et `p > 0` et `q > 0` et `X[p-1] = Y[q-1]` et
    `Z[r-1] = X[p-1]` $\Rightarrow$ `I(p-1, q-1, r-1)`
* `I(p, q, r)` et `p > 0` et `q > 0` et `X[p-1]` $\neq$ `Y[q-1]` et
    `L[p][q-1]` $\geq$ `L[p][q]` $\Rightarrow$ `I(p, q-1, r-1)`
* `I(p, q, r)` et `p > 0` et `q > 0` et `X[p-1]` $\neq$ `Y[q-1]` et
    `L[p][q-1]` < `L[p][q]` $\Rightarrow$ `I(p-1, q, r)`

```java
String construirePlsc(int[][] L, String X, String Y){
    int m = X.length(), n = Y.length();
    int p = m, q = n, r = L[m][n];
    char[] Z = new char[L[m][n]];
    while(p != 0 && q != 0){
        if(X.charAt(p-1) == Y.charAt(q-1)){
            Z[r-1] = X.charAt(p-1)
            p--; q--; r--;
        }
        else{
            if(L[p][q-1] >= L[p-1][q]) q--;
            else p--;
        }
    }
    return String(Z);
}
```

Construction en $\Theta(m + n)$
