# SSC
Fortify Software Security Center war

# Built for 18.10
use the ssc.autoconfig file to deploy a preconfigured ssc.war to your container

You need to put a copy of the ssc.war file and your license file in here

SSC persist it's config locally inaddition to data saved in the DB
Pt your license file in the fortify_home folder outside of this project space
The existing compose file expects ~/.fortify_home but you are free to put it where you want, just make sure it agrees with teh docker volume mount.
