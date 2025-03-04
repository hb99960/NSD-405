# NSD S14 : Monolith, Micro-services, Rate limiter, Messaging Queue

## **1. Monolith vs Microservices**

### **Monolithic Architecture**

All components (UI, business logic, database) are tightly coupled in a **single application**.

Simple to develop and deploy but difficult to scale.

**Practical Example:**

- A **basic e-commerce app** where the product listing, cart, and payments are all in one codebase.
- If one feature fails, the entire app can crash.

**Microservices Architecture**

Application is broken into **independent services**, communicating via APIs.

Scalable and fault-tolerant but **complex to manage**.

**Practical Example:**

- **Amazon, Netflix, Uber** use microservices to handle users, orders, and payments separately.
- Each service can be updated independently.

**Tools to Measure & Monitor:**

- **Monolith:** Application logs (`log4j`, `Winston` for Node.js).
- **Microservices:** **Jaeger, Zipkin** (distributed tracing), **Kubernetes** (orchestration).

---

## **2. Rate Limiter**

**Controls the number of requests** a user or service can make within a given time to prevent overload or abuse.

**Practical Example:**

- **GitHub API limits** unauthenticated users to **60 requests/hour** to prevent excessive API calls.
- **Login attempts** are restricted to avoid brute force attacks.

**Practical Tools:**

- **Redis + Rate-limiter** (Node.js example using `rate-limiter-flexible`).
- **Cloudflare Rate Limiting** (protects APIs).
- **nginx / AWS API Gateway** (built-in throttling).

---

## **3. Messaging Queue**

**Manages asynchronous communication** between services to improve performance.

Prevents **data loss** by storing messages until they are processed.

**Practical Example:**

- **E-commerce order processing**:
    1. User places an order (request goes to **Queue**).
    2. Payment, inventory, and notifications **consume** messages **asynchronously**.
- **WhatsApp** uses queues to handle **billions of messages** efficiently.

**🛠 Practical Tools:**

- **RabbitMQ** (popular, lightweight).
- **Apache Kafka** (high-throughput for event streaming).
- **AWS SQS** (serverless queueing).

---

## **4. Consistent Hashing**

Used in **distributed systems** to distribute data evenly across servers.

Helps in **load balancing and caching**.**Practical Example:**

- **CDN Load Balancing**: Requests are mapped to specific servers using hashing (e.g., **Cloudflare**).
- **Distributed Cache (Redis/Memcached)**: Stores data in **sharded** databases to avoid overload.

**🛠 Practical Tools:**

- **nginx + Consistent Hashing** for load balancing.
- **Redis Cluster** for distributed caching.
- **Consistent Hashing in Kafka** (partitioning).

---

## **5. REST vs RPC**

| Feature | REST (Resource-Based) | RPC (Function-Based) |
| --- | --- | --- |
| **Concept** | Uses **HTTP methods** (GET, POST, etc.) | Calls **remote methods** directly |
| **Data Format** | JSON, XML | JSON, Protocol Buffers (gRPC) |
| **Use Case** | Web APIs (Twitter, GitHub) | Microservices (gRPC, Thrift) |
| **Performance** | Slower (text-based) | Faster (binary-based) |

**Practical Example:**

- **REST API**: `GET /users/123` → Fetch user details.
- **RPC (gRPC)**: `getUser(123)` → Calls a function on the server.

**🛠 Practical Tools:**

- **Postman/Insomnia** for REST API testing.
- **gRPC UI / BloomRPC** for testing RPC services.
- **Wireshark** (analyze network calls).