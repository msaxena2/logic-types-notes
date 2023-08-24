A Semantics First Approach to Building Correct Systems
======================================================

Building *correct* systems, i.e. systems that always work as *expected*,
is challenging. To do so, one must be utilize languages that enable
specifying such systems both *precisely* and *formally*.
The *semantics first* approach dictates that
languages (for building systems) must have a formal
semantics from which tools such as interpreter, compilers, model
checkers and deductive verifiers, are derived in a *correct-by-construction*
manner. These can subsequently be used to run the system, and analyze
it to ensure desired properties are satisfied.

A common application of the semantics-first approach is
motivated by establishing properties of systems in a commonly-used
language. For example, ensuring financial transactions
on the Ethereum blockchain implemented as smart contracts
were free of exploits motivated the development of the
K semantics of the Ethereum Virtual Machine (EVM), or KEVM.

While existing languages present attractive targets for
the semantics-first approach, using it to develop
new languages has many benefits. The approach forces one
to painstakingly flush out small details about the language
which may be ignored in an informal paper-based description
of the language. For instance, in [cite-kevm-dsl], a new
Domain Specific Language for calling functions in
EVM smart contracts is described. Said DSL reduces the effort
required in writing correctness specifications for KEVM smart
contracts, but was limited in scope to verification of smart contracts in
the KEVM ecosystem.

The main focus of this work expands the *semantics-first* approach
beyond traditional programming languages into directly expressing
the *semantics of systems* themselves. For instance, consider
Clinical Decision Support Systems that encode Best Practice Guidelines (BPGs) to
assist healthcare providers with situation specific advice
for various clinical scenarios. In order to build such systems,
healthcare practitioners (HCPs) collaborate with software developers to translate paper-based BPGs into
a computable medium. Essentially, HCPs describe the *semantics* of
medical knowledge in BPGs to developers, who subsequently encode it
in some programming language. This "gap" between the paper-based
BPGs, and their encoded executable form, leaves it possible for the
*semantics* of medical knowledge in the system to diverge from that
of the BPG.






