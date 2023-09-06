---
header-includes: |
    \include{header.tex}
references:
- id: ClinicalGuidelinesURL
  title: Clinical practice guidelines
  url: "https://www.nccih.nih.gov/health/providers/clinicalpractice"
- author:
  - family: Hildenbrandt
    given: Everett
  - family: Saxena
    given: Manasvi
  - family: Zhu
    given: Xiaoran
  - family: Rodrigues
    given: Nishant
  - family: Daian
    given: Philip
  - family: Guth
    given: Dwight
  - family: Moore
    given: Brandon
  - family: Zhang
    given: Yi
  - family: Park
    given: Daejun
  - family: Ştefănescu
    given: Andrei
  - family: Roşu
    given: Grigore
  container-title: 2018 IEEE 31st computer security foundations
    symposium
  id: csf-18
  issued: 2018
  page: 204-217
  publisher: IEEE
  title: "KEVM: A complete semantics of the ethereum virtual machine"
  title-short: KEVM
  type: paper-conference
- author:
  - family: Park
    given: Daejun
  - family: Zhang
    given: Yi
  - family: Saxena
    given: Manasvi
  - family: Daian
    given: Philip
  - family: Roşu
    given: Grigore
  collection-title: ESEC/FSE 2018
  container-title: Proceedings of the 2018 26th ACM joint meeting on
    european software engineering conference and symposium on the
    foundations of software engineering
  doi: 10.1145/3236024.3264591
  id: fse-18
  isbn: 9781450355735
  issued: 2018
  keyword: K framework, Ethereum, formal verification, smart contracts
  page: 912-915
  publisher: Association for Computing Machinery
  publisher-place: New York, NY, USA
  title: A formal verification tool for ethereum VM bytecode
  type: paper-conference
  url: "https://doi.org/10.1145/3236024.3264591"
- author:
  - family: Saxena
    given: Manasvi
  - family: Chen
    given: Xiaohong
  - family: Song
    given: Shuang
  - family: Meng
    given: Shaoyu
  - family: Sha
    given: Lui
  - family: Rosu
    given: Grigore
  id: k-guidelines-tr
  container-title: Technical Report 2022
  title: Rewriting-based Computer-Interpretable Clinical Practice
    guidelines
  url: "https://hdl.handle.net/2142/116016"
- author:
  - family: Saxena
    given: Manasvi
  - family: Song
    given: Shuang
  - family: Sha
    given: Lui
  collection-title: FMCAD 23
  container-title: Formal methods in computer aided design
  id: fmcad-23
  issued: 2023
  title: "MediK: Towards safe guideline-based clinical decision support"
  title-short: MediK
  type: paper-conference
---


A Semantics First Approach to Building Correct Systems
======================================================

Building *correct* systems, i.e. systems that always work as *expected*,
is challenging. To do so, one must be utilize languages that enable
specifying such systems both *precisely* and *formally*.
The *semantics-first* approach dictates that
such languages must have a *formal
semantics* from which tools such as interpreter, compilers, model
checkers and deductive verifiers, are derived in a *correct-by-construction*
manner. These can subsequently be used to run the system, and analyze
it to ensure desired properties are satisfied.

Often, the *semantics-first* approach is used to analyze systems in
an existing commonly used language. To enable this,
said language's executable semantics are developed using
existing *informal* semantics.
For example, ensuring financial transactions
on the Ethereum blockchain implemented as smart contracts
were free of exploits motivated the development of the
$\K$ semantics of the Ethereum Virtual Machine (EVM), or KEVM [@csf-18].
The $\K$ derived tools can be used to ensure EVM programs satisfy
desired properties. But, beyond program analysis,
the executable semantics also serve as a viable alternative
to the informal paper-based semantics.

However, while existing languages present attractive targets for
the *semantics-first* approach, it truly shines when it is used to
develop new languages. It forces one
to carefully consider and flush out nuances of the language
that may be ignored in an informal paper-based description
of the language. For instance, in [@fse-18], a new
Domain Specific Language for calling functions in
EVM smart contracts is described. Said DSL reduces the effort
required in writing correctness specifications for KEVM smart
contracts, but was limited in scope to verification of smart contracts in
the KEVM ecosystem.

My work seeks expands the *semantics-first* approach
beyond traditional programming languages into thinking about
the *semantics of systems* themselves. For instance, consider
Clinical Decision Support Systems (CDSSs) that encode Best Practice Guidelines (BPGs) to
assist healthcare practitioners (HCPs) with situation specific advice
for various clinical scenarios. BPGs are systematically developed,
evidence-based statements that inform HCPs of appropriate treatment
for various clinical situations [@ClinicalGuidelinesURL].
In order to a build CDSSs, HCPs collaborate with software developers
to translate paper-based BPGs into
a computable medium. Essentially, HCPs describe the *semantics* of
medical knowledge in BPGs to developers, who subsequently encode it
in some computable format. This "gap" between the paper-based
BPGs, and their encoded executable form, leaves it possible for the
*semantics* of medical knowledge in the system to diverge from that
of the BPG. Such a divergence can potentially lead to worse patient
outcomes, and decrease HCP confidence in the system.

The aforementioned gap can be addressed if medical knowledge in
a CDSS is expressed in a manner that HCPs can *comprehend*, enabling
them to *validate* that the system indeed has intended semantics.
This can be challenging as CDSSs are complex multi-agent systems
that also contain execution-related details.
In [@k-guidelines-tr], instead of using a traditional programming language,
we used $\K{}$-rules to express medical knowledge.
$\K$'s support for contextual rewriting and
configuration abstraction enabled *concise* representations of
medical logic. But, while the $\K{}$-based definition was
*concise*, it was no more *comprehensible*
to HCPs than an equivalent program in a conventional programming language.

To address *comprehensibility*, we looked at
existing paper based BPGs, and extracted *constructs* commonly used to
describe medical knowledge.
We developed a DSL based on these *constructs*, called
$\MediK{}$, and used it to implement a complex CDSS [@fmcad-23]. As programs in our language
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

# References
