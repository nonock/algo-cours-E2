---
title: Arbres binaires
author: Romain Gille
date: 02/05/2016
...
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
