FROM       sonatype/nexus3:3.15.2

LABEL maintainer="Curtis Yanko<cyanko+docker@sonatype.com>"
USER root

# configure nexus - turn download trends back on
RUN  echo "nexus.assetdownloads.enabled=true" >> ${NEXUS_HOME}/etc/nexus-default.properties 

USER nexus