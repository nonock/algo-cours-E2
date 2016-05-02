
---
title: Algorithmique -- Programmation Dynamique
author: Romain Gille
date: 02/05/2016
geometry: margin=1in
...

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