The evaluation dataset and evaluation result of SinkFinder. "`SinkAPIs.tar.gz`" contains security-sensitive API pairs identified by **`SinkFinder`**, and "`Vulnerabilities.tar.gz`" contains the vulnerabilties or bugs we found.

# Sink APIs

`SinkFinder` is evaluated on three targets: `Linux`-v4.19, `OpenSSL`-v1.1.1 and `PostgreSQL`-v11.1. On the Linux kernel, eight categories of security-sensitive APIs are identified. While on `OpenSSL` and `PostgreSQL`, only "`Alloc` / `Free`" APIs is identified for vulnerability detection. 

For each target of evaluation, the file `frequent-pairs.txt` contains frequent API pairs, per line per frequent API pair. 
A pair is in the format of (`API1`, `API2`, `Relation 1`, `Relation 2`, ...), where `API1` and `API2` are the two APIs in pair, and a relation describes the data flow relationship between the two APIs. There may be multiple relations between two APIs. Currently, `SinkFinder` only supports two types of relations, **data dependent** (`DFATA_DEP`) and **data sharing** (`DATA_SHARE`). We use two frequent pairs mined from the Linux-v4.19 to illustrate this.

> (1) (`kmalloc` `kfree` `DATA_DEP`(0, 1, 2371)): APIs `kmalloc` and `kfree` frequently appear together, there are 2,371 instances that the return value of `kmalloc` is passed to `kfree`. We take the return value of an API as a special argument, and use 0 to represent it. We use _i_ (_i_ > 1) to indicate the ith argument. <br>
> (2) (`mutex_lock_nested` `mutex_unlock` `DATA_SHARE`(1, 1, 14874)): APIs `mutex_lock_nested` and `mutex_unlock` compose a frequent pair, there are 14,874 instances that there first arguments use the same data value.

For each category of security-sensitive API pairs, there are five files. <br>
* **seed_pair.txt**: the file contains the seed pair. That's, the prior knowledge. At least one pair of security-sensitive APIs is required. <br>
* **first_stage_positive.txt**: the file stores the reliable positive pairs (i.e., the security-sensitive ones) identified in the first stage.<br>
* **first_stage_negative.txt**: the file stores the reliable negative pairs (i.e., the non-security-sensitive ones) identified in the first stage.<br>
* **second_stage_positive.txt**: the file stores the positive pairs identified in the second stage.<br>
* **both_stages_positive.txt**: the file stores all the positive pairs identified by `SinkFinder`, i.e., a union of the positive result of the first stage and the second stage.<br>

The above files have the same format. 
> **Format**: <tag, pair>, the tag indicates whether we believe the pair is security-sensitive ('1'), non-security-sensitive ('0'), or not sure ('X'). And the pair is similar to those frequent pairs, except that the frequency is stripped. <br>
> **An example**: 1 kmalloc kfree DATA_FROM(0, 1)

`SinkFinder` originally generates a tag of 'X' for each identified pair. During manual inspection, if we believe the pair is positive, we mark it as '1', otherwise we mark it as '0'. A pair marked with '0' in a _\_positive.txt_ file can be regarded as a false positive of our tool. 



# Vulnerabilities

The vulnerabilities detected by our **use-after-free** checker and **mismatched-alloc-free** checker. It takes the allocation / deallocation APIs (located at AllocFree/both_stages_positive.txt) identified by our method to detect vulnerabilities. It should be noted that the two checkers directly takes the original output of SinkFinder to detect vulnerabilities. Therefore, our method is **completely automatically** from identifying security-sensitive APIs to detecting vulnerabilities. <br>
* **Vulnerabilities/Linux** contains the patches for the Linux kernel vulnerabilities
* **Vulnerabilities/OpenSSL** contains the link to our report of the vulnerability we found in OpenSSL
* **Vulnerabilities/PostgreSQL** contains the links to our reports of the vulnerabilities we found in PostgreSQL
