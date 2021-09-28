What is CouchDB?  
CouchDB is a NoSQL, open source database with JavaScript as its query language. It stores the data with JSON documents

1. CouchDB vs MongoDB?

    |     CouchDB |   MongoDB |
    | ----------- | --------  |
    | records are stored in the documents in the database | records are stored in the collection in the database |
    | Master-Master replication in CouchDB | Master-Slave replication in MongoDB |
    | CouchDB is written in Erlang | MongoDB is written in C++ |
    | In CouchDB, the interface is REST/HTTP | In MongoDB, the interface is TCP/IP Custom Protocol |
    | Slower than MongoDB | Faster than CouchDB |
    | JSON format | BSON format, extension of JSON |
  
2. main features of CouchDB?
   1. JSON Documents
   2. RESTful Interface
   3. N-Master Replication
   4. Built for Offline
   5. Replication filter

3. How can we connect to remote CouchDB?  
    On the local machine,set up an ssh tunnel to your server and tell it to forward requests to the local port 5984 to the remote server’s port 5984
4. How to use transactions with CouchDB?  
    CouchDB makes the use of a model which is known as an ‘Optimistic Currency’ model. ‘Optimistic Currency’ model grants approval to send a document version
    with the CouchDB update. CouchDB scans the document version, and if the present document version does not tally with document version which was was sent,
    then CouchDB rejects the change in the release.
5. How couchDB sync client db with server?  
    https://www.codemag.com/Article/1911041/Syncing-a-Client-Database-with-the-Server
6. What is PouchDB?  
    PouchDB is a browser-based database interface that's tailor-made to synchronize with CouchDB. This means that data manipulated in the browser can
    seamlessly flow up to the server
7. CouchBase mobile? - NoSQL Database for mobile
    Two parts -  
    1. Couchbase Lite - an embedded, schemaless, JSON database
    2. Sync Gateway - a mechanism to sync data to and from the server
8. Cassandra vs MongoDB vs CouchDB vs Redis vs Riak vs HBase vs Couchbase vs OrientDB vs Aerospike vs Neo4j vs Hypertable vs ElasticSearch vs Accumulo vs
    VoltDB vs Scalaris vs RethinkDB?\
    https://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis
9. State few elements in couchbase node?  
    1. Index service
    2. Data service
    3. query service
    4. cluster management component  


10.Functional blocks used in couchbase server?
    1. Data manager
    2. cluster manager
    
11.What is couchbase?  
    Couchbase is a Server, also known as Membase, is an open-source, distributed (shared-nothing architecture) multi-model NoSQL document-oriented database
    software package that is optimized for interactive applications.  
    
    
    
    
    
    
    

    
