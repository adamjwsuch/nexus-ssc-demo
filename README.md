# nexus-ssc-demo

Refernce platform implimentation to facilitate development and testing effort. Also can be used for demo's and as a reference implimentation.

Demonstrate the itegration between Nexus IQ and Fortify SSC

# IQ Server 52+
You must have a valid license to run the IQ server and you'll want to have some scanned apps to test/demo the functionality of moving data from IQ to SSC. We began testing with IQ 52 but have been updating the project with each new release since then.

# MYSQL
Image specifically designed to be used with SSC. Auto creates an empty DB and run's the create tables script on it as well as creating the super user for that DB.

# Fortify SSC
Built and tested against 18.10 and 18.20. ***You need to put the ssc.war and License files in ssc_master/files***

# IntSvc
Springboot JAR in an openJRE container that does the work here. http://localhost:8182/startScanLoad to trigger a run. Be sure to set the mapping.json file to point to project/phase that exist in IQ. You can make up new project/versions for SSC as they will be created if they don't exist.



# Running

Each container makes use of a persistent volume. Just running the docker-compose file will create them but I reccomend making them for yourself and pre-populating them with the files needed.

```
~/.mysql
~/.fortify_home   <-- ssc does persist a few things on it's side
~/.intSvcWorkFolder   <-- this is where JSON files will get created prior to being uploaded to SSC
~/iq-data           <--- this is for IQ
```

Now lets get started!

```
docker-compose up -d
```

...let the db fully initialize. You can use docker logs <container_name> or my preference, Kitematic to see the logs. Be patient, this takes a few minutes. It will restart itself after the create tables script run. Now stop everything. If you also looked at the SSC logs it is probably going to fail because the db wasn't ready.

```
docker-compose down
```
Look in the fortify home folder you created for a pv (the default value is for ~/.intSvcWorkFolder) and remove the ssc folder that was created on the failed startup. Copy in all of the files from ssc-master/fortify_home except for the readme and then restart the environment.This will force SSC to start over with it's initial auto-configuration.

```
docker-compose up -d
```
This should auto configure and apply the seed data. Now when you hit http:/localhost:8888/ssc you will be presented with a login screen. Use the default creds admin/admin and you will then need to set up a new password. You can also go into the Administration section and install the parser plugin. Be sure to 'enable' it as well.
Stop the environment again

Use this password to update the iqapplcation.properties in the intSvc. Remove the original image from your registry and restart the environment. This will re-build the intSvc image with your updated properties

You now have a running SSC instance with a MYSQL back end and persistence in place to survive tear down and re starts.

From now on you just have to start and stop the environment.
