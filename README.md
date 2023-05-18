# cryostat

To make cryostat work properly, with a quarkus application you need to modify the Dockerfile provided by quarkus.

For example, modify the Dockerfile.jvm adding:

EXPOSE 9091

ENV JAVA_OPTS="-Dquarkus.http.host=0.0.0.0 -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Dcom.sun.management.jmxremote.port=9091 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"


This allows us to modify our container. 
In addition we need to be sure that in the application.properties we add:

quarkus.kubernetes.deploy=true
quarkus.kubernetes.deployment-target=openshift
quarkus.openshift.build-strategy=docker

We also need to setup the port number we want to expose accordingly with the docker file, so:

openshift.ports[1].container-port=9091

Now, running mvn clean package, with the openshift extension the application will be deployed on openshift.
With this approach, we miss the jxm port declaration both in the Service and in the deploymentconfig, so we need to update manually. This could be a nice improvement


