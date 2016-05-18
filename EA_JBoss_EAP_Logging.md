Ada beberapa log file yang dapat dihasilkan oleh JBoss EAP.

* Console log
  
  Jika menggunakan script `jboss-as-standalone.sh` atau `jboss-as-domain.sh` yang ada di `<JBOSS_HOME_DIR>/bin/init.d/` sebagai wrapper untuk men-start JBoss EAP, console log akan disimpan di file `/var/log/jboss-as/console.log`. 
  
  Konfigurasi start/stop wrapper script tersebut ada di `<JBOSS_HOME_DIR>/bin/init.d/jboss-as.conf`
  
  ```
  # Location to keep the console log
  #
  # JBOSS_CONSOLE_LOG=/var/log/jboss-as/console.log
   ```
  
  
* File log utama: `<JBOSS_HOME_DIR>/standalone/logs/server.log`


