FROM ramonamez/hadoop-base:hadoop3.3.1-java8-python3.10

USER ${NONROOT}

# datanode
HEALTHCHECK CMD curl -f http://localhost:9864/ || exit 1
ENV DATANODE_DIR=${HADOOP_TMP_DIR}/dfs/data
ENV HDFS_CONF_dfs_datanode_data_dir=file://${DATANODE_DIR}
RUN mkdir -p ${DATANODE_DIR}
VOLUME ${DATANODE_DIR}

COPY --chown=${NONROOT}:${NONROOT} run.sh /run.sh
RUN chmod a+x /run.sh
EXPOSE 9864
CMD ["/run.sh"]