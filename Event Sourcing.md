# Event Sourcing — Revision Notes

## 1. What is Event Sourcing?

Event Sourcing is a system design pattern where a sequence of events is treated as the source of truth, instead of storing only the current state of an object.

**Traditional Approach**

The database stores the latest state:

```
User
 └── Balance = ₹500
```

When the balance changes, the old value is overwritten.

**Event Sourcing Approach**

Every change is stored as an immutable event:

```
Event 1: Account Created
Event 2: ₹100 Deposited
Event 3: ₹50 Deposited
Event 4: ₹30 Withdrawn
```

The current state is obtained by processing these events.

## 2. Problem with Traditional CRUD

In a traditional CRUD system:

```
Create → Read → Update → Delete
```

the database usually stores the current state of an entity.

For example:

```
Initial:
Balance = ₹1000

        ↓ Deposit ₹500

Balance = ₹1500

        ↓ Withdraw ₹300

Balance = ₹1200
```

The previous values are generally overwritten.

### Problems

**1. Loss of Historical Context**

Once data is updated, the previous state may no longer be available.

```
Before:
Balance = ₹1000

After:
Balance = ₹1500
```

The database primarily contains ₹1500, not the complete sequence of changes.

**2. Database Locks and Bottlenecks**

Frequent updates can require database locks.

```
Request A ──┐
            ↓
         Database
            ↑
Request B ──┘
```

Concurrent updates can lead to:
- Database contention
- Bottlenecks
- Concurrency issues
- Race conditions

**3. Synchronization Problems**

Modern systems often perform work asynchronously using background processes.

Example:

```
Upload Video
     ↓
 Database Update
     ↓
Video Processing
     ↓
Thumbnail Generation
     ↓
Notification
```

If one database operation fails while another process has already acted, different components can have inconsistent states.

```
Database
Status = PROCESSING

        ≠

Video Worker
Status = COMPLETED
```

The system can become out of sync.

## 3. Event Sourcing Architecture

Instead of continuously modifying the same database row, every change is recorded as an event.

```
             Commands
                ↓
          Application
                ↓
        ┌───────────────┐
        │  Event Store  │
        │ Append Only   │
        └───────────────┘
                ↓
        Event 1
        Event 2
        Event 3
        Event 4
                ↓
       Current Application
             State
```

The events become the source of truth.

## 4. Event Log

An event log is an ordered collection of events representing changes made to an entity.

Events are:
- **Immutable** — once written, they are not modified.
- **Append-only** — new events are added to the end.
- **Historical** — previous changes remain available.

Example:

```
Event Log

1. AccountCreated
2. MoneyDeposited ₹1000
3. MoneyDeposited ₹500
4. MoneyWithdrawn ₹300
```

Current balance:

```
₹0
 + ₹1000
 + ₹500
 - ₹300
 ─────────
 = ₹1200
```

The current state is derived from the event history.

## 5. Hydration

Hydration is the process of reconstructing the current application state by replaying historical events in order.

```
Event 1 ──→
Event 2 ──→   Replay Events   ──→ Current State
Event 3 ──→
Event 4 ──→
```

Example:

```
AccountCreated
      ↓
Balance = 0

Deposited ₹1000
      ↓
Balance = ₹1000

Deposited ₹500
      ↓
Balance = ₹1500

Withdrawn ₹300
      ↓
Balance = ₹1200
```

So:

```
Current State = Result of replaying all relevant events
```

## 6. Auditability

Because every change is stored as an event, the system has a complete history of what happened.

Example:

```
10:00 → Account Created
10:05 → ₹1000 Deposited
10:10 → ₹500 Deposited
10:15 → ₹300 Withdrawn
```

This makes it easier to:
- Track changes
- Debug problems
- Determine what happened
- Identify when a state changed
- Reconstruct previous states

## 7. Time Travel

Event Sourcing allows the system to reconstruct the state of an entity at a particular point in time.

For example:

```
Event 1 ── Event 2 ── Event 3 ── Event 4 ── Event 5
                    ↑
              Replay until here
                    ↓
              State at that time
```

You can replay events only up to a particular point to determine what the state looked like then.

This can be useful for:
- Debugging
- Historical analysis
- Auditing
- "Time machine" functionality

## 8. Performance Problem

A major problem with Event Sourcing is that replaying every event can become expensive.

Suppose an account has:

```
10,000,000 events
```

Replaying all 10 million events every time the current balance is needed would be inefficient.

**Solution: Store a Materialized Current State**

The system can maintain the current state separately.

```
             Event Log
                 ↓
              Events
                 ↓
        ┌─────────────────┐
        │ State Processor  │
        └─────────────────┘
                 ↓
        PostgreSQL / DB
        Current State
```

The event log remains the source of truth, while the separate database provides fast access to the current state.

## 9. Event Ordering

Event ordering is extremely important.

Consider:

```
Event 1: Deposit ₹100
Event 2: Withdraw ₹50
```

Correct order:

```
Deposit ₹100
      ↓
Balance = ₹100
      ↓
Withdraw ₹50
      ↓
Balance = ₹50
```

If events are processed in the wrong order, the resulting state may be incorrect.

Therefore:

> Events belonging to the same entity should be processed in the correct order.

## 10. Kafka for Event Ordering

Kafka can be used to maintain event ordering.

Kafka uses:
- Topics
- Partitions
- Consumer Groups

Events for a particular entity can be routed to the same partition.

```
                 Kafka Topic
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
      Partition 0 Partition 1 Partition 2
          │
          ↓
       Consumer
```

For example, events belonging to User-123 can consistently be placed in the same partition.

```
User-123 Event 1 ─┐
User-123 Event 2 ─┼──→ Same Partition
User-123 Event 3 ─┘
```

This allows the events for that entity to be processed sequentially.

**Consumer Groups**

A Consumer Group allows multiple consumers to process partitions while maintaining the partition's ordering guarantees.

## 11. Event Sourcing + CQRS

CQRS (Command Query Responsibility Segregation) separates:
- **Commands** → operations that change data
- **Queries** → operations that read data

```
                 Client
                /      \
               ↓        ↓
          Commands     Queries
              ↓           ↓
        Write Model    Read Model
              ↓           ↑
         Event Store ─────┘
```

Event Sourcing and CQRS are often used together.

**Why?**

Event Sourcing focuses on:
- How changes are stored

CQRS focuses on:
- Separating writes from reads

This allows read and write sides to be optimized independently.

## 12. Event Sourcing vs Traditional CRUD

| Feature | Traditional CRUD | Event Sourcing |
|---|---|---|
| Source of truth | Current database state | Event history |
| Updates | Existing data is modified | New events are appended |
| History | Often lost/limited | Complete history available |
| Debugging | More difficult | Replay events |
| Auditability | Limited | Strong |
| Previous state | Not always available | Can be reconstructed |
| Current state | Stored directly | Derived from events |
| Complexity | Simpler | More complex |

## 13. Key Takeaways

- Event Sourcing stores changes as immutable events rather than only storing the latest state.
- The event log is the source of truth.
- Events are stored in an append-only manner.
- Hydration reconstructs the current state by replaying events.
- Event history provides strong auditability and enables time travel.
- Replaying a very large event log can be expensive, so a materialized/current-state database can be maintained.
- Event ordering is critical to avoid incorrect state reconstruction.
- Kafka partitions can help ensure ordered processing for events belonging to the same entity.
- CQRS is commonly combined with Event Sourcing to separate write and read responsibilities.
