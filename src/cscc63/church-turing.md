# Church-Turing Thesis
As we can see, each of the modified turing machines can be simulated with a
single tape TM. So, they all have the same expressive power.

Anything that we can reasonably expect to do with an algorithm can also be
decided using a TM:

> Problems that can be solved using algorithms \\(\leftrightarrow\\) Problems
> that can be solved using TMs

We can now look at things that TMs can't do, and by this thesis, if a TM can't
do it, then no algorithm can.

## The Liar's Paradox
A statement of the form **"This statement is false."**

If we can encode this paradox into a program under assumption that a problem
is decidable, we can show that there is some flaw in our code, and therefore a
flaw in our assumption.

## Decidability
**Decidable**: A problem that can be solve by a TM or algorithm is decidable.

**Recognizable**: A problem is recognizable if we can build a TM that will
accept exactly the set of its yes-instances, and loop on some of its
no-instances.

**Co-recognizable**: A problem is co-recognizable if we can recognize all of
the no-instances, and loop on some of its yes-instances. We can prove
co-recognizability by writing a recognizer for the complement of the language a
problem accepts.

## Dovetailing
If a problem is both recognizable and co-recognizable, then it is decidable. We
can prove this by using a proof technique called dovetailing.

Let L be a language that is both recognizable and co-recognizable. Suppose M_R
is a TM that recognizes L, and M_C is a TM that co-recognizes L.
```
Let M = on input <x> 1. for k in N on x
2.     Run M_R for k steps
3.     If M_R accepts, accept
4.     Run M_C for k steps
5.     If M_C accepts, reject
```
We can see that if we run this machine for finite k steps, M_R or M_C will halt
eventually, and we can decide whether it is a yes or no instance.

## Enumerator
An enumerator is a TM designed to loop forever, but has access to a special
output tape.

```
Let M0,M1,... be an effective enumerator of the TMs
Let W0,W1,... be an effective enumerator of (sigma)*

E="Ignore input"
1. for k in N
2.     for i in {0,...,k}
3.         for j in {0,...,k}
4.             Run Mi on Wj for k steps
5.             If it accepts, print <Mi,Wj>
```

This TM loops over all possible turing machines and all possible strings. It
never halts, and prints turing machines in Atm.

## Certificates
When we want to prove that a TM and input <M,w> is in a language, we will need
something called a certificate. We can't generally check whether it is in the
language (may loop or just take a long time). However, with a certificate, we
can check whether it is in the langauge in a finite number of steps. is indeed
in the language.

For example, if <M,w> supposedly halts, we can check whether it does halt if we
are provided with a certificate k. Then, we can run M on w for k steps and see
if it does halt within k steps. Similarly, wrong certificates could be
provided. If M doesnt halt on w in k steps, but halts in k+1, we would say that
M does not halt.

With certificates, we have an alternative way of defining recognizability:

> For all x in L, there is some checkable proof c that x is in L  
> For all x not in L, there is no proof that x is in L  
> (There may or may not be proof that x not in L, though)

Checkable means that the verification is decidable.

Similarly, for co-recognizability:

> For all x not in L, there is some checkable proof c that x is not in L  
> For all x in L, there is no proof that x is not in L  
> (There may or may not be proof that x in L, though)

