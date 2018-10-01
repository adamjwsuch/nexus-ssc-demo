# nexus-ssc-demo

Refernce platform implimentation to facilitate development and testing effort. Also can be used for demo's and as a reference implimentation.

Demonstrate the itegration between Nexus IQ and Fortify SSC

# IQ Server 52
You must have a valid license to run the IQ server and you'll want to have some scanned apps to test/demo the functionality of moving data from IQ to SSC

# MYSQL
Image specifically designed to be used with SSC. Auto creates an empty DB and run's the create tables script on it as well as creating the super user for that DB.

# Fortify SSC
Built and tested against 18.10. You need to put the WAR file in ssc_master/files

TODO: Would like to fget the ssc.autoconf working to avaoid the setup wizard.

# Running
```
docker-compose up -d
```

Navigate to http://localhost:8888/ssc to go through the setup wizard (all values are in the ssc.autoconf file)
The init.token file will be in $FORTIFY_HOME/ssc

The License is pre-installed and just needs to be accepted.

Set the URL to http://localhost:8888/ssc

The super user for the DB is ssc_admin and the password is ssc_password (see docker compose in the MYSQL section)

The DB tuest connection string is
jdbc:mysql://mysql:3306/ssc_db?connectionCollation=latin1_general_cs&rewriteBatchedStatements=true

Test the connection!

Seed data
I seeded these three files that came in the SSC download bundle

Fortify_Report_Seed_Bundle-2018_Q1.zip: 
Fortify_Process_Seed_Bundle-2018_Q1.zip: 
Fortify_PCI_Basic_Seed_Bundle-2018_Q1.zip: 

Finished Setup Wizard!

Now run the following to restart the environment:
```
docker-compose down
docker-compose up -d
```
Navigate to http://localhost:8888/ssc

Use the default pusername and password to get in (not the same as the super user for the DB. You'll need to change the password and then re-login.

You now have a running SSC instance with a MYSQL back end and persistence in place to survive tear down and re starts.
