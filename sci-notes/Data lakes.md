Data lakes use object storage instead of file or block storage. This allows *massive* amounts of data to be stored of all types (IoT, operational logs, raw data, real-time, relational and non-relational) **and at a lower cost**. 

Data lakes are typically massive in size as they store all data that _might_ be used. 

Data lakes are schema-on-read rather than schema-on-write.

If a data lake does not get catalogued properly, it can quickly be termed a data swamp. 

#### Analytics
Typically, big data analytics are run on data lakes using tools such as [[Apache Spark]] or [[Hadoop]].

#### Loading Data
An [[ELT]] workflow is usually used to load the data into a data lake.

