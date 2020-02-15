# \\(A_{TM} = \\{ \langle M,w \rangle | \text{M accepts w} \\}\\)

This turing machine accepts the language of accepting turing machines.

## Recognizability
Trivially, we can see that this language is recognizable. We can build the
following TM to recognize this language:

\\(A\\) = "On input \\(\langle M,w \rangle \\)"
1. Run \\(M\\) on \\(w\\)
2. If M accepts, accept
2. If M rejects, reject

However, this machine loops on input M,w if M loops on w.

## Undecidability
We will do a proof by contradiction. Assume that \\(A_{TM}\\) can be solved by
some TM \\(M_A\\)

Use this to build another TM \\(D\\). \\(D\\) takes input of the form
\\(\langle M \rangle \\), where \\(M\\) is some TM.

\\(D\\) = On input \\(\langle M \rangle\\):
1. Run \\(M_A\\) on the input \\(\langle M,\langle M \rangle\rangle\\).
2. If \\(M_A\\) accepts, reject
3. Else, accept

What we get is a machine that does the following:

\\[
D(\langle M \rangle) =
\begin{cases}
\text{if M accepts }\langle M \rangle \text{, reject}\\\\
\text{if M rejects }\langle M \rangle \text{, accept}
\end{cases}
\\]

Now, we run D on the input \\(\langle D \rangle\\).

\\[
D(\langle D \rangle) =
\begin{cases}
\text{if D accepts }\langle D \rangle \text{, reject}\\\\
\text{if D rejects }\langle D \rangle \text{, accept}
\end{cases}
\\]

We have encoded the liar's paradox into this machine. Therefore, our original
assumption that \\(M_A\\) was decidable is incorrect, and \\(A_{TM}\\) is
undecidable.



