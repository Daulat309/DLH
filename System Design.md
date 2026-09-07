# System Design — Revision Notes

## 1. Client–Server Architecture

The foundation of a system is Client–Server Architecture.

- **Client:** Device/application used by the user to send requests.
- **Server:** A machine that runs continuously (24/7) and handles client requests.
- The server generally has a public IP address so clients can communicate with it.

```
Client
   |
   | Request
   ↓
Server
   |
   | Response
   ↓
Client
```

## 2. DNS (Domain Name System)

DNS acts as a global directory that maps human-readable domain names to machine-readable IP addresses.

Example:

```
amazon.com  →  IP Address
```

Instead of remembering an IP address, users can access the system using a domain name.

## 3. Scaling

Scaling means increasing the system's capacity to handle more users or requests.

There are two main types:

### Vertical Scaling

Increasing the capacity of a single server.

```
Before:
Server
CPU: 4 Core
RAM: 8 GB

       ↓ Upgrade

After:
Server
CPU: 16 Core
RAM: 32 GB
```

**Advantages:**
- Simple to implement.
- No need to manage multiple servers.

**Disadvantage:**
- Upgrading the server can cause downtime.

### Horizontal Scaling

Adding more server instances/replicas instead of increasing the capacity of one server.

```
             ┌── Server 1
             │
Client → Load Balancer ── Server 2
             │
             └── Server 3
```

**Advantages:**
- Supports higher traffic.
- Provides zero/minimal downtime.
- If one server fails, other servers can continue handling requests.

**Requirement:**
- A Load Balancer is generally needed to distribute traffic among servers.

## 4. Load Balancer

A Load Balancer sits in front of multiple servers and distributes incoming requests among them.

**Example algorithm:**

**Round Robin**

Requests are distributed sequentially.

```
Request 1 → Server 1
Request 2 → Server 2
Request 3 → Server 3
Request 4 → Server 1
Request 5 → Server 2
```

This prevents a single server from receiving all the traffic.

**AWS Example:** ELB (Elastic Load Balancer)

## 5. API Gateway & Microservices

An API Gateway acts as a centralized entry point for requests.

It routes requests to the appropriate microservice based on the request path.

```
                 ┌── /auth     → Auth Service
Client → API Gateway ── /orders   → Order Service
                 └── /payments → Payment Service
```

This allows different functionalities to be handled by separate microservices.

## 6. Asynchronous Processing & Queue Systems

Heavy or time-consuming tasks should not always be handled directly by the main server.

Instead, a queue can be used.

**Example:**

```
Client
   ↓
Server
   ↓
Queue
   ↓
Worker
   ↓
Send Email
```

The main server puts the task into the queue and can continue processing other requests.

**Example:** AWS SQS

**Benefits:**
- Prevents the main server from getting blocked.
- Handles heavy background tasks.
- Helps prevent bottlenecks.
- Workers can process queued tasks asynchronously.

## 7. Pub/Sub & Event-Driven Architecture

Pub/Sub (Publish/Subscribe) allows one event to notify multiple systems.

**Example:**

```
Payment Successful
        ↓
      Event
   ┌────┼────┐
   ↓    ↓    ↓
 Email WhatsApp SMS
```

A service publishes an event, and multiple subscribers react to that event.

**Example:** AWS SNS

## 8. Fan-Out Architecture

Fan-Out is a pattern where a single event is distributed to multiple queues.

```
              Event
                ↓
              SNS
        ┌───────┼───────┐
        ↓       ↓       ↓
     Queue 1 Queue 2 Queue 3
        ↓       ↓       ↓
    Service A Service B Service C
```

Different microservices can process the same event for different purposes.

## 9. Rate Limiting

Rate Limiting restricts the number of requests a user can make within a specific time period.

**Example:**

```
100 requests / minute / user
```

If a user exceeds the limit, additional requests can be rejected or delayed.

**Purpose:**
- Protects the system from excessive traffic.
- Prevents abuse.
- Helps protect against attacks such as DDoS attacks.
- Ensures resources are available for legitimate users.

## 10. Database Scaling

As the number of users increases, the database can become a bottleneck.

**Read Replicas**

A primary database handles writes, while read replicas can handle read queries.

```
              Application
                  ↓
             Primary DB
             ↙       ↘
        Read Replica  Read Replica
```

This helps offload read traffic from the primary database.

## 11. Caching

Caching stores frequently accessed data in memory so that it can be retrieved faster.

**Example:**

```
Client
  ↓
Server
  ↓
Redis Cache
  ↓
Database
```

If data is available in the cache, the system can return it without querying the database.

**Example:** Redis

**Benefits:**
- Reduces latency.
- Reduces database load.
- Improves response time.

## 12. Content Delivery Network (CDN)

A CDN stores/caches content at geographically distributed edge locations.

**Example:**

```
                 Origin Server
                      ↓
                    CDN
              ┌───────┼───────┐
              ↓       ↓       ↓
           India    Europe    USA
          Edge      Edge      Edge
```

Users receive content from an edge location that is geographically closer to them.

**Example:** AWS CloudFront

**Benefits:**
- Reduces latency.
- Faster content delivery.
- Reduces load on the origin server.
