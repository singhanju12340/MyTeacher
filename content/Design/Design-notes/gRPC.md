
Google Remote Procedure Calls
- **HTTP/2-based** → Multiplexing, streaming, lower latency.
- **Supports 4 communication modes**:
    - Unary (like REST).
    - Server streaming.
    - Client streaming.
    - Bidirectional streaming.

**Cons**:

- **Not human-readable** (requires tooling for debugging).
- **Limited browser support** (needs gRPC-Web).
- **Steeper learning curve** (vs. REST).

**When to use?**
- Internal microservices (e.g., finance, gaming, IoT).
- Real-time systems (chat, live updates).
- Performance-critical workloads (e.g., stock trading).
1. **Architecture scenarios**:
    - _“Design a chat app”_ → Use gRPC for **bidirectional streaming**.
    - _“Build a public weather API”_ → REST for **caching and wide access**.


- _“What are REST’s advantages for public APIs?”_ → Emphasize **simplicity, caching, and compatibility**.
- _“I learned that gRPC’s codegen reduces bugs, but debugging requires tools like Wireshark.”_

