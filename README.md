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

---

## Companies That Moved Away from Microservices Back to Monolith

Several high-profile companies have publicly documented their decision to reverse course
from microservices back to a monolithic (or "majestic monolith") architecture.



### 1. Amazon Prime Video (2023)
**Why they switched back:**
Their audio/video monitoring service was built as microservices using AWS Step Functions
and Lambda. The distributed architecture created two core problems:
- High costs from data passing between services
- Scalability bottlenecks at scale

**Outcome:** By consolidating into a monolith, they reduced infrastructure costs by **90%**
and improved scalability. Ironically, this came from the company that sells the cloud
infrastructure most microservices run on.



### 2. Segment (2020)
**Why they switched back:**
Segment split their data pipeline into microservices, which eventually led to:
- A tangled web of interdependencies between services
- Debugging becoming extremely difficult
- New engineers struggling to understand the system

**Outcome:** They consolidated back into a single monolith they called **Centrifuge**,
which was simpler to reason about, easier to debug, and performed better.

---

### 3. Istio (Google)
**Why they switched back:**
Istio's control plane was originally split into multiple microservices
(Pilot, Citadel, Galley, Mixer). The complexity was a major pain point for users and operators:
- Difficult to deploy and manage
- Hard to troubleshoot across components
- Operational overhead outweighed the benefits

**Outcome:** In version 1.5 (2020), Google merged all control plane components
into a **single monolithic binary** called `istiod`, dramatically simplifying operations.

---

### 4. Stack Overflow
**Why they didn't switch:**
Stack Overflow has famously *resisted* microservices despite massive scale.
They serve millions of requests daily from a relatively small number of servers
using a monolithic architecture, arguing that:
- Their monolith is fast, efficient, and easy to reason about
- Microservices would add overhead without meaningful benefit for their use case

**Outcome:** A reminder that monoliths, done well, can outperform microservices at scale.

---

### 5. Shopify
**Why they refactored (modular monolith):**
Rather than full microservices, Shopify found that their microservice experiments
introduced too much network latency and operational complexity. They instead moved to a
**"modular monolith"** — a single deployable unit with clear internal boundaries.

**Outcome:** Better performance, simpler deployments, without sacrificing code organization.



## Key Lessons Learned

| Problem | What went wrong |
|---|---|
| **Cost** | Data transfer and infrastructure costs explode at scale |
| **Complexity** | Too many services become impossible to debug and trace |
| **Latency** | Network calls between services add up quickly |
| **Team size mismatch** | Microservices shine with large, independent teams |
| **Premature splitting** | Services split too early before domain boundaries are clear |



## Key Takeaway

> Microservices are not inherently better than monoliths. The right architecture
> depends on your **team size**, **scale**, **domain complexity**, and
> **operational maturity**. Many companies now advocate for starting with a
> **modular monolith** and only extracting services when there is a clear,
> proven need.

## Other Companies Using Microservices


### 1.  Google
**How they use it:**
Google runs some of the largest distributed systems in the world.
- Services like Search, Maps, Gmail, and YouTube all operate as independent systems
- They developed **Kubernetes** internally (originally called Borg) to manage containers
  at scale — now the industry standard for microservices orchestration
- Also created **gRPC**, a high-performance communication protocol widely used
  between microservices

---

### 2. Uber
**How they use it:**
Uber transitioned from a monolith to microservices as they expanded globally.
- Separate services for driver matching, pricing (surge), payments, notifications,
  and trip management
- At peak scale they ran **thousands of microservices**
- Eventually faced challenges managing too many services and introduced
  **domain-oriented microservices (DOMA)** to bring structure back


### 3. Spotify
**How they use it:**
Spotify embraced microservices early and also pioneered a famous team structure to go with it.
- Organized around **Squads, Tribes, Chapters, and Guilds** — a model many companies copied
- Each squad owns its own microservice end-to-end
- Services cover recommendations, playlist management, search, social features, and streaming
- Built **Backstage**, an open-source developer portal for managing microservices,
  now widely adopted in the industry


### 4. PayPal
**How they use it:**
PayPal migrated from a monolith to microservices to handle growing transaction volumes.
- Decomposed their payments platform into independent services
- Improved deployment frequency and reduced time-to-market for new features
- Uses Node.js heavily across their microservices layer


### 5. eBay
**How they use it:**
eBay has been evolving its architecture since the early 2000s.
- Moved from a Perl monolith → Java monolith → microservices over decades
- Each product domain (search, checkout, listings, recommendations) is a separate service
- Strong focus on **event-driven architecture** using Kafka for service communication


### 6. Airbnb
**How they use it:**
Airbnb adopted microservices to scale their platform globally.
- Split their monolith (nicknamed **"the Monorail"**) into services covering
  bookings, payments, messaging, search, and reviews
- Faced significant challenges with **data consistency** and **service sprawl**
- Built internal tooling like **Chronos** (job scheduler) to manage complexity
- Later moved toward a **service-oriented architecture with stronger domain boundaries**


### 7. Twitter / X
**How they use it:**
Twitter decomposed their Ruby on Rails monolith (nicknamed **"the Fail Whale" era**) 
into microservices after repeated outages.
- Core services include timeline generation, tweet ingestion, search, notifications,
  and direct messages
- Built **Finagle**, an open-source RPC framework for inter-service communication
- Timeline fanout (delivering a tweet to all followers) is a classic microservices
  architecture case study


### 8. Walmart
**How they use it:**
Walmart re-platformed their entire e-commerce infrastructure to microservices.
- Handled **Black Friday traffic spikes** more reliably after the migration
- Moved away from expensive proprietary systems to open-source microservices on the cloud
- Reported significant cost savings and improved deployment speed after the transition


### 9. Capital One
**How they use it:**
Capital One is one of the most prominent financial institutions to embrace microservices
and cloud-native architecture.
- Moved entirely off data centers onto AWS using microservices
- Each banking domain (credit cards, accounts, fraud detection) operates independently
- Strong investment in **DevSecOps** — security built into every microservice pipeline



## Summary Table

| Company | Industry | Key Contribution |
|---|---|---|
| Google | Tech | Kubernetes, gRPC |
| Uber | Ride-sharing | DOMA framework for managing service sprawl |
| Spotify | Music Streaming | Squad model, open-sourced Backstage |
| PayPal | Fintech | Node.js-based microservices at scale |
| eBay | E-Commerce | Event-driven architecture with Kafka |
| Airbnb | Travel | Managed monolith-to-microservices migration |
| Twitter/X | Social Media | Finagle RPC, timeline fanout case study |
| Walmart | Retail | Black Friday scaling, cost reduction |
| Capital One | Finance / Banking | Cloud-native banking, DevSecOps |


> **Note:** Most of these companies did not have a smooth journey.
> Each faced unique challenges around **data consistency, service sprawl,
> latency, and operational complexity** — and invested heavily in
> internal tooling to manage it. Microservices work best when
> paired with a strong **DevOps culture, CI/CD pipelines,
> and distributed tracing infrastructure**.

