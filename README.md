# nexus-ssc-demo

Refernce platform implimentation to facilitate development and testing effort. Also can be used for demo's and as a reference implimentation.

Demonstrate the itegration between Nexus IQ and Fortify SSC

# IQ Server 52
You must have a valid license to run the IQ server and you'll want to have some scanned apps to test/demo the functionality of moving data from IQ to SSC

# MYSQL
Image specifically designed to be used with SSC. Auto creates an empty DB and run's the create tables script on it as well as creating the super user for that DB.

# Fortify SSC
Built and tested against 18.10. ***You need to put the WAR file in ssc_master/files***

# IntSvc
Springboot JAR in an openJRE container that does the work here. http://localhost:8182/startScanLoad to trigger a run.



# Running

Each container makes use of a persistent volume. Just running the docker-compose file will create them but I reccomend making them for yourself and pre-populating them with the files needed.

~/.mysql
~/.fortify_home   <-- ssc does persist a few things on it's side
~/.intSvcWorkFolder   <-- this is where JSON files will get created prior to being uploaded to SSC
~/iq-data           <--- this is for IQ


```
docker-compose up -d
```

let the db fully initialize. It will restart itself after the create tables script run. Now stop everything.
```
docker-compose down
```
Look in the fortify home folder you created for a pv and remove the ssc folder that was created. Copy in all of the files from ssc-master/fortify_home except for the readme and then restart the environment. This now support the auto-config process for SSC.
```
docker-compose up -d
```
This should auto configure and apply the seed data. Now when you hit http:/localhost:8888/ssc you will be presented with a login screen. Use the default creds admin/admin and you will then need to set up a new password. You can also go into the Administration section and install the parser plugin. Be sure to 'enable' it as well.
Stop the environment again

Use this password to update the iqapplcation.properties in the intSvc. Remove the original image from your registry and restart the environment. This will re-build the intSvc image with your updated properties

You now have a running SSC instance with a MYSQL back end and persistence in place to survive tear down and re starts.

From now on you just have to start and stop the environment.
