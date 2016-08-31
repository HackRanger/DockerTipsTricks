# Introduction
This documentation will help in creating Atlas cloud staging environmetn.
After we start the cloud, we will see what are the scripts contains, how salt is configured, and details about the services are provided.

# Steps
* Add the SSH keys to ~/.ssh/ folder.
``` Bash
ma_staging_rsa
ma_staging_rsa.pub
```

* Create cloud config:
  ``` Bash
  mastaging_init.py aws_small
  ```
* Add env variables:
``` Bash
export MASPOTKEY=secret
export MASTAGING_WINDOWS_USER=Administrator
export MASTAGING_WINDOWS_PASSWORD=<Ask Me>
export MASTAGING_PROXMOX_USERNAME=<Ask Me>
export MASTAGING_PROXMOX_PASSWORD=<Ask Me>
```
* Provision all VM's
``` Bash
mastaging_aws_provision.py cloud_config.yml
```

* Now start salt jobs and start services
```
mastaging_provision.py cloud_config.<UID>.yml
```

# Recommended features

* Add user ssh keys automatically. I feel its not a good idea to use ma_staging_rsa for everyone.

# Diagrams
* Below diagram helps in identifying which services are running on which host. This will help in troubleshooting the problems.

```
+--------------------------------------------------------------------------------------------------------+
|                                                                                                        |
|                                           Containers Diagram                                           |
|                                                                                                        |
+--------------------------------------------------------------------------------------------------------+

	+--------------------+                 +------------------+                         +------------------+
	|elasticsearch-master|                 |  cassandra-1r1   |                         |   mesos-master   |
	+---+--------------------+--+      +-------+------------------+-------+          +------+------------------+---------+
	|m-elasticsearch2:preprod   |      |m-storm-nimbus:preprod            |          |m-singularity:preprod              |
	|m-logstash:preprod         |      |m-kafka:preprod                   |          |m-logstash:preprod                 |
	+---------------------------+      |m-grafana:preprod                 |          |                                   |
	                                   |m-graphite:preprod                |          +-----------------------------------+
	                                   |m-ftp:preprod                     |
	                                   |m-redis:preprod                   |
	                                   |m-cassandra:preprod               |
	                                   |m-mariadb:preprod                 |
	                                   |m-logstash:preprod                |
	                                   +----------------------------------+



	                                             +------------------+
	                                             |   Salt Master    |
	                                             +------------------+
	                                             |                  |
	                                             |                  |
	                                             |    m-logstash    |
	                                             |                  |
	                                             |                  |
	                                             +------------------+







	   +--------------------+                +--------------------+                     +------------------+
	   |Elastic-Search Index|                |Swift-storage-z01r1 |                     |   mesos-slave    |
	 +-+--------------------+---+            ++------------------++              +------+------------------+---------+
	 |m-elasticsearch2:preprod  |             |                  |               |m-kafka-monitor:subbu              |
	 |m-logstash:preprod        |             |                  |               |m-spark-jobserver:preprod          |
	 |                          |             |m-logstash        |               |m-spark-jobserver:preprod          |
	 +--------------------------+             |                  |               |m-spark-jobserver:preprod          |
	                                          |                  |               |m-language-detection-server:preprod|
	                                          +------------------+               |m-web:preprod                      |
	                                                                             |m-prizmdoc:preprod                 |
	                                                                             |m-event-acceptance-service:preprod |
	                                                                             |m-mamta:preprod                    |
	                                                                             |m-kibana:preprod                   |
	                                                                             |m-storm-supervisor:preprod         |
	                                                                             |m-storm-ui:preprod                 |
	                                                                             |m-spark:preprod                    |
	                                                                             |m-spark:preprod                    |
	                                                                             |m-spark:preprod                    |
	                                                                             |m-frontend:frontend-http-version   |
	                                                                             |m-logstash:preprod                 |
	                                                                             +-----------------------------------+



+--------------------------------------------------------------------------------------------------------+
|                                              Role Diagram                                              |
|                                                                                                        |
+--------------------------------------------------------------------------------------------------------+

          +--------------------+       +------------------+     +------------------+
          |elasticsearch-master|       |  cassandra-1r1   |     |   mesos-master   |
          ++------------------++       +------------------+     +------------------+
           |                  |        |mariadb           |     |                  |
           |                  |        |mariadb_first     |     |                  |
           |                  |        |cassandra_seed    |     |                  |
           |elasticsearch_mast|        |cassandra         |     |                  |
           |er                |        |cassandra_coordina|     |   mesos_master   |
           |                  |        |tor               |     |                  |
           |                  |        |graphite          |     |                  |
           |                  |        |grafana           |     |                  |
           |                  |        |ftp               |     |                  |
           +------------------+        +------------------+     +------------------+








                                        +------------------+
                                        |   Salt Master    |
                                        +------------------+
                                        |                  |
                                        |                  |
                                        |   salt_master    |
                                        |                  |
                                        |                  |
                                        +------------------+







            +--------------------+       +--------------------+    +------------------+
            |Elastic-Search Index|       |Swift-storage-z01r1 |    |   mesos-slave    |
            ++------------------++       ++------------------++    +------------------+
             |                  |         |swift_proxy       |     |                  |
             |                  |         |swift_account     |     |                  |
             |   salt_master    |         |swift_container   |     |mesos_slave       |
             |                  |         |swift_object      |     |                  |
             |                  |         |                  |     |                  |
             +------------------+         +------------------+     +------------------+

```
# Development notes:

* Use the following guidelines for development of safe code: [Python Code Safe](https://security.openstack.org/guidelines/)
*
