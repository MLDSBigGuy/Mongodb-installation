# Mongodb-installation
Mongodb installation steps in mac 

## 1. Install Mongodb 
  - cd mongodb-folder-location
  
## 2. Create a /data/db directory at root as superuser. 
- sudo chown -R $USER /data/db
  
## 3. Start server
  - cd to /mongodb-folder-location/bin
  - ./mongod
  
## 4. start mongo shell
  -  cd to /mongodb-folder-location/bin
  - ./mongo --host 127.0.0.1:27017
      - shell commands: 
        - show dbs (shows all dbs)
        - db (shows current db)
        - use foo (use a foo db) 
        - help (general help)
        - ctrl- l: clear shell screen
        - ctrl-k: truncate the right hand text
        - show collections (this is not a realtional db. cant get docs from diff. collections 
        - Objectid() creates _id: if u forgot to create one! This Objectid have many properties like date-time etc.,. tHIS ID CAN MAKE U TO UNIQUELY RETRIEVE DOCS!
 
 ## 5. Data storage
    - doc must have _id field
    - size of doc is 16MB. to store more than that, store across multiple docs n use GridFD package
    

### 1. Open hadoop-env.sh and export your system java path, Hadoop installation path 
```xml
  export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home
  export HADOOP_PREFIX=/usr/local/Cellar/hadoop/3.0.0
  If using jdk 9, add export HADOOP_OPTS="--add-modules java.activation"
```
### 2. Open core-site.xml and add below data:

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```
### 3. Open hdfs-site.xml and add below data:
```xml
<configuration>

  <property>
     <name>dfs.replication</name>
     <value>1</value>
     <description> How many copies of data our hadoop will have. Value defines how many copies </description>
  </property>

  <property>
     <name>dfs.namenode.name.dir</name>
     <value>file:/usr/local/Cellar/hadoop/3.0.0/hadoop_tmp/namenode</value>
  </property>

  <property>
     <name>dfs.datanode.name.dir</name>
     <value>file:/usr/local/Cellar/hadoop/3.0.0/hadoop_tmp/datanode</value>
  </property>

  <property>
    <name>dfs.permissions</name>
    <value>false</value>
  </property>

</configuration>
```
### 4. Open mapred-site.xml and add below data:
```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
    <description> Mapreduce - maps the data and reduces that data to give it to you. This is done by deamon. This tracks all jobs running in hadoop cluster. </description>
  </property>
</configuration>
```
### 5. Open yarn-site.xml and add below data: 
```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>

</configuration>
```

### 6.start name node 
- /usr/local/Cellar/hadoop/3.0.0/bin/hadoop namenode -format 

### 7. start or stop the hadoop. 

- Start - '/usr/local/Cellar/hadoop/3.0.0/sbin/start-dfs.sh'  or   '/usr/local/Cellar/hadoop/3.0.0/sbin/start-all.sh'
- Stop - '/usr/local/Cellar/hadoop/3.0.0/sbin/stop-dfs.sh'   or    '/usr/local/Cellar/hadoop/3.0.0/sbin/stop-all.sh'


## Output after succesful hadoop installation looks like below:
```xml
krishnas-mbp:sbin krishna.damarla$ ./start-dfs.sh 
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [krishnas-mbp.widas.de]
2018-04-10 14:14:40,129 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```
(Last message is just a warning. Can be neglected) 

## jps in terminal shows all the processes running as below:
```xml
898 
9493 DataNode
10553 Jps
9630 SecondaryNameNode
9391 NameNode
```
* Note: If any one of the node is not running when u start jps, refer issues at the end. 

## URLs:
- From hadoop 3.0.0, 
    - Namenode moved from http://localhost:50070 to http://localhost:9870 
    - Datanode moved to http://localhost:9864
- For other ports info, [check this](https://issues.apache.org/jira/browse/HDFS-9427?focusedCommentId=15156476&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-15156476)
   - Namenode ports: 50470 --> 9871, 50070 --> 9870, 8020 --> 9820
   - Secondary NN ports: 50091 --> 9869, 50090 --> 9868
   - Datanode ports: 50020 --> 9867, 50010 --> 9866, 50475 --> 9865, 50075 --> 9864

### Create/store files in Hadoop

- Create dir - bin/hadoop dfs -mkdir /input 
- List files in hdfs - hdfs dfs -ls /
- Submit a map reduce job: https://app.pluralsight.com/player?course=building-blocks-hadoop-hdfs-mapreduce-yarn&author=janani-ravi&name=building-blocks-hadoop-hdfs-mapreduce-yarn-m2&clip=6&mode=live
- 1 hadoop cluster can have One name node, many data nodes
  - If data in hdfs is considered as book, namenode is table of contents, chapters/content in book are datanodes 
  Namenode responsibilites: No data is read from node. Any req from client is passed to namenode. It knows while file located in which node. Any permissions/ how file split up, where is the replica of file
  - Datanode stores unformated/unstructured data
- Storing data in hdfs
  - best file size in a block is 128mb
  - time taken to read block of data, time taken to seek block and time taken to read data. Optimum ratio is maintained with 128mb
  - Read file is 2 step process 
    - locate all split up files
    - [read files](https://github.com/MLDSBigGuy/Hadoop-installation/blob/master/Screen%20Shot%202018-04-12%20at%2010.05.18.png)
- Access hadoop through cmd line (make sure u r located at /usr/local/Cellar/hadoop/3.0.0 always) 
  - bin/hadoop fs -help (Gives all cmds u can try on hadoop) or bin/hdfs dfs -help
  - we focus on -mkdir (create a dir)
    - bin/hadoop fs -mkdir /test
    - bin/hadoop fs -mkdir /test/subdir (create subdir)
    - bin/hadoop fs -ls /test (List all files in test folder) 
    - Copy data from file hadoop-env.sh to hdfs test with copyFromLocal/put cmd: bin/hadoop fs -copyFromLocal ./libexec/etc/hadoop/hadoop-env.sh /test
  - Incase of errors, https://stackoverflow.com/a/29981409/3992695
  

## References: 
- https://www.tutorialspoint.com/hadoop/hadoop_enviornment_setup.htm
- http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/


## Issues/Common workarounds if namenode/datanode stops working:
- InconsistentFSStateException, dir doesnot exists: edit hdfs-site.xml by creating a tmp folder: https://www.youtube.com/watch?v=Gsr73K-7zN8
- kill 9234
- clear tmp cache at loc: /tmp/
- hadoop namenode -format

# Misc:
- Help:  mongod --help | more

