

# 🧩 Spring Boot & Apache Kafka Microservices Application

This project demonstrates a **microservices-based architecture** built using **Spring Boot** and **Apache Kafka**.
It consists of four independent modules that communicate asynchronously through Kafka topics, ensuring scalability, modularity, and resilience.

---

## 🏗️ Modules Overview

| Module Name       | Description                                                                            |
| ----------------- | -------------------------------------------------------------------------------------- |
| **Order Service** | Handles customer orders and publishes order-related events to Kafka.                   |
| **Stock Service** | Listens to order events and updates product inventory accordingly.                     |
| **Email Service** | Subscribes to Kafka events and sends notifications (e.g., order confirmation emails).  |
| **Base Domain**   | Contains common domain models, DTOs, and event definitions shared across all services. |

Each service runs independently, with its own Spring Boot application context and Kafka configuration.

---

## ⚙️ Requirements

To run this project successfully, ensure you have the following installed:

* **Java:** 17 or higher
* **Apache Maven:** 3.8+
* **Gradle:** (Optional alternative build tool)
* **Kafka & Zookeeper:** Running locally or on Docker
* **Database:** MySQL / PostgreSQL (depending on your setup)

---

## 🧠 Architecture Overview

The system follows a **4-layered clean architecture**, separating concerns and improving maintainability.

### 1️⃣ Presentation Layer

This is the topmost layer that interacts directly with the client or external APIs.

**Responsibilities:**

* Handles all **HTTP requests** and **responses**.
* Converts **JSON ↔ Java Objects (DTOs)** using Jackson.
* Performs **authentication** and **authorization checks** before accessing business logic.
* Delegates business processing to the **Service Layer**.

**Example Components:**

* `@RestController` classes
* Request/Response DTOs
* Exception Handlers

---

### 2️⃣ Business Layer

This layer encapsulates all **application logic and rules**.

**Responsibilities:**

* Performs **input validation**.
* Implements **authorization rules** (role-based access).
* Contains the **core business logic** (e.g., processing an order, validating stock).
* Publishes and consumes **Kafka events** between microservices.

**Example Components:**

* `@Service` classes
* Event producers and consumers
* Business logic handlers

---

### 3️⃣ Persistence Layer

This layer deals with **data persistence and retrieval**.

**Responsibilities:**

* Defines **repositories** to perform CRUD operations.
* Maps Java entities to database tables using **JPA/Hibernate**.
* Manages **data access transactions**.

**Example Components:**

* `@Repository` interfaces
* `JpaRepository` or `CrudRepository`
* Entity classes

---

### 4️⃣ Database Layer

This is the actual database that stores application data.

**Responsibilities:**

* Stores persistent data (e.g., orders, inventory).
* Supports **CRUD operations** through SQL.
* Can be any RDBMS like **MySQL**, **PostgreSQL**, or **H2 (for testing)**.

**Example:**

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/orderdb
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
```

---

## 🔁 Communication Flow (Kafka Event-Driven Design)

1. **Order Service** receives an order request from a client.
2. It **validates** and **saves** the order in the database.
3. It then **publishes an event** (e.g., `OrderCreatedEvent`) to a Kafka topic.
4. **Stock Service** consumes the event and updates inventory.
5. **Email Service** listens to the same topic and sends an order confirmation email to the customer.

This **event-driven communication** ensures that services are **loosely coupled** and can scale independently.

---

## 🧱 Software Structure

```
Kafka-Microservices-App/
│
├── base-domain/
│   └── src/main/java/com/example/basedomain
│       ├── events/
│       ├── dto/
│       └── model/
│
├── order-service/
│   └── src/main/java/com/example/orderservice
│       ├── controller/
│       ├── service/
│       ├── repository/
│       └── entity/
│
├── stock-service/
│   └── src/main/java/com/example/stockservice
│       ├── consumer/
│       ├── service/
│       └── repository/
│
├── email-service/
│   └── src/main/java/com/example/emailservice
│       ├── consumer/
│       ├── service/
│       └── config/
│
└── pom.xml / build.gradle
```

Each service has its own independent dependencies and configurations, while `base-domain` provides shared models and events.

---

## 🚀 Getting Started

### Step 1️⃣ – Clone the Repository

```bash
git clone https://github.com/yourusername/Kafka-Microservices-App.git
cd Kafka-Microservices-App
```

### Step 2️⃣ – Build the Project

```bash
./mvnw clean install
```

### Step 3️⃣ – Run Each Service

Navigate into each module (e.g., `order-service`, `stock-service`, `email-service`) and run:

```bash
./mvnw spring-boot:run
```

Each service will start on a different port (e.g., `8081`, `8082`, `8083`).

---

## ⚡ Usage

1. Start **Kafka** and **Zookeeper**:

   ```bash
   zookeeper-server-start.sh config/zookeeper.properties
   kafka-server-start.sh config/server.properties
   ```
2. Create necessary topics (e.g., `order_topic`):

   ```bash
   kafka-topics.sh --create --topic order_topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
   ```
3. Send an order request using **Postman**:

   ```json
   POST /api/orders
   {
     "orderId": "ORD123",
     "product": "Laptop",
     "quantity": 2,
     "email": "customer@example.com"
   }
   ```
4. Observe:

   * Order Service → publishes event
   * Stock Service → consumes and updates inventory
   * Email Service → sends confirmation

---

## 🔧 Configuration

Each service contains an `application.yml` or `application.properties` file where you can configure:

* Database connection
* Kafka broker address
* Service ports
* Email SMTP settings (for Email Service)

---

## 🧪 Testing

Run unit tests using Maven:

```bash
./mvnw test
```

You can also use **Kafka UI tools** like [Conduktor](https://www.conduktor.io/) or [Kafka Tool](http://www.kafkatool.com/) to monitor topics and events.

---

## 🧰 Tech Stack

| Technology          | Purpose                           |
| ------------------- | --------------------------------- |
| **Spring Boot**     | Microservice framework            |
| **Apache Kafka**    | Event streaming and communication |
| **Spring Data JPA** | ORM and database abstraction      |
| **MySQL**           | Data persistence                  |
| **Lombok**          | Boilerplate code reduction        |
| **Docker**          | Containerization (optional)       |
| **JUnit**           | Unit testing                      |

---

## 🧑‍💻 Author

**Abhhirram Veedhi**
Software Developer @ Encora
💼 Focus: Java | Spring Boot | Microservices | AWS | Kafka
📧 Email: [abhhirram2003@example.com](mailto:abhhirram2003@example.com)
🔗 GitHub: [github.com/abhhirramVeedhi](https://github.com/abhhirramVeedhi)
