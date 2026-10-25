# KafkaSpring

## Overview

**KafkaSpring** is a Spring Boot application that demonstrates event-driven architecture using Apache Kafka (via Redpanda) for real-time message streaming. This project showcases producer-consumer patterns, REST API integration, and topic configuration for building scalable, asynchronous systems.

### Key Features
- **Kafka Producer**: Send messages to Kafka topics via REST API
- **Kafka Consumer**: Listen and process messages from Kafka topics
- **Topic Configuration**: Automatic topic creation with Spring Boot
- **REST API**: HTTP endpoints for interacting with Kafka
- **Health Checks**: Built-in health endpoint to monitor application status
- **Containerized Setup**: Docker Compose with Redpanda for easy local development

---

## Why Redpanda Instead of Zookeeper?

### Traditional Kafka Architecture
Apache Kafka traditionally required **Apache Zookeeper** as a separate service for:
- Cluster coordination
- Leader election
- Metadata management
- Configuration storage

This added complexity, operational overhead, and required managing two separate distributed systems.

### Why Redpanda?
**Redpanda** is a Kafka-compatible event streaming platform that offers significant advantages:

1. **No Zookeeper Dependency**: Redpanda uses its own consensus algorithm (Raft), eliminating the need for Zookeeper
2. **Simpler Operations**: Single system to deploy, monitor, and manage
3. **Faster Development Cycles**: No complex cluster coordination setup required
4. **Lower Resource Consumption**: More efficient for development and testing
5. **Kafka-Compatible API**: Works seamlessly with existing Kafka clients and Spring Kafka
6. **Better Performance**: Optimized for modern hardware and cloud deployments

In this project, Redpanda is containerized via Docker Compose, providing a lightweight development environment that mimics production-grade Kafka behavior without operational complexity.

---

## Prerequisites

