
# Lect 19

## Challenges of Denormalization

1. **Redundant data** → wastage of memory

2. **Increased complexity**

3. **Data inconsistency**

4. **Slow write operations**  
   Since we need to write to multiple places due to redundancy.



# Lect 20

# Indexing

## What is Indexing?

**Indexing** creates a **lookup table** with the indexed column and a **pointer to the memory location of the row** containing that column.

### Example

Suppose we have a `Student` table:

| id | name | age | net_worth |
|---|---|---|---|
| 1 | Ram | 20 | ₹10000 |
| 2 | Shyam | 21 | ₹20000 |
| 3 | Akshay | 22 | ₹30000 |
| 4 | Sita | 23 | ₹40000 |

If we create an index on `net_worth`, the database creates a lookup structure:

```text
net_worth → Pointer to Row

₹10000 → Row 1
₹20000 → Row 2
₹30000 → Row 3
₹40000 → Row 4
```

**B-Tree data structure** is used to store the indexing as it is a **multilevel format of tree-based indexing**, which has **balanced binary search trees**.

### Key Points

- Indexing creates a **lookup table**.
- It stores the **indexed column value** and a **pointer to the corresponding row**.
- **B-Trees** are commonly used for database indexing.
- B-Tree provides **multilevel tree-based indexing**.
- The tree remains **balanced**, making searching efficient.

# Lect  21 

# Synchronous Communication

**Synchronous communication** is a **blocking communication** model where the sender waits for the receiver to process the request and return a response.

### Working

```text
APP  ──────── request ────────>  APP
APP  <─────── response ────────  APP
```
The sender remains blocked/waiting until the response is received.

Example

An ATM transaction is a common example of synchronous communication:
User
  ↓
ATM → Request
  ↓
Bank Server
  ↓
Response
  ↓
ATM → User

The user waits for the ATM to complete the transaction and provide a response.

### Why is Synchronous Communication Used?
1. To achieve consistency
2. For transaction processing

For example, in a transaction where:
a = 5
      ↓
   update
      ↓
a = 6

the operation must be completed consistently before proceeding to the next operation.

### Industrial Use Cases
  1. Stock Market 📈
  2. Bank Payments 💳
  3. Ticket Booking 🎟️
  4. Real-Time Decision Making
### Key Point
- Synchronous Communication = Blocking Call
- The caller sends a request and waits until it receives the response.


# Lect22
# Message-Based Communication — Short Revision Notes

## 1. Message-Based Communication

- Client sends a **request in the form of a message**.
- Client receives the **response in the form of a message**.
- It is generally **asynchronous**.
- Client is **not required to halt/wait** for the process to complete.

## 2. Key Components

- **Producer** → Produces/sends messages.
- **Consumer** → Receives/consumes messages.
- **Agent** → Acts as an intermediary to manage/deliver messages.

## 3. P2P Model

- **P2P (Peer-to-Peer)** communication allows peers to communicate directly.
- A peer can act as both **Producer and Consumer**.
- There is no central message broker necessarily required.

### Flow

`Producer/Peer ↔ Consumer/Peer`

## 4. Publish-Subscribe Model

- **Publisher** publishes messages to a **topic/channel**.
- **Subscribers** subscribe to topics they are interested in.
- A message can be delivered to **multiple subscribers**.
- Publisher does not need to know individual subscribers.

### Flow

`Publisher → Topic → Multiple Subscribers`

## 5. Examples

- **Apache Kafka**
- **RabbitMQ**

## Quick Comparison

| Model | Communication |
|---|---|
| **P2P** | Peer ↔ Peer |
| **Publish-Subscribe** | Publisher → Topic → Subscribers |

## Remember

> **P2P = Direct peer communication**  
> **Pub-Sub = Publish once, deliver to interested subscribers**  
> **Message-Based = Communication through messages, generally asynchronously**


# Lect23
# Communication Protocols — Short Revision Notes

