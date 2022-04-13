# Sets

## Basics
SQL supports four types of operations `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT`. The difference between `UNION` and `UNION ALL` is that the latter includes duplicates. 
Given set A = {L, M, N, O, P} and set B = {P, Q, R, S, T}, the four operations will return
1. A UNION B  = {L, M, N, O, P, Q, R, S, T}
2. A UNION ALL = {L, M, N, O, P, P, Q, R, S, T} -- Note the two Ps
3. A INTERSECT B = {P}
4. A EXCEPT B = {L, M N, O}

