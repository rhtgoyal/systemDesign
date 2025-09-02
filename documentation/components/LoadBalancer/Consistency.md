# 📘 Consistency in System Design

## 🧠 What Is Consistency?

Consistency refers to the property that ensures all nodes in a distributed system reflect the same data view at any given time. It guarantees that every read receives the most recent write or a clearly defined version of the data.

---

## 📚 Why Consistency Matters

- **Correctness**: Prevents stale or conflicting data.
- **Data Integrity**: Avoids corruption and duplication.
- **Concurrency Control**: Coordinates updates across clients.
- **User Experience**: Provides smooth and logical interactions.

---

## 🔺 CAP Theorem Context

In distributed systems, you can only guarantee **two out of three**:

| Property         | Description                                      |
|------------------|--------------------------------------------------|
| Consistency      | All nodes see the same data at the same time     |
| Availability     | Every request receives a response (success/fail) |
| Partition Tolerance | System continues to operate despite network splits |

> ⚠️ Trade-off: Systems must choose between consistency and availability during network partitions.

---

## 🧩 Types of Consistency Models

### 1. Strong Consistency
- Guarantees that every read reflects the most recent write.
- Requires coordination between nodes.
- **Example**: Traditional SQL databases with synchronous replication.

### 2. Eventual Consistency
- All replicas converge to the same value over time.
- Allows temporary divergence.
- **Example**: Amazon DynamoDB, Cassandra.

---

## 🛠️ Techniques to Achieve Consistency

- **Replication Protocols**: Leader-based (Raft, Paxos), quorum-based reads/writes.
- **Versioning**: Use timestamps or vector clocks to track updates.
- **Conflict Resolution**: Last-write-wins, merge functions, manual intervention.
- **Idempotency**: Ensure repeated operations don’t cause unintended effects.
- **Distributed Transactions**: Two-phase commit, saga patterns.

---

## 📏 Consistency vs Other Attributes

| Attribute     | Focus                        | Can coexist with consistency? |
|---------------|------------------------------|-------------------------------|
| Availability  | System responsiveness         | Sometimes (depends on CAP)    |
| Reliability   | Correct and continuous behavior | ✅ Yes                        |
| Durability    | Data persistence              | ✅ Yes                        |
| Performance   | Speed and responsiveness      | ⚠️ May be impacted            |

---

## 🧪 Interview Tips

- Define consistency clearly and contextually.
- Discuss trade-offs with availability and latency.
- Mention real-world systems and their consistency models.
- Use diagrams or examples to illustrate replication and conflict resolution.
- Highlight how consistency impacts user experience and correctness.

---

## 📎 References

- [Consistency in System Design – GeeksforGeeks](https://www.geeksforgeeks.org/system-design/consistency-in-system-design/)
- [System Design Guideline – GitHub Architecture Playbook](https://github.com/rinkachi9/system-design-guideline)

