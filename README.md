Containerized Hadoop cluster with Spark.

## Platforms supported
ARM64 (Apple Silicon) and AMD64 (Intel)


## Spark Considerations

### Pyspark installation

  * Installing Spark through Pyspark requires setting ``SPARK_HOME`` 
  and ``SPARK_CONF_DIR`` manually. These are set to the location of the 
  pyspark installation directory (**/usr/local/lib/python3.10/site-packages/pyspark/**).


### Logs

  * Logs are stored in hdfs by setting the ``spark.history.fs.logDirectory`` property to 
  **hdfs://namenode:9000/shared/spark-logs** in ``spark-defaults.conf``.


### Jars

  * ``spark.extraClassPath`` is set to  ``SPARK_HOME``/jars.
  


## Redshift Connection

### How to connect?


  1.  Download all jar files provided by Amazon. 
      Amazon provides a ZIP file containing every needed jar at 
      https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-install.html
  
  2. You'll probably still needed to install more, for example:
        * aws-java-sdk-s3-1.12.741.jar
        * aws-java-sdk-dynamodb-1.12.741.jar

  3.  Also add the following propertes at core-site.xml (I am not sure if these are really necessary)

    <property><name>fs.s3.impl</name><value>org.apache.hadoop.fs.s3a.S3AFileSystem</value></property>
    <property><name>fs.s3a.impl</name><value>org.apache.hadoop.fs.s3a.S3AFileSystem</value></property>
    <property><name>fs.s3a.aws.credentials.provider</name><value>com.amazonaws.auth.DefaultAWSCredentialsProviderChain</value></property>
    <property><name>fs.s3.aws.credentials.provider</name><value>com.amazonaws.auth.DefaultAWSCredentialsProviderChain</value></property>


