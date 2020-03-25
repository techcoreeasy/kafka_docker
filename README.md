# kafka_docker
This project will give simple steps to implement the local kafka cluster.

@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 1 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


git clone https://github.com/wurstmeister/kafka-docker

cd kafka-docker


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 2 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Edit docker-compose.yml
// 1. Change the docker host IP in KAFKA_ADVERTISED_HOST_NAME
// On macOS use the following script to set DOCKERHOST env var
// DOCKERHOST=$(ifconfig | grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" | grep -v 127.0.0.1 | awk '{ print $2 }' | cut -f2 -d: | head -n1)
// https://github.com/wurstmeister/kafka-docker/issues/17#issuecomment-370237590



// Know your host IP by below command 

ifconfig

//  Edit docker-compose.yml with Change the docker host IP in KAFKA_ADVERTISED_HOST_NAME



@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 3 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Start services based on the default docker-compose.yml
// You may want to use -d to detach from the terminal (daemon mode)

docker-compose up


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 4 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Connect to the container of the Kafka broker
// NOTE: Asciidoc would try to substitute curly braces
//       Remove the spaces between curly braces

docker-compose ps


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 5 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Print out the topics
// You should see no topics listed

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --list


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 6 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Create a topic t1

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --create --topic t1 --partitions 3 --replication-factor 1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 7 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Describe topic t1
docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --describe --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 8 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Connect with the Kafka console consumer in another terminal

docker exec -t kafka-docker_kafka_1 kafka-console-consumer.sh --bootstrap-server :9092 --group tech-core-easy --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 9 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Connect with the Kafka console producer in one terminal

docker exec -it kafka-docker_kafka_1 kafka-console-producer.sh --broker-list :9092 --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Final Clean Up !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Type a message in producer window to see the message printed out in consumer's
// Observe the logs of the running docker-compose up (with no -d)
// Or use docker logs -f kafka-docker_kafka_1

// Ctrl-C to shut down all the processes

// You may also consider removing the containers
// so you always start afresh
// docker-compose rm
