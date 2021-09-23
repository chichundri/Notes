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

3. How can we connect to remote CouchDB ?
    On the local machine,set up an ssh tunnel to your server and tell it to forward requests to the local port 5984 to the remote server’s port 5984:

