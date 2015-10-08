Beberapa JMX resource yang mungkin perlu dimonitor:

	```
	/subsystem=web/connector=http/:read-resource(recursive-depth=0,include-runtime=true)
	  errorCount: Number of error that occurs when processing requests by the connector.
	  processingTime: Processing time used by the connector. Im milli-seconds.
	  maxTime: Max time spent to process a requests
	  
	/deployment=HelloWebApp.war/:read-resource(recursive=true,include-runtime=true,include-defaults=true)
	  expired-sessions
	  active-sessions
	  session-avg-alive-time
	  

	/subsystem=ejb3/:write-attribute(name=enable-statistics,value=true)

	/subsystem=ejb3/thread-pool=default/:read-resource(recursive=true,,include-runtime=true)
	  active-count
	  largest-thread-count
	/subsystem=ejb3/:read-resource(recursive=true,include-runtime=true)

	/subsystem=jca/workmanager=default/:read-resource(recursive=true,include-runtime=true)
	```
