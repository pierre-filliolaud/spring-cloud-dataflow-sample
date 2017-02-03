# ===  SPRING CLOUD DATAFLOW

## Quick Start

### Step 1 - Download the Spring Cloud Data Flow Local Server and Shell apps:


	wget http://repo.spring.io/release/org/springframework/cloud/spring-cloud-dataflow-server-local/1.1.2.RELEASE/spring-cloud-dataflow-server-local-1.1.2.RELEASE.jar

	wget http://repo.spring.io/release/org/springframework/cloud/spring-cloud-dataflow-shell/1.1.2.RELEASE/spring-cloud-dataflow-shell-1.1.2.RELEASE.jar

### Step 2 - Download and Start Kafka 0.10 [used as: messaging middleware]
	./bin/zookeeper-server-start.sh config/zookeeper.properties
	./bin/kafka-server-start.sh config/server.properties

### Step 3 - Launch the Data Flow Local Server 
	java -jar ./lib/spring-cloud-dataflow-server-local-1.1.2.RELEASE.jar

### Step 4 - Launch Shell on the same machine where the Data Flow Local Server is runnign
	java -jar ./lib/spring-cloud-dataflow-shell-1.1.2.RELEASE.jar

### Step 5 - Import all the out-of-the-box application coordinates in bulk
	dataflow:>app import --uri http://bit.ly/Avogadro-SR1-stream-applications-kafka-10-maven

### Step 6 - Create ‘ticktock’ Stream
	dataflow:>stream create ticktock --definition "time | log" --deploy

You'll notice the following in ‘Local’ Server console.


2016-07-18 22:08:24.777  INFO 73058 --- [nio-9393-exec-9] o.s.c.d.spi.local.LocalAppDeployer       : deploying app ticktock.log instance 0
   Logs will be in /var/folders/c3/ctx7_rns6x30tq7rb76wzqwr0000gp/T/spring-cloud-dataflow-5011521526937452211/ticktock-1468904904769/ticktock.log
2016-07-18 22:08:25.081  INFO 73058 --- [nio-9393-exec-9] o.s.c.d.spi.local.LocalAppDeployer       : deploying app ticktock.time instance 0
   Logs will be in /var/folders/c3/ctx7_rns6x30tq7rb76wzqwr0000gp/T/spring-cloud-dataflow-5011521526937452211/ticktock-1468904905074/ticktock.time

### Step 7 - Verify the ‘ticktocks’: 
	tail -f /var/folders/ ... /ticktock.log/stdout_0.log

### Step 8 - Launch Dashboard at: 
	http://localhost:9393/dashboard
	
### Register custom module already in local maven
	mvn clean install -DskipTests

	dataflow:>module register --name my_custom_module --type source --uri maven:com.mycompany:mycustommodule:0.0.1-SNAPSHOT --force
	
### Info module
	dataflow:>module info source:my_custom_module
	
### Detroy all stream
	dataflow:>stream all destroy