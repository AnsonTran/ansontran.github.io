# \\(L=\\{1^{2^n}|n \in N\\}\\)

1. Replace first 1 with $ to indicate start
2. Go right, replace odd 1s with x  
   If we encounter even 1s in a pass, reject
3. Move back to after $, if we reach end of string
4. Accept if we dont encounter any 1s in a pass

```
- q1 qA qR
q1 1 q2 $ R
q2 x q2 . R
q2 1 q3 x R
q2 - qA . R
q3 x q3 . R
q3 1 q4 . R
q3 - q5 . L
q4 x q4 . R
q4 1 q3 x R
q5 x q5 . L
q5 x q5 . L
q5 1 q5 . L
q5 $ q2 . R
```
[Turing Machine Simulator](https://mustafaquraish.github.io/TMSim/)