## 1. Communication Protocol

A communication protocol defines:

- **Rules** of communication
- **Syntax** of messages
- **Semantics** (meaning) of messages
- **Timing** of communication
- Possible **error-recovery methods**

Protocols can be implemented using:

- **Hardware**
- **Software**
- **Combination of both**

---

# Communication Models

## 1. Push

- The **server pushes new events/messages to the client**.
- Client does not need to continuously request new data.
- Useful when the server needs to send updates to the client.

### Disadvantages
- **Ordering issues** may occur.
- Server may be **always busy** handling/pushing events.

---

## 2. Pull / Polling

- The **client repeatedly requests** the server for new data.
- Client initiates communication whenever it wants to check for updates.
- Requires **frequent requests** when updates are needed regularly.

### Example

```text
Client → Server : "Any new data?"
Client ← Server : Response
```

## 3. Long Polling
- Client sends a request to the server.
- Instead of immediately responding, the server can keep the request open until new data/event is available.
- After receiving the response, the client can make another request.
  
 Key Idea
 ```text
 Client → Server : Request
                  ↓
              Wait for event
                  ↓
 Client ← Server : New data
```

## 4. Socket
- A socket is an endpoint of a two-way connection between two servers/nodes over a network.
- Provides a two-way communication channel.
= The connection can remain active for continuous communication.

Key Idea
```text
Client/Node  ↔  Socket Connection  ↔  Server/Node
```

Use When
- We need continuous and frequent communication.
- A persistent connection is useful for real-time interaction.

## 5. Server-Sent Events (SSE)
- Client subscribes to a server stream.
- Server sends a stream of events to the client.
- Events continue to be sent until the client or server closes the stream.
- It is a one-way connection from server to client.
- The connection is long-lived.

Key Idea
```text
Client → Subscribe

Server ───────────────→ Client
       Event 1
       Event 2
       Event 3
       Event 4
       ...
```
      
Example
```text
Cricbuzz:
Client subscribes to the server stream, and the server continuously sends a stream of events
(e.g., live updates) until the stream is closed.
```

## Quick Comparison

| Model | Main Idea | Connection |
| ------------------------ | ------------------------------------------------------- | --------------- |
| **Push** | Server pushes updates to client | Server → Client |
| **Pull / Polling** | Client repeatedly asks for updates | Client → Server |
| **Long Polling** | Server keeps request open until data/event is available | Client ↔ Server |
| **Socket** | Continuous two-way communication | **Two-way** |
| **SSE** | Server continuously sends events to client | **One-way** |

## Remember

- **Push** → Server sends updates.
- **Polling** → Client keeps asking.
- **Long Polling** → Client asks, server waits for an event.
- **Socket** → Continuous **two-way** connection.
- **SSE** → Long-lived **one-way server → client** event stream.

# Lect26
# REST API — Revision Notes

## 1. Web Application

A **web application** is an application that runs over the internet and is accessed through a network.

Examples:

-  Instagram 
-  Netflix 
-  Online banking 
-  E-commerce websites 

---

## 2. Client–Server Interaction

Communication generally happens between a **client** and a **server**.

```
Client  ──────── Request ────────>  Server
Client  <─────── Response ────────  Server
```

### Client

The **client** is the system that **initiates communication** by sending a request.

Examples:

-  Mobile applications 
-  Web browsers / web-based consoles 
-  Laptops 

### Server

The **server** is the system that **receives the request** and processes it.

It can:

-  Process the request 
-  Access data 
-  Perform business logic 
-  Send a response back to the client 

### Important

A server can **also act as a client** when it needs to request information from another server/service.

```
Client → Server A → Server B
             ↑
       Server A is a
       client to B
```

---

# 3. Requirements for Communication

For client-server communication, the communication mechanism should ideally be:

1. **Language independent** 
   -  Client and server can be written in different programming languages. 
2. **Fast** 
   -  Communication should have low overhead and reasonable latency. 
