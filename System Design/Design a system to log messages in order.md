#  Functional Requirements
1) Need to store logs coming from different systems
2) Need to store the logs in time ordered
3) Users will able to retrieval logging

# Non Functional Requirements
1) Low retrieval latency
2) Reliability
3) Eventual Consistent

# Back of the envelope calculation
1) Number of micro services = 500
2) Number of logs per service = 1 millions/ per day
3) Number of logs per day = 500 x 1 millions = 0.5 billions
4) Number of logs per month = 0.5 x 30 = 15 billions
5) Average characters per log  = 50
6) Total characters in one month = 50 x 15 billions = 450 billion
7) Total storage required = 450 billion x byte = 450,000,000,000 byte = 450GB

# API Design
1) [POST] /api/log
   - timestamp
   - id
   - object {message, container-name, pod, ...}
   - type
1) [GET] /api/log?timestamp>2024-01-01&timestamp<2024-02-01&type=INFO


# System Components

## Message Ingestion Layer

- **Role**: Accept incoming messages from clients or services.
    
- **Components**:
    
    - **API Gateway**: Exposes an endpoint (e.g., REST, WebSocket) for clients to send messages.
        
    - **Load Balancer**: Distributes incoming messages across multiple logging servers for scalability.
        
    - **Message Queue**: Buffers messages to handle spikes in traffic (e.g., Kafka, RabbitMQ).
        

## Logging Service

- **Role**: Process and store messages in chronological order.
    
- **Components**:
    
    - **Timestamping**: Assign a timestamp to each message (use a distributed clock or NTP for synchronization).
        
    - **Sequencer**: Ensures messages are processed in the correct order (e.g., using a distributed lock or leader election).
        
    - **Storage Engine**: Stores logs in a time-ordered data structure (e.g., a time-series database or a log-structured storage system).
        

## Storage Layer

- **Role**: Persist logs durably and allow efficient retrieval.
    
- **Options**:
    
    - **Time-Series Database**: Optimized for time-ordered data (e.g., InfluxDB, TimescaleDB).
        
    - **Distributed Log Storage**: Systems like Apache Kafka or Amazon Kinesis for streaming and storing logs.
        
    - **File-Based Storage**: Append logs to files in chronological order (e.g., using a log rotation mechanism).
        

## Query Layer

- **Role**: Retrieve logs in chronological order.
    
- **Components**:
    
    - **Indexing**: Create indexes on timestamps for fast retrieval.
        
    - **API**: Expose endpoints for querying logs (e.g., by time range, severity, or source).
        
    - **Pagination**: Handle large result sets by returning logs in chunks.
        

## Monitoring and Fault Tolerance

- **Role**: Ensure system reliability and performance.
    
- **Components**:
    
    - **Health Checks**: Monitor the health of logging servers and storage systems.
        
    - **Replication**: Replicate logs across multiple nodes for fault tolerance.
        
    - **Backup**: Regularly back up logs to prevent data loss.
---

# Data Flow

1. A client sends a message to the API Gateway
2. The message is routed to a logging service via the load balancer.
3. The logging service assigns a timestamp and ensures the message is processed in order.
4. The message is stored in the storage layer (e.g., time-series database or distributed log).
5. Clients can query the logs via the query layer, which retrieves logs in chronological order.