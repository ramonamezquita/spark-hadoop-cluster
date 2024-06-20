Containerized Hadoop cluster with Spark.

## Platforms supported
ARM64 (Apple Silicon) and AMD64 (Intel)


## Spark Conf

Installing Spark through Pyspark requires setting ``SPARK_HOME`` 
and ``SPARK_CONF_DIR`` manually. These are set to the location of the 
pyspark installation directory (**/usr/local/lib/python3.10/site-packages/pyspark/**).


### Logs

Logs are stored in hdfs by setting the ``spark.history.fs.logDirectory`` property to 
**hdfs://namenode:9000/shared/spark-logs** in ``spark-defaults.conf``.


### Jars

``spark.extraClassPath`` is set to  ``SPARK_HOME``/jars.


### Verify installation

You should be able to run any python script inside ``SPARK_HOME``/examples 
either in local or cluster mode. For instance,

```bash
cd $SPARK_HOME
spark-submit --master yarn --deploy-mode cluster examples/src/main/python/pi.py
```

> [!IMPORTANT] 
> In cluster mode, python dependencies must be installed in all nodes!


## Redshift Connection

### How to connect?


1. Download all jar files provided by Amazon. 
Amazon provides a ZIP file containing every needed jar at 
https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-install.html.
Just download and uzip, that is

```
cd $SPARK_HOME/jars
wget https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/2.1.0.29/redshift-jdbc42-2.1.0.29.zip
unzip redshift-jdbc42-2.1.0.29.zip
rm redshift-jdbc42-2.1.0.29.zip
```

2.  Use the [RedshiftData Source for Apache Spark - Community Edition](https://github.com/spark-redshift-community/spark-redshift?tab=readme-ov-file)  .
    

* 2.1 Init Spark with the ``spark.jars.packages`` configuration set to
``com.amazon.redshift:redshift-jdbc42:2.1.0.24,org.apache.spark:spark-avro_2.12:3.5.0,io.github.spark-redshift-community:spark-redshift_2.12:6.2.0-spark_3.5``. In PySpark,

```python
spark = (
    SparkSession
    .builder
    .config('spark.jars.packages', 'com.amazon.redshift:redshift-jdbc42:2.1.0.24,org.apache.spark:spark-avro_2.12:3.5.0,io.github.spark-redshift-community:spark-redshift_2.12:6.2.0-spark_3.5')
    .getOrCreate()
)
```

* 2.2 Set the ``format`` to ``io.github.spark_redshift_community.spark.redshift``. In PySpark,
        
```python
df = spark.read \
    .format("io.github.spark_redshift_community.spark.redshift") \
    .option("url", "jdbc:redshift://redshifthost:5439/database?user=username&password=pass") \
    .option("query", "select x, count(*) my_table group by x") \
    .option("tempdir", "s3://path/for/temp/data") \
    .load()
```

* 2.3 Since this datasource unloads data to S3 as an intermediate step, 
authentication is something to keep in mind. Different authentication methods between Spark and Redshift are datailed here:
[Authenticating with Amazon Redshift integration for Apache Spark](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark-redshift-auth.html)

> [!IMPORTANT] 
> In cluster mode, AWS credentials must be set for all containers! This can be 
> achieved by installing ``aws-cli`` on every container and doing 
> ``aws configure``.


3. You'll probably still need to install more jars for example:
* aws-java-sdk-s3-1.12.741.jar
* aws-java-sdk-dynamodb-1.12.741.jar

4.  Also add the following propertes at core-site.xml (I am not sure if these are really necessary)

```bash
<property><name>fs.s3.impl</name><value>org.apache.hadoop.fs.s3a.S3AFileSystem</value></property>
<property><name>fs.s3a.impl</name><value>org.apache.hadoop.fs.s3a.S3AFileSystem</value></property>
<property><name>fs.s3a.aws.credentials.provider</name><value>com.amazonaws.auth.DefaultAWSCredentialsProviderChain</value></property>
<property><name>fs.s3.aws.credentials.provider</name><value>com.amazonaws.auth.DefaultAWSCredentialsProviderChain</value></property>
```


