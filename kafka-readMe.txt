Kafka Local setup-> https://www.geeksforgeeks.org/how-to-install-and-run-apache-kafka-on-windows/


1->Open a new command prompt in the location C:\kafka_2.11-0.9.0.0\bin\windows.
2-> hit cmd: 
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test


==> Producer: Open a new command prompt in the location C:\kafka_2.11-0.9.0.0\bin\windows
1-> hit cmd:
kafka-console-producer.bat --broker-list localhost:9092 --topic test


==> Consumer: Open a new command prompt in the location C:\kafka_2.11-0.9.0.0\bin\windows
1-> hit cmd:
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test


For List Topics: kafka-topics.bat --list --zookeeper localhost:2181 
