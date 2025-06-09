![YT](https://www.youtube.com/watch?v=DU8o-OTeoCc&t=1007s)
# 1. Overview  
  
## 1.1 System Purpose and Key Components Diagram  
```mermaid  
graph TD
    ZK[ZooKeeper Ensemble] -->|Coordination| KC[Kafka Cluster]
    KC -->|Produce| P[Producers]
    KC -->|Consume| C[Consumers]
    
    subgraph KC[Kafka Cluster]
        B1[Broker 1] -->|Replicate| B2[Broker 2]
        B2 -->|Replicate| B3[Broker 3]
    end
```  
  
Explanation:  
- Shows Kafka's fundamental architecture with ZooKeeper for coordination  
- Producers publish messages to the Kafka cluster  
- Consumers subscribe to topics and consume messages  
- Brokers form the cluster and replicate data between them
- ZooKeeper maintains cluster metadata and controller election   
# 2. High Level Design  
  
## 2.1 Multi-Cluster Architecture Diagram  
```mermaid  
graph LR  
    DC1[Data Center 1] -->|MirrorMaker| DC2[Data Center 2]  
    DC1 -->|MirrorMaker| DC3[Data Center 3]  
  
    subgraph DC1  
        KC1[Kafka Cluster]  
    end  
  
    subgraph DC2  
        KC2[Kafka Cluster]  
    end  
  
    subgraph DC3  
        KC3[Kafka Cluster]  
    end  
```  
  
Explanation:  
- Illustrates geo-redundant deployment across multiple data centers  
- MirrorMaker replicates data between clusters for disaster recovery  
- Each data center has its own independent Kafka cluster  
- Enables active-active or active-passive configurations  
  
## 2.2 Component Relationships Diagram  
```mermaid  
classDiagram  
    class ZooKeeper {  
        +ensemble: List~Server~  
        +storeControllerElection()  
        +storeBrokerHeartbeats()  
        +storeTopicConfigs()  
    }  
  
    class KafkaCluster {  
        +List~Broker~ brokers  
        +List~Topic~ topics  
        +Controller controller  
    }  
  
    class Broker {  
        +int id  
        +List~Partition~ leaderFor  
        +List~Partition~ followerFor  
    }  
  
    class Topic {  
        +String name  
        +List~Partition~ partitions  
    }  
  
    class Partition {  
        +int id  
        +Broker leader  
        +List~Broker~ ISR  
    }  
  
    ZooKeeper --> KafkaCluster: Coordinates  
    KafkaCluster --> Broker: Contains  
    KafkaCluster --> Topic: Contains  
    Topic --> Partition: Contains  
```  
  
Explanation:  
- ZooKeeper class shows its critical coordination functions  
- KafkaCluster contains all brokers and topics  
- Each broker tracks which partitions it leads/follows  
- Topics contain partitions which have leader/follower relationships  
- ISR (In-Sync Replicas) maintains list of up-to-date followers  
  
# 3. Low Level Design  
  
## 3.1 ZooKeeper Ensemble Diagram  
```mermaid  
flowchart TD  
    ZK1[ZK Node 1] -->|Quorum| ZK2[ZK Node 2]  
    ZK1 -->|Quorum| ZK3[ZK Node 3]  
  
    ZK1 -->|Ephemeral Nodes| B[Brokers]  
    ZK1 -->|Persistent Nodes| T[Topics]  
    ZK1 -->|Leader Election| C[Controller]  
```  
  
Explanation:  
- ZooKeeper runs in odd-numbered ensembles (typically 3 or 5 nodes)  
- Uses quorum for consensus and fault tolerance  
- Ephemeral nodes track live brokers (disappear if broker fails)  
- Persistent nodes store topic configurations  
- Manages controller election for cluster coordination  
  
## 3.2 Partition Distribution Diagram  
```mermaid  
pie  
    title Partition Distribution Across Brokers  
    "Broker 1" : 35  
    "Broker 2" : 30  
    "Broker 3" : 35  
```  
  
Explanation:  
- Shows how partitions are distributed across brokers  
- Kafka aims for even distribution of leadership  
- Ensures no single broker becomes a bottleneck  
- Rebalancing occurs when brokers join/leave the cluster  
  
## 3.3 Replication Mechanism Diagram  
```mermaid  
sequenceDiagram  
    participant L as Leader  
    participant F1 as Follower 1  
    participant F2 as Follower 2  
  
    L->>F1: Replicate Record  
    L->>F2: Replicate Record  
    par Parallel ACKs  
        F1->>L: ACK  
        F2->>L: ACK  
    end  
    L->>L: Commit Record  
```  
  
Explanation:  
- Leader replicates records to all followers in parallel  
- Followers acknowledge successful writes  
- Leader commits only after receiving required ACKs (configurable)  
- Implements Kafka's durability guarantees  
  
# 4. Data Flow Discussion  
  
## 4.1 Producer Flow Diagram  
```mermaid  
flowchart TB  
    P[Producer] -->|1 Metadata Request| B[Any Broker]  
    B -->|2 Metadata Response| P  
    P -->|3 Produce to Leader| L[Partition Leader]  
    L -->|4 Replicate| F[Followers]  
    F -->|5 ACK| L  
    L -->|6 ACK| P  
```  
  
Explanation:  
1. Producer first fetches metadata to discover partition leaders  
2. Broker responds with current cluster metadata  
3. Producer sends records directly to partition leaders  
4. Leader replicates to followers (based on replication factor)  
5. Followers acknowledge successful replication  
6. Leader acknowledges back to producer  
  
## 4.2 Consumer Flow Diagram  
```mermaid  
sequenceDiagram  
    participant C as Consumer  
    participant B as Broker  
    participant ZK as ZooKeeper  
  
    C->>B: Fetch Request  
    alt Has data  
        B->>C: Return Records  
    else No data  
        B->>C: Return empty  
    end  
    C->>ZK: Commit Offset (or __consumer_offsets topic)  
```  
  
Explanation:  
- Consumers periodically fetch records from brokers  
- Brokers return available records or empty response  
- Consumers commit their offsets to track consumption progress  
- Modern Kafka versions use internal topic (`__consumer_offsets`) instead of ZooKeeper  
  
## 4.3 Controller Failover Diagram  
```mermaid  
stateDiagram-v2  
    [*] --> Follower  
    Follower --> Leader: Elected as Controller  
    Leader --> Follower: Controller fails  
  
    state Leader {  
        [*] --> ManagingPartitions  
        ManagingPartitions --> HandlingFailures: Broker down  
        HandlingFailures --> ManagingPartitions: Rebalance complete  
    }  
```  
  
Explanation:  
- Shows controller failover state machine  
- Only one broker acts as controller at a time  
- Controller manages partition leadership and ISR changes  
- If controller fails, new election occurs automatically  
- Controller handles partition reassignment during failures  
  
# 5. Cluster Synchronisation (MirrorMaker) Diagram  
```mermaid  
flowchart LR  
    subgraph Source Cluster  
        SC[Source Topics]  
    end  
  
    subgraph MirrorMaker  
        MM1[Consumer]  
        MM2[Producer]  
    end  
  
    subgraph Target Cluster  
        TC[Target Topics]  
    end  
  
    SC -->|Consume| MM1  
    MM1 --> MM2  
    MM2 -->|Produce| TC  
```  
  
Explanation:  
- MirrorMaker bridges two Kafka clusters  
- Consumer group reads from source cluster  
- Producer writes to target cluster  
- Maintains message ordering within partitions  
- Can be configured for active-active or active-passive  
  
# 6. Fault Tolerance Scenarios  
  
## 6.1 Broker Failure Diagram  
```mermaid  
sequenceDiagram  
    participant ZK as ZooKeeper  
    participant C as Controller  
    participant B as Broker  
  
    B ->> ZK: Session Expires  
    ZK ->> C: Broker Failure Notification  
    C ->> C: Elect New Leaders  
    C ->> All Brokers: Update Metadata  
```  
  
Explanation:  
- When broker fails, ZooKeeper session expires  
- Controller receives failure notification  
- Controller reassigns leadership for affected partitions
- Updates metadata propagated to all brokers
- Producers/consumers get new metadata on next request  
  
## 6.2 Network Partition Diagram  
```mermaid  
graph TD  
    B1[Broker 1] -->|Partitioned| B2[Broker 2]  
    B1 -->|Connected| B3[Broker 3]  
    B2 -->|Connected| B4[Broker 4]  
  
    ZK[ZooKeeper] -->|Quorum Lost| B1  
    ZK -->|Quorum Alive| B2  
```  
  
Explanation:  
- Shows split-brain scenario in network partition
- ZooKeeper maintains quorum on majority side
- Partitioned brokers may be marked offline
- Kafka prioritizes consistency over availability in partitions  
  
# 7. Performance Considerations  
  
## 7.1 Partition Rebalancing Diagram  
```mermaid  
gantt  
    title Partition Rebalance During Scaling  
    dateFormat  HH:mm  
    section Add Broker  
    Rebalance Partitions :active, 00:00, 5m  
    section Remove Broker  
    Replicate Partitions :crit, 00:05, 10m  
```  
  
Explanation:  
- Adding brokers triggers partition rebalancing  
- Removing brokers requires partition replication  
- Critical operation to maintain availability  
- Time varies based on partition size and count  
  
## 7.2 Resource Allocation Diagram  
```mermaid  
pie  
    title Resource Distribution  
    "Network" : 40  
    "Disk I/O" : 35  
    "CPU" : 15  
    "Memory" : 10  
```  
  
Explanation:  
- Network bandwidth is primary bottleneck  
- Disk throughput critical for message persistence  
- CPU for compression/encryption (if used)  
- Memory mainly for page cache (not JVM heap)  

Each diagram in this document visually explains a critical aspect of Kafka's architecture, accompanied by detailed textual explanations of the components and their interactions. The combination provides a comprehensive understanding of Kafka's distributed systems design.