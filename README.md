# collections


Documentation README-style:

Abstract:
- there is some notation how to define applications to simplify management
- there is exampe of simple application that  consists of few components/services
- how quick can we change it ?
- what are the possible use cases of Hybrid Cloud ? PaaS/IaaS  


Structure:
- README (face page with two pictures that looks good in raw format)
- README-part2 (more words)
- INSTALL (how to)
- change-test-list.txt

- manifests:
  - starter-java-web.yml (petclinic) (clonned from repo, minor changes)
  - component-haproxy.yml (clonned from parent repo)
  - component-mysql-dev.yml (clonned from partent repo)
  - component-tomcat-dev.yml (modified to support OpenShift Service)
  - MiracleFever.yaml - example Openshift service based on rest calls (pure and lightweght proxy service)
  - example OpenShift service based on command line tool and bash (proxy service that add OpenShift functionality to Qubell)
  - meta_for_install.yaml (links on manifests for install.md)
  - environment_export.yaml (example with environment properties)


Picture #1 Hybrid Cloud

    +--------------------------+                                     
    |                          |                                     
    | OpenShift (PaaS)         |                                     
    |                          |                                     
    |                          |                                     
    +--------------------------+                                     
                                                                     
    +---------------------------------------------------------------+
    |                                                               |
    | Amazon AWS EC2 (IaaS)                                         |
    +---------------------------------------------------------------+


Picture #2 Application Management as reacting on signals and processed by some engine with some meta information
state 1 set of applications and resources 
    
                                               +-------+              
             +------------+                    |       |              
             |            |                    +-------+              
             |            |    +-----------+                          
             +------------+    |           |                          
                               |           |                          
                               |           |       +---------+        
                               +-----------+       |         |        
    +-----------+                                  +---------+        
    |           |           +--------------------+                    
    |           |           |Tomcat   +----------|                    
    +-----------+           |         |Petclinic||                    
                            |         +----------|        +----------+
                            +--+-----------------+        |          |
      +------------+           ^              |           +----------+
      |            |           |              |                       
      |            |           |              |                       
      +------------+   +-------+---+       +--+----------+            
                       |Haproxy    |       |Mysql        |            
                       +-----------+       +-------------+            
    
    
    
    							+ + +
    							| | | transform to anothere state as response on: 
    							| | | - new feature branch
    							| | | - new cloud resources available
    							| | | - more new clients from specific location
    							v v v
    
    state 2
    
                                               +-------+                 
             +------------+                    |Postgre|XXXXXXXXXXXX     
             |            |                    +-------+           XXXX  
             |  Mysql Cluster  +-----------------+                    X  
             ++-----------+    |Jboss            |                    X  
              ^                |           +-----+                    X  
              |                |   PetClinica v3 | +---------+        X  
              |                +-----------+     + |Nginx    |XXXXX    X 
    +---------+-+                                  +---------+    X    XX
    |           |           +--------------------+                X     X
    |Petclinic^2|           |Tomcat   +----------|                X     X
    +-----+-----+           |         |Petclinic|XX XXX           X     X
          ^                 |         +----------|    X   +----------+ XX
          |                 +--+-----------------+    XXXX|etcd      |XX 
      +---+--------+           ^              |           +----------+   
      |Haproxy     |           |              |                  X       
      |            |           |              |                  XX      
      +------------+   +-------+---+       +--+----------+   X XXXX      
                       |Haproxy    |       |Mysql Dev    |XXX            
                       +-----------+       +-------------+               
    


References:
- Qubell: http://qubell.com/ http://docs.qubell.com/
- Openshift: https://www.openshift.com/ http://docs.openshift.org/
- Amazon AWS ec2: https://console.aws.amazon.com/ec2/v2/

- OpenShift deployment: https://help.openshift.com/hc/en-us/articles/202399740-How-to-deploy-pre-compiled-java-applications-WAR-and-EAR-files-onto-your-OpenShift-gear-using-the-java-cartridges
- example set of applications: https://github.com/qubell-bazaar/starter-java-web
