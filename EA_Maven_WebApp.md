
[Download Maven site](http://maven.apache.org/download.cgi)

Membuat project aplikasi web (Java EE) dengan menggunakan Maven

`mvn archetype:generate -DgroupId=com.ejlp12 -DartifactId=HelloWebApp -DarchetypeArtifact=maven-archetype-webapp -DinteractiveMode=false`

`cd HelloWebApp
mvn package`

```
.
├── pom.xml
└── src
    └── main
        ├── resources
        └── webapp
            ├── WEB-INF
            │   └── web.xml
            └── index.jsp
```


Tambahkan ini di `pom.xml` jika menggunakan Eclipse

```
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.1.0</version>
  <scope>provided</scope>
</dependency>
```

```
.
├── pom.xml
├── src
│   └── main
│       ├── resources
│       └── webapp
│           ├── WEB-INF
│           │   └── web.xml
│           └── index.jsp
└── target
    ├── HelloWebApp
    │   ├── META-INF
    │   ├── WEB-INF
    │   │   ├── classes
    │   │   └── web.xml
    │   └── index.jsp
    ├── HelloWebApp.war
    ├── classes
    └── maven-archiver
        └── pom.properties
```


Membuat folder menjadi eclipse project

`mvn eclipse:eclipse -Dwtpversion=2.0`

