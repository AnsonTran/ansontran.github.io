# \\(HALT_{TM} = \\{\langle M,w \rangle | M \text{ halts on input } w \\}\\)

This turing machine accepts the language of turing machines that halt on their
input.

## Recognizablility
Its easy to see that HALT is recognizable. Just run M on w and accept once it
halts.

## Undecidability
We know that \\(A_{TM}\\) is an undecidable language. We can show halt is
undecidable, by:
* Pretending we have a TM we can call to solve halt for us
* Use this TM to write a program that would solve another problem we know is
  undecidable, such as \\(A_{TM}\\)
* However, \\(A_{TM}\\) is undecidable, so such a program can't exist, so that
  language is undecidable

```
Suppose that M_H decides HALT:
M_A = On input <M,w>
1. Run M_H on <M,w>
2. If it rejects, reject
3. Run M on w
4. If it accepts, accept Else, reject
```

## Uncorecognizability
HALT is uncorecognizable. We can't tell if M will loop on some input w. We can
run M on w for some finite steps to check if it loops, but it may be that M is
taking even longer.
