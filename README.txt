README
==========================================
Openshift
---------------------------------------
URL:  https://openshift.ngdcatc.local
USER: demouser/demouser
CLOUD ADMIN: admin/WWTwwt1!

MASTER LOGIN
-----------------------------------------
ocp-master1.ngdcatc.local

oc commands Catalog
-----------------------------------------

==========================================================================================

DEMO SCRIPT

===========================================================================================

1)CREATE S2I BLUEGREEN
-----------------------------------
2) AB-DEPLOY

A) Start with AB-Deploy v1 -- Call it ABv1
B) Show Route
C) Edit Git and create version 2
D) Create new app and deploy as ABv2
E) Edit Route and setup 90/10
F) Open Putty and ssh to MASTER and execute curl while (in file abcurl.text in root home) loop to show 

----------------------------------------------------

3) DEPLOY INVENTORY VIA MVN USING WILDFLY SWARM

A) inventory is located in C:\Users\JohnGammon8356\software\workspaces\cloud-native-labs-master
B) login to oc as demouser
C) execute mvn fabric8:deploy
D) Show the builder image from configuration of builds
E) test it with url link

-----------------------------------------------------

4) LETS SEE SOME COMMANDS OF WHAT IS HAPPENING IN THE CLUSTER:

 oc get pods --all-namespaces -o wide
 oc get pods -n smoketest -o wide
 
 show the multi-tenant plugin located on master in masterconfig.yaml
 /etc/origin/master -- just search for multi
 
5) LET'S VIEW INFRASTRUCTURE COMPONENTS

A) Registry
B) Logging
c) Metrics
d) Image Streams for catalog
e) Storage

6) LETS DEPLOY A SPRING BOOT APP WITH STORAGE CREATE A NEW PROJECT CALLED DEMO

A) Deploy catalog to openshift with mvn
B) Show url -- using h2 embedded file ds store
C) create Catalog DB

oc new-app postgresql-persistent --param=DATABASE_SERVICE_NAME=catalog-postgresql --param=POSTGRESQL_DATABASE=catalog --param=POSTGRESQL_USER=catalog --param=POSTGRESQL_PASSWORD=catalog --labels=app=catalog
	
D) Create the config map (root of catalog project)
oc create configmap catalog --from-file=./application.properties

now configure springboot so it explicitly has rights to auto find the config map in the project
Although Spring Cloud Kubernetes tries to discover config maps, due to security reasons containers by default are not allowed to snoop around OpenShift clusters and discover objects. 
Security comes first, and discovery is a privilege that needs to be granted to containers in each project.

E) first kill the catalog pod and check db to show it can't be found because of security

F) oc policy add-role-to-user view -n catalogproj -z default

G) now check the db first to make sure it isn't loaded yet

psql -U catalog -c "select item_id, name, price from product" -- should show nothing

H) now delete the pod and recheck after security and redeploy steps

psql -U catalog -c "select item_id, name, price from product" and data should be there

I) this shows that catalog pod is using the config map:
look in web console pod logs for hibernate

7) SHOW JENKINS IN THE WWT CLUSTER







