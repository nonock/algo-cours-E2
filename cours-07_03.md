---
title: Algorithmique -- Recherches
author: Romain Gille
date: 07/03/2016
geometry: margin=1in
...

\newpage

# Recherche Dichotomique

**Entrée :** `T` est strictement croissant

**Sortie :** `T[i]`$\leq e <$ `T[i+1]`

**3 cas particuliers :**

-   $e <$ `T[0]` $\rightarrow$ retourner $-1$
-   $e =$ `T[n-1]` $\rightarrow$ retourner $n - 1$
-   $e >$ `T[0]`$\leq e <$ `T[n-1]`

**Propriété :** `I(i, j)` `T[i]`$\leq e <$ `T[j]`

**Arrêt :** $j = i + 1$

**Initialisation :** $i = 0, j = 0$

**Progression :** `I(i, j)` et $j \neq i + 1$ et ...

-   $T[\dfrac{i + j}{2}]\leq e \Rightarrow$ $I(\dfrac{i+j}{2}, j)$
-   $e <$ $T[\dfrac{i + j}{2}]$$\Rightarrow I(i, \dfrac{i+j}{2})$

```java
int rD(int e, int[] T){
  int n = T.length;
  if(e<T[0]) return -1;
  if(e == T[n-1]) return n-1;
  if(e>T[n-1]) return n;
  //T[0] <= e < T[n-1]
  int i = 0, j = n-1; // I(i, j)
  while(j != i+1){
    if(T[(i+j)/2]<= e)/*I((i+j)/2, i)*/ i = (i+j)/2; //I(i, j)
    else /*I(i, (i+j)/2)*/ j = (i+j)/2 //I(i, j)
  }
  return i;
}
```

$n = 2^p$\
**Exécution du corps de boucle**\
$j-i = 2^p, 2^{p-1}, ..., 2^1$

p exécution ($\log_2 n$) d'un corps de boucle dont le temps d'exécution
est en $\Theta(1)$ (majoré par une constante).
Donc, $\Theta(\log n)$

**Exemple :**
$n = 2^{20} \approx 10^6$
Recherche séquentielle : $10^6 \approx 2^{20}$
Recherche dichotomique : $20$
$\Rightarrow \text{ rapport : } \dfrac{10^6}{20} = 50 000$

\newpage

# Recherche arrière

`T[0:m][0:n]`, tableau d'entiers les lignes et les colonnes sont dans
l'ordre croissant.

**Problème :** calculer le nombre d'occurences, x, de la valeur e dans
`T[0:m][0:n]`

*Solution 1 :* `m` Recherches séquentielles
$\Rightarrow \Theta(m \times n)$

*Solution 2 :* `m` Recherches dichotomiques
$\Rightarrow \Theta(m \log n)$

*Solution 3 :* Recherche arrière $\Rightarrow \Theta(m + n)$


## Résolution du problème avec la recherche arrière

`T[p:m][0:q+1]` est le sous-tableau "restant à traiter".
`e(p, q)` : nombre d'occurences dans `T[p:m][0:q+1]`

Le nombre d'occurences de `e` dans `T[0:m][0:n]` est `e(0, n-1)`

**Propriété :** $e(0, n-1) = x + e(p, q)$$\Rightarrow$ dépend de
`I(x, p, q)`.

**Initialisation :** $x = 0, p = 0, q = n-1$ **Arrêt :**
$p = m ou q = -1$ **Progression :** `I(x, p, q)` et
($p \neq m \text{ et } q \neq -1$) et ...

-   $e = $`T[p][q]` $\Rightarrow$ `I(x+1, p+1, q-1)`
-   $e < $`T[p][q]` $\Rightarrow$ `I(x, p, q-1)`
-   $e > $`T[p][q]` $\Rightarrow$ `I(x, p+1, q)`

```java
int rA(int e, int[] T){
  int x = p = 0, q = n-1;
  // I(x, p, q)
  while(p != m && q != -1){
    if(e == T[p][q]){ //I(x+1, p+1, q-1)
      x++;
      p++;
      q--;
    } // I(x, p, q)
    else if(e < T[p][q]){ // I(x, p, q-1)
      q--; // I(x, p, q)
    }
    else{ // I(x, p+1, q)
      p++; // I(x, p, q)
    }
  } // I(x, p, q) et (p = m ou q = -1)
  return x;
}
```
