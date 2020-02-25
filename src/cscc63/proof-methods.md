# Proof Methods

## Diagonalization Method

Assume we have two sets A, B, and a function mapping A to B.

f is **one-to-one**(injective) if f no two elements in A map to the same element in B.

f is **onto**(surjective) if every element in B is mapped to by at least one element in A.

f is a **correspondence**(bijective) if f is both one-to-one and onto

## Mapping Reductions

Given any two languages A and B, we say \\(A \leq _m B\\) (A mapping reduces to
B) if we have some algorithm f such that:
* If x in A, then f x in B
* If x not in A, then f x not in B

**DEFAULT PROOF FORMAT** for proving that \\(HALT \leq _m B \\):  
Consider the TM P "On input <M,w> (i.e., the input to HALT):"
1. Construct the TM M0 as follows:
	M0 = "On input <input>:"
		1. <Step 1>
		2. Run M on w
		3. <Step 3>
2. Return <M0>
Proof that <M0> in B iff <M,w> in HALT
