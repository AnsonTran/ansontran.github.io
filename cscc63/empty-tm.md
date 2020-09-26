# \\(E_{TM} = \\{\langle M \rangle | L(M) = \emptyset \\}\\)

This turing machine accepts TMs that don't accept anything.

## Co-recognizability
The complement of this language is
\\(\overline{E_{TM}} = \\{\langle M \rangle |
\exists w \in \Sigma ^*, M \text{ accepts } w \\}\\).

It's easy to provide a certificate for
this, just give an input k that when M runs on k, M accepts.

## Unrecognizability
We can show that empty TM is not recognizable by showing that its complement is
not co-recognizable. \\(\overline{E_{TM}} = \\{\langle M \rangle | \exists w
\in \Sigma ^*, M \text{ accepts } w \\}\\). We know HALT is not
co-recognizable, so we show that HALT mapping reduces to E complement.

```
P: "On input <M,w>"
1. M0 = "On input <w>"
	1. Pass
	2. Run M on w
	3. If M accepts accept, else reject
2. Return <M0>
```

```
Suppose M in HALT
Line 2 halts
Line 3 accepts if it accepts w
M0 in E complement
```

```
Suppose M not in HALT
Line 2 loops
Line 3 never runs, so it doesn't accept anything
M0 not in E complement
```
