# Mengkonfigurasi HTTPS/SSL pada JBoss EAP 


> Berikut ini adalah tutorial untuk membuat aplikasi web yang di deploy di JBoss EAP dapat diakses melalui HTTPS.
> Certificate yang akan digunakan adalah self signed certificate yang sebaiknya digunakan hanya untuk keperluan test atau development environment.



1. Buat self signed certificate dengan `keytool`, sebuah tool dari Java (JDK), di direktori `<JBOSS_EAP_HOME>/standalone/configuration`

    ```
    $ cd /Server/EAP-6.4/standalone/configuration
    $ keytool -genkey -keyalg RSA -alias jboss -keystore keystore.jks -storepass secret -validity 3650 -keysize 2048
    What is your first and last name?
      [Unknown]:  http://www.ejlp12.com
    What is the name of your organizational unit?
      [Unknown]:  My WWW
    What is the name of your organization?
      [Unknown]:  EJLP12
    What is the name of your City or Locality?
      [Unknown]:  Jakarta
    What is the name of your State or Province?
      [Unknown]:  Jakarta
    What is the two-letter country code for this unit?
      [Unknown]:  ID
    Is CN=http://www.ejlp12.com, OU=My WWW, O=EJLP12, L=Jakarta, ST=Jakarta, C=ID correct?
      [no]:  Yes

    Enter key password for <jboss>
    	(RETURN if same as keystore password):
    Re-enter new password:
    ```

    Cukup enter pada saat ditanya key password. 
    
    Setelah selesai keystore file (dengan ekstensi `.jks`) akan di-generate di current directory:

    ```
    $ ls -la keystore.jks
    -rw-------  1 ejlp12  wheel  2258 May 16 11:45 keystore.jks
    ```

    Untuk keamanan sebaiknya file tersebut hanya bisa dibaca dan ditulis oleh user yang akan menjalankana JBoss EAP di laptop saya user tersebut adalah `ejlp12`

2.  Tambahkan konfigurasi ini subsystem `<subsystem xmlns="urn:jboss:domain:web:2.2" ...>`

    ```
                <connector name="https" protocol="HTTP/1.1" socket-binding="https" scheme="https" secure="true">
                    <ssl key-alias="jboss" password="secret"
                         certificate-key-file="${jboss.server.base.dir}/configuration/keystore.jks"
                         protocol="TLSv1.2" verify-client="false"
                         certificate-file="${jboss.server.base.dir}/configuration/keystore.jks"/>
                </connector>
    ```
    
    > Nilai `key-alias` dan `password` yang ada di elemen `ssl` tersebut harus sama dengan nilai yang digunakaan saat men-generate jks file (nilai pada argument `-alias` dan `-storepass`)
    
    > Jika menggunakan FQDN (hostname) maka masukan juga elemen alias di elemen `virtual-server` misal `<alias name="www.ejlp12.com"/>`

    Sehingga konfigurasi menjadi seperti ini:
    ```
            <subsystem xmlns="urn:jboss:domain:web:2.2" default-virtual-server="default-host" native="false">
                <configuration>
                    <jsp-configuration x-powered-by="false"/>
                </configuration>
                <connector name="http" protocol="HTTP/1.1" scheme="http" socket-binding="http"/>
                <connector name="https" protocol="HTTP/1.1" socket-binding="https" scheme="https" secure="true">
                    <ssl key-alias="jboss" password="secret"
                         certificate-key-file="${jboss.server.base.dir}/configuration/keystore.jks" protocol="TLSv1.2" verify-client="false"
                         certificate-file="${jboss.server.base.dir}/configuration/keystore.jks"/>
                </connector>
                <virtual-server name="default-host" enable-welcome-root="true">
                    <alias name="localhost"/>
                    <alias name="www.ejlp12.com"/>
                    <access-log pattern="%h %l %u %t %r %s %b %{Referer}i %{User-Agent}i %S %T">
                        <directory path="./"/>
                    </access-log>
                </virtual-server>
            </subsystem>
    ```

3. Pastikan di bagian bawah konfigurasi

     ```
     <socket-binding-group name="standard-sockets" default-interface="public" port-offset="${jboss.socket.binding.port-offset:0}">
        ...
        <socket-binding name="https" port="8443"/>
        ...
     ```
   
4. Restart JBoss EAP dan akses [http://localhost:8443](http://localhost:8443)
