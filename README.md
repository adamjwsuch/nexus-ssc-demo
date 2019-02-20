# nexus-ssc-demo

Reference platform implementation to facilitate development and testing effort. Also can be used for demo's and as a reference implementation.

Demonstrate the integration between Nexus IQ and Fortify SSC.

# IQ Server 52+
You must have a valid license to run the IQ server and you'll want to have some scanned apps to test/demo the functionality of moving data from IQ to SSC. We began testing with IQ 52 but have been updating the project with each new release since then. As of tyhis edit we're on 60!

# MYSQL
Image specifically designed to be used with SSC. Auto creates an empty DB and run's the create tables script on it as well as creating the super user for that DB.

# Fortify SSC
Built and tested against 18.10 and 18.20. ***You need to put the ssc.war and License files in ssc_master/files***
Once up you can hit http://localhost:8888/ssc  (please note that within the docker network it is accessed by it's service name and internal port http://ssc:8080/ssc as reflected in the iqapplication.properties file for the integration service)

# IntSvc
Springboot JAR in an openJRE container that does the work here. http://localhost:8182/startScanLoad to trigger a run. Be sure to set the mapping.json file to point to project/phase that exist in IQ. You can make up new project/versions for SSC as they will be created if they don't exist.



# Running

Each container makes use of a persistent volume. Just running the docker-compose file will create them but I recommend making them for yourself and pre-populating them with the files needed.

```
~/.mysql
~/.fortify_home   <-- ssc does persist a few things on it's side
~/.intSvcWorkFolder   <-- this is where JSON files will get created prior to being uploaded to SSC
~/iq-data           <--- these iare for IQ
~/iq-logs
```

Now lets get started! With these bigger docker compose files it's not a good idea to try and start everything at once for the first time. Once we get everythibng working we'll be able to start it all but for now we'll use a trick to just start one part at a time.

```
docker-compose up -d mysql  <- the -d 'detaches' the process from the console
```

...let the db fully initialize. You can use docker logs <container_name> or my preference, Kitematic to see the logs. Be patient, this takes a few minutes. It will restart itself after the create tables script run.
Create the fortify home folder then copy in all of the files from ssc-master/fortify_home except for the readme and also add the fortify license then start the SSC container. Now that the dartabase is at the ready, the SSC app can start properly.

```
docker-compose up -d ssc
```
This should auto configure and apply the seed data. Now when you hit http:/localhost:8888/ssc you will be presented with a login screen. Use the default creds admin/admin and you will then need to set up a new password. You can also go into the Administration section and install the parser plugin. Be sure to 'enable' it as well.
Stop the environment again

Use this password to update the iqapplcation.properties in the intSvc. Now we can start the rest of the environment

You now have a running SSC instance with a MYSQL back end and persistence in place to survive tear down and re starts.

```
docker-compose up -d iq-server
```
The IQ server should start up. You'll need a valid license file once you log in. These days, the IQ server auto-loads the latest policy set so the only left to do is crearte an app in the ui and then upload a binary to get a report. You can download the Webgoat v6 WAR here [ https://www.sonatype.com/hubfs/URLs/WebGoat-6.0.1.war] as a sample. I'f you give it an ID of webgoat6 in IQ it'll saave ypu a step later ;-)

Before we bring the integration service up we need to check the config file to get user names and passwords in place plus update the mappings.json file to point to the app(s) you created in Lifecycle. Once all of that is set go ahead and bring it up.

```
docker-compose up -d intSvc
```
If yopu make a mistake and need to try again. Just stop and remove the container ( docker-compose down) and delte the image from your local registry. Then the next time you try to run it it will rebuild the container with any edits you've made to the config. Once you get to the point that want to retain the previous container, start just incrimenting the version in the docker-compose file.

You can visit http://localhost:8182/startScanLoad to trigger a run.

If you don't have a docker log viewer I reccomend Kitematic or Dockstation. They come in handy to see the logs of a running container for troubleshooting or just peeking under the hood a bit.

If everthing worked, you should now be able to see some apps in SSC with Vulnerable OSS data!


To stop everything, use
```
docker-compose down
```
from now on you can just start it all together
```
docker-compose up -d
```
