EA_JBoss_EAP_Basic_Tuning

## JVM Heap Size

Change minimum and maximum JVM heap size in `bin/standalone.conf` or `bin/standalone.conf.bat` (In Windows)

```
# Specify options to pass to the Java VM.
#
if [ "x$JAVA_OPTS" = "x" ]; then
	JAVA_OPTS="-Xms1024m -Xmx1024m -XX:MaxPermSize=256m -XX:NewRatio=2 -XX:PermSize=64m -Djava.net.preferIPv4Stack=true"
 	JAVA_OPTS="$JAVA_OPTS -Djboss.modules.system.pkgs=$JBOSS_MODULES_SYSTEM_PKGS -Djava.awt.headless=true"
else
 	echo "JAVA_OPTS already set in environment; overriding default settings with values: $JAVA_OPTS"
fi```


## Konfigurasi data source & connection pool:

1. Set connection pool min & max size. Dengan mendefinisikan parameter ini, pembuatan object koneksi ke database jadi lebih efisien.
   Nilai min & max harus di-tuning dengan cara memonitor penggunaan koneksi pada saat load test.
2. Set agar query yang dijalankan menggunakan prepared statement akan di-cache sehingga lebih cepat.
   Nilai ini akan membantu jika di kode aplikasi kita banyak menggunakan object PreparedStatement untuk menjalankan query database
3. Set flush-strategy menjadi IdleConnections agar jika ada koneksi yang error dan idle akan di-flush dari pool
4. Set idle-timeout-minutes agar koneksi yang idle (tidak digunakan) dibuang dari pool. 
   Kadang koneksi yang idle sebenarnya sudah tidak valid karena koneksi sudah dibuang di sisi database (konfigurasi timeout di database)
5. Set query-timeout agar koneksi yang digunakan untuk query yang terlalu lama akan distop sehingga tidak menghabiskan pool

 ```
 <datasources>
 	...
    <pool>
        <min-pool-size>10</min-pool-size>
        <max-pool-size>100</max-pool-size>
        <prefill>true</prefill>
        <flush-strategy>IdleConnections</flush-strategy>
        <prepared-statement-cache-size>50</prepared-statement-cache-size>
        <idle-timeout-minutes>5</idle-timeout-minutes>
        <blocking-timeout-millis>5000</blocking-timeout-millis>
    	<query-timeout>300</query-timeout>
    </pool>
 </datasources>
```

## Disable deployment scanner:

Pada mode Domain, deployment scanner secara default tidak diaktifkan. Jika anda menggunakan mode *standalone*, anda bisa mematikan fasilitas ini agar tidak memberatkan server atau mengubah `scan-interval` agar tidak terlalu sering melakukan pengecekan ke folder deployment.

```
<subsystem xmlns="urn:jboss:domain:deployment-scanner:1.0">
   <deployment-scanner scan-interval="5000"
      relative-to="jboss.server.base.dir" path="deployments" scan-enabled="false"/>
</subsystem>
```

----

For some reason the pooling of stateless EJBs is disabled per default in WildFly. If you have  expensive initialization in your stateless session EJBs it is very useful to add pooling for that kind of beans. This can increase the performance of your application dramatically.

Edit the standalone.xml file in the WildFly /standalone/configuration/ directory and change the section “session-bean” like in the following example:

```
 <subsystem xmlns="urn:jboss:domain:ejb3:2.0">
   <session-bean>
       <stateless>
           <bean-instance-pool-ref pool-name="slsb-strict-max-pool"/>
       </stateless>
       <stateful default-access-timeout="5000" 
                 cache-ref="simple" 
                 passivation-disabled-cache-ref="simple"/>
       <singleton default-access-timeout="5000"/>
   </session-bean>
   <pools>
       <bean-instance-pools>
           <strict-max-pool name="slsb-strict-max-pool" 
                            max-pool-size="32" 
                            instance-acquisition-timeout="5" 
                            instance-acquisition-timeout-unit="MINUTES"/>
           <strict-max-pool name="mdb-strict-max-pool" 
				           max-pool-size="20" 
				           instance-acquisition-timeout="5" 
				           instance-acquisition-timeout-unit="MINUTES"/>
       </bean-instance-pools>
    </pools>
    ...
```


http://ralph.soika.com/wildfly-performance-tuning/
