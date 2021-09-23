1. CouchDB vs MongoDB?

    |     CouchDB |   MongoDB |
    | ----------- | --------  |
    | records are stored in the documents in the database | records are stored in the collection in the database |
    | Master-Master replication in CouchDB | Master-Slave replication in MongoDB |
    | CouchDB is written in Erlang | MongoDB is written in C++ |
    | In CouchDB, the interface is REST/HTTP | In MongoDB, the interface is TCP/IP Custom Protocol |
    | Slower than MongoDB | Faster than CouchDB |
  
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
5. 
    
