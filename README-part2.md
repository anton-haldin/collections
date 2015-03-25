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
