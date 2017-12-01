### Lecture 3 Notes


* Lecture notes link - http://cs.rice.edu/~vardi/comp409


#### Relevance lemma

If two models M and M', map the propositions occuring in a formula \phi, then M \enatils \phi iff M' \enails \phi.


### Decision Problem 

Givem a  model, find whether a foumola satisifes can be done in P time (O(n)) where is is  \phi|


### Cook's theory

If a sat problem can be reduced to another problem, then the other problem must also be NP complete.


### Validity

\phi is valid iff ~\phi is not satisfiable.

### Compactness Theorem

let X be a subset of formulae, then X is satisfiable iff every Y, a finite subset of X is satisfiable.

### Konig's Lemma

An infinite finitely - branching tree has an infinite branch. A finitely branching tree is a rooted tree, such that every node has finitely man children. The tree is inifinite since the number of node are infinite.


#### Proof 
```
Consider the root - one of the children must be a finitely branching tree. Thus, you can find an infinite path in one of the children subtrees. Choose that subtree, and it has an inifinte path. 
```

#### Proof
Represent infinite set of propositions as an inifinite finitely branching tree. Continued in next lecture. 



