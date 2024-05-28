FROM ubuntu:latest

LABEL maintainer="Malik Chettih <malik.chettih.ia@gmail.com>"

WORKDIR /root

# Setup bash shell 
SHELL ["/bin/bash", "-c"]

# Copy Image configuration files
COPY config/* /tmp/

# Update Ubuntu Distribution    
RUN apt-get update 

# Install Zscaler certificates
RUN apt-get install -y ca-certificates
RUN mv /tmp/*.crt /usr/local/share/ca-certificates/ && \
    chmod 644 /usr/local/share/ca-certificates/Zscaler*.crt
RUN update-ca-certificates    

# Install aditional Ubuntu components
RUN apt-get install -y openssh-server openjdk-8-jdk ssh wget curl vim python3 && \ 
    rm -rf /var/lib/apt/lists/* 
    
# install hadoop 3.3.6
RUN wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz &&  \
    tar -xf hadoop-3.3.6.tar.gz && \
    mv hadoop-3.3.6 /usr/local/hadoop && \
    rm hadoop-3.3.6.tar.gz 


# set environment variables
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
ENV HADOOP_HOME=/usr/local/hadoop
ENV HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
ENV LD_LIBRARY_PATH=/usr/local/hadoop/lib/native:$LD_LIBRARY_PATH
ENV PATH=$PATH:/bin:/usr/sbin:/usr/local/hadoop/bin:/usr/local/hadoop/sbin

# Setup passphraseless ssh
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && \
    chmod 0600 ~/.ssh/authorized_keys

# Setup Haddop directories
RUN mkdir -p ~/hdfs/namenode &&  \   
    mkdir -p ~/hdfs/datanode && \
    mkdir $HADOOP_HOME/logs # buildkit 


# Dispatch Configuration files into the container
RUN mv /tmp/ssh_config ~/.ssh/config && \  
    mv /tmp/hadoop-env.sh /usr/local/hadoop/etc/hadoop/hadoop-env.sh &&  \   
    mv /tmp/hdfs-site.xml $HADOOP_HOME/etc/hadoop/hdfs-site.xml &&  \   
    mv /tmp/core-site.xml $HADOOP_HOME/etc/hadoop/core-site.xml && \    
    mv /tmp/mapred-site.xml $HADOOP_HOME/etc/hadoop/mapred-site.xml && \    
    mv /tmp/yarn-site.xml $HADOOP_HOME/etc/hadoop/yarn-site.xml && \    
    mv /tmp/workers $HADOOP_HOME/etc/hadoop/workers && \
    mv /tmp/start-hadoop.sh ~/start-hadoop.sh

RUN chmod +x ~/start-hadoop.sh &&  \   
    chmod +x $HADOOP_HOME/sbin/start-dfs.sh &&  \   
    chmod +x $HADOOP_HOME/sbin/start-yarn.sh


# Format the filesystem
RUN /usr/local/hadoop/bin/hdfs namenode -format

#CMD ["service ssh start"]



