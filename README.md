# Quick Start to build and run Tomcat w/ SSL

Pre-reqs for Docker host: 
- Internet access to pull the Docker 'java:7-jre' image.
- Java installed (for keytool utility) (yum -y install openjdk-1.7.0)
- 'server.xml' file (with SSL settings) is in same directory as the Dockerfile

1. Generate a Java JKS format certificate w/ a password of "changeit"
 Example:
$ keytool -genkey -dname "cn=Mike Edmonds, ou=DSC, o=DSC, c=US" -alias tomcat \
 -keypass changeit -storepass changeit -validity 360 -keyalg RSA \
 -keystore /home/centos/certs/.keystore

2. Verify the keystore is valid:
$ keytool -list -keystore /home/centos/certs/.keystore

3. Build the Docker image (run command from the directory with the Dockerfile):
$ docker build -t dsc/tomcat:8.0.24-ssl . 

4. Run the container, mounting the cert directory
 Example:
$ docker run --rm -v /home/centos/certs:/certs -p 8000:8443 \
 dsc/tomcat:8.0.24-ssl

5. With a web browser, test the connection by browsing to <dockerip>:8000
