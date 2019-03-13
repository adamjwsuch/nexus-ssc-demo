FROM quay.io/cnyanko/ubi-iq-server
COPY config.yml /etc/nexus-iq-server/

HEALTHCHECK CMD curl http://localhost:8071/ping