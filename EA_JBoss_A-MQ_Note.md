Download A-MQ
JConsole 1.6.0_30-b12

Untuk menjalankan AMQ secara interaktif, yaitu kita masuk ke console Karaf OSGi engine, gunakan perintah `./bin/amq`
perintah ini sama dengan perintah `bin/karaf`

> Perintah start lainnya:
> * `./bin/start` untuk menjalankan A-MQ dalam mode background tanpa masuk ke console
> * `./bin/standalone` untuk menjalankan A-MQ dalam mode standalone walaupun sudah dikonfigurasi cluster.

```
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=128M; support was removed in 8.0
Please wait, JBoss A-MQ is initializing...
100% [========================================================================]

      _ ____                                __  __  ____
     | |  _ \                    /\        |  \/  |/ __ \
     | | |_) | ___  ___ ___     /  \ ______| \  / | |  | |
 _   | |  _ < / _ \/ __/ __|   / /\ \______| |\/| | |  | |
| |__| | |_) | (_) \__ \__ \  / ____ \     | |  | | |__| |
 \____/|____/ \___/|___/___/ /_/    \_\    |_|  |_|\___\_\

  JBoss A-MQ (6.2.0.redhat-133)
  http://www.redhat.com/products/jbossenterprisemiddleware/amq/

Hit '<tab>' for a list of available commands
and '[cmd] --help' for help on a specific command.

Open a browser to http://localhost:8181 to access the management console

Hit '<ctrl-d>' or 'osgi:shutdown' to shutdown JBoss A-MQ.

JBossA-MQ:karaf@root>
```
Beberapa perintah dasar untuk 

* `query` untuk melihat konfigurasi atribut dari A-MQ broker serta statistik
  
  `query --view ProducerCount,ConsumerCount` untuk melihat atribut atau statistic tertentu 
  
  `query -QQueue=FOO.BAR` untuk melihat atribut atau statistic tertentu untuk suatu queue tertentu, misal queue dengan nama FOO.BAR
* `bstat` - Performs a predefined query that displays useful statistics regarding the specified broker

  ```
  BrokerName = amq
  TotalEnqueueCount = 1
  TotalDequeueCount = 0
  TotalMessageCount = 2
  TotalConsumerCount = 0
  Uptime = 10 hours 52 minutes
  
  Name = FOO.BAR
  destinationName = FOO.BAR
  destinationType = Queue
  EnqueueCount = 0
  DequeueCount = 0
  ConsumerCount = 0
  DispatchCount = 0
  ```

* `dstat` - Performs a predefined query that displays useful tabular statistics regarding the specified destination type

  ```
  Name                                    Queue Size  Producer #  Consumer #   Enqueue #   Dequeue #   Forward #
  ActiveMQ.Advisory.MasterBroker                   0           0           0           1           0           0
  FOO.BAR                                          2           0           0           0           0           0
  ```
* `stop` men-stop broker
* `restart` men-stop broker yang sedang jalan dan menjalankan kembali broker baru


* Lihat log file dari AMQ di `<AMQ_INSTALL_DIR>/data/amq.log`
* Lihat log file dari Karaf engine di `<AMQ_INSTALL_DIR>/data/`

`activemq:list`

brokerName = amq
