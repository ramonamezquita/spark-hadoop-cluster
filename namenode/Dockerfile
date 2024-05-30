FROM ramonamez/hadoop-base:hadoop3.3.1-java8-python3.10


RUN pip3 install wheel
RUN pip3 install setuptools
RUN pip3 install py4j
RUN pip3 install pyspark
RUN pip3 install numpy


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
      libxslt1.1\
      gnuplot


HEALTHCHECK CMD curl -f http://localhost:9870/ || exit 1

ENV HDFS_CONF_dfs_namenode_name_dir=file:///hadoop/dfs/name
RUN mkdir -p /hadoop/dfs/name
VOLUME /hadoop/dfs/name

ADD run.sh /run.sh
RUN chmod a+x /run.sh

EXPOSE 9870

CMD ["/run.sh"]