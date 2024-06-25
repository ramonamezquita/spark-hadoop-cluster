FROM ramonamez/hadoop-base:hadoop3.3.1-java8-python3.10


# --- More OS dependencies ---
RUN apt-get update

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
      gcc \
      git-core \
      libffi-dev

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
      libjpeg-dev \
      libpcre3 \
      libpcre3-dev \
      autoconf \
      libtool \
      pkg-config\
      zlib1g-dev \
      libssl-dev \
      libexpat1-dev \
      libxslt1.1 \
      gnuplot


#  --- Install Spark ---
# In the pip installation environment:
#  - $SPARK_HOME points to the Python installation directory.
#  - $SPARK_HOME/conf directory needs to be created manually.
ENV PYSPARK_HADOOP_VERSION=3
RUN pip3 install pyspark
ENV SPARK_HOME=/usr/local/lib/python3.10/site-packages/pyspark
ENV SPARK_CONF_DIR=${SPARK_HOME}/conf
RUN mkdir ${SPARK_CONF_DIR}
COPY conf ${SPARK_CONF_DIR}

# --- Run Hadoop entrypoint ---
RUN hdfs dfs -mkdir -p /shared/spark-logs
HEALTHCHECK CMD curl -f http://localhost:9870/ || exit 1
ENV HDFS_CONF_dfs_namenode_name_dir=file:///hadoop/dfs/name
RUN mkdir -p /hadoop/dfs/name
VOLUME /hadoop/dfs/name
ADD run.sh /run.sh
RUN chmod a+x /run.sh
EXPOSE 9870
CMD ["/run.sh"]