Reactive Systems Verification
=============================

Some notes based on Verfication of Reactive Systems by Klaus Schneider.

Generalogy of verification
--------------------------

### Modal Logic

Modal logic formulas $\varphi$ are defined inductively as:

$$
    \varphi ::= p \mid \neg p \mid p \vee p \mid \langle p \rangle
$$

Other formulas, such as $[\varphi] \equiv \neg \langle \neg \varphi \rangle$

Modal logic, unlike propositional logic, is interpreted in models called
*Kripke Structures*, defined as $\mathcal{K} = (S, I, \mathcal{R})$, where
$S$ is the set of worlds, $I$ the set of initial worlds, and $\mathcal{R}
\subseteq S \times S$ an accessibility relation s.t. $(s, s') \in \mathcal{R}$
signifies $s'$ is *accessible* from $s$.

### Dynamic Logic

Dynamic logic adds to model logic, *programs* or *actions*. Given a Kripke
structure $\mathcal{K} = (S, I, \Sigma, \mathcal{R}, \rho)$, where $S$ is the
set of words, $I$ the set of intial worlds, $\Sigma$ a set of actions,
$\mathcal{R} \subseteq S \times \Sigma \times S$,
a transition relation on $S$ s.t. $(s, a, s') \in \mathcal{R}$ means
$s'$ is accessible from $s$ on action $a$, $\rho: S \rightarrow 2^{P}$ an
interpretation function defining propositions that hold in a world $s \in S$,
where $P$ is the set of atomic propositions.

The set of programs is defined as:
$$\alpha ::= p \in P \mid \alpha_1 ; \alpha_2 \mid \alpha_1 \cup \alpha_2$$

Let $\overline{\rho} \subseteq S \times \alpha \times S$ be recursively defined
as:

 - $(s, a, s') \in \overline{\rho}$ iff $(s, a, s') \in \rho$
 - $(s, \alpha_1 ; \alpha_2, s') \in \overline{\rho} \iff \exists s'' \in S
   . (s, \alpha_1, s'') \in \overline{\rho} \wedge (s'', \alpha_2, s') \in \overline{\rho}$.
 - $(s, \alpha_1 \cup \alpha_2, s') \in \overline{\rho}$ iff
   $(s,\alpha_1, s') \in \overline{\rho} \vee (s, \alpha_2, s') \in \overline{\rho}$


The set of dynamic logic formulas are given by:
$$ \varphi ::= p \in P \mid \varphi \vee \varphi \mid \neg \varphi \mid \langle\alpha\rangle\varphi $$

Other formulas, such as $[\alpha]\varphi \equiv \neg\langle\alpha\rangle\neg\varphi$.

Given $s \in S$, we say:

- $s \vDash p$ iff $p \in \rho(s)$
- $s \vDash \neg \varphi$ iff $s \not\vDash \varphi$
- $s \vDash \langle\alpha\rangle\varphi \iff \exists (s, \alpha, s') \in
  \overline{\rho}$ s.t. $s' \vDash \varphi$.

