---
title: Algorithmique -- Structures de données liste et arbres binaires
author: Romain Gille
date: 02/05/2016
geometry: margin=1in
...

# Fret de coût minimum

$X = {0, ..., n-1} n\text{ villes}$
$\forall i, j, i < j,$ il existe un trajet direct $i \rightarrow j$
Les coûts directs sont publiés :
$\forall i, j,$ $0 \leq i < j < n,$ coût direct $c(i, j)$
La matrice $C$ telle que $C[i][j] = c(i, j)$

**Notation :** $m(k)$ est le coût minimum d'un affrètement de la ville 0 à la
ville $k$.

Nous cherchons à calculer $m(n-1)$.

## Équation de récurrence du coût minimum $m(k)$

**Hypothèse :** le problème est résolu

Possibilités :

    * $m(n-1) = m(n-2) + c(n-2, n-1)$
    * $m(n-1) = m(n-3) + c(n-3, n-1)$
    * ...
* $m(k) + c(k, n-1)$
* $m(n-1) = c(0, n-1)$

**Équation de récurrence :**

1. Chemin direct
    $m(n-1) = c(0, n-1$

2. Chemin non direct
    $m(n-1) = min{m(k) + c(k, n-1)}$


On a donc :

$m(n-1) = min{c(0, n-1), min{m(k) + c(k, n-1)}}$

Finalement :

**Base de la récurrence :** $m(1) = c(0, 1)$

**Cas général :** $2 \leq k < n$
$m(k) = min{c(0, k), min{m(k') + c(k', k)}}$ avec $1 \leq k' < k$

**Programme :**

On calcule, par taille de problème croissant, un vecteur $M[0:n]$ de terme
général $M[k] = m(k)$ et un vecteur $A[0:n]$ de terme général
$A[k] = arg{m(k)} = a(k)$. C'est à dire $a(k)$ est la dernière étape sur le
chemin de coût minimum allant de $0$ jusqu'à $k$.
Par convention, on pose $a(k) = -1$ si le chemin est direct.

```java
int[][] calculerMetA(int[][] C){
    int n = C.length;
    int[] M = new int[n], A = new int[n];
    M[1] = C[0][1];
    A[1] = -1;
    for(int k = 2; k < n; k++){
        M[k] = C[0][k];
        A[k] = -1;
        for(int kp = 1; kp < k; kp ++){
            int mkp = M[kp] + C[kp][k];
            if(mkp < M[k]){
                M[k] = mkp;
                A[k] = kp;
            }
        }
    }
    int[][] MA = {M, A};
    return MA;
}
```

**Complexité :** $\Theta(n^2)$

## Affichage d'un chemin de coût minimum

```java
void accm(int[] A, int k){
    if(A[k] == -1) System.out.println(0);
    else{
        System.out.print(A[k] + "<-");
        accm(A, A[k]);
    }
}

void accm(int[] A){
    int n = A.length;
    System.out.print(n-1 + "<-");
    accm(A, n-1);
}
```

## Autre équation de récurrence

$m(i, j)$ est le coût minimum pour aller de $i$ en $j$, avec $i \leq j$.

**Base :** $m(i, i) = 0$

**Cas général :** $0 \leq i < j < n$
$m(i, j) = min{m(i, k) + m(k, j)}$


En général, le nombre de trajets directs est très petit en comparaison au
nombre de villes.

Dans le problème que nous avons traité, le nombre de valeurs est :
$(n-1) + (n-2) + ... + 1 = \dfrac{n(n-1)}{2} \Rightarrow \Theta(n^2)$

On a donc un problème en $\Theta(n^2)$ en temps et en mémoire.

$G$ est une table de listes d'arcs

* la somme des longueurs des listes est $m$
* la taille de la table est $n$

La taille totale de la représentation est exactement la taille du graphe $m+n$
et la complexité de l'algo est également en $\Theta(m+n)$.

```java
class Liste{
    int j, c;
    Liste reste;

    Liste(int j, int c, Liste reste){
        this.j = j; this.c = c; this.reste = reste;
    }

    boolean vide(Liste l){
        return l == null;
    }

    Liste reste(Liste l){
        return l.reste;
    }

    int n = 4;
    Liste[] G = new Liste[n];
    for(int i = 0; i < n; i++) G[i] = null;
    G[0] = new Liste(1, 2, G[0]);
    G[0] = new Liste(3, 7, G[0]);
    G[1] = new Liste(2, 3, G[1]);
    G[2] = new Liste(3, 1, G[2]);

    void afficher(Liste[] G){
        int n = G.length;
        for(int i = 0; i<n; i++) afficher(G[i]);
    }

    void afficher(Liste l){
        for(Liste l1 = l; l1 != l; l1 = reste(l1)){
            System.out.print("j = " + j + "c = " + c + "t");
        }
        System.out.println();
    }
}
```

## Équation de récurrence, représentation par matrice des coûts

* $m(0) = 0$
* $m(k) = min{m(k) + c(k', k)}$

Avant on avait les flèches sortantes de chaque ville, maintenant on cherche les
flèches entrantes dans chaque ville.

On a donc besoin d'un graphe parallèle à celui de base.

```java
Liste[] symetrique(Liste[] G){
    int n = G.length;
    Liste[] Gp = new Liste[n];
    for(int i = 0; i < n; i++) Gp[i] = null;
    for(int i = 0; i < n; i++)
        for(Liste[] l = G[i]; ! vide(l); l = reste(l)){
            int j = l.j, c = l.c;
            Gp[j] = new Liste(i, c, Gp[j]);
        }
    return Gp;
}

int[][] calculerMetA(Liste[] G){
    int n = G.length;
    int[] M = new int[n];
    int[] A = new int[n];
    Liste[] Gp = symetrique(G);
    M[0] = 0; A[0] = -1;
    for(int k = 1; k < n; k++){
        M[k] = Integer.MAX_VALUE;
        for(int l = Gp[k]; !vide(l); l = reste(l)){
            int kp = l.j, c = l.c;
            int m = M[kp] + c;
            if(m < M[k]){
                M[k] = m;
                A[k] = kp;
            }
        }
    }
    int[][] MA = {M, A};
    return MA;
}

void accm(int[] A){ //affichage chemin coût minimum de 0 à n-1
    int n = A.lenght;
    accm(A, n-1);
    System.out.println();
}

void accm(int[] A, int j){ //affiche chemin coût minimum 0 à j
    if(j != -1){
        System.out.print(j + ", ");
        accm(A, A[j]);
    }
}
```

Si on veut afficher les chemins à prendre dans l'ordre croissant ($0, 3, 4$ au
lieu de $4, 3, 0$), il suffit d'inverser le `accm(A, A[j])` et le
`System.out.print(...)` dans la fonction `accm(int[] A, int j)`.