3. **Network compatible** 
   -  It should enable communication over a network. 
4. **Lightweight** 
   -  Communication should avoid unnecessary complexity and overhead. 

---

# 4. REST

**REST** stands for:

> **REpresentational State Transfer**

REST is an **architectural style / set of principles** used for designing network-based applications and APIs.

### Key idea

REST provides a standard way for a client and server to communicate with resources over a network.

---

# 5. REST API

A server exposes a **REST API** so that clients can interact with its resources.

Think of it as:

```
Client
   ↓
REST API
   ↓
Server
   ↓
Resources / Data
```

A REST API uses two important things:

```
REST API
   ├── URI
   └── HTTP Verb
```

---

# 6. URI

**URI = Uniform Resource Identifier**

A URI identifies the **resource** that the client wants to interact with.

Example:

```
173.76.310.45:7001/ios/nflx/plan-listing
```

Here, the URI identifies the required resource.

In practice, REST APIs commonly use resource-oriented paths such as:

```
/users
/users/101
/products
/products/25
```

### Remember

> **URI → Which resource?**

---

# 7. HTTP Verbs

The HTTP method/verb specifies **what operation should be performed** on the resource.

The four commonly taught CRUD mappings are:

| CRUD Operation | HTTP Verb | Meaning |
| ------------------------------ | -------- | ------------------------- |
| **Create**                     | `POST`   | Create a new resource     |
| **Read**                       | `GET`    | Retrieve/read a resource  |
| **Update**                     | `PUT`    | Update/replace a resource |
| **Delete**                     | `DELETE` | Delete a resource         |

### Easy way to remember

```
POST    → Create
GET     → Read
PUT     → Update
DELETE  → Delete
```

---

# 8. REST API Request

A REST request can be thought of as:

```
HTTP Verb + URI
```

For example:

```
GET /users/101
```

means:

> "Give me the user resource with ID 101."

Another example:

```
POST /users
```

means:

> "Create a new user."

---

# 9. CRUD + REST — Quick Revision

Suppose we have a resource called `users`.

### Create

```
POST /users
```

Creates a new user.

### Read

```
GET /users
```

Gets users.

```
GET /users/101
```

Gets the user with ID `101`.

### Update

```
PUT /users/101
```

Updates the user with ID `101`.

### Delete

```
DELETE /users/101
```

Deletes the user with ID `101`.

---

# 10. Most Important Points

```
Web Application
      ↓
Client ↔ Server
      ↓
Communication over Network
      ↓
REST
      ↓
REST API
      ↓
URI + HTTP Verb
```

### Remember these two questions:

**URI answers:**

> "Which resource do I want?"

**HTTP verb answers:**

> "What do I want to do with that resource?"

# SOA — Service-Oriented Architecture

## 1. Definition

**Service-Oriented Architecture (SOA)** is a style of architecture that promotes loose coupling and granular applications to make the components of software reusable.

### Key Idea

SOA divides an application into independent services that can communicate with each other.

-  **Loose Coupling** → Services have minimal dependency on each other. 
-  **Reusability** → Services can be reused by different applications. 
-  **Granularity** → Application functionality is divided into smaller services. 

---

## 2. Advantages of SOA

### 1. Selective Scaling

Individual services can be scaled independently according to their requirements.

**Example:**

> If the payment service receives heavy traffic, only the payment service can be scaled instead of the entire application.

### 2. Different Technology Stacks

Different services can be developed using different technologies or programming languages.

**Example:**

```
Java Service  <----->  SOA  <----->  Django/Python Service
```

This provides flexibility in choosing technologies for different services.

### 3. Loose Coupling

Services are independent of each other, so changes in one service have minimal impact on other services.

**Benefit:** Easier maintenance and modification.

### 4. Agile

SOA supports agile development because services can be developed, tested, deployed, and modified independently.

---

## 3. Disadvantages of SOA

### 1. Higher Latency

