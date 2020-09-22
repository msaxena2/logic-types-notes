---
title: Summary Based Symbolic Evaluation of Smart Contracts
author: Notes on ASE'20 paper
header-includes: |
  \include{commands}
---

Synopsis
--------

This paper introduces SOLAR, a tool to analyze
security vulnerabilities in ethereum smart contracts
via summary based symbolic execution. The tool works
as follows -

1. Given a program, an analyst writes a pattern
   that expresses the vulnerability. For example,
   a pattern like (transfer transfer .. store)
   specifies the re-entrancy bug, where the
   vulnerable program calls transfers to the attacker
   multiple times (due to renetrancy) before updating
   its state to relfect the transfer. The pattern
   is represented via a FOL formula.

2. Next step is to confirm the vulnerability exists
   by constructing an attack where a trace exhibits
   the vulnerability. For this, the analyst
   provides boilerplate code (the attacker contract)
   and the tool fills in the details.

The contract as modeled as a transition system over
states, where the state space consists of registers,
memory and storage.

SOLAR takes as input a vulnerability description
expressed in Racket as a boolean predicate over the
transition system. The goal is to synthesize an attack
program that exercises the vulnerability. The key
challenge is to find such a program. It relies on
the ethereum's ABI to actually generate such a program.
For a program with n public ABI methods, there
will be $n^k$ candidate inputs of length k. Thus,
exhaustive exploration of the entire state space to
synthesize such a method simply doesn't scale.















