FROM ramonamez/hadoop-base:hadoop3.3.1-java8-python3.10

USER ${NONROOT}

# ssh
COPY --chown=${NONROOT}:${NONROOT} ssh /home/${NONROOT}/.ssh

# jars
COPY --chown=${NONROOT}:${NONROOT} jars ${SPARK_HOME}/jars

# spark
ENV PYSPARK_HADOOP_VERSION=3
RUN pip3 install --user pyspark jupyterlab
ENV SPARK_HOME=${PYTHON_SITE_PACKAGES}/pyspark
ENV SPARK_CONF_DIR=${SPARK_HOME}/conf
RUN mkdir ${SPARK_CONF_DIR}
COPY --chown=${NONROOT}:${NONROOT} conf ${SPARK_CONF_DIR}

# namenode
HEALTHCHECK CMD curl -f http://localhost:9870/ || exit 1
ENV NAMENODE_DIR=${HADOOP_TMP_DIR}/dfs/name
ENV HDFS_CONF_dfs_namenode_name_dir=file://${NAMENODE_DIR}
RUN mkdir -p ${NAMENODE_DIR}
VOLUME ${NAMENODE_DIR}

COPY --chown=${NONROOT}:${NONROOT} run.sh /run.sh
RUN chmod a+x /run.sh
EXPOSE 9870
CMD ["/run.sh"]