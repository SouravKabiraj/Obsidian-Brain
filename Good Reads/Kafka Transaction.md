# Kafka Transactions: Payment Processing Example

Let's walk through a payment processing system that uses Kafka transactions to ensure atomic operations across multiple services.

## Scenario: E-commerce Payment Flow

We have three services that need to coordinate:
1. **Payment Service** - Processes the payment
2. **Order Service** - Updates order status
3. **Inventory Service** - Updates product stock

### Without Transactions:
- If any step fails, we could have inconsistent states (e.g., payment taken but inventory not updated)

### With Kafka Transactions:
All operations either complete successfully or none do.

## Kafka Topics Needed:
1. `payment-events` - Payment processing updates
2. `order-events` - Order status changes
3. `inventory-events` - Stock level updates

## Example Code (Java)

```java
// Configure transactional producer
Properties props = new Properties();
props.put("bootstrap.servers", "kafka-broker:9092");
props.put("transactional.id", "payment-processor-1"); // Must be unique per instance
props.put("enable.idempotence", "true");
props.put("acks", "all");

KafkaProducer<String, String> producer = new KafkaProducer<>(props);

// Initialize transactions
producer.initTransactions();

try {
    // Begin transaction
    producer.beginTransaction();
    
    // 1. Process payment (send to payment-events)
    producer.send(new ProducerRecord<>("payment-events", 
        "order-123", 
        "{\"status\":\"completed\",\"amount\":99.99}"));
    
    // 2. Update order status (send to order-events)
    producer.send(new ProducerRecord<>("order-events", 
        "order-123", 
        "{\"status\":\"paid\",\"paymentId\":\"pay-789\"}"));
    
    // 3. Update inventory (send to inventory-events)
    producer.send(new ProducerRecord<>("inventory-events", 
        "product-456", 
        "{\"action\":\"decrement\",\"qty\":1}"));
    
    // Commit if all succeeds
    producer.commitTransaction();
    
} catch (Exception e) {
    // Abort transaction if any operation fails
    producer.abortTransaction();
    throw new RuntimeException("Payment processing failed", e);
}
```

## What Happens Behind the Scenes

1. **Transaction Start**:
   - The transaction coordinator (a Kafka broker) begins tracking this transaction
   - All three messages are marked as part of the transaction but aren't visible yet

2. **Successful Path**:
   ```mermaid
   sequenceDiagram
       Producer->>Transaction Coordinator: Begin Transaction (T1)
       Producer->>Payment Topic: Send message (buffered)
       Producer->>Order Topic: Send message (buffered)
       Producer->>Inventory Topic: Send message (buffered)
       Producer->>Transaction Coordinator: Commit T1
       Transaction Coordinator->>All Topics: Write commit marker
       Topics->>Consumers: Messages now visible (read_committed)
   ```

3. **Failure Path**:
   - If the inventory update fails (maybe product is out of stock):
     - The `abortTransaction()` call tells the coordinator to discard all three messages
     - Consumers never see any of these messages
     - Payment is rolled back, order remains unpaid, inventory unchanged

## Consumer Configuration

Consumers must be configured to respect transactions:

```java
props.put("isolation.level", "read_committed");
```

This ensures:
- Consumers only see messages from completed transactions
- No partial updates are visible during processing

## Benefits in This Example

1. **Atomicity**: Either all three operations succeed or none do
2. **Consistency**: No "half-paid" orders or incorrect inventory counts
3. **Reliability**: System remains consistent even if a service crashes mid-process

## Real-World Considerations

1. **Error Handling**: You'd want to add retry logic for transient failures
2. **Compensating Transactions**: For business logic failures (e.g., insufficient funds)
3. **Monitoring**: Track transaction success/failure rates
4. **Performance**: Transactions add overhead, so batch where possible

This pattern is particularly valuable in microservices architectures where you need to maintain consistency across multiple services without distributed transactions across different databases.