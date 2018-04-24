## QuickFacts

| RDBMS| MongoDB  | 
|------|----------|
| Table|Collection| 
|Column|Key       |  
|Value	|Value|
|Records/Rows	 |Document/Object|


- doc must have _id field
- size of doc is 16MB. to store more than that, store across multiple docs n use GridFS package in python

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
       
## 5. Create Collections (foo, db...). In ur api, each new group(car, book) is one collection. Add user_ids to this collection as id. and pass enco as encoded byte array, retreive the encoded byte array later! 
  - use foo (use a foo db) 
  - show collections (this is not a realtional db. cant get docs from diff. collections 
  - Objectid() creates _id: if u forgot to create one! This Objectid have many properties like date-time etc.,. tHIS ID CAN MAKE U TO UNIQUELY RETRIEVE DOCS!
  - OBJECTID().GETIMESTAMP()

## 6. Insert unique ids. Mongo automatically generates error if duplicate id is supplied
  - db.dbname.insert() - unique id. If no unique id, mongo generates error! handle it in python and return jsonify
      - db.dbname.save() - no unique id 
  - db.dbname.find().pretty()

## Output after succesful hadoop installation looks like below:
```xml
krishnas-mbp:sbin krishna.damarla$ ./start-dfs.sh 
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [krishnas-mbp.widas.de]
2018-04-10 14:14:40,129 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```
(Last message is just a warning. Can be neglected) 



# Misc:
- Help:  mongod --help | more
# In shell:
 - help (general help)
  - ctrl- l: clear shell screen
  - ctrl-k: truncate the right hand text
