Apache Kafka is an **open-source distributed event streaming platform** originally built at LinkedIn and open-sourced through Apache Software Foundation.
**Core idea:** It acts as a high-throughput, fault-tolerant message bus between systems — producers write data, consumers read it, and Kafka stores it durably in between.**Key concepts:**
- **Topic** — a named category/feed of messages (e.g. "orders", "clicks")
- **Partition** — topics split into partitions for parallelism and scalability
- **Broker** — a Kafka server; multiple brokers form a cluster
- **Producer** — any system that writes messages to a topic
- **Consumer group** — group of consumers that share the reading load
- **Offset** — position of a message; consumers track their own offset (can replay)
- **Retention** — messages are stored for a configurable time (e.g. 7 days), not deleted after reading
**Why Kafka vs traditional message queues?**
|Feature|Kafka|RabbitMQ / ActiveMQ|
|---|---|---|
|Message replay|Yes (retained by time/size)|No (deleted after ack)|
|Throughput|Millions/sec|Thousands/sec|
|Consumer model|Pull-based|Push-based|
|Ordering guarantee|Per partition|Per queue|
|Scalability|Horizontal (add brokers)|Limited|
|Use case|Event streaming, logs|Task queues, RPC|
**Common use cases in India:**
- Fintech (Zerodha, PhonePe, Paytm) — real-time transaction streams & fraud detection
- E-commerce (Flipkart, Meesho) — order events, inventory sync
- Telecom (Jio, Airtel) — network event processing at massive scale
- UPI payment rails — high-volume event pipelines
**Managed Kafka offerings (no self-hosting needed):**
|Provider|Service|Approx. cost (INR/month)|
|---|---|---|
|Confluent Cloud|Fully managed|₹1,700+ (pay-per-use)|
|AWS|Amazon MSK|₹4,000–₹20,000+|
|Azure|Azure Event Hubs|₹1,500–₹15,000+|
|Aiven|Aiven for Kafka|₹2,000+|