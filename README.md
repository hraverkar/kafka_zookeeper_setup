## 🐳 Kafka + Zookeeper + Kafka UI (Docker Setup)

This setup provides a fully functional **Kafka cluster** with **Zookeeper** and a **Kafka Web UI** — all inside Docker containers.
You can run it locally with a single command.

---

### 📁 **docker-compose.yml**

```yaml
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:7.6.1
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - "2181:2181"

  kafka:
    image: confluentinc/cp-kafka:7.6.1
    container_name: kafka
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092
      KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1

  kafka-ui:
    image: provectuslabs/kafka-ui:latest
    container_name: kafka-ui
    depends_on:
      - kafka
    ports:
      - "8090:8080"
    environment:
      KAFKA_CLUSTERS_0_NAME: local
      KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:9092
      KAFKA_CLUSTERS_0_ZOOKEEPER: zookeeper:2181
```

---

### ⚙️ **How to Run**

1. Save the above file as `docker-compose.yml` in your project root.
2. Run the following command:

   ```bash
   docker compose up -d
   ```

   This will:

   * Pull all necessary images from Docker Hub
   * Start Zookeeper, Kafka, and Kafka UI containers

---

### ✅ **Service Endpoints**

| Service          | Address                                        | Description                    |
| ---------------- | ---------------------------------------------- | ------------------------------ |
| **Kafka UI**     | [http://localhost:8090](http://localhost:8090) | Web interface for Kafka        |
| **Kafka Broker** | `localhost:9092`                               | Main Kafka broker endpoint     |
| **Zookeeper**    | `localhost:2181`                               | Zookeeper coordination service |

---

### 🧠 **Useful Commands**

Check running containers:

```bash
docker ps
```

View logs:

```bash
docker logs kafka -f
```

Stop and remove all containers:

```bash
docker compose down
```

Remove containers + volumes (clean reset):

```bash
docker compose down -v
```

---

### 💡 **Quick Tips**

* To connect from **outside Docker** (e.g., .NET, Node.js, Python apps):
  use `localhost:9092`
* To connect from **another container in the same network**:
  use `kafka:9092`
* You can create topics or inspect messages easily via the Kafka UI dashboard.

---

### 🧾 **Summary**

| Component | Image                             | Port                           | Purpose            |
| --------- | --------------------------------- | ------------------------------ | ------------------ |
| Zookeeper | `confluentinc/cp-zookeeper:7.6.1` | 2181                           | Kafka coordination |
| Kafka     | `confluentinc/cp-kafka:7.6.1`     | 9092                           | Message broker     |
| Kafka UI  | `provectuslabs/kafka-ui:latest`   | 8090 (host) / 8080 (container) | Management UI      |

---

✅ **In short:**
Run this command ⤵️

```bash
docker compose up -d
```

Then open **[http://localhost:8090](http://localhost:8090)** — your Kafka setup is ready to go 🎉
