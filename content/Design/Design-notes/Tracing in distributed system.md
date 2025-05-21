
https://blogs.halodoc.io/log-standardization-best-practices/?ref=dailydev


### 1. For HTTP Calls
![[TrackingExample.png]]

Implement filters to to extract or generate  reqId from ServletRequest.

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