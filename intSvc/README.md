## Docker Environment

This folder is setup to build the Integration Service container. You'll need to modify the config.yml file to point to your SSC instance and set the login credentials. You may want to modify the config.yml file for the cron expression which defaults to 'every 6 hrs at 12 and 6'. 

The rest of the defaults are set to work for the demo environment assuming you set the SSS and IQ pw's to match.

## Non-Docker Environment

If you want to run this JAR the old fashioned way, the files you need are in teh 'files' folder in addition to the Parser plugin in the SSC folder.

Modify the start script to reference the JAR (it's renamed when copied into the container to simplify things so gthe script references IntSvc.jar instead of the full name.
