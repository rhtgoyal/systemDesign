## 📌 What Is High Availability?

**High Availability (HA)** means a system should be up and running with minimal downtime.

- **Availability** is measured as the percentage of time a system is up and running.
- **High availability** typically refers to **99.9% uptime or higher** (less than 8.76 hours of downtime per year).

```

## 📊 Availability Levels & Downtime

| Availability % | Max Downtime/Year | Nickname     |
|----------------|-------------------|--------------|
| 99%            | ~3.65 days        | Two nines    |
| 99.9%          | ~8.76 hours       | Three nines  |
| 99.99%         | ~52.6 minutes     | Four nines   |
| 99.999%        | ~5.26 minutes     | Five nines   |

```

## 🚀 How to Achieve High Availability

High availability is achieved by building **redundancy**, **eliminating single point of failure**, and **failover mechanism** into your system. 
### 🔥 Must-Know: Load Balancer has all above capability, so high availability can be acheived using Load Balancer. Whenever in system design, need high availability, use load balancer.
Key strategies include:

### 1. 🔁 Eliminate Single Points of Failure
- Use **multiple servers** behind a load balancer.
- If one server fails, others continue serving traffic.

### 2. 🩺 Health Checks & Failover
- Continuously monitor server health.
- Automatically reroute traffic away from unhealthy nodes.

### 3. 🧱 Redundant Load Balancers
- Use **active-passive** or **active-active** configurations.
- Prevent downtime if the load balancer itself fails.

### 4. 🌍 Geographic Load Balancing
- Route users to the **nearest data center**.
- Reduces latency and isolates regional failures.

### 5. 📊 Monitoring & Alerts
- Use tools like **Prometheus**, **Grafana**, or **Datadog**.
- Detect problems early and respond quickly.

---

## 💸 Cost of High Availability

High availability comes with trade-offs in **budget**, **complexity**, and **consistency**.

### 1. 🏗️ Infrastructure Redundancy
- Multiple servers across zones or regions.
- Backup databases and replicated storage.
- Load balancers and failover systems.

### 2. ⚖️ Consistency Trade-offs
- **CAP Theorem**: You may sacrifice consistency to maintain availability during network partitions.
- **Strong consistency** requires coordination → increases latency.
- **Eventual consistency** improves availability → may return stale (old) data.

### 3. 🔧 Operational Complexity
- Requires robust monitoring and alerting systems.
- Automated failover and recovery scripts.
- **Chaos engineering** to test system resilience.

---

## 🎯 Interview Insight: Coordinating Software Upgrades

**Challenge**: How do you upgrade software across hundreds of servers without downtime?

### ✅ Key Requirements
When you have dozens or hundreds of servers, upgrading them without affecting availability or consistency requires:
- Careful orchestration
- Version compatibility
- Traffic routing control
- Monitoring and rollback mechanisms

### 🛠️ Deployment Strategies

### 1. Blue-Green Deployment
- Maintain two environments: Blue (current) and Green (new).
- Deploy new version to Green, test it, then switch all traffic from Blue to Green.
- If issues arise, rollback is instant by switching back.

### 2. Canary Deployment
- Deploy canary pod: You create a new pod (or set of pods) with the updated version. These pods have a different label (e.g., version: v2).
- Service Routing via Labels: Your Kubernetes Service uses selectors to route traffic. You can:
    - Create a separate service for the canary pods.
    - ANd split traffic between v1 and v2.
- The original pod (v1) stays in the load balancer pool. The canary pod (v2) is added with controlled traffic — say, 5%.
- Monitor for errors, latency, or regressions.
- If the canary performs well, you scale it up and eventually replace the old pods. 
- If it fails, you simply stop routing traffic to it. (Rollback policy)

### 3. Rolling Update with Load Balancer Coordination
- New Pod Is Created: Kubernetes spins up a new pod with the updated version.
- Health Checks & Readiness Probes: The pod must pass health checks (readinessProbe) before it’s marked as “ready.”
- Load Balancer Adds Pod to Pool Once ready: the pod is automatically added to the load balancer’s target pool.
- Old Pod Is Terminated After the new pod is serving traffic: Kubernetes gracefully shuts down one old pod.
- Repeat Until Complete This cycle continues until all old pods are replaced.

### 🔥 Must-Know: Caanry Deployement is idle for high-risk updates as only small percentage of traffic is routed to upgraded servers initially. Also canary gives you good control on traffic exposure, and rollback. It is just you need some traffic routing logic to use canary deploymen. While Rolling update, exposes the upgraded server to full traffic immedietly once it passes health checks. Great for routine updates where risk is low and downtime must be avoided.

### 4. Version Compatibility
- Design services to be backward compatible.
- Use feature flags to toggle new behavior without full redeploy.
- Ensure DB schema changes are non-breaking (e.g., additive changes).

---

## 🧠 Summary

High availability is essential for modern systems, especially those serving global users or critical workloads. Achieving it requires:
- Redundant infrastructure
- Smart traffic routing
- Careful deployment strategies
- Monitoring and recovery planning

Understanding these principles is key for system design interviews and real-world reliability engineering.

