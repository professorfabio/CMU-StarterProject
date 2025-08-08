# CMU Starter Project

This repository contains the three main components used in the class project, namely:

1. IoT Code: Code to run on the Raspberry Pi, which acts as an IoT device with sensors and actuators. Obs.: If a Raspberry Pi sensor kit is not available, you may run this component on a usual machine (either local or on the cloud), simulating the temperature and light sensors (e.g., with a routine that generates random temperature and luminosity values), as well as the actuator (e.g., replacing the led with a simple on-off variable).

2. Cloud Code: Code to run on the cloud servers, composed of two parts: Kafka client (consumer and producer); and gRPC-based Web service that represents an IoT device (as a rudimentary digital twin of the device). 

3. Client Code: command line gRPC clients used to illustrate access to the the Web service that represents the IoT device.

## Steps to run the demo:

### a. Start Kafka on a cloud-based server (server-1):

#### Installing Kafka (only when doing it for the first time):
- Install Java (JDK)
```
$ sudo apt update

$ sudo apt install default-jdk
```
- Download, install (just uncompress) and configure Apache Kafka. See instructions in Kafka's Quickstart page: https://kafka.apache.org/quickstart or, else, ask ChatGPT at your own risk ;-)

$ curl --output kafka_2.13-4.0.0.tgz https://dlcdn.apache.org/kafka/4.0.0/kafka_2.13-4.0.0.tgz

$ tar -xzf kafka_2.13-4.0.0.tgz

$ cd kafka_2.13-4.0.0/

$ KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"

$ bin/kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties

Important: Edit config/server.properties to uncomment the line starting with advertised_listeners and replace the domain name with the public IP address of the machine where the Broker will run (server-1). It is recommended to use a fixed public IP address for this machine.

#### Once Kafka is properly installed and configured (see above), run both the Zookeeper and Kafka servers: 

$ bin/kafka-server-start.sh config/server.properties

### b. On another cloud-based server (server-2):

#### When doing it for the first time:

- Install gRPC for Python - see instructions on https://grpc.io/docs/languages/python/quickstart/

- Clone the repo: 

$ git clone https://github.com/professorfabio/CMU-StarterProject

- Compile the interface (protocol buffer definition):

$ cd CloudCode/python

$ python3 -m grpc_tools.protoc -I../protos --python_out=. --grpc_python_out=. ../protos/iot_service.proto

#### Run the Kafka client and the Web service that represents an IoT device:

- Run virtual_device_service.py (it contains the cloud-based Consumer and Producer, and well as the gRPC Web service):

$ python3 virtual_device_service.py

(If necessary, edit the const.py file with the IP address of the Kafka Broker -- server-1)

### c. On the Raspberry Pi:

#### When doing it for the first time:

- Install the Kafka Python client:

$ pip3 install kafka-python

(If necessary, install python3-pip first)

Obs.: read these instructions to enable communication with the temperature sensor via GPIO: https://www.waveshare.com/wiki/Raspberry_Pi_Tutorial_Series:_1-Wire_DS18B20_Sensor

(If necessary, edit the const.py file with the **public** IP address of the Kafka Broker -- server-1)

- Clone the repo:

$ git clone https://github.com/professorfabio/CMU-StarterProject

(If necessary, install git)

#### If the Kafka Python client has already been installed and no changes have been made to the code in the repo, jump straight to this step:

- Run device-controller.py (it contains IoT-based Producer and Consumer, which produce events from sensors and consume events for the actuators)

$ cd IoTCode

$ python3 device-controller.py

### d. On a client machine (may be on the cloud or on a local machine):

- Clone the repo:

$ git clone https://github.com/professorfabio/CMU-StarterProject

- Compile the interface:

$ cd ClientCode/python

$ python3 -m grpc_tools.protoc -I../protos --python_out=. --grpc_python_out=. ../protos/iot_service.proto

- Run the client code (for led control and thermometer access)

$ python3 thermometer_client.py

$ python3 led_client.py 1 red  --or-- $ python3 led_client.py 0 red (turn on and off, respectively. Just examples)

(If necessary, edit the const.py file with the **public** IP address of the gRPC server -- server-2)

## Overall structure of the system

![image](https://user-images.githubusercontent.com/13460193/204534405-b17b1abb-77e1-479a-8171-807dc610ee5d.png)
