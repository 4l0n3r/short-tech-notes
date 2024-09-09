Kafka

Source - > Target 

Cluster :
	Broker :
		Topics :
			Partition :
				Increased Id
				many partitions
				limited time
				can’t be changed
 
Data is replicated
	leader and members

Producer :
	Sending key with data :
		same key data will go to same partition

Consumer :
	Consumer Group:
		consumer group members should be less than or equal to partitions
	reading in order within partition and parallel across topics
	Consumer offset :
		used to read data from last offset if the consumer dies in between