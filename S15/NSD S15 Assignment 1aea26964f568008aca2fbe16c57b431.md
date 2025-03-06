# NSD S15 Assignment

### **Multiple-Choice Questions (MCQs)**

1. **What is the main purpose of a URL shortener?**
    
    a) To increase the security of a URL
    
    b) To shorten a long URL while preserving its redirection
    
    c) To encrypt the URL for private access
    
    d) To improve the SEO ranking of the URL
    
2. **Which encoding method is commonly used to generate short URLs?**
    
    a) Base 16
    
    b) Base 32
    
    c) Base 62
    
    d) Base 128
    
3. **Why do we use hashing in a URL shortener?**
    
    a) To make URLs easier to remember
    
    b) To uniquely map long URLs to short ones
    
    c) To store URLs in a database efficiently
    
    d) To prevent URL redirections
    
4. **Which hashing algorithms can be used to generate short URLs?**
    
    a) MD5
    
    b) SHA-256
    
    c) Both MD5 and SHA-256
    
    d) AES
    
5. **What is the main limitation of using a simple hash function for URL shortening?**
    
    a) It increases the URL length
    
    b) It makes the URL harder to decode
    
    c) Hash collisions can occur
    
    d) It requires excessive computation power
    
6. **Which distributed system approach is best suited for a URL shortener to ensure scalability and unique ID generation?**
    
    a) Single Database
    
    b) Ticket Server
    
    c) Snowflake Algorithm
    
    d) Vertical Partitioning
    
7. **Why is Base 62 encoding preferred for short URL generation?**
    
    a) It provides more combinations using alphanumeric characters
    
    b) It is the fastest encoding method
    
    c) It ensures the highest security
    
    d) It reduces storage requirements significantly
    
8. **Which type of NoSQL database is best suited for a URL shortener?**
    
    a) Document-based (MongoDB)
    
    b) Key-Value Store (DynamoDB, Redis)
    
    c) Column Store (Cassandra)
    
    d) Graph Database (Neo4j)
    
9. **How does sharding help in scaling a URL shortener?**
    
    a) By storing data in different formats
    
    b) By distributing URL mappings across multiple servers
    
    c) By making URLs shorter
    
    d) By caching frequently accessed URLs
    
10. **What is the primary reason for padding short URLs?**
    
    a) To ensure all URLs are of equal length
    
    b) To increase the randomness of the hash
    
    c) To make URLs more secure
    
    d) To store URLs more efficiently
    

---

### **Subjective Question**

🔹 **Design a High-Level Architecture (HLD) for a URL Shortener that can handle 10 million requests per day.**

- Explain the key components required (Load Balancer, Database, Cache, etc.).
- How will you ensure unique URL generation?
- Which storage method will you use (SQL vs. NoSQL) and why?
- How will you scale the system for future growth?

Submit Guidelines

- Submit your Masai Github Repo link