# 🛡️ Reliability in System Design

## 🧠 What Is Reliability?

A reliable service consistently performs its intended function correctly, timely, and resiliently, even under adverse conditions. A reliable system minimizes failures, handles errors gracefully, and maintains trust over time.

resilient means without fault.
---

## 📚 Why Reliability Matters

- **User Trust**: Predictable behavior builds confidence.
- **Business Continuity**: Reduces downtime and revenue loss.
- **Compliance**: Critical in regulated industries (e.g., finance, healthcare).
- **Brand Reputation**: Reliable systems enhance credibility.

---

## 🔍 Reliability vs Availability

| Attribute     | Reliability                          | Availability                          |
|---------------|--------------------------------------|----------------------------------------|
| Definition    | Ability to perform correctly over time | Ability to remain operational at a point in time |
| Measurement   | MTBF, MTTR, failure rate             | Uptime percentage (e.g., 99.99%)       |
| Focus         | Correctness and continuity           | Reachability and responsiveness        |
| Dependency    | Requires availability                | Can exist without full reliability     |

---

## 📏 Key Metrics

- **MTBF (Mean Time Between Failures)**  
  `MTBF = Total Operational Time / Number of Failures`  
  Higher MTBF = better reliability.

- **MTTR (Mean Time to Repair)**  
  `MTTR = Total Downtime / Number of Failures`  
  Lower MTTR = faster recovery.

- **Failure Rate**  
  `Failure Rate = Number of Failures / Time`

- **SLOs (Service Level Objectives)**  
  Define reliability goals (e.g., 99.9% success rate for API calls).

---

## 🧰 Strategies to Achieve Reliability

### 1. Redundancy
- Duplicate critical components (servers, databases, load balancers).
- Use active-active or active-passive configurations.

### 2. Fault Tolerance
- Detect and recover from failures automatically.
- Use retries, circuit breakers, and fallback mechanisms.

### 3. Monitoring & Alerting
- Track system health, latency, and error rates.
- Use tools like Prometheus, Grafana, ELK stack.

### 4. Graceful Degradation
- Maintain partial functionality during failures.
- Example: Show cached data if live service fails.

### 5. Load Balancing
- Distribute traffic to prevent overload.
- Route around failed or slow nodes.

### 6. Chaos Engineering
- Inject failures to test system resilience.
- Example: Netflix’s Chaos Monkey.

### 7. Idempotency
- Ensure repeated operations don’t cause unintended effects.
- Critical for retries and fault recovery.

---

## 🧪 Real-World Examples

- **Payment Gateway**: Must process transactions correctly even under load or partial failure.
- **Pipeline Executor for Fab Tools**: Should safely coordinate wafer transfers with fallback and retry logic.

---

## 🧠 Interview Tips

- Define reliability clearly and contextually.
- Identify failure points and mitigation strategies.
- Apply strategies (redundancy, retries, monitoring).
- Discuss trade-offs (cost, complexity, latency).
- Back it up with metrics and real-world examples.
- Highlight how reliability impacts user experience and business goals.

---
