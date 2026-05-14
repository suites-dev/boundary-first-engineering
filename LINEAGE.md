# Lineage

This manifesto extends ideas from people who have thought about verification, boundaries, and trust in software for a long time. Nothing here is invented in isolation.

## Foundational

- **Gerard Meszaros**: *xUnit Test Patterns* (2007). The vocabulary of test infrastructure: fixtures, doubles, the patterns that make testing at scale possible.

- **Vladimir Khorikov**: *Unit Testing Principles, Practices, and Patterns* (2020). The managed versus unmanaged distinction underwrites the entire doubles-policy of this manifesto.

- **Steve Freeman and Nat Pryce**: *Growing Object-Oriented Software, Guided by Tests* (2009). Outside-in TDD, the walking skeleton, and the discipline of letting tests drive design from the boundary inward.

- **Alistair Cockburn**: Hexagonal Architecture / Ports and Adapters (2005). The architecture that made boundary-first thinking coherent. Adapters are what the manifesto's blackbox-style testing acts on.

- **Sam Newman**: *Building Microservices* (2015, 2nd ed. 2021). The service test and the application of test boundaries to distributed systems.

- **Kent Beck**: *Test-Driven Development: By Example* (2002) and the broader xUnit family (1994 onward). The xUnit pattern is the ancestor of every test framework discussed here. Beck's recent framing of TDD as immutable specification for AI agents directly informs Principle 2.

- **Martin Fowler**: *Refactoring* (1999) and *Mocks Aren't Stubs* (2007). The framing of self-testing code as a prerequisite for sustainable design, and the vocabulary that distinguishes the kinds of doubles a verification system uses.

- **Mike Cohn**: *Succeeding with Agile* (2009). The test pyramid, which this manifesto inherits and inverts: the load-bearing layer has moved.

- **Brian Marick**: *How to Misuse Code Coverage* (1999). The original argument that coverage measures execution, not verification. Principle 4 updates Marick's claim for the agent era.

- **Michael Feathers**: *Working Effectively with Legacy Code* (2004). Seams, characterization tests, the discipline of testing what the system actually does rather than what it should.

## Contemporary

- **Justin McCarthy, Jay Taylor, and Navan Chauhan**: The StrongDM Software Factory (2025–2026). The Digital Twin Universe, the reframing of tests as scenarios stored outside the codebase, and the case study that boundary-first verification can work at the most extreme end of the agent-built spectrum.

- **Leo de Moura**: *When AI Writes the Software, Who Verifies It?* (2026). The verification thesis stated in its sharpest form, and the work on formal verification that gives the question technical weight.

- **Simon Willison**: chronicling the agentic moment in software engineering throughout 2025 and 2026, including the visit to the StrongDM AI team that surfaced the Software Factory for a wider audience.

- **Shay Cohen and the Wix AI Coding Agent team**: *The AI Coding Agent Manifesto* (April 2026). Direct prior art for Principle 2: their third principle ("Never let the same AI write the code and judge the code") articulates the implementer-verifier identity problem in nearly identical terms. Their value pair "Verification over generation" frames the same economic shift. Boundary-First Engineering extends their work by formalizing the artifact-vs-authority distinction and by making the outside-the-loop approval step explicit.

- **George Tsiokos**: *Circular Validation: The Hidden Risk in AI-Generated Tests* (February 2025). The canonical name for the failure mode Principle 4 prohibits. Tsiokos's "student grading their own exam" framing is the most-referenced shorthand in 2025–2026 discourse on this problem.

## The Discourse

The problems this manifesto answers were named, in 2025 and 2026, by the people working through them in public. The Hacker News threads ["When AI writes the software, who verifies it?"](https://news.ycombinator.com/item?id=47234917) and ["Verification debt: the hidden cost of AI-generated code"](https://news.ycombinator.com/item?id=47289406) contain the audience writing its own diagnosis. This manifesto would not exist without those threads. Specific contributors, those who articulated the circular validation problem, the verification complexity barrier, and the spec-skeptic critique, have shaped the principles of this document.
