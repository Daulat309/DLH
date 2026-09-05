
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
