# NSD S13 : Introduction of HLD

# **System Design Basics & Important Trade-offs**

## **Introduction: High-Level Design (HLD) Components**

At the **infrastructure level**, a system consists of various components that help in **handling, storing, processing, and delivering** data efficiently. These components are classified into **hardware and software** elements.

### **🔹 Hardware Components**

1️⃣ **Servers** – Machines that process requests and execute computations.

2️⃣ **Load Balancer (LB)** – Distributes traffic across multiple servers to prevent overload.

3️⃣ **Database** – Stores structured or unstructured data (SQL, NoSQL).

### **🔹 Software Components**

4️⃣ **CDN (Content Delivery Network)** – Stores cached copies of static content across multiple locations for faster access.

5️⃣ **Caching** – Temporarily stores frequently accessed data to improve performance.

6️⃣ **Messaging Queue** – A system that enables asynchronous communication between different services (e.g., Kafka, RabbitMQ).

7️⃣ **Rate Limiter** – Prevents system abuse by restricting the number of requests a user can send within a time window.

These components work **together** to create **scalable, efficient, and reliable** software systems.

---

## **Important Terminologies & Trade-offs in System Design**

Let's explore some fundamental **concepts** in system design with **real-world examples**.

---

### **CAP Theorem (Consistency, Availability, Partition Tolerance)**

CAP Theorem states that a distributed system can only achieve two out of three properties at a time:

- **Consistency (C)** – All nodes return the most recent data.
- **Availability (A)** – The system is always responsive.
- **Partition Tolerance (P)** – The system continues working even when network failures occur.

**Example:**

- **CP (Consistency + Partition Tolerance):** MongoDB (sacrifices availability during network failures).
- **AP (Availability + Partition Tolerance):** Cassandra (sacrifices strong consistency to ensure uptime).

---

### **Vertical vs. Horizontal Scaling**

- Scaling means increasing system capacity to handle more requests.
- **Vertical Scaling:** Adding more power (CPU, RAM) to a single machine.
- **Horizontal Scaling:** Adding more machines (servers) to distribute the load.

**Example:**

- **Vertical Scaling:** Upgrading a server from 16GB RAM to 32GB.
- **Horizontal Scaling:** Adding 10 servers instead of a single powerful one (used by Google, Amazon).

---

### **Concurrency vs. Parallelism**

- Concurrency: Multiple tasks are processed at the same time but not necessarily executed simultaneously.
- **Parallelism:** Multiple tasks are executed **at the exact same moment** using multiple CPU cores.

**Example:**

- **Concurrency:** A single-threaded Node.js server handling multiple API requests.
- **Parallelism:** Running a Machine Learning model on a multi-core processor.

---

### **Long Polling vs. WebSockets**

- Long Polling: Client repeatedly requests updates from the server.
- **WebSockets:** A continuous, open connection between client and server for real-time communication.

**Example:**

- **Long Polling:** Chat applications before WebSockets (e.g., AJAX-based chat).
- **WebSockets:** WhatsApp Web or Google Docs real-time collaboration.

---

### **Batch Processing vs. Stream Processing**

- Batch Processing: Data is processed in large chunks at scheduled times.
- **Stream Processing:** Data is processed in real-time as it arrives.

**Example:**

- **Batch Processing:** Payroll processing (salary calculated at the end of the month).
- **Stream Processing:** Fraud detection in banking (transactions analyzed instantly).

---

### **Stateful vs. Stateless Design**

- Stateful: The server remembers past interactions with a client.
- **Stateless:** Each request is independent, and the server does not store session data.

**Example:**

- **Stateful:** Online gaming sessions (game state is stored).
- **Stateless:** REST APIs (each request carries all necessary information).

---

### **Strong Consistency vs. Eventual Consistency**

- Strong Consistency: Every read gets the latest data immediately.
- **Eventual Consistency:** Reads might return old data, but all nodes will eventually synchronize.

**Example:**

- **Strong Consistency:** Banking systems (money transfers must be consistent).
- **Eventual Consistency:** Social media feeds (you might see an old post before updates arrive).

---

### **Read-Through vs. Write-Through Cache**

- Read-Through Cache: If data is not in the cache, it is fetched from the database and stored in the cache.
- **Write-Through Cache:** Every write operation is **simultaneously** updated in the cache and database.

**Example:**

- **Read-Through:** Redis caching website products (if not in cache, fetch from DB).
- **Write-Through:** Updates in the user profile instantly reflect in cache & DB.

---

### **Push vs. Pull Architecture**

- Push: The server sends updates to clients as soon as they happen.
- **Pull:** Clients **request updates** at intervals.

**Example:**

- **Push:** Live cricket score notifications.
- **Pull:** Checking emails manually.

---

### **REST vs. RPC**

- REST (Representational State Transfer): Uses HTTP methods (GET, POST) and is stateless.
- **RPC (Remote Procedure Call):** Calls functions on a remote server **directly**.

**Example:**

- **REST:** `GET /users/123` (fetch user info).
- **RPC:** `getUser(123)` (direct function call).

---

### **Synchronous vs. Asynchronous Communication**

- Synchronous: A request waits for a response before continuing.
- **Asynchronous:** A request continues executing without waiting.

**Example:**

- **Synchronous:** Calling an API and waiting for a response (`fetch()` without `async/await`).
- **Asynchronous:** Sending an email (does not block execution).

---

### **Latency vs. Throughput**

- Latency: Time taken to complete a single request.
- **Throughput:** Number of requests processed **per second**.

**Example:**

- **Low latency:** Google Search (results appear instantly).
- **High throughput:** Netflix handling millions of streams at once.