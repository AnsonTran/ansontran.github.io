# Greedy Algorithms (KT Ch. 4, DPV Ch. 5)
Our goal with optimization problems is to take in some input and constraints on
those inputs, and find a greedy picking strategy that always produces an output
that:
1. Satisfies the given constraints (a "feasible" solution)
2. Optimizes (min/max) a desired criterion (objective function)

Greedy algorithms do so by always choosing the best solution "myopically".
Greedy approaches work when we can:
* Build a solution incrementally
* Each step produces a partial solution, and we can extend it to a more
	complete solution greedily, choosing the immediate best step
* Each step is irreversible, meaning we cannot backtrack.

## Greedy Stays Ahead
We can use the 'Greedy Stays Ahead' argument in order to prove the optimality
of greedy algorithms. The general format of such a proof goes as such:

1. Let A be a solution given by following the greedy choice of the algorithm.
2. Let A* be a optimal solution, such that A and A* are the same up to some
	 k-th element, where they differ.
3. Show that by swapping the k-th value of A* with the k-th value of A, we
	 maintain the optimality of A*.
4. Therefore, the greedy strategy of A produces a solution that is optimal.

Remember that there can be multiple optimal solutions, and if we can show
that optimality is maintained at the k-th step, the same argument applies for
any other step where A and A* differ.

## Promising Set Technique
The promising set technique maintains an invariant:
Let A be the solution produced by the greedy algorithm.

At the end of iteration t, let A_t be the set contained in A at the end of
iteration t. A_t is contained in **some** optimal set A*.

Now consider iteration t+1:
1. A_{t+1} = A_t in which case A_t is optimal
2. A_{t+1} = A_t \union {j} - we must prove that A_{t+1} is in some A*.
