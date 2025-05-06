# Digi Bank Lending  
## Project Overview (Context)  
I worked on Grab’s digital lending platform, which provides instant credit lines and loans to it's customer. The backend handled loan underwriting, disbursement, repayment tracking, risk assessment, and integrations with payment gateways and credit bureaus.  
  
![[lending_overview|1000]]  
### Why this matters:  
- Financial systems demand **high accuracy, security, and compliance**.  
- Grab’s scale means the system had to handle **high throughput** (e.g., thousands of loan applications per minute during peak hours).  
## Technical Deep Dive (Your Contributions)  
##### Scalability & Performance  
- **Problem**: How did you handle spikes in loan applications?  
- **Solution**:  
    - "Used Kafka for async processing of loan requests to decouple frontend traffic from backend underwriting workflows."  
    - "Implemented Redis to cache credit scores and user eligibility checks, reducing database load by 60%."  
    - "Optimised SQL queries for repayment tracking (e.g., batch processing for EMI deductions)."  
##### Security & Compliance  
- **Problem**: Financial systems require strict data protection and audit trails.  
- **Solution**:  
    - "Enforced end-to-end encryption for sensitive data (PII, bank details) using AWS KMS."  
    - "Implemented idempotent APIs for repayment callbacks to avoid duplicate transactions."  
    - "Audit logs for every transaction (loan disbursal, repayment) to comply with financial regulations."  
##### Integrations  
- **Problem**: Coordinating with external systems (credit bureaus, banks).  
- **Solution**:  
    - "Designed a retry-and-circuit-breaker mechanism for third-party API calls (e.g., credit score checks) to handle failures gracefully."  
    - "Used gRPC for high-performance internal microservices communication (e.g., between risk engine and ledger service)."  
##### Data Consistency  
- **Problem**: Ensuring accurate loan balances across distributed services.  
- **Solution**:  
    - "Used Saga pattern with compensating transactions to handle failures in multi-step processes (e.g., loan approval → disbursal → notification)."  
    - "Database sharding for user transaction history to avoid hotspots."  
  
```  
I’m the Backend Lead for Grab’s digital lending platform, which provides instant credit lines and loans to millions of customer across Southeast Asia. My team owns the lending underwriting and management services. I oversee architecture, scalability, and technical roadmap while mentoring engineers.  
  
In my recent project, I designed automatic protfolio management system, mentoring 2 engineers to build 2 new service, this system is capable of processing 100K loan portfolios in each batch job that happens every day.  
  
Scaled the system to process 10K+ loans/month with 99.99% uptime.  
Reduced loan approval TAT from 2hrs to 2mins by optimizing risk-engine pipelines.  
Cut cloud costs by 30% via right-sizing pods, DB and spot instances, despite 3x traffic growth.  
  
```


# Automatic Portfolio Management
The idea was to automate credit reviews for existing customers by pulling fresh bureau data and internal loan performance, then making data-driven decisions like increasing credit limits.
**Teams**: Lending Backend, DE, Rule Engine, Bureau System
### System Design
I designed the system as a pipeline of loosely coupled services:
- **Customer Selector**: Identifies eligible users based on custom rules.
- **Data Collector**: Fetches tax IDs, loan history.
- **Bureau Fetcher**: Interfaces with EVOTEK over SFTP.
- **Decision Preparer**: Packages all customer data and sends it to the Decision Engine via S3.
- **Batch Processor**: Handles zip responses, breaks them down per customer.
- **Kafka-based Event System**: For downstream updates in `asset-offline` and `loan-core`.

Each service was independently deployable, stateless, and followed domain-driven boundaries.
### Challenges
- **SFTP-based Integration** was unreliable. We added checksum validation, auto-retry, and alerting to improve reliability.
- **Data consistency** across services was a concern. We solved this with idempotent operations and Kafka replay mechanisms.
- **Orchestration vs Choreography**: Initially we had tight coupling; we moved to event-driven (Kafka) to decouple flows.

### Result
The system could process thousands of customer decisions daily, completely automated, with clear audit trails and high fault tolerance.

![[automatic_portfolio_management|1000]]