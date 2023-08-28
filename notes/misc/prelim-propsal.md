---
header-includes: |
  \include{header.tex}
---


A Semantics First Approach to Building Correct Systems
======================================================

Building *correct* systems, i.e. systems that always work as *expected*,
is challenging. To do so, one must be utilize languages that enable
specifying such systems both *precisely* and *formally*.
The *semantics first* approach dictates that
such languages must have a *formal
semantics* from which tools such as interpreter, compilers, model
checkers and deductive verifiers, are derived in a *correct-by-construction*
manner. These can subsequently be used to run the system, and analyze
it to ensure desired properties are satisfied.

Often, the semantics-first approach is used to
systems in an existing commonly used language
satisfy desired properties.
For example, ensuring financial transactions
on the Ethereum blockchain implemented as smart contracts
were free of exploits motivated the development of the
K semantics of the Ethereum Virtual Machine (EVM), or KEVM.
The K derived tools can be used to analyze EVM programs,
and the executable semantics serve viable alternative
to the informal paper-based semantics.

While existing languages present attractive targets for
the semantics-first approach, using it to develop
new languages has many benefits. It forces one
to flush out nuances of the language
that may be ignored in an informal paper-based description
of the language. For instance, in [cite-kevm-dsl], a new
Domain Specific Language for calling functions in
EVM smart contracts is described. Said DSL reduces the effort
required in writing correctness specifications for KEVM smart
contracts, but was limited in scope to verification of smart contracts in
the KEVM ecosystem.

My work seeks expands the *semantics-first* approach
beyond traditional programming languages into thinking about
the *semantics of systems* themselves. For instance, consider
Clinical Decision Support Systems that encode Best Practice Guidelines (BPGs) to
assist healthcare practitioners (HCPs) with situation specific advice
for various clinical scenarios. BPGs are systematically developed,
evidence-based statements that inform HCPs of appropriate treatment
for various clinical situations.
In order to a build CDSSs, HCPs collaborate with software developers
to translate paper-based BPGs into
a computable medium. Essentially, HCPs describe the *semantics* of
medical knowledge in BPGs to developers, who subsequently encode it
in some programming language. This "gap" between the paper-based
BPGs, and their encoded executable form, leaves it possible for the
*semantics* of medical knowledge in the system to diverge from that
of the BPG. Such a divergence can potentially lead to worse patient
outcomes, and decrease HCP confidence in the system.

The aforementioned "gap" can be addressed if medical knowledge in
a CDSS is expressed in a manner that HCPs can *comprehend*, enabling
them to *validate* that the system has intended semantics. In
[cite-rewriting-CDSS], instead of using a traditional programming language,
we used $\K{}$-rules to express medical knowledge. While $\K{}'s$
configuration abstraction entailed *concise* representations of
medical logic, the $\K{}$-based definition was no more *comprehensible*
to HCPs than an equivalent program in a conventional programming language .

To address *comprehensibility*, we looked at
existing paper based BPGs, and extracted *common constructs* from them.
We developed a new programming language based on these *constructs*, called
$\MediK{}$, and used it to implement a complex CDSS. As programs in our language
resemble their paper-based counterparts, they're intended to be directly
*comprehensible* to HCPs. The use of the
semantics-first paradigm ensures that our language has analysis tools, such as
a model-checker, that we used to ensure that the CDSS satisfies desired safety
properties.

My proposed work aims to futher expand on our *semantics-first* approach
along the following lines:

 * Use of *semantics-based compilation* to generate *correct-by-construction*
   summaries of programs that HCPs can use to validate medical knowledge in a
   CDSS.
 * Incorporation of Learning Enabled Components (LEC) using *simplex*
 architecture, where the LEC's recommendation are rejected if they are unaligned
 with known best practice specified in $\MediK{}$.
 * Generation of execution proof objects that serve as evidence of adherence to
   known best practices.





