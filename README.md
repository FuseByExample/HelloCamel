HelloCamel Camel OSGi example
=============================

This project demonstrates a minimal Camel project that creates an OSGi bundle, has a unit test and
a test client that uses Camel. The Camel route is simple: an HTTP request-response service that
prefixes 'Hello ' to the String body sent to it. The goal of this project is to provide a good
starter project for creating your own more complicated and useful Camel Routes that can be deployed
in the excellent ServiceMix OSGi integration server.

To build
> mvn clean install

As part of the basic build it will run a standalone unit test every time. Once you deploy this
project to ServiceMix, you may see a build error where the unit test tries to start Camel, and
it fails because it can't acquire the localhost port 8888 (because your running route in ServiceMix
would be using it). To build without running the unit test
> mvn clean install -Dmaven.test.skip=true

To test Standalone, you can start the Camel route using the included camel-maven-plugin, which will
start all Spring based camel context files located in `META-INF/spring` (like this projects)
> mvn camel:run

To run the standalone client, which will call the service with the string 'Scott', and print the
response (which should be 'Hello Scott') to stdout
> mvn exec:java

To install in ServiceMix (4.3.1 or later)
> bin/servicemix

In ServiceMix Console

    karaf@root> features:install camel-jetty
    karaf@root> osgi:install -s mvn:com.fusesource.byexample.hellocamel/HelloCamel

Once you've started the service - the '-s' option to osgi:install will start the bundle once installed - you
can test using the standalone client
> mvn exec:java
