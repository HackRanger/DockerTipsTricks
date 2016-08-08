__My Conclusion: Just deploy it and see the usefulness.__

# Notes by others:
1. [Kafka manager: Initial notes from others](http://edbaker.weebly.com/blog/install-and-evaluation-of-yahoos-kafka-manager): Conclusion ( But its old from Mar.2015 ):

>  As I started playing around with Kafka Manager it became apparent to me that this tool was only intended to be used as a UI for administration tasks, not for monitoring health (such as lag) of existing production topics. So soon enough I will also be looking at web-console

----

# Pros
1. Not just UI
2. Gives live view of consumer, topics, partitions, replica etc.
3. Partition management
4. Can hold configuration for kafka (Some things which is nice , sometime not so nice)
5. Consumer lags are displayed. Ops can identify easily if the consumers needs to be restarted.
6. Can be extended.
7. Can be used when our monitoring fails.

-----
# Cons
1. No view for zookeeper & kafka combined
2. Another attempt create a configuration manager for a specific application. than going for existing CM's.
3. We get metrics into monitoring and manage kafka containers with other tools.  

----

# Questions
1. Right now what's important?
   1. Getting metrics about the kafka or managing it?
   2. Or managing the partitions / replica
2. What's important later when we have kafka in production?
   1. Aggregate the metics in one place.
   2. Manage the  cluster from one tool.
   3. Use with some CM tools to configure kafka.  
3. If not kafka manager: How do we manage kafka? Docker + sysmtemd ?


----

### Some devops notes:
#### Dev related:
1. Number of consumers
2. Number of topic
3. Number of message per second

#### Ops related:
1. Number of configured partitions in each topic
2. Number of nodes in kafka cluster
3. Zookeeper failover
4. Managing consumers
5. Managing Brokers
6. Managing producers
7. Throughput for each kafka cluster
8. Tolerable latency
9. Systemd for managing kafka clusters?
