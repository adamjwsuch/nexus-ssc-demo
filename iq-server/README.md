# Custom IQ Server Image

A Dockerfile for creating a custom image based on the Official Image

```
FROM sonatype/nexus-iq-server
COPY config.yml /opt/sonatype/iq-server/

HEALTHCHECK CMD curl http://localhost:8071/ping
```

The key is make a copy of this folder so you can customize the config.yml with settings for your Demo, PoC or Test environment.

You'll need a license to run the IQ server. Partners should have one already or can contact me.
