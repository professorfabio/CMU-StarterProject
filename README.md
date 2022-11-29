# SSU-2022_2-StarterProject

This repository contains the three main components used in the class project, namely:

1. IoT Code: Code to run on the Raspberry Pi, which will act as an IoT device with sensors and actuators

2. Cloud Code: Code to run on the cloud servers - Kafka consumer and producer; gRPC IoT service

3. Client Code: command line gRPC clients of the IoT service

## Steps to run the demo:

### a. Start Kafka on a cloud-based server:

$ bin/zookeeper-server-start.sh config/zookeeper.properties

$ bin/kafka-server-start.sh config/server.properties


### b. On another cloud-based server:

Run virtual_device_service.py (it contains the cloud-based Consumer and Producer, and well as the gRPC service)

### c. On the Raspberry Pi:

Run device-controler.py (it contains IoT-based Producer and Consumer, which produce events from sensors and consume events for the actuators)

### d. On a client machine (may be on the cloud or on a local machine):

Run the client code (for led control and thermometer access)

## Overall structure of the system

![image](https://user-images.githubusercontent.com/13460193/204534405-b17b1abb-77e1-479a-8171-807dc610ee5d.png)
