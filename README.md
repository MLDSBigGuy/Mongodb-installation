## QuickFacts

| RDBMS| MongoDB  | 
|------|----------|
| Table|Collection| 
|Column|Key       |  
|Value	|Value|
|Records/Rows	 |Document/Object|


- doc must have _id field
- size of doc is 16MB. to store more than that, store across multiple docs/use GridFS package in python

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
       
## 5. Create Collections (foo, db...). In ur api, each new group(car, book) is one collection. Add user_ids to this collection as id and pass enco as encoded byte array, retreive the encoded byte array later! 
  - use foo (use a foo db) 
  - show collections (this is not a realtional db. cant get docs from diff. collections 
  - Objectid() creates _id: if u forgot to create one! This Objectid have many properties like date-time etc.,. tHIS ID CAN MAKE U TO UNIQUELY RETRIEVE DOCS!
  - OBJECTID().GETIMESTAMP()

## 6. Insert unique ids. Mongo automatically generates error if duplicate id is supplied
  - db.dbname.insert() - unique id. If no unique id, mongo generates error! handle it in python and return jsonify
      - db.dbname.save() - no unique id 
  - db.dbname.find().pretty()
  
## 7. update docs (No 2 clients can update same doc at same time! They are only updated one after other)
 - which collection u update (car/book) ?
 - which doc/docs u r targeting ? (isue this by mongo query)
 - db.foo.update(query, update, options) update - what change, options - one many, upsert ? 
 - update cmd to incremet a number!$inc:{x:10} , increments to x:11
 - one clinet wants to add field to doc while other client what s to increment a doc! 
    ex: $unset without chaning others - remove a filed
        $rename to change key name 
## 8 . Array ops
- create array with $push Ex: db.a.update({_id:1}, {$push:{things:'one'}}) -->  ['one']
- push can add duplicae items too. To prevent duplicates, use $addtoset .
- $pull to remove the things out / $pop : 1 removes last element in array. $pop: -1 removes first element in array
- If ur doc has field which is not array. then push pop dont work! 
- MULTIPLE RECORDS update at once IN DB with $multi: true
- Only update those docs who has a paricular element in a doc: (querying is another big concept) 


## 9. db.foo.findandModify - (get only partiular field out of 16mb doc) 
All docs: query, update, upsert(create one if one doesnot exists), remove,  sort, fields 
bool: new ( returns new/old one) 

## 10. Indexing (for fast database search)
-  - create index with ensureIndex
 - mongo uses B-Tree,
 - Geo index (proximity of points to center. doesnot need to be geo locations), 
 - text(for social neterokd to index the sentences which are closest to others, 
 - hashed(even districution of docs), 
 - TTL (time to live index for expiring docs, designate a date time in r doc as a expiration date. mONGO WILL AUTOMATICALLY REMOVES THISDOC AFTER SOMETIME WIHOUT U DELETING IT...CAN BE USED FOR MODELS DELETION BASED ON PREVIOUSDATE !)  )

- ENSUREiNDEX(gEO/TEXT/..., NAME THE INDEX/sparse index/ttl index/language.....)

- db.test.ensureIndex({name:1}), 1 is ascending., -1 is desc. 

- normal db search find() is a for loop that goes through each doc!
- In index, u create a index field to jump directly to disk location! dont need to load much docs into memeory! 

- query math can be on index rather than on document itself!

- if index keys are in some sort condition, docs can be retreived much faster! 

- indexes also used fo rrange queries like >, <, string comparision Ex: find string between t and l 
    - $lt: 'donkey', lt is less than 
    
- dropIndex to remove any field except _id. 

- indexing is supported on arrays. create index on encoding of user, 

- If multiplle docs are matched for index qury, use sort on another filed to even minimize 

- unique:true on name field (just like _id). cant insert duplicates! 

- $exists: true, 

- $sparse:true. sparse index store only entries for docs that have that field. 

- indexing query
    



 ## 11. explain() gives the cursor or btree used explanation
 
 ## 12. concurrent r/w
 

 - mongo blocks if u simpy give only ensureindex without background:true. so, set bg to trie for concurrent r/w operations! This bg = true takes more long time than u build it in foreground!
 
 - THIS IS USED FOR LIVE PRODUCTION 

## 13. Limitation
- index name has to be max 128 character name
- USE INDEXES FOR FAST PERFORMANCES!!! USE INDEXES !!


# USE INDEXES. PERIOD. 






# Misc:
- Help:  mongod --help | more
# In shell:
 - help (general help)
  - ctrl- l: clear shell screen
  - ctrl-k: truncate the right hand text
  
# Ref:

https://www.w3resource.com/mongodb/databases-documents-collections.php



