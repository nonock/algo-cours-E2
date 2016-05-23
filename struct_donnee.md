
---
title: Algorithmique -- Structures de données liste et arbres binaires
author: Romain Gille
date: 02/05/2016
geometry: margin=1in
...

\newpage

# Rappel : Tableaux

* structure de données non dynamique (taille fixée avant exécution du
    programme)
* temps d'accès constant à un élément du tableau

# Liste

* structure dynamique
* temps d'accès à un élément est fonction de sa position dans la liste

Une liste c'est :

* une liste vide `()`
* ou une liste non vide `(v, reste)` `v` un entier, `reste` une liste

## Exemples

* `()` $\Rightarrow$ `()`
* `(1, ())` $\Rightarrow$ `(1)`
* `(1, (2, ()))` $\Rightarrow$ `(1, 2)`
* `(1, (2, (3, ())))` $\Rightarrow$ `(1, 2, 3)`

```java
class Liste{
    int v, Liste reste;

    Liste(int v, Liste reste){
        this.v = v;
        this.reste = reste;
    }
}


boolean vide(Liste l){
    return l == null;
}


int premier(Liste l){
    return l.v;
}


Liste reste(Liste l){
    return l.reste;
}


Liste l = null;

l = new Liste(1, null);

l = new Liste(1, new Liste(2, null));

l = new Liste(1, new Liste(2, new Liste(3, null)));


void afficher(Liste l){
    Liste l1 = l;
    while(! vide(l1){
        System.out.print(premier(l1) + ", ");
        l1 = reste(l1);
    }
}


void afficherRecursif(Liste l){
    if(! vide(l)){
        System.out.print(premier(l) + ", ");
        afficherRecursif(reste(l));
    }
}


void afficherEnvers(Liste l){
    if(! vide(l)){
        afficherEnvers(reste(l));
        System.out.print(", " + premier(l));
    }
}
```

\newpage

# Arbre binaire

* un arbre vide `()`
* ou un arbre non vide `(v, g, d)` où `g` et `d` sont des arbres binaires.

```java
class ArbreBinaire{
    int v;
    ArbreBinaire g, d;

    ArbreBinaire(int v, ArbreBinaire g, ArbreBinaire d){
        this.v = v;
        this.g = g;
        this.d = d;
    }
}

ArbreBinaire g1 = new ArbreBinaire(3, new ArbreBinaire(0, null, null), null);
ArbreBinaire d1 = new ArbreBinaire(16, new ArbreBinaire(10, null, null),
                                        new ArbreBinaire(20, null, null));

ArbreBinaire a = new ArbreBinaire(9, g1, d1);
```

```java
static boolean vide(ArbreBinaire a){
    return a == null;
}

static int r(ArbreBinaire a){
    return a.r;
}

static ArbreBinaire g(ArbreBinaire a){
    return a.g;
}

static ArbreBinaire d(ArbreBinaire a){
    return a.d;
}
```

## Exemples

$()$

$(1, (), ())$

$(1, (2, (), ()), (3, (), ()))$

## Affichage des arbres binaires

### Affichage en grd (gauche-racine-droite)

Affichage de la partie gauche de l'arbre (on descend les racines jusqu'à avoir
une partie gauche nulle), puis on affiche la racine, puis on affiche la partie
droite de l'arbre au niveau de la racine.

```java
static void grd(ArbreBinaire a){
    if(!vide(a)){
        grd(g(a));
        System.out.println(r(a) + " ");
        grd(d(a));
    }
}
```

### Affichage en drg (droite-racine-gauche)

Affichage de la partie droite de l'arbre (on descend la racine jusqu'à avoir une
partie droite nulle), puis on affiche la racine, puis on affiche la partie
gauche de l'arbre au niveau de la racine.

```java
static void drg(ArbreBinaire a){
    if(!vide(a)){
        drg(d(a));
        System.out.println(r(a) + " ");
        drg(g(a));
    }
}
```

### Affichage en rgd (racine-gauche-droite)

Affichage de la racine, puis la partie gauche (on descend la racine jusqu'à
avoir une partie gauche nulle), puis on affiche la partie droite.

```java
static void rgd(ArbreBinaire a){
    if(!vide(a)){
        System.out.println(r(a) + " ");
        rgd(g(a));
        rgd(d(a));
    }
}
```

## Arbre binaire de recherche (ABR)

* Un arbre binaire vide ()
* ou un arbre binaire non vide, $(r, g, d)$ tel que :
  * $v \in g \Rightarrow v < r$
  * $v \in d \Rightarrow v > r$
  * $g$ et $d$ sont des arbres binaires de recherche

Dans un ABR, il ne peut pas y avoir de répétition de valeur.

Les ABR sont créés par une fonction `inserer(v, a)`, où a est un ABR et v est une
valeur.

* si `a` est vide : l'arbre binaire est uniquement constitué de `v`
* `a` non vide et `r(a) = v` : l'arbre binaire est constitué de `a`
* `a` non vide et `v`$<$`r(a)` : `v` est rangé dans `g(a)`
* `a` non vide et `v`$>$`r(a)` : `v` est rangé dans `d(a)`

```java
static ABR(int v, ArbreBinaire a){
    if(vide(a))     return new ArbreBinaire(v, null, null);
    if(v == r(a))   return a;
    if(v < r(a))    return new ArbreBinaire(r(a), inserer(v, g(a)), d(a));
    /* v > r(a) */  return new ArbreBinaire(r(a), g(a), inserer(v, d(a)));
}
```

### Exemple

```java
ArbreBinaire a = null;
a = inserer(7,  a);
a = inserer(7,  a);
a = inserer(5,  a);
a = inserer(11, a);
a = inserer(2,  a);
a = inserer(15, a);
a = inserer(11, a);
a = inserer(6,  a);
```

```java
ArbreBinaire recherche(int v, ArbreBinaire a){
    if(vide(a))     return a;
    if(v == r(a))   return a;
    if(v < r(a))    return recherche(v, g(a));
    /* v > r(a) */  return recherche(v, d(a));
}
```

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
        for(int q = 1; q <= n; q++)
            if(X.charAt(p-1) == Y.charAt(q-1))
                L[p][q] = 1 + L[p-1][q-1];
            else
                L[p][q] = Math.max(L[p][q-1], L[p-1][q]);
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