Communication between services can introduce network overhead, resulting in higher latency compared to communication within a single application.

```
Service A → Network → Service B
             ↑
        Extra latency
```

### 2. Complex to Secure

Multiple independent services and communication channels make security management more complex.

Security may need to be handled across:

-  Multiple services 
-  APIs 
-  Network communication 
-  Authentication and authorization 

### 3. Cascading Failures

Failure of one service can potentially affect other dependent services.

```
Service A → Service B → Service C
               ↓
             Failure
               ↓
       Other services affected
```

### 4. Complex Understanding

A system containing many independent services can be difficult to understand and manage.

Developers need to understand:

-  Individual services 
-  Service dependencies 
-  Communication between services 
-  Overall system behavior 

---

## Quick Revision

| Advantages | Disadvantages |
| --- | --- |
| Selective scaling | Higher latency |
| Different technology stacks | Complex to secure |
| Loose coupling | Cascading failures |
| Agile development | Complex understanding |

---

## Remember

> **SOA = Services + Loose Coupling + Reusability**

**Advantages:**

```
Scale selectively → Use different tech → Loose coupling → Agile
```

**Disadvantages:**

```
Latency → Security complexity → Cascading failures → Complex understanding
```

# SOA — Service-Oriented Architecture

## 1. Definition

**Service-Oriented Architecture (SOA)** is a style of architecture that promotes loose coupling and granular applications to make the components of software reusable.

### Key Idea

SOA divides an application into independent services that can communicate with each other.

-  **Loose Coupling** → Services have minimal dependency on each other. 
-  **Reusability** → Services can be reused by different applications. 
-  **Granularity** → Application functionality is divided into smaller services. 

---

## 2. Advantages of SOA

### 1. Selective Scaling

Individual services can be scaled independently according to their requirements.

**Example:**

> If the payment service receives heavy traffic, only the payment service can be scaled instead of the entire application.

### 2. Different Technology Stacks

Different services can be developed using different technologies or programming languages.

**Example:**

```
Java Service  <----->  SOA  <----->  Django/Python Service
```

This provides flexibility in choosing technologies for different services.

### 3. Loose Coupling

Services are independent of each other, so changes in one service have minimal impact on other services.

**Benefit:** Easier maintenance and modification.

### 4. Agile

SOA supports agile development because services can be developed, tested, deployed, and modified independently.

---

## 3. Disadvantages of SOA

### 1. Higher Latency

Communication between services can introduce network overhead, resulting in higher latency compared to communication within a single application.

```
Service A → Network → Service B
             ↑
        Extra latency
```

### 2. Complex to Secure

Multiple independent services and communication channels make security management more complex.

Security may need to be handled across:

-  Multiple services 
-  APIs 
-  Network communication 
-  Authentication and authorization 

### 3. Cascading Failures

Failure of one service can potentially affect other dependent services.

```
Service A → Service B → Service C
               ↓
             Failure
               ↓
       Other services affected
```

### 4. Complex Understanding

A system containing many independent services can be difficult to understand and manage.

Developers need to understand:

-  Individual services 
-  Service dependencies 
-  Communication between services 
-  Overall system behavior 

---

## Quick Revision

| Advantages | Disadvantages |
| --- | --- |
| Selective scaling | Higher latency |
| Different technology stacks | Complex to secure |
| Loose coupling | Cascading failures |
| Agile development | Complex understanding |

---

## Remember

> **SOA = Services + Loose Coupling + Reusability**

**Advantages:**

```
Scale selectively → Use different tech → Loose coupling → Agile
```

**Disadvantages:**

```
Latency → Security complexity → Cascading failures → Complex understanding
```

# Microservices Architecture — Revision Notes

## 1. Definition

**Microservices Architecture** is an evolved version of SOA (Service-Oriented Architecture) that promotes software components to be loosely coupled.

It is a highly granular architecture design where each service is completely independent of the other services.

### Key Idea

