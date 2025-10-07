
## **Memory issue while processing long file:**

XL size adfter download become more.
service memory with 700MB was crasing 3GB memory machine for HHFL(HDFC home finance)
used duckDB to download and process xl data and directly upload to s3 via streams from duck db.
For GCP direct stream to GCS from duckDB not there. so used extra mounted memory to copy data from duck db and upload to GCS.
openCSV lib provided automated enchanced chunking and parallel processing on csv data so we convertted xl to csv and did processing.




**Add and Update issue?**




## Custom matrix issue?
No of max loan allocationID
allocation record Id: around 30 thousands in 1 hr
No of record in XL file of loan:

Why can not handle high cardinality data? but pinot can?

#### Prometheus:
Designed for a **pull-based monitoring model** and **real-time alerting** on a manageable number of metrics. Its strength is in the robust PromQL (Prometheus Query Language) and its ability to quickly alert on predefined conditions. It's not built for arbitrary, exploratory analytics on dimensions with a huge number of unique values.

####  Apache Pinot: 
Designed as a **real-time OLAP database** for **user-facing analytics**. It's built from the ground up to handle complex analytical queries, including slice-and-dice operations and drill-downs on multi-dimensional data. 
Its rich indexing and distributed architecture make it capable of handling the very high cardinality that would overwhelm Prometheus.
Ex:
It excels at answering questions like "What are the top 100 products by sales in the last hour, broken down by country and device type?" even with millions of unique products.


