# \\[ L_1 = \\{ \exists \text{ a TM } N \text{ such that } M \text{ accepts } \langle N \rangle \text{ and } N \text{ accepts } \epsilon \\}\\]

## Recognizability
If we want to prove that an < M > is in L1, we need a certificate for M and N.
```
Let <N1>,<N2>,... be an effective enumeration of the TMs
R = "On input <M0>"
1. for k=1 to infinity
2.     for k1=1 to k
3.         for k2=1 to k
4.             for N=N to M
5.                 Run M0 on <N> for k1 steps
6.                 Run N on empty string for k2 steps
7.                 If both accept, accept.
```

## Uncorecognizability
We know that HALT is not corecognizable. We can prove that L1 is not
corecognizable by map reducing HALT to L1.

```
Show HALT <=_m L1
P="On input <M,w>"
1. Let M0: "On input <x>"
    1. Pass
	2. Run M on w
	3. Accept
2. Return <M0>
```

```
Suppose <M,w> not in HALT
Line 2 loops
Line 3 never runs L(M0) = empty set
M0 not in L1
```

```
Suppose <M,w> in HALT
Line 2 halts
Line 3 runs L(M0) = Sigma*
Exists N, M accepts <N> | N accepts epsilon
M0 in L1
```

