# Backend Technical Interview Questions Guide

## Core Concepts

### 1. RESTful APIs
**Q: What makes an API RESTful?**
```text
Answer:
- Stateless communication
- Client-server architecture
- Uniform interface (URI resources, HTTP methods)
- Cacheable responses
- Layered system
- Resource-based URLs
- Proper use of HTTP methods (GET, POST, PUT, DELETE)
```

**Q: Explain the difference between PUT and PATCH**
```text
PUT: Complete resource update, needs all fields
PATCH: Partial resource update, only sends changed fields

Example:
PUT /users/1
{
    "name": "John",
    "email": "john@email.com",
    "age": 30
}

PATCH /users/1
{
    "age": 31
}
```

### 2. Database Concepts

**Q: Explain the difference between SQL and NoSQL databases**
```text
SQL:
- Structured schema
- ACID compliance
- Vertical scaling
- Table relationships
- Example: PostgreSQL, MySQL

NoSQL:
- Flexible schema
- Eventual consistency
- Horizontal scaling
- Document/Key-Value based
- Example: MongoDB, Redis
```

**Q: What is database sharding?**
```text
Answer:
- Horizontal partitioning of data
- Data split across multiple databases
- Based on shard key (e.g., user_id, region)
- Benefits:
  * Improved performance
  * Better scalability
  * Reduced query time
- Challenges:
  * Complex joins
  * Data rebalancing
  * Maintaining consistency
```

### 3. Caching

**Q: Explain different caching strategies**
```text
1. Cache-Aside:
   - Application checks cache first
   - If miss, get from DB and update cache
   
2. Write-Through:
   - Write to cache and DB simultaneously
   
3. Write-Behind:
   - Write to cache first
   - Asynchronously update DB
   
4. Read-Through:
   - Cache automatically loads from DB on miss
```

**Q: How would you handle cache invalidation?**
```text
Strategies:
1. TTL (Time-To-Live)
   - Set expiration time
   - Automatic invalidation

2. Event-Based
   - Invalidate on data updates
   - Publish-subscribe pattern

3. Version Tags
   - Track content versions
   - Update on changes
```

### 4. System Design

**Q: Design a URL shortener service**
```text
Components:
1. API Layer:
   - Endpoint for URL submission
   - Endpoint for URL retrieval

2. Encoding Service:
   - Generate short URL
   - Base62 encoding or Hash function

3. Database:
   - Store URL mappings
   - Index on short URL

4. Cache Layer:
   - Redis for frequent URLs
   - LRU cache policy

Architecture:
URL → Load Balancer → API Servers → Cache → Database
```

**Q: How would you design a real-time notification system?**
```text
Components:
1. Event Source:
   - User actions
   - System events

2. Message Queue:
   - Kafka/RabbitMQ
   - Event processing

3. Notification Service:
   - WebSocket connections
   - Push notifications

4. User Preferences:
   - Notification settings
   - Delivery channels
```

### 5. Microservices

**Q: What are the challenges in microservices architecture?**
```text
Challenges:
1. Service Discovery
   - Finding service instances
   - Load balancing

2. Data Consistency
   - Distributed transactions
   - Event consistency

3. Communication
   - Network latency
   - Protocol selection

4. Monitoring
   - Distributed logging
   - Transaction tracing

Solutions:
- Service mesh (Istio)
- API Gateway
- Circuit breakers
- Distributed tracing (Jaeger)
```

### 6. Security

**Q: Explain JWT (JSON Web Tokens)**
```text
Structure:
- Header (algorithm, token type)
- Payload (claims)
- Signature

Example:
header.payload.signature

Usage:
1. Authentication
2. Information exchange
3. Stateless sessions

Security Considerations:
- Token expiration
- Secure storage
- HTTPS transmission
```

**Q: How do you handle API security?**
```text
1. Authentication:
   - JWT
   - OAuth 2.0
   - API keys

2. Authorization:
   - Role-based access
   - Scope-based access

3. Rate Limiting:
   - Request throttling
   - Token bucket algorithm

4. Input Validation:
   - Sanitize inputs
   - Validate data types
```

### 7. Performance Optimization

**Q: How would you optimize database queries?**
```text
Strategies:
1. Indexing:
   - Proper index selection
   - Covering indexes

2. Query Optimization:
   - Explain plans
   - Minimize joins
   - Use appropriate fields

3. Connection Pooling:
   - Reuse connections
   - Configure pool size

4. Caching:
   - Result caching
   - Query caching
```

**Q: Explain horizontal vs vertical scaling**
```text
Horizontal Scaling:
- Add more machines
- Distribute load
- Better for microservices
- Example: Adding web servers

Vertical Scaling:
- Add more resources
- Upgrade existing machine
- Simpler architecture
- Example: Upgrading RAM/CPU
```

### 8. Coding Challenges

**Q: Implement a rate limiter**
```python
from time import time
from collections import defaultdict

class RateLimiter:
    def __init__(self, requests_per_second):
        self.requests_per_second = requests_per_second
        self.requests = defaultdict(list)
    
    def is_allowed(self, user_id):
        current_time = time()
        user_requests = self.requests[user_id]
        
        # Remove old requests
        while user_requests and user_requests[0] <= current_time - 1:
            user_requests.pop(0)
            
        # Check if allowed
        if len(user_requests) < self.requests_per_second:
            user_requests.append(current_time)
            return True
            
        return False
```

**Q: Design a connection pool**
```python
from queue import Queue
import threading

class ConnectionPool:
    def __init__(self, size):
        self.size = size
        self.connections = Queue(size)
        self.lock = threading.Lock()
        
        # Initialize pool
        for _ in range(size):
            self.connections.put(self.create_connection())
    
    def create_connection(self):
        # Create new database connection
        return {"connection": "db_connection"}
    
    def get_connection(self):
        return self.connections.get()
    
    def release_connection(self, connection):
        self.connections.put(connection)
```

### 9. System Architecture

**Q: Explain event-driven architecture**
```text
Components:
1. Event Producers
   - Generate events
   - Publish to topics

2. Event Bus
   - Message queue
   - Event routing

3. Event Consumers
   - Subscribe to topics
   - Process events

Benefits:
- Loose coupling
- Scalability
- Flexibility

Challenges:
- Event ordering
- Error handling
- Eventual consistency
```

### 10. Best Practices

**Q: What are the principles of clean code?**
```text
1. SOLID Principles:
   - Single Responsibility
   - Open-Closed
   - Liskov Substitution
   - Interface Segregation
   - Dependency Inversion

2. Code Organization:
   - Clear naming
   - Small functions
   - DRY principle
   - Comments when needed

3. Error Handling:
   - Proper exceptions
   - Meaningful messages
   - Graceful degradation
```

## Interview Tips

### Technical Discussion
1. Ask clarifying questions
2. Think aloud while solving problems
3. Discuss trade-offs
4. Consider scalability
5. Address security concerns

### System Design Approach
1. Requirements gathering
2. High-level design
3. Detailed component design
4. Scalability considerations
5. Monitoring and maintenance

### Code Review
1. Write clean, readable code
2. Handle edge cases
3. Consider performance
4. Add error handling
5. Follow best practices
