# Order State Transitions

## In-order

1. $E \rightarrow a \rightarrow f_{1..n} \space (\forall i \in [1, n], filled(f_i) \bmod 100 = 0)$
2. $E \rightarrow a \rightarrow f_{1..n} \space (\exists i \in [1, n], filled(f_i) \bmod 100 \ne 0)$
3. $E \rightarrow a \rightarrow C \rightarrow c \space$
4. $E \rightarrow a \rightarrow f_{1..n} \rightarrow C \rightarrow c \space$
5. $E \rightarrow a \rightarrow f_{1..k} \rightarrow C \rightarrow f_{k+1..n} \rightarrow c \space (k \in [1,n))$

## Out-of-Order
1. $E \rightarrow f_{1..n} \rightarrow a$
2. $E \rightarrow f_{1..k} \rightarrow a \rightarrow f_{k+1..n}$
3. $E \rightarrow a \rightarrow C \rightarrow c \rightarrow f_{1..n}$
4. $E \rightarrow a \rightarrow C \rightarrow f_{1..k} \rightarrow c \rightarrow f_{k+1..n } \space (k \in [1,n))$
5. $E \rightarrow C \rightarrow a$
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjgzMjg4NDkzXX0=
-->