Install JBoss EAP di `~/apiman-1.2.5.Final/jboss-eap-7.0`

Download dan install Apiman

```
mkdir ~/apiman-1.2.5.Final/
cd ~/apiman-1.2.5.Final
curl http://downloads.jboss.org/apiman/1.2.5.Final/apiman-distro-wildfly10-1.2.5.Final-overlay.zip -o apiman-distro-wildfly10-1.2.5.Final-overlay.zip
unzip -o apiman-distro-wildfly10-1.2.5.Final-overlay.zip -d jboss-eap-7.0
cd jboss-eap-7.0
./bin/standalone.sh -c standalone-apiman.xml
```

Clone Echo Service Quickstart project:
```
cd /tmp
git clone https://github.com/apiman/apiman-quickstarts.git
cd apiman-quickstarts/echo-service
git checkout 1.2.0.Final
mvn clean install
```

```
[INFO] Installing /Servers/EAP-7.0/apiman-quickstarts/echo-service/target/apiman-quickstarts-echo-service-1.2.0.Final.war to /Users/ejlp12/.m2/repository/io/apiman/apiman-quickstarts-echo-service/1.2.0.Final/apiman-quickstarts-echo-service-1.2.0.Final.war
...
[INFO] Installing /Servers/EAP-7.0/apiman-quickstarts/echo-service/target/apiman-quickstarts-echo-service-1.2.0.Final-sources.jar to /Users/ejlp12/.m2/repository/io/apiman/apiman-quickstarts-echo-service/1.2.0.Final/apiman-quickstarts-echo-service-1.2.0.Final-sources.jar
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
...
```
Deploy in JBoss EAP (WildFly)

```
mvn wildfly:deploy
```

Test service

```
$ brew install httpie
==> Downloading https://homebrew.bintray.com/bottles/httpie-0.9.3.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring httpie-0.9.3.yosemite.bottle.tar.gz
🍺  /usr/local/Cellar/httpie/0.9.3: 290 files, 3.7M

$ http GET http://localhost:8080/apiman-echo/hello
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 317
Content-Type: application/json
Date: Wed, 18 May 2016 23:34:23 GMT
Server: JBoss-EAP/7
X-Powered-By: Undertow/1

{
    "bodyLength": null,
    "bodySha1": null,
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Connection": "keep-alive",
        "Host": "localhost:8080",
        "User-Agent": "HTTPie/0.9.3"
    },
    "method": "GET",
    "resource": "/apiman-echo/hello",
    "uri": "/apiman-echo/hello"
}
```

Sukses!
