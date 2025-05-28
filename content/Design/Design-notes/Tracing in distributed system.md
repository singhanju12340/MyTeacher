
https://blogs.halodoc.io/log-standardization-best-practices/?ref=dailydev


Log structure: 
_[{Timestamp}] - [{Parent-Transaction-Id} - {Transaction-Id} - {Request-Id}] - [{Referer}] - [{Message}]_

_By implementing Request ID, Transaction ID, and Parent Transaction ID, we have established a structured logging approach that ensures end-to-end traceability in distributed systems.
_**Using this log patterns:
1.  Correlate logs across multiple services using the Request ID.
2. Trace logs of the upstream service by correlating entries using the downstream Parent Transaction ID as the Transaction ID
3. Find all logs related to a specific service using the Transaction ID
4.  Consistent log correlation in concurrent workflows with MDCAwareCompletableFuture & MDCAwareExecutorService, eliminating context loss in multi-threaded executions.

### 1. For HTTP Calls
![[TrackingExample.png]]


1. **HTTP Request Interceptor:** Intercepts HTTP requests, extracts existing trace identifiers, or generates new ones, and injects them into the request context.
2. **Outgoing API Calls with Trace Context:** Outgoing requests must carry the trace headers for consistent logging and better traceability across services.
3.  **Kafka Producer - Context Propagation:** When publishing Kafka messages, trace identifiers must be included in headers to ensure logs remain consistent and traceable across services.
4. **Kafka Consumer - Extracting and Setting Context:** Consumers must extract trace identifiers from the Kafka message headers and initialize the logging context to maintain traceability across events.

```Java
Class RequestTrackingHelper implements Filter{

	@Override
	public void doFilter(ServletRequest request, ServlerResponse response, FilterChain chain){

		String requestId = extractOrGenerate((HttpSErvletRequest)request);
		MDC.put("REQUEST_ID", requestId);
		MDC.put("PARENT_ID", MDC.get("TRANSACTION_ID"));
		MDC.put("TRANSACTION_ID", UUID generate id);
		chain.doFilter();
		MDC.clear();
	}
}
```

### For Kafka Operations

![[Screenshot 2025-05-21 at 3.52.27 PM.png]]

### For Concurrent Operations

![[Screenshot 2025-05-21 at 3.53.37 PM.png]]