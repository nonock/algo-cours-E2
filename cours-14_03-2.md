---
title: Algorithmique -- Système formel et axiomes
author: Romain Gille
date: 14/03/2016
geometry: margin=1in
...

# Système formel C.A.R HOARE

Si l'entrée E du programme est vérifiée, après le programme P, la sortie S du
programme est vérifiée. ($E P S$)

## Axiomes de l'affectation

**`E(..., exp, ...)` $\rightarrow$ `x = exp` $\rightarrow$ `E(..., x, ...)`**
`(2(x + 1) + 3)(x + 1) >` $y^{x+1}$ $\rightarrow$ `x = x + 1` $\rightarrow$ `(2x + 3)x >` $y^{x}$

### Règles

* Règle du ';'
  $E P_1 ; P_2 S \Rightarrow E P_1 X , X P_2 S$
* Règle du 'si sinon'
  $E \text{ if } B P_1; \text{ else } P_2 S \Rightarrow E \text{ et } B P_1 S, E \text{ et } \overline{B} P_2 S$
* Règle du 'si'
  $E \text{ if } B P S \Rightarrow E \text{ et } B P S, E et \overline{B} \rightarrow S$
* Règle des '{}'
  $E\{P_1;P_2\}S \Rightarrow E P_1 ; P_2 S$
* Règle du 'while'
  $E \text{ while } B P S \rightarrow S = E \text{ et } \overline{B} \Rightarrow E \text{ et } B P E$
* Règle du 'for'
  $E \text{ for(Init; Condition; Incrémentation) } P S \Rightarrow ?$
