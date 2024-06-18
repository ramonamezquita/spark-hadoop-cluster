Containerized Hadoop cluster with Spark.

## Platforms supported
ARM64 (Apple Silicon) and AMD64 (Intel)


# Spark Notes

### After install

  * Installing Spark through Pyspark requires setting ``SPARK_HOME`` 
  and ``SPARK_CONF_DIR`` manually.
  

### Logs

  * Make them be store in hdfs by setting the spark-defaults.conf property to 
  hdfs://namenode:9000/shared/spark-logs 
  (you can do this in spark-defaults.conf).


### Jars

  * For the sake of simplicity, define spark.extraClassPath. 
  Tipically it is ${SPARK_HOME}/jars.


# Redshift Notes

### How to connect?


  1.  Download all jar files provided by Amazon. 
      Amazon provides a ZIP file containing every needed jar at 
      https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-install.html
  
  2. You'll probably still needed to install more, for example:
    - aws-java-sdk-s3-1.12.741.jar
    - aws-java-sdk-dynamodb-1.12.741.jar

  3.  Also add the following propertes at core-site.xml (I am not sure if these are really necessary)

    ```
    <property><name>fs.s3.impl</name><value>org.apache.hadoop.fs.s3a.S3AFileSystem</value></property>
    <property><name>fs.s3a.impl</name><value>org.apache.hadoop.fs.s3a.S3AFileSystem</value></property>
    <property><name>fs.s3a.aws.credentials.provider</name><value>com.amazonaws.auth.DefaultAWSCredentialsProviderChain</value></property>
    <property><name>fs.s3.aws.credentials.provider</name><value>com.amazonaws.auth.DefaultAWSCredentialsProviderChain</value></property>
    ```

