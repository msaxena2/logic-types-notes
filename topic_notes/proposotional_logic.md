## Notes on Proposotional Logic

Propositional Logic allows for reasoning with propositions. 

We're trying to prove completeness of propositional logic (with a common axiomatization, I omit the details here).

Completeness simply means that one can derive (or prove), any valid propositional formula using the axiomatization. 

More formally, $\models \beta \rightarrow \vdash \beta$. 

We prove the contrapositive, or, $\not \vdash \beta \rightarrow \not \models \beta$.

#### Henkin's Lemma 

$\forall$ formulae, $\beta$ is consistent implies $\beta$ is satisfiable. Now, $\beta$ is consistent means that $\not\vdash \neg \beta$.
Now, if $\beta$ was consistent, but not satisfiable, then, we'd be able to say that $\neg \beta$ is valid, since $\neg \neg \beta = \beta$ is not satisfiable.
This would be a contradiction, because $\not\vdash \neg \beta$, as $\beta$ is consistent, but we proved $\neg \beta$ is valid. If the proof system is complete, 
we'll be able to show the contradiction holds.

### Maximal Consistent Sets

Quite simply put, the maximal consistent set of formulas is the largest set, such that each element in the set is consistent, and 
adding any other propositional element to it would make it inconsistent. A maximal consistent set cannot be extended by adding propositional 
formulas. Also, every subset of a maximal consistent set is also consistent. 

Formally put, a set $\Phi = \{\beta_1, \beta_2 \dots \beta_n\}$ is consistent means $\not \vdash (\beta_1 \wedge \beta_2 \dots \beta_n)$.

Also, X is a maximal consistent set, if $X \subseteq \Phi$ is consistent, but for any $a \notin X$, $X \cup \{a\}$ is not consistent.


### Lindenbaum's Lemma

Simple - Every consistent set can be extended to an MCS. Omitting the formal proof, but its simple. Basically,
 we start with an arbitrarily large set X of propositions. We keep adding propositions from X that are consistent to our set 
 of consistent formulas. Eventually, we end up with the MCS.



Now, given an MCS X let $v_X$ be an evaluation such that $v_X (p) = T$ iff $p \in X$. In other words, we chose a model such that 
every formula in X is true in that model. Note the use of evaluation and model. With propositional logic, I've come to think of
the evaluation as the model. There's a distinction between the two terms, however. An evaluation is simply a mapping of propositions to 
T/F. A model is a more abstract concept. I tend to think of it as world in which all propositions have an evaluation. Think of a proposition - Mustangs
are fast cars. Now, there may be a world (not this world), where that may be the case. Both worlds are models. One world has the evaluation of the proposition
as T, the other as F (how preposterous). The point is - evaluations and models go hand in hand, but they're not the same. This distinction will become useful later.


### Compactness - 

Formally - $X \subseteq \Phi \models \alpha$, iff there exists a Y, $Y \subseteq_{fin} X, Y \models \alpha$. Intuitively, if a subset of propositional formulas $X \models \alpha$, i.e. in every model in which X holds, $\alpha$ also holds, then, there exists a finite subset of X, say Y, s.t. $Y \models \alpha$.

The proof is given using Konig's Lemma, which states that infinite tree having nodes with finite degree has an infinite branch. 

We first prove X is satisfiable, iff every finite subset of X is also satisfiable. 

We unroll the evaluation of every atomic proposition in P. That is, consider all atomic proposition $\{p_0, p_1, \dots \}$, then the tree is a binary tree, which assigns $p_0$ to T along one branch, and F along the other. Then, we take $\alpha \in X$, we prune the tree, by removing 
nodes in which $\alpha$ evaluates to F. That is we remove all nodes, in which $\alpha$ evaluates to $X$, except for the first such node encountered along a path. 

Now, every leaf in the tree, corresponds to an evaluation, s.t. a fromula $\alpha \in X$ evaluates to F using the leaf. This is a contradiction, since 
$X$ itself has to satisfiable. So, we know that there exists an infinite path along which X is satisfiable. If we assume the tree to be finite, 
Then, every leaf in the tree would represent an evaluation of $\alpha$, in which $\alpha$ is unsat. 
