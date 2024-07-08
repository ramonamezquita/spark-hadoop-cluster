FROM ramonamez/hadoop-base:hadoop3.3.1-java8-python3.10

USER ${NONROOT}

HEALTHCHECK CMD curl -f http://localhost:8042/ || exit 1
COPY --chown=${NONROOT}:${NONROOT} run.sh /run.sh
RUN chmod a+x /run.sh
EXPOSE 8042
CMD ["/run.sh"]