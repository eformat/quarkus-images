###
# Quakus *native* S2I builder
###
FROM registry.access.redhat.com/ubi8/ubi-minimal:latest

ARG GRAAL_VERSION=19.1.1
ARG MAVEN_VERSION=3.6.1

ENV GRAAL_CE_URL=https://github.com/oracle/graal/releases/download/vm-${GRAAL_VERSION}/graalvm-ce-linux-amd64-${GRAAL_VERSION}.tar.gz
ARG MAVEN_URL=https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz
ARG USER_HOME_DIR="/home/quarkus"
ARG MAVEN_BASE_URL=https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
ENV GRAALVM_HOME /opt/graalvm
ENV JAVA_HOME /opt/graalvm
ENV QUARKUS_HOME=/home/quarkus
ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"

ENV PATH=$PATH:$JAVA_HOME/bin

RUN microdnf -y update && \
  	 microdnf install -y tar gzip gcc glibc-devel zlib-devel curl shadow-utils && \     
     ### Installation of Maven
     mkdir -p ${MAVEN_HOME} ${MAVEN_HOME}/ref && \
     curl -fsSL -o /tmp/apache-maven.tar.gz ${MAVEN_URL}  && \
     tar -xzf /tmp/apache-maven.tar.gz -C ${MAVEN_HOME} --strip-components=1 && \
     ln -s ${MAVEN_HOME}/bin/mvn /usr/bin/mvn && \
     ### Installation of GraalVM
     mkdir -p ${GRAALVM_HOME} && \
     cd ${GRAALVM_HOME} && \
     curl -fsSL $GRAAL_CE_URL | tar -xzC ${GRAALVM_HOME} --strip-components=1 && \
#COPY native-image-installable-svm-linux-amd64-19.0.2.jar ${GRAALVM_HOME}/native-image-installable-svm-linux-amd64-19.0.2.jar
#RUN     /opt/graalvm/bin/gu -L install ${GRAALVM_HOME}/native-image-installable-svm-linux-amd64-19.0.2.jar && rm -f ${GRAALVM_HOME}/native-image-installable-svm-linux-amd64-19.0.2.jar
    /opt/graalvm/bin/gu install native-image && \
     ### User management
     adduser -u 1001 -g 0 quarkus --home-dir ${QUARKUS_HOME} && \
     chown -R 1001:0 ${QUARKUS_HOME} && \
     chmod -R ug+rwX ${QUARKUS_HOME}  && \
     ### Cleanup     
     microdnf clean all && \
     rm -f /tmp/apache-maven.tar.gz  /tmp/graalvm-ce-amd64.tar.gz && \
     rm -rf /var/cache/yum && \
     mkdir -p /project
     ###

# This part is firmly inspired by fabric8io-images/s2i
LABEL io.openshift.s2i.scripts-url="image:///usr/local/s2i" \
      io.openshift.s2i.destination="/tmp" \
      io.k8s.description="Quarkus.io S2I image for building Kubernetes Native Java GraalVM applications and running its Native Executables" \
      io.k8s.display-name="Quarkus.io S2I (GraalVM Native)" \
      io.openshift.tags="builder,java,quarkus,native"

### S2I part
COPY s2i /usr/local/s2i
RUN chmod 755 /usr/local/s2i/*
ADD README.md /usr/local/s2i/usage.txt

ENV PATH=$PATH:"/usr/local/s2i"

USER 1001

WORKDIR ${QUARKUS_HOME}

EXPOSE 8080

# Use the run script as default since we are working as an hybrid image which can be
# used directly to. (If we were a plain s2i image we would print the usage info here.)
CMD [ "/usr/local/s2i/run" ]
