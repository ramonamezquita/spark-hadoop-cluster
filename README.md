Containerized Hadoop cluster with Spark.

## Platforms supported
ARM64 (Apple Silicon) and AMD64 (Intel)


## Run locally
Just docker-compose up the services with

```bash
docker-compose up
```

This command will automatically pull and build the needed images 🐳.

## Run remotely
A full guided tutorial on running all servives in mulitple hosts using AWS EC2
instances through docker-swarm can be found here (not ready yet).

## Verify installation

You should be able to run any python script inside ``SPARK_HOME``/examples 
either in local or cluster mode. For instance,

```bash
cd $SPARK_HOME
spark-submit --master yarn --deploy-mode cluster examples/src/main/python/pi.py
```

> [!IMPORTANT] 
> In cluster mode, python dependencies must be installed in all nodes!


## ⚠️ Important Variables

Installing Spark through Pyspark with pip requires setting and creating 
``SPARK_CONF_DIR`` manually. Adittionally,  Spark logs are stored in hdfs and 
the jars class  path is set to  ``SPARK_HOME``/jars. Both of these are 
defined in ``spark-defaults.conf`` through the ``spark.history.fs.logDirectory`` 
and ``spark.driver.extraClassPath`` properties, respectively.