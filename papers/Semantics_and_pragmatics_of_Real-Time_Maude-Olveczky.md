---
title: Abstract Interpretation
subtitle: Higher Order and Symbolic Computation
author: Peter Olveczky and Jose Meseguer
header-includes: |
  \include{commands}
---

Real Time Maude
---------------

Real-time Maude is an extension to full maude with support for modeling timed systems.


### Equational Theory of Time
  The equational theory $\TIME$ is defined as an order commutative monoid
  $(\Time, 0, \leq)$ with a monus operator  $\dotdiv$ .

### Timed Rewrite Theory
Given an equational theory $\mathcal{R} = \left( \Sigma, E, \varphi, R \right)$, a
  real time rewrite theory $\mathcal{R}_{\phi, \tau}$ is a tuple
  $\left(\mathcal{R}, \phi, \tau \right)$, where
  \begin{itemize}
    \item $\phi: \TIME \to (\Sigma, E)$ is an equational theory
      morphism for the theory $\TIME$ to the underlying theory $\mathcal{R}$,
      i.e., $\phi$ interprets $\TIME$ in $\mathcal{R}$.
    \item $(\Sigma, E)$ defines opertaor $\{\_\}: \State \to \System$ that takes
      a system state to the global top-level state. The sort $\System$ has no
      subsorts.
    \item $\tau$ is an assignment $\tau_l$ of sort $\phi(\Time)$ to every rewrite rule
      of the form $ l: \{t\}  \rightarrow \{t'\}\ \text{if}\ \psi$. This is
      abbreviated as $l: \{t\} \xrightarrow{\tau_l} \{t'\}\ \text{if}\ \psi$.
      $\tau_l$ denotes the duration of rewrite, called a tick rule, and may
      contain variables outside $t$, $t'$.
  \end{itemize}
\end{definition}

## Specifying Systems in Real-Time Maude
Modules in Real-Time Maude are called timed modules and specified as
`tmod .. endtm` or `tomod .. endtom`.
The theory $\TIME$ is implicitly implemented in Real-Time Maude by
a functional module with the appropriate operators $\leq, \geq, \dotdiv$.
