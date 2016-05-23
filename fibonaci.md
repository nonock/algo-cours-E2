
---
title: Algorithmique -- Multiplications et Fibonaci
author: Romain Gille
date: 14/03/2016
geometry: margin=1in
...

\newpage

# Retour sur la multiplication séquentielle et dichotomique

## Multiplication séquentielle

`I(m, b1) : m + a b1 = a b`

**Initialisation :** `m = 0, b1 = b`

**Arrêt :** `b1 = 0`

**Progression :** `I(m, b1)` et `b1 != 0` $\Rightarrow$
`I(m + a, b1 - 1)`

```java
m = 0, b1 = b; // I(m, b1)
while b1 != 0{ // I(m, b1), b1 != 0 => I(m + a, b1 - 1)
  m = m + a;
  b1 --; // I(m, b1)
} // I(m, 0) donc m = ab
```

## Multiplication dichotomique

`I(m, a1, b1) : m + a1 b1 = a b`

**Initialisation :** `m = 0, a1 = a, b1 = b`

**Arrêt :** `b1 = 0`

**Progression :** `I(m, a1, b1)` et `b1` pair $\Rightarrow$
`I(m, 2a1, b1/2)`
`I(m, a1, b1)` et `b1` impair $\Rightarrow$ `I(m + a1, 2a1, b1/2)`

```java
m = 0, a1 = a, b1 = b; // I(m, a1, b1)
while b1 != 0{
  if(b1 % 2 == 0){ // I(m, 2 a1, b1 / 2)
    a1 = a1 << 1;
    b1 = b1 >> 1; // I(m, a1, b1)
  } else{ // I(m + a1, 2 a1, b1 / 2)
    m = m + a1;
    a1 = a1 << 1;
    b1 = b1 >> 1; // I(m, a1, b1)
  }
} // I(m, a1, 0) donc m = ab
```

\newpage

# Retour sur Fibonaci

`I(d, e, k)` : `d = fibo(k-2)` et `e = fibo(k-1)` pour `k` $\geq$ `2`
`I(d, e, k)` et `k != n` $\Rightarrow$ `I(e, d + e, k + 1)`

```java
int fibo(int n){
  if (n <= 1){
    return n;
  }
  int k = 2, d = 0, e = 1; // I(d, e, k)
  while(k != n){ // I(d, e, k) et (k != n) : I(e, d + e, k + 1)
    e = d + e;
    d = e - d;
    k++;
  } // I(d, e, n)
  return d + e;
}
```
