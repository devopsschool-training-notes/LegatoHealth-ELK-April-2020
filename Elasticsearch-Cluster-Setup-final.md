Elasticsearch Cluster Setup and Configuration Tutorial step by step in AWS ec2.

### Step 1. File Descriptor

```json
sudo vi /etc/security/limits.conf
ec2-user  -  nofile  65535

This change will only take effect the next time the ec2-user user opens a new session.
```

### Step 2. Adjust vm.max_map count to recommended level

To make it persistent, you can add this line:
```json
$ sudo vi /etc/sysctl.conf
vm.max_map_count=262144
$ sudo sysctl -p
```

### Step 3. List down Public IP and Private IP and decide their roles.
|Role	| Public IPs		| 	Private IPs|
| :------------ |:---------------:| -----:|
|Master 	|13.234.59.125		|172.31.20.87|
|Data	|13.126.179.145			|172.31.30.190|		
|Client	|35.154.20.21			|172.31.23.201|

### Step 4. Change host name of each machine
In Master (Pub-13.234.59.125 Pvt-172.31.20.87)
```sh
$ sudo hostnamectl set-hostname es-master01
$ hostname
```

In Data (Pub-13.126.179.145 Pvt-172.31.30.190)
```sh
$ sudo hostnamectl set-hostname es-data01
$ hostname
```

In Client (Pub-35.154.20.21 Pvt-172.31.23.201)
```sh
$ sudo hostnamectl set-hostname es-client01
$ hostname
```

### Step 5. Modify /etc/hosts with following in Master(s), Data(s) and Client(s) node

In 13.234.59.125(Master)
```json
$ sudo vi /etc/hosts

172.31.20.87	es-master01
172.31.30.190	es-client01
172.31.23.201	es-data01
```


### Step 6. download and Install Elasticsearch 

### Step 7. Set the minimum and maximum heap size

The more heap available to Elasticsearch, the more memory it can use for its internal caches, but the less memory it leaves available for the operating system to use for the filesystem cache. Also, larger heaps can cause longer garbage collection pauses.

**Method 1 - in jvm.options file**
Here are examples of how to set the heap size via the jvm.options file:
```json
-Xms1g 	# Set the minimum heap size to 1g.
-Xms2g 	# Set the minimum heap size to 2g.
-Xmx2g 	# Set the maximum heap size to 2g.
```

**Method 2 - Set environment variable using ES_JAVA_OPTS**
```json
**Set the minimum 1000 MB and maximum heap size to 2000 MB.**
ES_JAVA_OPTS="-Xms1000m -Xmx2000m" ./bin/elasticsearch 

**Set the minimum and maximum heap size to 2 GB.**
ES_JAVA_OPTS="-Xms2g -Xmx2g" ./bin/elasticsearch 

**Set the minimum and maximum heap size to 4000 MB.**
ES_JAVA_OPTS="-Xms4000m -Xmx4000m" ./bin/elasticsearch 
```

**Recommended: For 1 CPU and 2 GB RAM for demo - this is recommended settings.**
```sh
CLIENT
-Xms1g
-Xmx1g

Master
-Xms2g
-Xmx2g

DATA
-Xms1g
-Xmx1g
```

### Step 8. Configure Elasticsearch for 3 node Cluster

**In Master (Pub-13.234.59.125 Pvt-172.31.20.87)**

```json
cluster.name: devopsschool
node.name: es-master01
node.master: true
node.data: false
network.host: 172.31.20.87
cluster.initial_master_nodes: ["es-master01"]
```
**In Data (Pub-13.126.179.145 Pvt-172.31.30.190)**
```json
cluster.name: devopsschool
node.name: es-client01
node.master: false
node.data: false
network.host: 172.31.30.190
cluster.initial_master_nodes: ["es-master01"]
```

**In Client (Pub-35.154.20.21 Pvt-172.31.23.201)**
```json
cluster.name: devopsschool
node.name: es-client01
node.master: false
node.data: false
network.host: 172.31.23.201
cluster.initial_master_nodes: ["es-master01"]
```
### Step 9. Run it








