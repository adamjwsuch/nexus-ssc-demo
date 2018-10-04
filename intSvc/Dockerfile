FROM tomcat:8.5

# args; expected that there be a directory named files that contains:
# 
#	- SonatypeFortifyIntegration.war

ARG MY_FILES_DIR=files
ARG TMP_DIR=/root/mytemp
ARG TOMCAT_DIR=/usr/local/tomcat
ARG SONATYPE_WORK=/usr/local/sonatype/work/

# fortify ssc home dir (log/conf locations default=/root/.fortify/ssc)
# keep it with TOMCAT_HOME in its own ssc directory struture
#ENV JAVA_OPTS="$JAVA_OPTS -Dfortify.home=$FORTIFY_HOME"

# create temp dir for needed files
RUN mkdir $TMP_DIR/
COPY $MY_FILES_DIR/* $TMP_DIR/

# clear out "default" apps that tomcat includes
# put intSvc war in webapps dir for deployment
# create sonatype work folder
RUN rm -rf $TOMCAT_DIR/webapps/* && \
	mkdir -p $SONATYPE_WORK && \
	mv $TMP_DIR/SonatypeFortifyIntegration.war $TOMCAT_DIR/webapps

# cleanup temp dir
RUN rm -rf $TMP_DIR
