# collections


Task definition v0:
- there is some notation how to define application to simplify management
- there is exampe of simple application which is consist of few components/services
- how quick can we change it ?
- what are possible use cases of Hybrid Cloud ? PaaS/IaaS  


Example implementation (task definition v01) "hybrid cloud with orchestration tool (Qubell)":  

- Hybrid cloud . What does it mean in this  example : some applications lives in IaaS cloud (AWS EC2) , some in PaaS Cloud (OpenShift Online, AWS EC2 )

- "umbrella" management system Qubell (www.qubell.com). Core functionality of Qubell's engine is to compute how some deployment configuration of some applications can be transformed from one state to another as a reaction on some signals/changes/events. 
- what do we need to compute such todolist reacting on some changes ?
we should have information about applications and about relations between. also we should have information about environment. 
Qubell proposes the way how such information can be described - some specific and unversal ontology language ( dsl, structure for metainformation) and how it can be processed. 

- set of applications: existen  "PetClinic" Spring example: HAproxy as load balancer, Java App on Tomcat, Mysql. This example already works with AWS EC2.
- let's move java app from tomcat in AWS EC2 to JBoss in OpenShift (task definition v02)

task definition v03:
- describe how  OpenShift functionality can be used
- modify exisen "Application Server"-level component. at this moment it uses tomcat , let it use openshift. 


task definition v04 mvp definition:
- create service component definition for Qubell which define minimal set of interfaces which can be mapped to existen components definitions
  - current set of Application Server's actions/commands: 
    - build-app 
    - deploy-war
    - deploy-libs
    - reconfigure
    - manage 
  - minimal set of atomic actions in OpenShift (co-author: https://github.com/dieu): 
    - create app
    - deploy app
    - delete app
- changes for existen component definition should as minimal as possible:
  - current hierarchy. java-starter depends on Application Server (tomcat), Load balancer(haproxy), Database (mysql)
  - java starter, database, load balancer components should be not changed
  - application server (tomcat) should be changed and should use interface of new OpenShift service
- mapping between Application Server and OpenShift service:
  - build app -> create-app (on init stage we don't have such information like git uri )
  - deploy app -> deploy app
- technical implementation by using command line tool (rhc) and bash scripts because one of main cretaria is speed of implementation just to plat with use case (PoC-like stage)
- - simple implementation based on rest calls should be created to check possible production way for implemetation

task definition v05 clarification based on ways to deploy java app in OpenShift:
- there are few ways to deploy application:
   - "git push" deployment style (aka heroku). change code, push to remote repo, build and deploy will be started automatically ( git hooks, mvn )
   - "git push" deployment . push artefacts by using git 
   - deployment artefact by using scp
   - native application server deployment 
- artefacts push by using git was choosen. artefacts already builded by existen configuration. one of artefcats is context.xml with mysql ip. mysql ip is already presented on this level as part of context.xml
- new command should be created for OpenShift service - deploy with War and Context
- artefacts should be published to be used in OpenShift service

Initial estimation for task was about 2-3 days. First working version was created after 3 days. Additional two iterations were spent on debug and exception handling. 


Current list of dependecies:
- Qubell account ( free account include AWS EC2 account)
- AWS account (cloud account should be configured in Qubell)
- AWS EC2 network configuration. security should allow income tcp 22, 8080, 80, 3306
- AWS EC2 network configuration: elastic ip.
- OpenShift account (openshift creadentials and endpoints should be added)
- Amazon AMI Linux (configuration was tested with latest Amazon AMI)
- Amazon package repositories (many packages)
- chef client (should be accessible in execution time)
- Qubell cookbooks for PetClinic example (should be accesible in execution time)
- rhc gem 
- OpenShift domain name should be created

Current status:
- PetClinic is working with OpenShift Service:
- -  launch works fine.
- -  rebuild-app works fine.
- -  app-scale does not work.


Current todo list:
- clean up workflows in OpenShift service.
- add additional exception handling and proper status string in OpenShift service
- experiment with returning json from execrun steps in OpenShift service
- implement deployment to OpenShift through scp
- test deployment without context.xml deployment
- add test automation for openshift service
 
Current implementation's specifics/limitation/defects:
- possible infinite cycle in create-app step checkFqdn. need to add timeout . 
- list of cartridges is hardcoded
- mysql ip  should be injected through environment variables. nice to have
- application in openshift creation on build-app of application server .
- one trigger was removed from PetClinic manifest.  trigger on app-host change. because after deplyment in OpenShift app-hosts should be updated without build-app retriggering, timeout for few commands was changed
- integration with openstift works through command line tool (designed)
- ssh keys are generated every time with command line tool setup
- current Application server manifest is based on stable version of
this manifest. there is fresh version (backlog)

Questions:
- lifecycle application entity in OpenShift 

Failures which happened  in process of testing:
- 'yum update => python -c "import yum" => failed '. was caused by changes in amazon package repository. was fixed.
  - https://forums.aws.amazon.com/thread.jspa?messageID=610503
- mvn repo was not available :
  >> Ran cd /tmp/webapp; mvn clean package -Dmaven.test.skip=true returned 1
  >> (http://repository.springsource.com/maven/bundles/external): Connect to repository.springsource.com:80 [repository.
  >> springsource.com/54.231.17.169] failed: Connection timed out -> [Help 1]
-  "gem install rhc" failed
  >> ERROR:  While executing gem ... (Gem::DependencyError)
  >>    Unable to resolve dependencies: rhc requires highline (~> 1.6.11); commander requires highline (~> 1.7.1)
  https://github.com/openshift/rhc/issues/678
  http://help.rubygems.org/discussions/problems/20587-gem-install-rhc-failed-gemdependencyerror/autosuggest