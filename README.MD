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

## Clone the repo

```bash
git clone https://github.com/Atef-Mousa/realtime-data-pipeline-mysql-snowflake.git
cd realtime-data-pipeline-mysql-snowflake


## Start all services

bash
Copy
Edit
docker-compose up -d

## Initialize MySQL (optional)
Exec into the MySQL container and apply the schema:

bash
Copy
Edit
docker exec -i mysql mysql -uroot -p123456 < init-mysql/init.sql

##  Register connectors

Use curl or Postman to create the connectors:

bash
Copy
Edit
curl -X POST -H "Content-Type: application/json" \
--data @connectors/mysql-source.json \
http://localhost:8083/connectors

curl -X POST -H "Content-Type: application/json" \
--data @connectors/snowflake-sink.json \
http://localhost:8083/connectors
📊 Sample Use Case: E-Commerce Orders
When a new order is inserted into MySQL, it's streamed in real-time to Kafka.
Kafka Connect ingests this into Snowflake's raw table.
Snowflake can then transform this into analytics tables or Power BI dashboards.

🔍 Monitoring & Debugging
Kafka UI: http://localhost:8080

Kafka Connect logs: docker logs connect

Schema Registry: http://localhost:8081
