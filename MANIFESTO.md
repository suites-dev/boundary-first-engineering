# Boundary-First Engineering

*A manifesto for verifying software in the agentic era.*

---

## Preamble

Software engineering had a trust stack: unit tests checked intent, code review caught what tests missed, coverage
measured reach, static analysis enforced structure, and continuous integration ran all of it on every change. That stack
worked at human-scale code volume.

Agentic development changes the rate and density of what the stack must absorb.[^1] The same implementation loop can now
generate code, unit tests, coverage, and reviewable diffs around its own internal story. The legs of the old stack do
not fail uniformly. Static analysis and CI survive and become more important. Unit tests, code review, and coverage fail
as trust mechanisms when the rate and density of generated implementation exceed the bandwidth of independent human
understanding.

The bottleneck has moved from writing software to knowing whether software works.[^2] The team that ships AI-generated
code without a way to trust it does not ship faster. It ships further from working software, faster.

What survives is verification at the **boundaries** between things: managed state, transports, external surfaces,
contracts between services. Boundaries are where systems are observable, where intent is contestable, and where trust
accrues independent of whom, or what, wrote the implementation.

This manifesto is for engineers and the agents they work with. Agents now read CI logs, parse linter output, iterate
against test failures, and respond to PR feedback. They are no longer just authors of code. They are full participants
in the engineering pipeline. Every guardrail, type check, schema violation, and test failure is now a message in a
shared protocol between humans and the implementations they oversee. The principles below describe pre-merge
verification: the trust that must accrue before code runs in production. Trust continues to accrue at runtime through
telemetry, tracing, and runtime contract checking; that half of the discipline is real and outside this document's
scope. We value:

* **Behavior at boundaries** over **assertions about internals**
  *because internals serve whoever wrote them; boundaries serve whoever uses them.*

* **Real systems for what you own** over **stubs for what you build**
  *because what you control should be tested as it runs, not as you imagine it runs.*

* **What crosses the boundary** over **what happens inside it**
  *because the contract outlasts every implementation that honors it.*

* **Verification from outside the implementation loop** over **tests written from within it**
  *because no writer, human or agent, can be trusted to grade their own work.*

That is, the items on the right have their place. In the era we are now in, the items on the left bear the load.

---

## Principles

**1. Boundaries are where trust accumulates.**
A system is what it does at its boundaries: managed state, transports, external surfaces, contracts between services.
Internals are private and changeable; boundaries are public and durable. Verify what the system shows the world, not
what it does to itself.

<details>
<summary>💡 Where boundaries actually are</summary>

In a typical backend service, the boundaries that bear trust are: the HTTP or gRPC interface published to consumers, the
database writes and reads, the messages produced to and consumed from queues, the calls to upstream third-party
services, the events emitted to event buses, and the contracts (OpenAPI, JSON Schema, Protobuf, AsyncAPI) that describe
each of these. Verification work concentrates here. Internal function calls, private helper modules, and intra-process
orchestration are not boundaries; they are implementation.

</details>

**2. The implementer cannot be the verifier.**[^3]
Whoever writes the code, human or agent, cannot also be the authority that proves it correct. The distinction is between
an *artifact* and an *authority statement*: tests written by the implementer are design artifacts that describe what was
built; verification is an authority statement that proves what was needed. Implementer drafts become verification only
when reviewed and approved as statements of system intent by someone outside the implementation loop. Independent
verification is to software what independent audit is to accounting. Approval is not laundering. An artifact from inside
the loop does not become verification when someone outside signs off; the artifact itself must originate outside, or be
checked against something that did. Outside the loop means a different point of origin: a different author, a different
time, a different source of authority. Not a different reviewer of the same artifact, not a fresh context window in the
same agent, not a clean PR description. The test is provenance, not appearance.

<details>
<summary>💡 What counts as outside the loop</summary>

**Outside the loop:**

- Another team publishes an OpenAPI doc for the service you're about to build. You implement against their doc.
- Compliance writes the rules your data handling must follow. You implement; an audit checks.
- A product manager writes acceptance scenarios in a separate doc before sprint planning. Your tests run those
  scenarios.

**Borderline:**

- A teammate writes acceptance tests after reading the ticket, before you start coding. Counts if the tests existed
  before your implementation; doesn't count if they wrote them after seeing your draft.

**Inside the loop:**

- You write a unit test next to the function you just wrote.
- Your agent generates tests for the code it generated. You approve them in the same PR.
- A reviewer approves your tests after reading your PR description.

</details>

**3. Specifications describe boundaries, not implementations.**
A schema is not a program; a contract is not an algorithm. The boundary description names what crosses the boundary and
what must hold there. It is thin enough that a human can read it in one sitting; durable enough that the implementation
can be regenerated against it; contestable enough that humans can disagree about meaning. Anything that prescribes how
the internals work is implementation by another name.

