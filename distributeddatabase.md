
Important point to remember (order of importance):

 Durability (data loss cannot happen)
 Availability (system is always available)
 Performance (low latency)
 
 Operations supported:

-  Create & Delete Table
-  GET
-  PUT
-  LIST

 

Topics to cover:

1. Sequential disc writes
    - BTree
    - LSM Tree

 2. Sequencer for table records
         Format: Timestamp in nanos (8bytes) # Unique Sequence Number (4 bytes) # Unique Node ID (4bytes) 
                This is required when multiple requests for writes to same record is received, during reconciliation, latest sequence ID wins and overwrites other requests.
    
3. Components of Distributed database design:
  

- RequestManager:

               All the CRUD operations are handled by this component.
      

- MetadataManager:
            Maintains and manages tables metadata and the location of tables (mapping info of tables → replication groups)
               Performs leader election in replication group
              Storage: zookeeper or redis, etcd
                 (paxos / raft algorithm)
- Replication Groups

               Contains nodes (odd num of nodes) with partial data of tables.
               One leader, multiple followers.
               Followers syncup with leader
               Follow becomes leader if leader is down.
  

- Controller 
    a) Manages hotdata: 

       split table into multiple partitions
      b) Leader Management
      c) Failover 
      
      Architectural Diagram
      
      ![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4C3BFDF316DDFD03BC03DB4722F7DC778A34C6E7CBD4A75BAED4DB02AF004B4_1545704283358_DDB.png)
     Strongly consistent, highly performant distributed db  https://www.cs.princeton.edu/events/25507
