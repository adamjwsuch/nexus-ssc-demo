# Derived from official mysql image (our base image)
FROM mysql:5.7
# Add a database and superuser
ENV MYSQL_DATABASE ssc_db
ENV MYSQL_USER ssc_admin
ENV MYSQL_PASSWORD ssc_password
# Add the content of the sql-scripts/ directory to your image
# All scripts in docker-entrypoint-initdb.d/ are automatically
# executed during container startup
COPY ./sql-scripts/ /docker-entrypoint-initdb.d/
COPY ./config-file.cnf /etc/mysql/conf.d/