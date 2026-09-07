# CQRS — Revision Notes

## 1. Understanding CQRS

Command Query Responsibility Segregation (CQRS) is an architectural pattern that separates:

- **Command side** → responsible for creating, updating, and deleting data.
- **Query side** → responsible strictly for reading data.

The main goal is to allow the read and write sides of a system to be optimized and scaled independently.

## 2. Problem with Traditional CRUD

In a traditional CRUD-based system, both read and write operations are performed on the same database.

```
                 ┌──────────────┐
                 │   Client     │
                 └──────┬───────┘
                        │
                ┌───────▼────────┐
                │   Application  │
                └───────┬────────┘
                        │
              ┌─────────▼─────────┐
              │   Single Database │
              └───────────────────┘
                  ▲           ▲
                Reads       Writes
```

**Problems**
- Read and write workloads compete for the same database resources.
- Updates can cause database lock contention.
- Heavy read operations can affect write performance.
- Scaling the read and write workloads independently becomes difficult.
- A single database may not be optimal for both reading and writing.

## 3. Core Idea of CQRS

CQRS separates the command model from the query model.

```
                    Client
                      │
             ┌────────┴────────┐
             │                 │
          Command            Query
             │                 │
             ▼                 ▼
      ┌─────────────┐   ┌─────────────┐
      │ Command Side│   │  Query Side │
      └──────┬──────┘   └──────┬──────┘
             │                 │
             ▼                 ▼
       Write Database     Read Database
```

This allows both sides to be designed according to their specific requirements.

## 4. Command Side

The command side handles operations that modify data.

Examples:
- Create
- Update
- Delete

Before persisting the change, the command side typically performs:
- Validation
- Authorization
- Business-rule checks

```
Command
   │
   ▼
Validation
   │
   ▼
Authorization
   │
   ▼
Write Database
```

## 5. Query Side

The query side handles only read operations.

Its primary goal is to provide fast and efficient reads.

A query-side database can use denormalized data so that data required for a query is already available together.

This reduces the need for expensive JOIN operations.

```
                 Query
                   │
                   ▼
             Query Model
                   │
                   ▼
              Read DB
                   │
                   ▼
              Response
```

## 6. Independent Scaling

One of the major benefits of CQRS is that the read and write sides can be scaled independently.

```
                 Application
                     │
             ┌───────┴────────┐
             │                │
          Commands          Queries
             │                │
             ▼                ▼
       Command Side       Query Side
             │                │
             ▼                ▼
        Write DB           Read DB
```

For example:
- If the system receives many more reads, scale the query side.
- If the system receives many writes, scale the command side.
- Different technologies can be used for the read and write databases.

## 7. Eventual Consistency

Since the command and query sides use separate databases, the read database may not be updated immediately after a write.

This results in eventual consistency.

```
        Command
           │
           ▼
     Command Side
           │
           ▼
      Write DB
           │
           │ Event
           ▼
     Message Queue
           │
           ▼
      Query Side
           │
           ▼
       Read DB
```

The typical flow is:
1. A command modifies the write database.
2. An event is generated.
3. The event is sent through a message queue.
4. The query side receives the event.
5. The read database is updated.

Therefore, the read model eventually becomes consistent with the write model.

## 8. CQRS with Event Sourcing

CQRS can naturally be combined with Event Sourcing.

In Event Sourcing, instead of storing only the current state, the system stores the history of changes as an append-only event log.

For example:

```
Event 1 → Account Created
Event 2 → Money Deposited
Event 3 → Money Withdrawn
Event 4 → Money Deposited
```

The current state can be reconstructed by replaying these events.

```
             Event Store
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
    Event 1     Event 2    Event 3
       │          │          │
       └──────────┼──────────┘
                  ▼
             Event Replay
                  │
                  ▼
           Current State
```

Technologies such as Kafka can be used as part of an event-driven architecture for maintaining an event log.

## 9. System Recovery

One advantage of Event Sourcing with CQRS is that a read database can be reconstructed if it becomes corrupted or needs to be rebuilt.

```
          Event Log
              │
              ▼
        Replay Events
              │
              ▼
        Rebuild Read DB
              │
              ▼
        Restored State
```

Instead of manually recovering the entire read database, the system can replay the stored events and regenerate the required state.

## 10. CQRS vs Traditional CRUD

| Traditional CRUD | CQRS |
|---|---|
| Read and write use the same model/database | Read and write are separated |
| Shared database resources | Separate read and write models |
| Difficult to scale reads and writes independently | Reads and writes can scale independently |
| Usually simpler | More complex |
| Less infrastructure overhead | More infrastructure overhead |
| Suitable for simple applications | Suitable for complex, large-scale systems |

## 11. When to Use CQRS

CQRS is useful when:
- Read and write workloads are significantly different.
- Read and write operations have different latency requirements.
- Read and write workloads have different throughput requirements.
- Independent scaling is important.
- The system is large and complex.
- Denormalized read models can significantly improve query performance.
- Event-driven architecture is already being used.

**When NOT to Use CQRS**

CQRS is not recommended for simple applications because it introduces significant architectural complexity and infrastructure overhead.

## 12. AWS Implementation

In an AWS-based architecture, CQRS can be implemented using services such as:
- **API Gateway** → receives and routes requests.
- **Load Balancer** → distributes traffic and supports horizontal scaling.
- **SQS** → handles asynchronous communication between components.
- **Lambda** → processes events asynchronously.
- **Separate databases** → maintain the write model and read model.

```
                         Client
                           │
                           ▼
                    ┌─────────────┐
                    │ API Gateway │
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │             │
                 Command        Query
                    │             │
                    ▼             ▼
             ┌────────────┐  ┌────────────┐
             │ Write Side │  │  Read Side │
             └─────┬──────┘  └─────┬──────┘
                   │               │
                   ▼               ▼
              Write DB          Read DB
                   │
                   ▼
                  SQS
                   │
                   ▼
                Lambda
                   │
                   ▼
                Read DB
```

## 13. Key Takeaways

- CQRS = Command Query Responsibility Segregation.
- Commands change data; queries read data.
- CQRS separates the write model from the read model.
- Read and write databases can be scaled independently.
- The read model can use denormalized data for faster queries.
- Separate databases commonly result in eventual consistency.
- CQRS can be combined with Event Sourcing.
- Event Sourcing stores changes as an append-only event log.
- Events can be replayed to reconstruct state.
- CQRS is powerful for large and complex systems, but adds significant complexity.
