FROM ramonamez/hadoop-base:hadoop3.3.1-java8-python3.10

USER ${NONROOT}

# history server
HEALTHCHECK CMD curl -f http://localhost:8188/ || exit 1
ENV YARN_TIMELINE_DIR=${HADOOP_TMP_DIR}/yarn/timeline
ENV YARN_CONF_yarn_timeline___service_leveldb___timeline___store_path=${YARN_TIMELINE_DIR}
RUN mkdir -p ${YARN_TIMELINE_DIR}
VOLUME ${YARN_TIMELINE_DIR}

COPY --chown=${NONROOT}:${NONROOT} run.sh /run.sh
RUN chmod a+x /run.sh
EXPOSE 8188
CMD ["/run.sh"]