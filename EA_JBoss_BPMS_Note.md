
List port yang djbuka oleh JBPM

```
*:63196 (LISTEN)
localhost:8080 (LISTEN) -> web
localhost:4447 (LISTEN) -> remoting
localhost:9999 (LISTEN) -> management native
localhost:3528 (LISTEN) -> jacorb
localhost:9990 (LISTEN) -> management web
localhost:5445 (LISTEN) -> messaging
localhost:5455 (LISTEN) -> messaging-thoughput
*:9418 (LISTEN) -> GIT 
*:8001 (LISTEN) -> SSH GIT
```

http://localhost:8080/business-central/

Ubah konfigurasi 
<JBOSS_EAP_HOME>/standalone/configuration/standalone.xml

```xml
        <subsystem xmlns="urn:jboss:domain:datasources:1.1">
            <datasources>
                <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
                    <connection-url>jdbc:h2:tcp://localhost:9091/C/jbpms-db</connection-url>
                    <driver>h2</driver>
                    <security>
                        <user-name>sa</user-name>
                        <password>sa</password>
                    </security>
                </datasource>
                <drivers>
                    <driver name="h2" module="com.h2database.h2">
                        <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
                    </driver>
                </drivers>
            </datasources>
        </subsystem>
```

Jalankan ulang JBoss EAP

## Start H2 Database standalone
Buat file `h2start.bat` di direktori <JBOSS_EAP_HOME> dan isi dengan text seperti dibawah ini:

Windows:
```
java -cp “C:/BPMS_603/jboss-eap-6.1/modules/system/layers/base/com/h2database/h2/main/h2-1.3.168-redhat-2.jar;%CLASSPATH%” org.h2.tools.Server -tcp -tcpPort 9091 -baseDir C:/jbpms-db/ -web -webPort 8083
```

Linux/Mac OS-X:
```
java -cp /home/jboss/BPMS_603/jboss-eap-6.1/modules/system/layers/base/com/h2database/h2/main/h2-1.3.168-redhat-2.jar:$CLASSPATH org.h2.tools.Server -tcp -tcpPort 9091 -baseDir /home/jboss/jbpms-db/ -web -webPort 8083 &
```

Jalankan `h2start.bat` dari command line prompt
Test koneksi ke H2 dengan membuka browser dan coba akses H2 console dengan URL http://localhost:8083 

Masukan field text berikut:

```
JDBC URL = jdbc:h2:tcp://localhost:9091/C/jbpms-db
USERNAME = sa
PASSWORD = <kosongkan>
```

Klick tombol “Test Connection”




```
drwxr-xr-x  17 eariobow  staff   578B Mar  5 04:19 ./
drwxr-xr-x   9 eariobow  staff   306B Mar  5 04:21 ../
-rw-r--r--   1 eariobow  staff   425B Aug 21  2013 JBossEULA.txt
-rw-r--r--   1 eariobow  staff    26K Aug 21  2013 LICENSE.txt
drwxr-xr-x   3 eariobow  staff   102B Mar  5 04:18 appclient/
drwxr-xr-x  38 eariobow  staff   1.3K Mar  5 04:18 bin/
drwxr-xr-x   3 eariobow  staff   102B Mar  5 04:18 bundles/
drwxr-xr-x  10 eariobow  staff   340B Mar  5 04:18 cli-scripts/
drwxr-xr-x   5 eariobow  staff   170B Mar  5 04:18 docs/
drwxr-xr-x   7 eariobow  staff   238B Mar  5 04:20 domain/
-rw-r--r--   1 eariobow  staff   321K Aug 21  2013 jboss-modules.jar
drwxr-xr-x   4 eariobow  staff   136B Mar  5 04:19 modules/
drwxr-xr-x   8 eariobow  staff   272B Mar  5 04:19 standalone/
drwxr-xr-x   3 eariobow  staff   102B Mar  5 04:19 vault/
-rw-r--r--   1 eariobow  staff   498B Mar  5 04:19 vault.keystore
-rw-r--r--   1 eariobow  staff    87B Mar  5 04:19 version.txt
drwxr-xr-x   9 eariobow  staff   306B Mar  5 04:18 welcome-content/
```

After starting BPM:
```
drwxr-xr-x   3 eariobow  staff   102B Mar  5 04:22 .index/
drwxr-xr-x   4 eariobow  staff   136B Mar  5 04:22 .niogit/
drwxr-xr-x   2 eariobow  staff    68B Mar  5 04:22 .security/
drwxr-xr-x   3 eariobow  staff   102B Mar  5 04:22 repositories/
```
