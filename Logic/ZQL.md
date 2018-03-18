# Brief Notes On QL

Axioms formulated from: http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.101.6448&rep=rep1&type=pdf

Axioms originally formulated by:

*M. Pavicic*  
*Department of Mathematics, University of Zagreb,*  
*Kaci´ceva 26, Post. pret. 217, CRO–41001 Zagreb, Croatia.*  

# WFF for Zero Order QL (ZQL)

Lexicon:
```
Logical Connectives: ~, >, v, &  
Propositions: p, q, r, ....  
```

Grammar:
```
If p is a Proposition in ZQL, p is a WFF.
If p is a WFF, so is ~p.
If p, q are WFF, so is p > q.
If p, q are WFF, so is p v q.
If p, q are WFF, so is p & q.
There are no other WFF of ZQL.
```

# Inference System for Zero Order QL

Axioms:
```
[A1] A > A
[A2] A > ~~A & ~~A > A
[A3] A > A v B
[A4] B > A v B
[A5] B > A v ~A
```

Rules of Inference (here '|' denotes *turnstile*):
```
[R1] (A > B), (B > C) | A > C
[R2] (A > B) | (~B > ~A)
[R3] (A > C), (B > C) | (A v B > C)
[R4a] (B v ~B) > A | A
[R4b] A | (B v ~B) > A
```

# Kripke Frames

See notes on page 9 for a *Modal Logic* semantics for ZQL.
