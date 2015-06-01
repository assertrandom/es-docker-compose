FROM centos:centos7
MAINTAINER mlk

RUN yum -y install openssh-server epel-release && \
    yum -y install pwgen && \
    rm -f /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_ecdsa_key && \
    ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key && \
    sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config

RUN yum -y update

RUN yum -y install curl tar openssh-server openssh-clients wget python-pip  net-tools wget unzip

RUN (curl -L -k -b "oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u45-b14/server-jre-8u45-linux-x64.tar.gz | gunzip -c | tar x) \
&& mv /jdk1.8.0_45 /jdk

ENV JAVA_HOME /jdk
ENV JRE_HOME  $JAVA_HOME/jre
ENV PATH $PATH:$JAVA_HOME/bin

WORKDIR /opt/

RUN wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.5.2.tar.gz -O elasticsearch-1.5.2.tar.gz

RUN tar -xzf elasticsearch-1.5.2.tar.gz

RUN mv elasticsearch-1.5.2 elasticsearch

RUN chmod 777 elasticsearch

EXPOSE 9200 9300 8080

ENV PATH /opt/elasticsearch/bin:$PATH

RUN plugin -i elasticsearch/marvel/latest

RUN plugin -install mobz/elasticsearch-head

ADD run.sh /run.sh

RUN chmod 777 /run.sh

CMD ["elasticsearch"]



#  docker build -t es_master .

#  docker run -d -P -p 9200:9200 -p 9300:9300  es_master
