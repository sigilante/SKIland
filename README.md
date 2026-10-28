# SKIland

A centralized explanation of the SKI combinator tools of SKIland.

![](https://pbs.twimg.com/media/HVgZkuVbkAA7uEE?format=jpg&name=medium)

The [S, K, I combinator calculus](https://grokipedia.com/page/SKI_combinator_calculus) is a fundamental computing system discovered in the 1920s by Moses Schönfinkel and elaborated by Haskell Curry.  The three rules are:

$$
I x \equiv x
$$
$$
K x y \equiv x
$$
$$
S x y z \equiv (x z)(y z)
$$

These three operators together are Turing complete and can express all computation.  They strike a balance between minimalism and expressiveness (i.e., both simpler and more complex combinator calculi are possible).

## What SKIland Is

SKIland is a collection of tools built on the SKI combinator calculus which make it viable to express simple computations, high-level computer languages, and even zero knowledge proofs in terms of SKI.

The original insight I had was that SKI could be used to interpret itself, in the tradition of Amin & Ropf's “Collapsing Towers of Interpreters” paper.  And if self-interpreting, then self-extending, like the Nock ISA.

* [Davis (2026, in review) A self-interpreter for SKI: Authoring semantics for new symbols.](https://github.com/sigilante/paper-ski-in-ski/blob/master/mss.pdf)

![](https://pbs.twimg.com/media/HVgUpYcakAE50Eo?format=jpg&name=medium)

With such a self-interpreting expression in hand, it became possible to produce higher-level languages and evaluation tools which use only S, K, and I.

* [SKIjack](https://github.com/sigilante/skijack), a declarative “assembly language” for pure SKI.
* [SKIlark](https://github.com/sigilante/skilark), an implementation of Joy (a Lisp-like Forth) which compiles via SKIjack to pure SKI.
* [Avon](https://github.com/sigilante/avon), a pure SKI runtime with modern features like supercombinator acceleration and memory sharing.  Avon matches supercombinators (higher-order patterns) and reduces them in one step.
  ![](https://pbs.twimg.com/media/HVgXL4daUAAUFT1?format=jpg&name=medium)
* [lean-ski](https://github.com/sigilante/lean-ski), a collection of Lean proofs related to the above tools.
* [ski-tools](https://github.com/sigilante/ski-tools), small tools for reading and visualizing SKI combinator terms as used in my papers.

With SKIjack, the self-interpreter expression looks like this:

![](https://pbs.twimg.com/media/HVgU004a8AAN8FI?format=jpg&name=medium)

Scott encoding lets us write interpreted literals in a unary format:

```
0 = K
Succ = S (K K) (S (K (S I)) K)
1 = Succ K = K (S I (K K))
2 = K (S I (K (K (S I (K K)))))
...
```

This is not efficient!  Each unit wraps the previous numeral in `K (S I (K ·))` so it grows as a constant number.

In SKIjack's unary notation for numbers, 2⁶⁴-1 would consume 150 exabytes.  It would take 5,800 years to compute it at 10⁸ contractions a second.

## zkSKI

Zero-knowledge computing with SKI is possible.  By zkSKI I mean two things:

1. SKI as statement language. A statement is a combinator term. It holds when the term, applied to the public input & some private witness, reduces to K (true). The verifier is a combinator term so checking a proof is itself an SKI reduction.
2. Zero-knowledge proofs of SKI statements. A proof convinces the verifier that some witness makes the term reduce to K (true) & reveals nothing else about the witness.

As you can imagine, things like Poseidon2 hashing are incredibly expensive—but Avon makes it actually viable!

## History of SKI Computing

The combinator calculi like SKI and BCKW were early on proven to be equivalent to Church's lambda calculus and to the Turing-complete system of general-purpose computing.  However, they have only rarely been used outside of mathematical logic for much of their history.  Two notable exceptions exist:

1. The SKI Machine, developed by David A. Turner in 1979 as a target for compiling functional languages like SASL.  While the basic SKI system uses just the three combinators S, K, and I, Turner introduced an extended set (including B, C, and others) to optimize bracket abstraction and reduce the inefficiencies of standard lambda-to-combinator translation.

  His approach utilizes combinator graph reduction, where programs are represented as directed graphs of composed combinators rather than abstract syntax trees. This method allows for self-optimizing behavior and eliminates the need for runtime scope management, making it a foundational technique for lazy functional language implementations.  This led to—

2. The Reduceron, a special-purpose FPGA-based graph reduction machine designed to execute lazy functional programs (specifically Haskell).  It was developed by Matthew Naylor, Colin Runciman, and Jason Reich at the University of York, funded by the EPSRC (2008–2010).

  For various reasons in computing economics, this line of research was abandoned around 2012.  However, it was influential on the contemporary Heron project (2023, Craig Ramsay and Robert Stewart at Heriot-Watt), which positions itself as a "spiritual successor" to the Reduceron, re-implementing its operational semantics on modern Xilinx UltraScale+ FPGAs.

SKIland continues in this tradition by providing a portable computing virtual machine, a substrate for general-purpose computer languages, and a kernel system for persistent stateful computing sessions.

![](https://pbs.twimg.com/media/HVgaRW3bwAERbBg?format=jpg&name=medium)
