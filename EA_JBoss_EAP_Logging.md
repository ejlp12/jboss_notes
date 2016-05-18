Ada beberapa log file yang dapat dihasilkan oleh JBoss EAP.

## Console log
  
  Jika menggunakan script `jboss-as-standalone.sh` atau `jboss-as-domain.sh` yang ada di `<EAP_HOME>/bin/init.d/` sebagai wrapper untuk men-start JBoss EAP, console log akan disimpan di file `/var/log/jboss-as/console.log`. 
  
  Konfigurasi start/stop wrapper script tersebut ada di `<EAP_HOME>/bin/init.d/jboss-as.conf`
  
  ```
  # Location to keep the console log
  #
  # JBOSS_CONSOLE_LOG=/var/log/jboss-as/console.log
   ```
  
  
## File log utama: 

   File log dari Jboss EAP yang utama ada di 
   
   * Standalone: `<EAP_HOME>/standalone/logs/server.log`
   * Domain: `<EAP_HOME>/domain/servers/<SERVER_NAME>/log/server.log`
   * Domain host controller: `<EAP_HOME>/domain/log/host-controller.log`
   * Process controller: `<EAP_HOME>/domain/log/process-controller.log`

## File handler & Formatter

   Contoh file handler dengan custom formater
   ```
   <periodic-size-rotating-file-handler name="sample-handler" autoflush="false">
     <formatter>
       <pattern-formatter pattern="%d{HH:mm:ss,SSS} %-5p [%c] (%t) %s%E%n"/>
     </formatter>
     <level name="DEBUG"/>
     <file relative-to="jboss.server.log.dir" path="Sample.log"/>
     <max-backup-index value="10"/>
     <suffix value=".yyyy.MM.dd"/>
     <append value="true"/>
   <periodic-size-rotating-file-handler>
   ```

## Garbage Collector (GC) Log

   GC log sangat bermanfaat untuk diagnosis (analisis) performance JBoss EAP. Secara default, log ini sudah di-enable dan disimpan di
   
   `EAP_HOME/standalone/log/gc.log.digit` -> digit adalah angka 
   
   Untuk meng-enable GC log, ubah konfigurasi file `standalone.conf` atau `standalone.conf.bat` (untuk Windows) yang ada di direktori `<JBOSS_HOME_DIR>/bin/` dan masukan `-verbose:gc` sebagai argumen dari java command line. 

   ```
   #
   # Specify options to pass to the Java VM. 
   #
   if [ "x$JAVA_OPTS" = "x" ]; then
      JAVA_OPTS="-Xms1303m -Xmx1303m -XX:MaxPermSize=256m -Djava.net.preferIPv4Stack=true"
      JAVA_OPTS="$JAVA_OPTS -Djboss.modules.system.pkgs=$JBOSS_MODULES_SYSTEM_PKGS -Djava.awt.headless=true"
      JAVA_OPTS="$JAVA_OPTS -Djboss.modules.policy-permissions=true"
      JAVA_OPTS="$JAVA_OPTS -verbose:gc -Xloggc:/opt/jboss/jboss-eap-6.4/standalone/log/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=3M -XX:-TraceClassUnloading"
   else
      echo "JAVA_OPTS already set in environment; overriding default settings with values: $JAVA_OPTS"
   fi
   ```

   Untuk membuat file `gc.log` tidak terus membesar maka dapat ditambahakan argumen berikut:
   ```
   -Xloggc:/direktori/lokasi/file/log/gc.log 
   -XX:+PrintGCDetails 
   -XX:+PrintGCDateStamps 
   -XX:+UseGCLogFileRotation 
   -XX:NumberOfGCLogFiles=5 
   -XX:GCLogFileSize=3M 
   -XX:-TraceClassUnloading"
   ```



