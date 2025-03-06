# NSD S15 : URL Shortener and Database Concepts

# **URL Shortener - System Design Notes**

## **Problem Statement**

A URL shortener takes a **long URL** and converts it into a **short, unique alias (tiny URL)**. This alias can be used to quickly redirect users to the original long URL.

### **Example**

**Input:**

🔗 `https://www.example.com/articles/how-to-build-a-url-shortener`

**Output:**

🔗 `https://tinyurl.com/abc123`

### **Why Do We Need It?**

- Long URLs are hard to **share** (e.g., in tweets, messages, QR codes).
- Short URLs look **cleaner** and are **easier to remember**.
- Useful for tracking clicks and analytics.

## **Requirement Gathering**

### **Short URLs**

The system should **generate the shortest possible URL** while maintaining uniqueness.

### **Handle High Traffic**

The system should support at least **10 million URLs per day** without slowing down.

### **Character Set for Short URLs**

- Uses **62 characters** → `0-9`, `a-z`, `A-Z`.
- This allows for **billions of unique short URLs**.
- Example: Instead of using numbers (`1, 2, 3`), Base62 encoding gives a **compact** version (`a, b, c, d...`).

---

## **Need for Hash Value**

A **hash function** converts a long URL into a **unique**, fixed-length code (like a fingerprint).

### **Why Use Hashing?**

- **Uniqueness** → Each URL gets a distinct short code.
- **Fast Lookup** → Quickly retrieves the original URL.
- **Security** → Makes URLs harder to guess.

### **Example**

Long URL:

 `https://www.example.com/articles/how-to-design-url-shortener`

MD5 Hash Output:

`e4d909c290d0fb1ca068ffaddf22cbd0`

We take only **6-8 characters** from this hash → `e4d909`

Now, we encode it in **Base62** → `xYz123`

Final Short URL:

`https://tinyurl.com/xYz123`

---

## **Generating Hash Values**

We can generate short URLs using different **hashing techniques**:

### **1. MD5 (Message Digest Algorithm 5)**

G**enerates a 128-bit hash value.**

**Fast and widely used.**

**Not collision-free (two different URLs might get the same hash).**

**Example:**

MD5(`"https://google.com"`) → `8ffdefbdec956b595d257f0aaeefd623`

We take first **6 characters** → `8ffdef`

---

### **SHA (Secure Hash Algorithm)**

**More secure than MD5**

**SHA-256 produces a 256-bit hash (longer but safer).**

**Slower than MD5.**

**Example:**

SHA-256(`"https://google.com"`) →

`2a978d2296bd243f52f372b352dd064d83224bb9eb7edb4e44d5bfbecfcd6f3c`

We take the first **6 characters** → `2a978d`

---

## **Limitations of Hashing**

**Collision Problem:** Two different URLs may generate the same short code.

**Solution:** Add **random characters, timestamp, or user ID** to make it unique.

**Irreversible:** Hashing is a **one-way function** – you **cannot get the original URL** from the hash.

**Solution:** Store the mapping in a database.

**Length Issues:** Full hash values are too **long**.

**Solution:** Use only **first 6-8 characters** and encode in **Base62**.

---

## **Converting Decimal to Base 62**

Since we are using **62 characters (0-9, a-z, A-Z)**, we need to **convert numbers to Base62**.

---

## **Distributed System Approaches**

Since we handle **billions of URLs**, we need a **scalable** system. Here are different approaches:

### **1. Single Database Approach**

- **Simple to implement.**
- **Not scalable – one machine can’t handle billions of URLs.**
- **Single point of failure.**

### **2. Multiple Databases (Sharding)**

- **Splits data across multiple databases → Faster queries.**
- **Complex shard lookup (which database has the data?).**

### **3. Ticket Server Approach**

A **separate server** generates **unique sequential IDs**.

- **No collisions.**
- **Ticket server can become a bottleneck.**

### **4. Snowflake Algorithm** (Best Choice )

- **Generates unique IDs across multiple servers.**
- **Scalable and fast.**
- **Slightly complex to set up.**

### **5. Zookeeper-Based ID Management**

- **Manages unique IDs across servers.**
- **Overhead due to constant coordination.**
- **Best Choice:** **Snowflake Algorithm** 🚀

---

## **TinyURL ID Ranges**

Each **tinyURL ID** is assigned to a **specific range** on different servers.

**Example:**

- **Server 1** handles IDs **0 - 999,999**
- **Server 2** handles IDs **1,000,000 - 1,999,999**
- **Server 3** handles IDs **2,000,000 - 2,999,999**

This **prevents duplication and speeds up lookup time**.

---

## **Handling URL Length with Padding**

Sometimes, a **generated short URL** is too short and needs to be **padded** for consistency.

### **Example:**

Short hash → `xYz`

Padded version → `00xYz`

**Why use padding?**

- **Keeps short URLs of uniform length**.
- **Better for indexing in databases**.

---

## **Conclusion**

A **URL Shortener System** requires:

- **Efficient Hashing** (MD5/SHA) & **Base62 Encoding** for short URLs.
- **Distributed System** for **scalability** (using Snowflake Algorithm).
- **Proper ID Range Allocation** to avoid collisions.
- **Padding Techniques** to maintain URL length consistency.

**Database Partitioning**

- **Definition**: Dividing a large table into smaller, more manageable pieces (partitions) within the same database.
- **Types**:
    - **Horizontal Partitioning**: Splits rows based on a column (e.g., users with ID 1-1000 in one partition, 1001-2000 in another).
    - **Vertical Partitioning**: Splits columns (e.g., storing frequently accessed columns separately).
- **Use Case**: Improves performance but still within a single database instance.

**Database Sharding**

- **Definition**: A form of horizontal partitioning where data is split across multiple databases (shards), each with its own instance.
- **Use Case**: Essential for high-scale applications (like URL shorteners) where a single database can't handle the load.

### **Types of Databases & Best Choice**

1. **Relational Databases (SQL)**
    - Structured data, ACID compliance.
    - Examples: MySQL, PostgreSQL, SQL Server.
    - **Best for**: Banking, ERP, transactional applications.
2. **NoSQL Databases**
    - Flexible schema, high scalability.
    - Types:
        - **Key-Value Stores** (Redis, DynamoDB) → Fast lookups.
        - **Document Stores** (MongoDB) → JSON-like storage, great for flexibility.
        - **Column Stores** (Cassandra) → Optimized for big data.
        - **Graph Databases** (Neo4j) → Relationship-heavy data.
    - **Best for**: Real-time apps, big data, social networks.
3. **NewSQL** (e.g., CockroachDB, Google Spanner)
    - Combines SQL reliability with NoSQL scalability.
    - **Best for**: Modern distributed applications.