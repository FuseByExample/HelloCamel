HelloCamel Camel OSGi example
=============================

This project demonstrates a minimal Camel project that creates an OSGi bundle, has a unit test and
a test client that uses Camel. The Camel route is simple: an HTTP request-response service that
prefixes 'Hello ' to the String body sent to it. The goal of this project is to provide a good
starter project for creating your own more complicated and useful Camel Routes that can be deployed
in JBoss Fuse 

### Requirements:
* JBoss Fuse 6.2.0 (http://www.jboss.org/jbossfuse)
* Maven 3.2.3 (http://maven.apache.org/)
* Java SE 7 or Java SE 8

Building
--------

To build

    mvn clean install

As part of the basic build it will run a standalone unit test every time. Once you deploy this
project to JBoss Fuse, you may see a build error where the unit test tries to start Camel, and
it fails because it can't acquire the localhost port 8888 (because your running route in JBoss Fuse
would be using it). To build without running the unit test

    mvn clean install -Dmaven.test.skip=true

Running from the command line
-----------------------------

To test Standalone, you can start the Camel route using the included camel-maven-plugin, which will
start all Spring based camel context files located in `META-INF/spring` (like this projects)

    mvn camel:run

To run the standalone client, which will call the service with the string 'Scott', and print the
response (which should be 'Hello Scott') to stdout

    mvn exec:java

Deploying to JBoss Fuse
-----------------------

To install in JBoss Fuse

    <JBoss Fuse Home>/bin/fuse

In JBoss Fuse Console

    karaf@root> features:install camel-jetty
    karaf@root> osgi:install -s mvn:com.fusesource.byexample.hellocamel/HelloCamel/1.0.0

Once you've started the service - the '-s' option to osgi:install will start the bundle once installed - you
can test using the standalone client

    mvn exec:java
