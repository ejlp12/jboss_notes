# Menkongigurasi JBoss BPM Suite agar menggunakan ProsgreSQL Database



## Buat Module DJBC Driver di JBoss EAP

Download PostgreSQL JDBC driver dari https://jdbc.postgresql.org/download.html
Jika kita menggunakan Java 1.7 atau maka download file driver JDBC41, sedangkan jika JRE yang digunakan adalah versi 1.6
download file driver JDBC4.

1. Buat folder `<BPMS_INSTALL_DIR>/modules/system/layers/base/org/postgresql/main`

2. Simpan file JDBC driver, misalnya `postgresql-9.3-1102.jdbc4.jar` di folder tersebut.
Buat file `module.xml` di folder tersebut yang isinya seperti ini:

	```
	<?xml version="1.0" encoding="UTF-8"?>  
	<module xmlns="urn:jboss:module:1.0" name="org.postgresql">  
	 <resources>  
	 <resource-root path="postgresql-9.3-1102.jdbc4.jar"/>  
	 </resources>  
	 <dependencies>  
	 <module name="javax.api"/>  
	 <module name="javax.transaction.api"/>  
	 </dependencies>  
	</module>
	```

3. Ubah file konfigurasi EAP yang digunakan (`standalone.xml`). 
   
    Pada elemen `datasources` ubah konfigurasi datasource dengan `pool-name` ExampleDS menjadi seperti berikut:

	```
	<datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" 
	            use-java-context="true" spy="true">
	    <connection-url>jdbc:postgresql://localhost:5432/bpms</connection-url>
	    <driver-class>org.postgresql.Driver</driver-class>
	    <driver>postgresql</driver>
	    <security>
	        <user-name>postgres</user-name>
	        <password>password</password>
	    </security>
	</datasource> 
	```

    Tambahkan elemen dibawahnya

	```
	<drivers>
	    <driver name="postgresql" module="org.postgresql">
	        <xa-datasource-class>org.postgresql.xa.PGXADataSource</xa-datasource-class>
	    </driver>
	    ...
	</drivers>
	```

## Menkonfigurasi JBoss BPM Suite

4.  Pada file `<BPMS_INSTALL_DIR>/standalone/deployments/business-central.war/WEB-INF/classes/META-INF/persistence.xml`, ubah line berikut

	```
	<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect" />
	```
      
    Menjadi seperti ini:

	```
	<property name="hibernate.dialect" value="org.hibernate.dialect.PostgreSQLDialect" />
	```

5. Pada file `<BPMS_INSTALL_DIR>/standalone/deployments/dashbuilder.war/WEB-INF/jboss-deployment-structure.xml`, tambahkan line berikut sebelum tag `</dependencies>`:

    ```
    <module name="org.postgresql" export="true" services="import" meta-inf="import"/>
	```
	
6. Start atau restart JBoss BPMS

## Test JDBC driver 

7. Untuk melihat apakah driver sudah benar dikonfigurasi kita bisa cek dengan perintah berikut. Pertama jalankan jboss command-line:

    ```
    cd <BPMS_INSTALL_DIR>/bin
    ./jboss-cli.sh
    ```
    
    Setelah masuk ke command prompt seperti ini `[disconnected /]` 
    
    ```
    connect localhost
    /subsystem=datasources:installed-drivers-list
    
    ```
    
    Jika perintah `connect` diatas tidal berhasil dan muncul error `ERROR: The controller is not available at 127.0.0.1:9999`, coba cek port management JBoss EAP mungkin tidak menggunakan port yang standar.
    
    Hasil dari perintah tersebut seperti ini:
    

    ```
    {
	    "outcome" => "success",
	    "result" => [
	        {
	            "driver-name" => "h2",
	            "deployment-name" => undefined,
	            "driver-module-name" => "com.h2database.h2",
	            "module-slot" => "main",
	            "driver-datasource-class-name" => "",
	            "driver-xa-datasource-class-name" => "org.h2.jdbcx.JdbcDataSource",
	            "driver-class-name" => "org.h2.Driver",
	            "driver-major-version" => 1,
	            "driver-minor-version" => 3,
	            "jdbc-compliant" => true
	        },
	        {
	            "driver-name" => "postgresql",
	            "deployment-name" => undefined,
	            "driver-module-name" => "org.postgresql",
	            "module-slot" => "main",
	            "driver-datasource-class-name" => "",
	            "driver-xa-datasource-class-name" => "org.postgresql.xa.PGXADataSource",
	            "driver-class-name" => "org.postgresql.Driver",
	            "driver-major-version" => 9,
	            "driver-minor-version" => 3,
	            "jdbc-compliant" => false
	        }
	    ]
	}
    ```
    
8. Test koneksi dengan menggunakan perintah berikut dari JBoss CLI prompt:

    ```
    /subsystem=datasources/data-source=ExampleDS:test-connection-in-pool
    ```
    
    ```
    { 
       "outcome" => "success", 
       "result" => [true] 
    }
    ```
    
## Cara membuat DataSource dan JDBC driver module dengan command-line

Dengan menggunakan `jboss-cli.sh`

```
module add --name=org.mysql --resources=/path/to/mysql-connector-java-5.1.18-bin.jar --dependencies=javax.api,javax.transaction.api
 
/subsystem=datasources/jdbc-driver=mysql:add(driver-module-name=org.mysql,driver-name=mysql,driver-class-name=com.mysql.jdbc.Driver)
 
/subsystem=datasources/data-source=MySQLDS:add(jndi-name=java:jboss/datasources/MySQLDS, driver-name=mysql, connection-url=jdbc:mysql://localhost:3306/dbdev,user-name=root,password=admin)
```
