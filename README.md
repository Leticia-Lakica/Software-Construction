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