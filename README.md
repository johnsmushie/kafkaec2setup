# Setup Apache Kafka on EC2
* Create a Downloads folder where you are going to download the kafka binary to

```
mkdir ~/Downloads
```

* Change to the Downloads folder

```
cd ~/Downloads
```

* Get the binaries by running the wget command below (and pasting the link you copied from the apache,org website)

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