### System Requirements
- **Docker Desktop**: Ensure Docker and Docker Compose are installed
  - [Download Docker Desktop](https://www.docker.com/products/docker-desktop)
  - Verify installation: `docker --version && docker-compose --version`

- **Java 21 or Higher**: Required for Spring Boot 3.2.2
  - [Download Java JDK](https://www.oracle.com/java/technologies/downloads/)
  - Verify: `java -version`

- **Maven 3.6+**: For building the project
  - Verify: `mvn --version`

---

## Project Structure

```
KafkaSpring/
├── src/main/java/org/kafkaspring/
│   ├── KafkaSpringApplication.java       # Spring Boot entry point
│   ├── KafkaTopicConfig.java             # Topic configuration & creation
│   ├── KafkaProducer.java                # Message producer service
│   ├── KafkaConsumer.java                # Message consumer service
│   ├── KafkaController.java              # REST API endpoints
│   └── StartupRunner.java                # Startup initialization
├── docker-compose.yml                    # Redpanda container configuration
├── pom.xml                               # Maven dependencies
└── README.md                             # This file
```

---

## Setup & Installation

### 1. Start Redpanda Container

```bash
cd KafkaSpring
docker-compose up -d
```

This starts a single-node Redpanda cluster:
- **Kafka Port**: `9092` (external access)
- **Internal Port**: `29092` (container-to-container communication)
- **Data Volume**: `redpanda-data` (persists messages)

**Verify Redpanda is running**:
```bash
docker ps | grep redpanda
```

### 2. Build the Project

```bash
mvn clean install
```

This compiles the code, runs tests, and packages the application.

### 3. Run the Application

```bash
mvn spring-boot:run
```

Or run the compiled JAR:
```bash
java -jar target/KafkaSpring-1.0-SNAPSHOT.jar
```

**Expected Output**:
```
Started KafkaSpringApplication in X seconds
Sent startup message: Hello from producer at 2026-08-23T...
```

---

## Usage

### Available Endpoints

#### 1. **Health Check**
```bash
curl http://localhost:8080/api/kafka/health
```
**Response**: `Kafka Producer is running`

#### 2. **Send Message to Topic**
```bash
curl -X POST "http://localhost:8080/api/kafka/send" \
  -G --data-urlencode "topic=user-activity-events-topic" \
  --data-urlencode "message=User logged in from IP 192.168.1.1"
```

**Response**: `Message sent to topic: user-activity-events-topic`

#### 3. **Send Message with Custom Topic**
```bash
curl -X POST "http://localhost:8080/api/kafka/send?topic=my-custom-topic&message=Hello+Kafka"
```

### Consumer Output
Messages are consumed and logged to console in real-time:
```
Received: User logged in from IP 192.168.1.1
Received: Hello Kafka
```

---

## Configuration

### Application Properties
Create `src/main/resources/application.properties` to customize:

```properties
# Kafka Bootstrap Servers
spring.kafka.bootstrap-servers=localhost:9092

# Producer Configuration
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer

# Consumer Configuration
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.group-id=user-activity-events-group

# Server Port
server.port=8080
```

### Docker Compose Customization
Edit `docker-compose.yml` to:
- Adjust partitions/replicas in `KafkaTopicConfig.java`
- Change container port mappings
- Modify Redpanda configuration options

---

## Topic Details

### Predefined Topics

**`user-activity-events-topic`**
- **Partitions**: 2
- **Replication Factor**: 1
- **Purpose**: Tracks user activity events (logins, actions, etc.)
- **Consumer Group**: `user-activity-events-group`

The topic is automatically created on application startup via `KafkaTopicConfig`.

---

## Troubleshooting

### Issue: Connection Refused (`localhost:9092`)
**Solution**: Ensure Redpanda is running:
```bash
docker-compose up -d
docker logs redpanda
```

### Issue: Application won't compile
**Solution**: Ensure `TopicBuilder` import is present in `KafkaTopicConfig.java`:
```java
import org.apache.kafka.clients.admin.TopicBuilder;
```

### Issue: Messages not being consumed
**Solution**: Verify consumer group matches topic listener:
```properties
# Should match @KafkaListener groupId
spring.kafka.consumer.group-id=user-activity-events-group
```

### Issue: Docker container runs but Kafka is unavailable
**Solution**: Check Docker logs and network:
```bash
docker-compose logs redpanda
docker network ls
```

---

## Testing

This project includes comprehensive JUnit 5 and Spring Boot tests using embedded Kafka for integration testing.

### Test Coverage

The project includes the following test classes:

1. **KafkaProducerTest** - Tests for message production
   - Verifies KafkaProducer bean initialization
   - Tests single and multiple message sending
   - Validates KafkaTemplate configuration

2. **KafkaConsumerTest** - Tests for message consumption
   - Verifies KafkaConsumer bean initialization
   - Tests consumer listener annotation

3. **KafkaControllerTest** - REST API endpoint tests
   - Health check endpoint (`GET /api/kafka/health`)
   - Send message with default topic (`POST /api/kafka/send`)
   - Send message with custom topic
   - Tests with special characters

4. **KafkaTopicConfigTest** - Topic configuration tests
   - Verifies topic bean creation
   - Validates topic properties (name, partitions, replication factor)

5. **KafkaSpringApplicationTest** - Application startup tests
   - Verifies Spring Boot application context loads

### Running Tests

**Run all tests:**
```bash
mvn test
```

**Run specific test class:**
```bash
mvn test -Dtest=KafkaProducerTest
```

**Run specific test method:**
```bash
mvn test -Dtest=KafkaControllerTest#testHealthEndpoint
```

**Skip tests during build:**
```bash
mvn clean package -DskipTests
```

**Run tests with verbose output:**
```bash
mvn test -X
```

### Test Dependencies

The tests use the following libraries (automatically downloaded):

| Dependency | Purpose |
|---|---|
| spring-boot-starter-test | Spring Boot testing framework |
| spring-kafka-test | Embedded Kafka for integration tests |
| JUnit 5 | Test runner and assertions |
| Mockito | Mocking framework (via Spring Boot Test) |

### Embedded Kafka

Tests use **EmbeddedKafka** which provides a lightweight, in-memory Kafka broker for testing without requiring Docker or an external Kafka instance.

**Configuration:**
- Port: `9093` (isolated from production `9092`)
- Partitions: 1
- Automatically cleaned up after each test

### Test Execution Flow

1. Spring Boot test context initializes
2. EmbeddedKafka broker starts on port 9093
3. Application components are autowired for testing
4. Test methods execute
5. Test context is cleaned up (including Kafka broker)
6. Next test starts fresh

### Writing New Tests

To add new tests, follow this pattern:

```java
@SpringBootTest
@EmbeddedKafka(partitions = 1, brokerProperties = {"listeners=PLAINTEXT://localhost:9093", "port=9093"})
@DirtiesContext
public class MyNewTest {

    @Autowired
    private MyComponent myComponent;

    @Test
    public void testMyFeature() {
        // Arrange
        String input = "test";
        
        // Act
        String result = myComponent.process(input);
        
        // Assert
        assertNotNull(result);
    }
}
```

### Test Troubleshooting

| Issue | Solution |
|---|---|
| Port 9093 already in use | Tests use @DirtiesContext to isolate, but ensure no other tests are running |
| Tests hang or timeout | Increase test timeout in pom.xml or IDE settings |
| Bean not found during tests | Verify @SpringBootTest and bean is in main classpath, not test-only |

---

## Stopping Services

### Stop Redpanda Container
```bash
docker-compose down
```

### Stop and Remove All Data
```bash
docker-compose down -v
```

---

## Dependencies

| Dependency | Version | Purpose |
|---|---|---|
| Spring Boot | 3.2.2 | Framework |
| Spring Kafka | Latest (from parent POM) | Kafka integration |
| Spring Web | Latest | REST API support |
| Apache Kafka | Latest (via Spring) | Message streaming |
| Java | 21+ | Runtime |

---

## Development Workflow

1. **Run Tests**: `mvn test`
2. **Build**: `mvn clean package`
3. **Run Locally**: `mvn spring-boot:run`
4. **View Logs**: Observe console output for consumed messages

---

## Next Steps

- Add **persistent storage** for consumer offsets
- Implement **error handling** and dead letter queues
- Add **message serialization** (JSON, Avro)
- Implement **unit tests** for producer/consumer
- Deploy to **Kubernetes** using Helm charts
- Add **monitoring** with Prometheus/Grafana

---

## License

This project is open source and available under the MIT License.

---

## Support

For issues or questions:
- Check the troubleshooting section above
- Review [Spring Kafka Documentation](https://spring.io/projects/spring-kafka)
- Review [Redpanda Documentation](https://docs.redpanda.com/)
