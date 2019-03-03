The evaluation dataset and evaluation result of SinkFinder. "SinkAPIs.tar.gz" contains security-sensitive API pairs identified by SinkFinder, and "Vulnerabilities.tar.gz" contains the vulnerabilties or bugs we found.

# Sink APIs

SinkFinder is evaluated on three targets: Linux-v4.19, OpenSSL-v1.1.1 and PostgreSQL-v11.1. On the Linux kernel, eight categories of security-sensitive APIs are identified. While on OpenSSL and PostgreSQL, only "Alloc / Free" APIs is identified for vulnerability detection. 

For each target of evaluation, the file frequent-pairs.txt contains frequent API pairs, per line per frequent API pair. The format of a pair is (API1, API2, Relation 1, Relation 2, ...), where API1 and API2 are the two APIs in pair. A relation describes the data flow relationship between the two APIs. A relation is in the format of Rel(i, j, frequency), where i and j are two correlated arguments of API1 and API2, respectively, and frequency is the number of . For example, because kfree uses the return value of kmalloc, kfree is data dependent on kfree. The pair is represented as 
