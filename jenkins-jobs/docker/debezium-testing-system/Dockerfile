FROM registry.access.redhat.com/ubi8/openjdk-17
USER root
RUN microdnf install git   
RUN microdnf install unzip

ARG MAVEN=3.8.4
ARG OCP_CLIENT=3.7.23

RUN curl --retry 7 -Lo /tmp/client-tools.tar.gz "https://mirror.openshift.com/pub/openshift-v3/clients/${OCP_CLIENT}/linux/oc.tar.gz"
RUN curl --retry 7 -Lo /tmp/maven.tar.gz "https://repo1.maven.org/maven2/org/apache/maven/apache-maven/${MAVEN}/apache-maven-${MAVEN}-bin.tar.gz"

RUN mkdir /opt/maven && tar zxf /tmp/maven.tar.gz -C /opt/maven \
    && rm /tmp/maven.tar.gz 

RUN tar zxf /tmp/client-tools.tar.gz -C /usr/local/bin oc \
    && rm /tmp/client-tools.tar.gz 

ENV MAVEN_HOME "/opt/maven/apache-maven-$MAVEN"
ENV PATH=$MAVEN_HOME/bin:$PATH

RUN microdnf clean all 
COPY testsuite-deployment.sh /testsuite/testsuite-deployment.sh
COPY library.sh /testsuite/library.sh
RUN chmod a+x /testsuite/testsuite-deployment.sh

ENTRYPOINT /testsuite/testsuite-deployment.sh 
