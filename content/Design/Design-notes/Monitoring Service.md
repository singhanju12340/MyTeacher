
Helm
_**Graphana**_
Grafana is an open-source observability platform.
	1. Can visualise  from various `time series data source like prometheus, influx db, Graphite.
	2. visualise logs from Loki and elastic search.
	3. Visualise traces from Jaeger and Zip-kin.

`Comparator
New Relic:It provides an all-in-one APM solution with its own integrated dashboarding and visualisation capabilities. convenient to use but data collection, storage, and visualisation is tightly coupled.

_**Prometheus:**_
Prometheus is an open-source systems monitoring and alerting toolkit, highly popular for matrix collection in Kubernetes and microservice architecture.
* Prometheus focuses on numeric time-series data (metrics) specifically designed for `monitoring trends, rates, and thresholds, which is often more efficient for this purpose than parsing logs for metrics.
* Prometheus scrapes metrics from HTTP endpoints on configured targets.
* it can integrate with remote storage solutions (e.g., Thanos, Cortex, M3DB)

_**PagerDuty:**_ Alert manager


_**Istios**_ It is an open-source, platform-independent `**service mesh**` that provides a transparent way to `manage, secure, and observe network traffic between microservices`. It's most commonly used with Kubernetes.
Advantage : `Resilience4J libraries require you to embed them in your application code` for each service and for each language used. But Istio handles this externally in the `sidecar proxy` and intercept all network traffic into and out of your service.

Other alternative:
Linkerd, consul.

_**datadog**_
Collect matrixes from different agents.
for aws it directly integrates with cloud watch apis and get aws resources matrix from there.





