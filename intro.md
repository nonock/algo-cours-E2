---
title: Algorithmique -- Introduction
author: Romain Gille
date: 27/01/2016
geometry: margin=1in
...

\newpage

# Exemple

`T[0:n]` est un tableau d'entiers à n valeurs.
Calculer dans `s`, la somme des éléments de `T`.

```java
int somme (int[] T){
  int n = T.length;
  int k = 0, s = 0; // initialisation des variables
  while(k != n){    // arrêt quand k = n
    s = s + T[k];
    k++;            // incrémentation de k
  }
  return s;         // retourne s
}
```

## Modélisation

`s` = somme des valeurs du `k`-préfixe de `T`

`s` = $\sum\limits_{x \in [0:k]} T[x]$ $\equiv I(s, k)$

```java
int somme (int[] T){
  int n = T.length;
  int k = 0, s = 0; // I(s, k)
  while(k != n){    // I(s, k) et k != n => I(s + T[k], k + 1)
    s = s + T[k];   // I(s, k + 1)
    k++;            // I(s, k)
  }
  return s;
}
```

\newpage

## Modélisation n°2 (variation sur thème)

`s` = somme des valeurs du `k`-suffixe de `T`

`k`-suffixe de $T[k:n] = [T[k], T[k + 1], ..., T[n - 1]]$

**Initialisation :** `s = 0, k = n`

**Condition d'arrêt :** `k = 0`

**Progression :** $I(s, k)$ et $(k \neq 0) \Rightarrow I(s + T[k - 1], k - 1 )$

```java
int somme (int[] T){
  int n = T.length;
  int k = n, s = 0; // I(s, k)
  while(k != 0){    // I(s, k) et k != 0 => I(s + T[k - 1], k - 1)
    s = s + T[k];   // I(s, k - 1)
    k--;            // I(s, k)
  }
  return s;
}
```

## Modélisation n°3

`s` = somme de T[i:j]

On connait :

  * $i \geq j : s = 0$
  * $j = i + 1 : s = T[i]$

`s = sg + sd`

```java
int somme(int[] T, int i, int j){
  if(j - i <= 0) return 0;
  if(j - i == 1) return T[i];
  else{
    int k = (i + j) / 2;
    int sg = somme(T, i, k);
    int sd = somme(T, k, j);
    return sg + sd;
  }
}
```

**Invariant** : I(s, k) : $s = \sum\limits_{i \in [0:n]}$

*On utilise le premier programme du cours précédent*

```java
int somme (int[] T){
  int n = T.length;
  int k = 0, s = 0; // I(s, k)
  while(k != n){    // I(s, k) et k != n => I(s + T[k], k + 1)
    s = s + T[k];   // I(s, k + 1)
    k++;            // I(s, k)
  }
  return s;
}
```

# Temps de calcul

L'initialisation est de temps constant $\alpha$.

Pour la boucle while :

* L'addition est une opération de temps constant			=> $\beta$
* L'accès à un élément d'un tableau est également de temps constant	=> $\beta$
* La vérification de `k != n` est de temps constant $\gamma$ et exécuté (n + 1)
fois

Donc le corps de boucle est de temps constant $\beta + (n + 1) \gamma$.

Le temps d'exécution est :
$T(n) = \alpha + n \beta + (n + 1) \gamma$

$T(n) = (\alpha + \gamma) + n (\beta + \gamma) = A + Bn$

# Efficacité d'un programme

C'est la forme du temps de calcul dans le pire cas d'exécution exprimé en
fonction de n (taille du problème).


*On utilise le dernier programme du cours précédent*

```java
int somme(int[] T, int i, int j){
  if(j - i <= 0) return 0;
  if(j - i == 1) return T[i];
  else{
    int k = (i + j) / 2;
    int sg = somme(T, i, k);
    int sd = somme(T, k, j);
    return sg + sd;
  }
}
```

On traite le cas `somme(T, 0, n)`

* $T(n \leq 0) \rightarrow$ le programme s'exécute en temps constant $\alpha$
* $T(n = 1) \rightarrow$ le programme s'exécute en temps constant $\beta \neq
\alpha$
* $n > 1$ et $n = 2^p$ avec $p \geq 1$
    * $\gamma$ est le temps constant de la division par 2 et du
      `return sg + sd;`
    * $\delta = \alpha + \beta + \gamma$
    * le programme s'exécute en $\delta + 2 * T(2^{p-1})$

\newpage

*On teste avec des valeurs de p*

$T(n = 2^0) = \beta$

$T(n = 2^1) = \delta + 2T(2^0) = \delta + 2 \beta$

$T(n = 2^2) = \delta + 2T(2^1) = \delta + 2(\delta + 2 \beta)$

$T(n = 2^3) = \delta + 2T(2^2) = \delta + 2(\delta + 2(\delta + 2 \beta))$

$T(n = 2^3) = \delta (2^0 + 2^1 + 2^2) + 2^3 \beta$

$T(n = 2^3) = \delta (2^3 - 1) + 2^3 \beta$

$T(n = 2^3) = (\delta + \beta) 2^3 - \delta$


On peut généraliser à $n = 2^p$

$T(n = 2^p) = (\beta + \gamma) 2^p - \delta$

$T(n = 2^p) = A * 2^p + B$

$T(n = 2^p) = \Theta(2^p) = \Theta(n)$


En posant le problème en parallèle :

$T_{//}(n = 2^p) = p * \gamma + \beta = \Theta(log_2 n)$

\newpage
