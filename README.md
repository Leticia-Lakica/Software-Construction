# Software-Construction
## How Netflix Uses Microservices

Netflix is one of the most well-known adopters of microservices architecture.

### The Transition
Netflix moved away from a monolithic architecture around 2008–2009 after a major outage, migrating to AWS and rebuilding their system as microservices over several years.

### How They Use Microservices
Netflix runs hundreds of microservices, each responsible for a specific function. Separate services handle user authentication, recommendations, billing, video encoding, search, and playback. This means a problem in one service doesn't necessarily bring down the rest of the system.

### Key Patterns They Pioneered

- **API Gateway** : Netflix built [Zuul](https://github.com/Netflix/zuul), a gateway that routes incoming requests from devices to the appropriate backend microservices.
- **Service Discovery** : They built [Eureka](https://github.com/Netflix/eureka), which lets services find and communicate with each other dynamically without hardcoded addresses.
- **Circuit Breaker** : They created [Hystrix](https://github.com/Netflix/Hystrix) to prevent cascading failures. If one service goes down, Hystrix "trips" and falls back gracefully rather than letting failures ripple through the system.
- **Chaos Engineering** : Netflix built [Chaos Monkey](https://github.com/Netflix/chaosmonkey), a tool that randomly kills services in production to ensure the system can handle real failures.

### Scaling
Because each service is independent, Netflix can scale only the parts that need it for instance, massively scaling their streaming service during peak evening hours without touching billing or account management.

### The Tradeoffs They Manage
Managing hundreds of services introduces complexity around deployment, monitoring, and distributed tracing. Netflix invested heavily in internal tooling (like [Spinnaker](https://spinnaker.io/)) to handle this at scale.

> Many of the tools Netflix built internally were eventually open-sourced and became the foundation for modern microservices patterns industry-wide.



## Companies That Moved Away from Microservices Back to Monolith

Several high-profile companies have publicly documented their decision to reverse course
from microservices back to a monolithic (or "majestic monolith") architecture.

---

### 1. 🛒 Amazon Prime Video (2023)
**Why they switched back:**
Their audio/video monitoring service was built as microservices using AWS Step Functions
and Lambda. The distributed architecture created two core problems:
- High costs from data passing between services
- Scalability bottlenecks at scale

**Outcome:** By consolidating into a monolith, they reduced infrastructure costs by **90%**
and improved scalability. Ironically, this came from the company that sells the cloud
infrastructure most microservices run on.

---

### 2. 🛠️ Segment (2020)
**Why they switched back:**
Segment split their data pipeline into microservices, which eventually led to:
- A tangled web of interdependencies between services
- Debugging becoming extremely difficult
- New engineers struggling to understand the system

**Outcome:** They consolidated back into a single monolith they called **Centrifuge**,
which was simpler to reason about, easier to debug, and performed better.

---

### 3. 💬 Istio (Google)
**Why they switched back:**
Istio's control plane was originally split into multiple microservices
(Pilot, Citadel, Galley, Mixer). The complexity was a major pain point for users and operators:
- Difficult to deploy and manage
- Hard to troubleshoot across components
- Operational overhead outweighed the benefits

**Outcome:** In version 1.5 (2020), Google merged all control plane components
into a **single monolithic binary** called `istiod`, dramatically simplifying operations.

---

### 4. 🧑‍💻 Stack Overflow
**Why they didn't switch:**
Stack Overflow has famously *resisted* microservices despite massive scale.
They serve millions of requests daily from a relatively small number of servers
using a monolithic architecture, arguing that:
- Their monolith is fast, efficient, and easy to reason about
- Microservices would add overhead without meaningful benefit for their use case

**Outcome:** A reminder that monoliths, done well, can outperform microservices at scale.

---

### 5. 📦 Shopify
**Why they refactored (modular monolith):**
Rather than full microservices, Shopify found that their microservice experiments
introduced too much network latency and operational complexity. They instead moved to a
**"modular monolith"** — a single deployable unit with clear internal boundaries.

**Outcome:** Better performance, simpler deployments, without sacrificing code organization.

---

## Key Lessons Learned

| Problem | What went wrong |
|---|---|
| **Cost** | Data transfer and infrastructure costs explode at scale |
| **Complexity** | Too many services become impossible to debug and trace |
| **Latency** | Network calls between services add up quickly |
| **Team size mismatch** | Microservices shine with large, independent teams |
| **Premature splitting** | Services split too early before domain boundaries are clear |

---

## The Takeaway

> Microservices are not inherently better than monoliths. The right architecture
> depends on your **team size**, **scale**, **domain complexity**, and
> **operational maturity**. Many companies now advocate for starting with a
> **modular monolith** and only extracting services when there is a clear,
> proven need.

**Further Reading:**
- [Amazon Prime Video's microservices reversal (2023)](https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90)
- [Martin Fowler on the Majestic Monolith](https://martinfowler.com/bliki/MonolithFirst.html)
