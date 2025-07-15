# Real-Time MySQL to Snowflake CDC Pipeline Using Kafka

This project demonstrates a real-time data pipeline that captures changes from a MySQL database and streams them into Snowflake using Apache Kafka and Kafka Connect with Debezium.

> ✅ Ideal for real-time analytics, change data capture (CDC), and streaming use cases.

---

## 🧱 Architecture

MySQL (source DB)  
│  
│ CDC via Debezium (Kafka Connect Source Connector)  
▼  
Kafka Topics (Change Events)  
│  
│ Kafka Connect Snowflake Sink Connector  
▼  
Snowflake  

---

## ⚙️ Tools & Technologies

- **MySQL** – Source database with transactional data  
- **Apache Kafka** – Message broker for streaming data  
- **Kafka Connect** – Used to move data between systems with source/sink connectors  
- **Debezium** – Captures CDC events from MySQL  
- **Snowflake** – Data warehouse sink  
- **Confluent Schema Registry** – Manages Avro schemas for Kafka messages  
- **Kafka UI** – Web UI to monitor Kafka topics and connectors  
- **Docker Compose** – Used for local containerized deployment  

---

## 🚀 Features

- Real-time ingestion of insert, update, and delete events from MySQL  
- Schema-aware streaming using Avro and Schema Registry  
- Automatic delivery of Kafka topic data to Snowflake tables  
- End-to-end containerized deployment using Docker Compose  
- Modular configuration for connectors (JSON files)  

---

## 📁 Project Structure

├── docker-compose.yml               # Services: Kafka, Schema Registry, Kafka Connect, MySQL, Kafka UI  
├── connectors/  
│   ├── mysql-source.json            # Debezium source connector config  
│   └── snowflake-sink.json          # Snowflake sink connector config  
├── init-mysql/  
│   └── init.sql                     # Sample data + table creation  
├── README.md  

---

## 🛠️ Setup Instructions

1. Clone repository
bash
git clone https://github.com/Atef-Mousa/realtime-data-pipeline-mysql-snowflake.git
cd realtime-data-pipeline-mysql-snowflake
2. Start services
bash
docker-compose up -d
3. Initialize MySQL (optional)
bash
docker exec -i mysql mysql -uroot -p123456 < init-mysql/init.sql
4. Register connectors
bash
# Register MySQL source
curl -X POST -H "Content-Type: application/json" \
--data @connectors/mysql-source.json \
http://localhost:8083/connectors

# Register Snowflake sink
curl -X POST -H "Content-Type: application/json" \
--data @connectors/snowflake-sink.json \
http://localhost:8083/connectors
📊 Sample Use Case: E-Commerce Orders
New order inserted in MySQL → Streamed to Kafka → Loaded to Snowflake

Enables real-time dashboards in BI tools like Power BI

Supports analytics on fresh data without batch delays

🔍 Monitoring & Debugging
Kafka UI: http://localhost:8080

Schema Registry: http://localhost:8081

View Kafka Connect logs:

bash
docker logs connect
Check connector status:

bash
curl http://localhost:8083/connectors | jq
⚠️ Important Notes
Configure Snowflake credentials in snowflake-sink.json

Allow network access from Kafka Connect to Snowflake

Verify table auto-creation in Snowflake or pre-create tables

Monitor Snowflake stages via LIST @<your_stage>;

text

## Key Components Explained

1. **Debezium Source Connector**:
   - Monitors MySQL binlog for changes
   - Publishes events to Kafka topics in Avro format
   - Schema Registry tracks schema evolution

2. **Snowflake Sink Connector**:
   - Auto-creates tables matching Kafka topics
   - Uses Snowpipe for continuous loading
   - Writes to internal stages before loading

3. **Data Flow**:
   ```mermaid
   sequenceDiagram
       MySQL->>Debezium: Row Change
       Debezium->>Kafka: Avro Message
       Kafka->>Snowflake Connector: Poll Events
       Snowflake Connector->>Snowflake: COPY INTO TABLE
Performance Considerations
Tune batch.size and max.poll.records in sink config

Use Snowflake medium/large warehouses for heavy loads

Monitor Kafka consumer lag via Kafka UI

For production deployments, consider:

Adding authentication to Kafka Connect REST API

Implementing error handling with dead-letter queues

Setting up monitoring with Prometheus/Grafana
