
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





\newpage




