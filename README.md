# Setup Apache Kafka on EC2
# Install prerequisites for Kafka
* Install java
```
sudo apt update
```
Check java version
```
java –version
```
Output
Command 'java' not found, but can be installed with:
```
sudo apt install default-jre
```
Check the java version again
```
java -version
```
Output
openjdk version "11.0.11" 2021-04-20

# Create kafka user
* Login as the root user then create a user called kafka, by running the following commands
```
sudo adduser kafka
```
Enter the password and verify it when prompted
Add the kafka user to the sudo group
```
sudo adduser kafka sudo
```
Now login as kafka user and enter the password you gave above
```
su -l kafka
```
Create another folder called logs in the kafka home directory – this is where the kafka logs will be stored
```
mkdir ~/logs
```
You should have folders as shown on screen-shot below
* Create a Downloads folder where you are going to download the kafka binary to

```
mkdir ~/Downloads
```

* Change to the Downloads folder

```
cd ~/Downloads
```

# Get the Kafka binaries 

```
wget https://downloads.apache.org/kafka/3.6.1/kafka_2.13-3.6.1.tgz
```

Rename the downloaded kafka binary by using the following command

```
mv kafka_2.13-3.6.1.tgz kafka.tgz
```

Create another folder called kafka and change to that folder using the command below
```
mkdir ~/kafka && cd ~/kafka
```

Extract the kafka binary by running the command below

```
tar -xvzf ~/Downloads/kafka.tgz --strip 1
```
# Configure the kafka Server
```
nano ~/kafka/config/server.properties
```
Add the following settings at the end of the file to enable topic deletion and automatic topic creation
```
delete.topic.enable = true
auto.create.topics.enable = true
```
Find the log.dirs entry (should be closer to the top) and change it to the following

```
log.dirs=/home/kafka/logs
```
Save and close the file by pressing Ctrl+S and Ctrl+X
# Create the systemd unit files to Start Services

* Zookeeper Service Unit file
Run the below command to create zookeeper service file
```
sudo nano /etc/systemd/system/zookeeper.service
```
* Copy and paste below code block then save and close the file
```
[Unit]
Requires=zookeeper.service
After=zookeeper.service

[Service]
Type=simple
User=kafka
ExecStart=/bin/sh -c '/home/kafka/kafka/bin/kafka-server-start.sh /home/kafka/kafka/config/server.properties > /home/kafka/kafka/kafka.log 2>&1'
ExecStop=/home/kafka/kafka/bin/kafka-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```

* Kafka Service Unit file
* Run the below command to create kafka service file

```
sudo nano /etc/systemd/system/kafka.service
```
* Copy and paste below code block then save and close the file
```
[Unit]
Requires=zookeeper.service
After=zookeeper.service

[Service]
Type=simple
User=kafka
ExecStart=/bin/sh -c '/home/kafka/kafka/bin/kafka-server-start.sh /home/kafka/kafka/config/server.properties > /home/kafka/kafka/kafka.log 2>&1'
ExecStop=/home/kafka/kafka/bin/kafka-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```
# Start the zookeeper service

* Run the command below to start zookeeper
```
sudo systemctl start zookeeper
```
* Check status by running the command below
```
sudo systemctl status zookeeper
```

# Start the kafka service
* Run the command below to start zookeeper
```
sudo systemctl start kafka
```
* Check status by running the command below
```
sudo systemctl status kafka
```
