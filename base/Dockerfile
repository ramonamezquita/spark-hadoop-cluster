FROM wxwmatt/hadoop-release:2.1.0-hadoop3.3.1-java8


# --- Base OS dependencies ---
RUN apt update -y && apt upgrade -y && apt-get install -y \ 
    procps \
    wget \
    build-essential \ 
    iputils-ping \ 
    libreadline-gplv2-dev \  
    libncursesw5-dev \  
    libssl-dev \  
    libsqlite3-dev \ 
    tk-dev \ 
    libgdbm-dev \ 
    libc6-dev \ 
    libbz2-dev \ 
    libffi-dev \ 
    liblzma-dev \
    zlib1g-dev \
    vim \
    zip \
    libpq-dev \
    openssh-client


#  --- Install Python3.10 from source and minimal requirements ---
RUN cd /usr/src && \
    wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz && \
    tar xzf Python-3.10.0.tgz && \
    cd Python-3.10.0 && \
    ./configure --enable-optimizations && \
    make install


COPY requirements.txt /root/requirements.txt
RUN pip3 install -r /root/requirements.txt


# --- Run Hadoop entrypoint ---
ENV HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
ADD entrypoint.sh /entrypoint.sh
RUN chmod a+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]