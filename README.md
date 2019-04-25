# Olap_Benchmarks

## Description


## Required packages

Ubuntu:

	apt-get install python-boto3 python-boto zstd

## Database requirements

The *tpc-ds* benchmark includes commom table expressions like:

	with customer_total_return as
	(select sr_customer_sk as ctr_customer_sk
	,sr_store_sk as ctr_store_sk
	,sum([AGG_FIELD]) as ctr_total_return
	...

Some database engines like MySQL 5.7 do not support these query constructs.


## Ansible command line variables

### DbType

The prefix tells which aws component the benchmark will be using

* prefix of 'ec2' is for all standalone instances, it includes clusters of multiple servers.
	* ec2-pg11
	* ec2-mysql8
	* ec2-spark
	* ec2-clickhouse
	* ec2-mariadbanalytics
	* ec2-presto
	* ec2-pgcolstor
* prefix of 'rds' is for databases built using rds
	* rds-oracle
	* rds-pg11
* prefix of 'redshift' is for database built using redshift
	* redshift

### Benchmark

The type of benchmark to run, possible values are:

* tpcds (default)
* tpch

### BenchSize

The size in GB of the dataset for the benchmark, referred as the scaling factor.

* 1 (testing)
* 100 (default)
* 1024
* 10240
* 102400

### InstanceType 

The type of instance to use.  The value *auto* is a special case where the dataset size is used to guess an appropriate instance type in the r5ad family.

* r5ad.large (default), r5ad.xlarge, r5ad.2xlarge, r5ad.4xlarge, etc. (for ec2)
* i3.xlarge, i3.2xlarge, i3.4xlarge, etc. (for ec2)
* db.m5.large, db.m5.xlarge, db.r5.large, db.x1e.xlarge, etc. (for rds)
* dc2.large, dc2.8xlarge, ds2.xlarge, etc. (redshift)
* auto, determine the instance type based on BenchSize and DbType

### ClusterSize

Number of nodes in the cluster (default is 1), only valid for:

* ec2-spark
* ec2-presto
* redshift

### StorageType

Valid for ec2 and rds:

* gp2 (default), general purpose SSD with 3 iops/GB and a burst capacity for the smaller sizes
* io1, provisioned iops, you specify the iops using a '-' like *io1-10000* for 10k iops. The number of iops must be between 30xGB (min. 1000) and 40k. 
* st1, standard magnetic
* sc1, cold magnetic
* ephemeral, local SSD/NVMe, the instance must be a type with such disks.

### StorageExtra

Type of filesystem:

* xfs (default)
* encrypted
* zfs
* zfsL2arc (the chosen instance type must have a local SSD/NVMe ephemeral disk)

### NRun

Number of benchmark run to complete (default is 1).

