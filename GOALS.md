# Goals

* KV store, with suport for column families
* Single digit latency when run on SSDs, hopefully lower when read from memtable or NVMe
* Built on top of seastar
* Built in metrics reporter
* Ability to run multiple instances on the same node
* Extendable interface for easily adding more features
* Optimized for SSDs
* Optimized for NVMe