<details>
<summary>💡 Thin enough vs. too heavy</summary>

**Thin enough:**

- An OpenAPI doc with the endpoints, request and response shapes, error codes, and the business rules a consumer must
  understand. Five to fifty pages of YAML for a typical service.
- A protobuf file with the message types and RPC methods. The wire format is fully specified; the server implementation
  is not.
- An event schema listing the events the service publishes, when each is emitted, and what fields each carries.

**Too heavy:**

- A spec that names internal classes, database table layouts, or which library handles JSON parsing. That's
  implementation, written in prose.
- A spec long enough that regenerating the service from it requires reading the spec, the implementation, and the diff
  between them. If the spec and the code together are needed to understand the system, the spec lost its job.

</details>

**4. Circular validation is not verification.**
An agent can generate code, generate checks for that code, make the checks pass, and raise coverage without ever leaving
its own interpretation of the task. That is an infinite loop of agreement, not a proof of correctness. Coverage measures
what tests reach; it does not measure whether the tests describe something true. The loop breaks only when the system is
constrained by specifications and verification artifacts held outside the implementation path. The outside artifact is
the deterministic anchor against which non-deterministic generation can be measured.

<details>
<summary>💡 What circular validation looks like</summary>

**Circular:** An agent generates an endpoint that returns user data. It generates tests asserting the endpoint returns
user data. Coverage is 100%. The endpoint silently filters out unsubscribed users; no test catches it because the agent
didn't notice the requirement either.

**Loop broken:** The same endpoint, tested against scenarios a product owner wrote before the agent started. The filter
bug is caught on the first run, because the scenario didn't come from the same source as the code.

</details>

**5. Feedback loops decide whether agents can iterate.**
Verification that takes minutes is verification that gets skipped. An agent that can run a check in under a second
corrects itself; an agent that waits for human review converges on whatever satisfies the human. Speed is not authority.
A fast implementer-authored test is still an implementer-authored test. The speed of the verification layer determines
whether the agent learns from the system or learns to game the human.

<details>
<summary>💡 Loops the agent can use vs. loops it can't</summary>

**Fast enough for the agent's inner loop:** type checks, lint rules, unit tests, a single integration test against a
local containerized database, a schema validation against the OpenAPI spec.

**Too slow:** a full system test suite, a manual QA pass, a security review, a stakeholder demo. The agent will either
skip these or shape its work to pass them in name only.

</details>

**6. Guardrails enable agentic coding.**
Types, schemas, linters, dependency rules, architectural constraints, security policies, and granular CI survive every
refactor. They encode engineering judgment in forms agents consume repeatedly, scaling past what human review can cover.
Established guardrails escape the circular-validation critique because their provenance is independent: authored before
the code, by people who will not write it, persisting across implementations. Guardrails added inside the implementation
loop, a lint rule tuned until the branch passes, fall under the same circular-validation critique. Guardrails too
strict, too verbose, or too numerous for humans to hold in working memory are the point. They cap structural complexity
at the source, keeping what does get written within human reading range. Code review remains valuable for design intent
and architectural alignment; it can no longer be the primary mechanism for catching defects. When implementations
rotate, the guardrails are the deliverable.

<details>
<summary>💡 Established guardrails vs. loop-introduced ones</summary>

**Established:** A dependency-cruiser config the architect wrote last year, preventing the data layer from importing
controllers. An OpenAPI schema check that has gated every PR for six months.

**Loop-introduced:** A lint rule the implementer added in the same PR as the code that triggered it. A test exclusion
list updated to skip the test the agent's code fails.

</details>

---

See [LINEAGE.md](./LINEAGE.md) for the thinkers and works this manifesto extends. \
See [SIGNATORIES.md](./SIGNATORIES.md) for the people who have signed. \
See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to sign or propose changes.

---

[^1]: In April 2026, GitHub announced it had moved from a 10× capacity plan to designing for 30× today's scale because
agentic development workflows had accelerated sharply since the second half of December 2025.
See: https://github.blog/news-insights/company-news/an-update-on-github-availability/

[^2]: METR's 2025 randomized controlled trial of experienced open-source developers found a 19% slowdown when using AI
tools, against a self-reported 20% speedup: a 39-point perception gap between intuition and
outcome (https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/).

[^3]: This principle has direct prior art in Shay Cohen, *The AI Coding Agent Manifesto* (Wix Engineering, April
2026, https://medium.com/wix-engineering/the-ai-coding-agent-manifesto-c8f61629d677) and is framed as security
architecture in Leo de Moura, *When AI Writes the World's Software, Who Verifies It?* (February
2026, https://leodemoura.github.io/blog/2026/02/28/when-ai-writes-the-worlds-software.html). George Tsiokos coined the
related term "circular validation" in February
2025 (https://george.tsiokos.com/posts/2025/02/circular-validation-ai-testing/). See [LINEAGE.md](./LINEAGE.md) for full
attribution.
