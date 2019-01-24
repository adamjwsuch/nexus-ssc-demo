FROM       sonatype/nexus3:3.15.1
MAINTAINER Curtis Yanko<cyanko+docker@sonatype.com>

USER root

# configure nexus
RUN  echo "nexus.assetdownloads.enabled=true" >> ${NEXUS_HOME}/etc/nexus-default.properties 

USER nexus