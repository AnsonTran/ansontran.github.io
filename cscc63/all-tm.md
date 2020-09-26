# \\(ALL_{TM} = \\{ \langle M \rangle | M \text{ halts on every input} \\} \\)

## Unrecognizability
We know HALT complement is not recognizable. So, we show that HALT complement
mapping reduces to ALL tm.

```
P: "On input <M,w>"
1. Let M0: "On input <k>"
    1. Pass
	2. Run M0 on w for k steps
	3. If M halts, loop, otherwise accept
2. Return <M0>
```

```
Suppose <M,w>  not in HALT complement
Line 2 reject for all k
Line 3 always accepts
L(M0) = Sigma*
<M0> in ALL tm
```

```
Suppose <M,w> in HALT complement
in k, M halts on w after k steps
in <k>, line 2 accepts on k
in <k>, line 3 loops
M0 doesn't halt on every input
<M0> not in ALL tm
```

