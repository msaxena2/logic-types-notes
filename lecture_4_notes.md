### Lecture 4

#### Every infinite planar graph is 4 colorable

* Planar graph is one that can be drawn on a plane without the edges crossing each other. Comple graph with 4 nodes in planar. Above 4, any complete graph above 4 notes cannot be planar. 
* Any non planar graph has a minor graph with K5 (Complete with 5 nodes). 

#### Want to show that an infinite planar graph is 4 colorable as well. We know that every finite subgraph is planar, and 4 colorable.


```

We have an infinite planar graph i.e. V = {v0, v1, .....} is countable. E (set of edges) is a subset of V * V. 


We choose a set of propositions - {C^i_R, C^i_G, ..., i \in N}. 

That is the ith node in the graph has the color from {R, G, B, Y}. 

Want to assign an evaluation such that - 

C^i_R \/ C^i_G .... and C^i_R => !C^i_G and so on. 


Also, For every V = (Vi, Vj) => the two vertices of don't have adjacent veritices.

Using compactness theorem - X is unsat iff there is a finite subgraph Y \in X s.t. it is unsat. 

But every Y \in X is sat (using 4 CT). So X is Sat, and G is 4 colorable.
```

#### Axiomatization

A formula can be proven to be valid using computation i.e. by showing that a formula holds for all evaluations. 

Proving on the other hand, using rules to come up with the validity of a formula.

An axiom system is a finite set of axioms, and we prove formula modulo the axioms. Then we use inference rules (ways of proving facts from other facts, and must be sound), along with axioms to prove formulae. 


##### Completeness - Any valid formula is provable in the proof system. 

Continued in the next class.






