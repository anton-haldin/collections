Before push install:
- create Qubell free account ( it include AWS EC2 account)
- create OpenShift Online account
- check Cloud Account is configured in Qubell
- convigure environment properties with information to initialize openshift and git (please look into environment_export.yaml as example)

[![Install](https://raw.github.com/qubell-bazaar/component-skeleton/master/img/install.png)](https://express.qubell.com/applications/upload?metadataUrl=https://raw.githubusercontent.com/anton-haldin/collections/dev/meta_for_install.yaml)

After component manifests will be installed:
- launch OpenShift NSN5 service and add it to your default environment
- launch PetClinic NSN5 to build and deploy PetClinic application on OpenShift with load balancer and database in AWS EC2