```
Application
    ↓
┌─────────────┐
│ Microservice│
├─────────────┤
│ Microservice│
├─────────────┤
│ Microservice│
└─────────────┘
```

Each microservice performs a specific functionality and operates independently.

---

## 2. Microservices vs SOA

| Aspect | SOA | Microservices |
| --- | --- | --- |
| Data Storage | Services can share data storage | Each microservice has separate and independent data storage |
| Scalability | Less scalable architecture | Highly scalable architecture |
| Deployment | Deployment is time-consuming | Deployment is easy and less time-consuming |
| Main Focus | Focused on maximizing application service reusability | More focused on decoupling |
| Granularity | Service-oriented | More granular |
| Independence | Services may have dependencies | Each service is completely independent |

---

## 3. Key Points to Remember

**Microservices**

-  Evolved version of SOA 
-  Promotes loose coupling 
-  Highly granular 
-  Each service is independent 
-  Each microservice can have separate data storage 
-  Highly scalable 
-  Easy and less time-consuming deployment 
-  Focuses more on decoupling 

### Quick Memory Trick

```
Microservices = Independent + Granular + Decoupled + Scalable + Easy Deployment
```

# Tier Architecture — Revision Notes

## 1. Tier Architecture

A web application can be designed according to **n-tier architecture**.

In n-tier architecture, tiers are different layers of the application architecture.

A **tier** is a logical separation between different components of an application.

### Why Tier Architecture?

Tier architecture helps to:

-  Make modifications and updates of different components easier. 
-  Assign dedicated tasks and roles to each component. 
-  Separate different responsibilities of the application. 

---

## 2. 1-Tier Architecture

In 1-tier architecture, all major components of the application are present within one tier.

The components shown are:

-  User Interface / Presentation 
-  Application / Program Logic 
-  Database / Data 

```
+-------------------------------------------+
|              1-Tier Application           |
|                                           |
|   UI  +  Application Logic  +  Database  |
|                                           |
+-------------------------------------------+
```

### Key Point

Presentation, application logic, and data are combined into a single tier.

---

## 3. 2-Tier Architecture

In 2-tier architecture, the application is divided into two tiers.

### Tier 1 — Client

Contains the:

-  User Interface / Presentation 

### Tier 2 — Server

Contains:

-  Application / Program Logic 
-  Database / Data 

```
+-------------------+       +---------------------------+
|       Client      |       |          Server           |
|                   |       |                           |
|   User Interface  | <--> |  Application Logic        |
|                   |       |  Database                 |
+-------------------+       +---------------------------+
```

### Key Point

The presentation layer is separated from the application and data components.

---

## 4. 3-Tier Architecture

In 3-tier architecture, the application is divided into three separate tiers.

### Tier 1 — Presentation Tier

-  Handles the User Interface (UI). 
-  Interacts with the user. 

### Tier 2 — Application / Business Logic Tier

-  Contains the application logic. 
-  Processes requests and performs the required operations. 

### Tier 3 — Data Tier

-  Contains the database / data. 
-  Responsible for storing and retrieving data. 

```
+-------------------+
| Presentation Tier |
|     (UI)          |
+-------------------+
          |
          v
+-------------------+
| Application Tier  |
| (Business Logic)  |
+-------------------+
          |
          v
+-------------------+
|     Data Tier     |
|    (Database)     |
+-------------------+
```

### Key Point

Each tier has a dedicated responsibility, making the application easier to modify, update, and maintain.

---

## Quick Comparison

| Architecture | Number of Tiers | Main Separation |
| --- | --- | --- |
| 1-Tier | 1 | UI + Application Logic + Database |
| 2-Tier | 2 | Client/UI + Server/Application & Database |
| 3-Tier | 3 | Presentation + Application Logic + Database |

---

## Remember

-  **1-Tier:** Everything together 
-  **2-Tier:** Client + Server 
-  **3-Tier:** Presentation + Business Logic + Data
