FROM wxwmatt/hadoop-release:2.1.0-hadoop3.3.1-java8

ARG nonroot=hadoopuser

# HADOOP_TMP_DIR is Hadoop's temporary directory, it's a local directory (non-HDFS).
# Different processes/services use subfolders of HADOOP_TMP_DIR for their temporary data.
ENV HADOOP_TMP_DIR=/tmp/hadoop
RUN mkdir ${HADOOP_TMP_DIR}

# nonroot user
ENV NONROOT=$nonroot
RUN adduser --system --group ${NONROOT}
RUN chown -R hadoopuser:hadoopuser ${HADOOP_CONF_DIR}
RUN chown -R hadoopuser:hadoopuser ${HADOOP_HOME}
RUN chown -R hadoopuser:hadoopuser ${HADOOP_TMP_DIR}

# os
COPY packages.txt packages.txt
RUN apt update -y && apt upgrade -y
RUN xargs apt-get -y install < packages.txt

# python
RUN cd /usr/src && \
    wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz && \
    tar xzf Python-3.10.0.tgz && \
    cd Python-3.10.0 && \
    ./configure --enable-optimizations && \
    make install
ENV PYTHON_SITE_PACKAGES=/home/hadoopuser/.local/lib/python3.10/site-packages


# python requirements
USER ${NONROOT}
WORKDIR /home/${NONROOT}
COPY  --chown=${NONROOT}:${NONROOT} requirements.txt requirements.txt
RUN pip3 install --upgrade pip
RUN pip3 install -r requirements.txt
ENV PATH=/home/${NONROOT}/.local/bin:${PATH}


ENV HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
COPY --chown=${NONROOT}:${NONROOT} entrypoint.sh /entrypoint.sh
RUN chmod a+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]