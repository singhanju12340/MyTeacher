---
Creation Time: Monday, August 5th 2024
Modified Time: Wednesday, January 22nd 2025
---
- **How It Works**:
    - The client sends periodic requests to the server to fetch updates.
- **Use Case**:
    - Simple or legacy systems where real-time updates aren’t strictly required.
- **Advantages**:
    - Easy to implement.
    - Reliable and works in all environments.
- **Disadvantages**:
    - Inefficient and increases server load due to frequent requests.
    - Delayed updates depending on polling frequency.


### **HTTP Long Polling**

- **How It Works**:
    - The client sends an HTTP request to the server and waits for a response.
    - The server holds the request until there’s new data to send or a timeout occurs.
    - After the response, the client immediately sends another request to keep the connection alive.
- **Use Case**:
    - When WebSockets or other real-time protocols are unavailable due to infrastructure limitations or legacy system constraints.
- **Advantages**:
    - Works with standard HTTP, no special libraries or protocols needed.
    - Reliable in environments where WebSockets might not be supported.
- **Disadvantages**:
    - Increased latency compared to WebSockets or SSE.
    - Inefficient due to repeated HTTP request overhead.